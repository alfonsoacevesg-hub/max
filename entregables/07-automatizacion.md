# 07. Blueprint de Automatización e Integraciones — E-commerce de Mobiliario (CDMX/EdoMéx + Nacional)

## Resumen ejecutivo

Este documento es la especificación técnica que Claude Code (en el proyecto de implementación) debe construir sin ambigüedad para operar el flujo venta → confirmación de existencia → entrega → postventa con la mínima intervención manual de Alfonso y Adriana. El sistema se apoya en cuatro piezas: (1) un escenario de Make.com que orquesta la venta y coordina con Mundo In y Lalamove; (2) un servicio propio (CarrierService de Shopify) que cotiza en tiempo real Lalamove (local, +5% de fee) y Skydropx (nacional, tarifa neta); (3) una página de rastreo en vivo alimentada por webhooks de Lalamove; y (4) facturación CFDI automatizada. Se verificó documentación vigente de Lalamove API (v3) y Shopify CarrierService API al 08-jul-2026; las URLs se citan en cada sección. El hallazgo crítico: **Mundo In no publica una API ni catálogo B2B en línea** — el catálogo se obtiene por WhatsApp bajo pedido — por lo que la sincronización de existencias arranca obligatoriamente en el nivel de menor automatización (lista compartida manual) y el "webhook o palabra clave" de confirmación de existencia debe construirse sobre WhatsApp Business, no sobre una API del mayorista.

Costo incremental mensual de la automatización (fuera de los $9,300 MXN de costos fijos ya presupuestados, que ya incluyen Make Core): **~$150–450 MXN/mes** adicionales por WhatsApp Business (mensajería utilitaria) y facturación CFDI, dependiendo del volumen (ver tabla de costos). A 5–14 ventas/mes, el consumo de operaciones de Make se mantiene dentro del plan Core (10,000 operaciones/mes, $10.59 USD) ya presupuestado. Fuente de pricing Make: Make.com, plan Core $10.59 USD/mes por 10,000 operaciones (consultado 08-jul-2026, https://www.make.com/en/pricing).

---

## 1. Escenario Make principal: venta → entrega

### 1.1 Diagrama de flujo (texto, paso a paso, con módulos exactos de Make)

```
[TRIGGER] Shopify — Watch New Orders (módulo nativo "Shopify > Watch Orders", filtro: financial_status = paid)
   │
   ├─(1 op)→ [Router] Make Router — 3 ramas en paralelo:
   │
   ├── RAMA A: Notificación interna
   │     ├─(1 op) WhatsApp Business Cloud API (HTTP > Make a request, POST a graph.facebook.com)
   │     │        → mensaje a Alfonso: "Nueva venta #{order.name}, {order.line_items}, ${order.total_price} MXN, cliente {order.shipping_address}"
   │     ├─(1 op) WhatsApp Business Cloud API → mismo mensaje a Adriana
   │     └─(1 op) Email (Gmail/Microsoft 365 > Send an Email) a ambos como respaldo si WhatsApp falla (ver 1.3)
   │
   ├── RAMA B: Notificación a Mundo In + espera de confirmación
   │     ├─(1 op) WhatsApp Business Cloud API → mensaje a vendedor de Mundo In con SKU(s), cantidad, folio de orden y
   │     │        instrucción: "Responde CONFIRMADO o SIN-STOCK a este mensaje"
   │     ├─(1 op) Email de respaldo al correo del vendedor (mismo contenido, con Reply-To dedicado)
   │     ├─(1 op) Data Store — Make (crear registro): order_id, sku, estado="esperando_confirmacion", timestamp
   │     ├─(0 op, standby) [Webhook] Make Webhook — recibe la respuesta de Mundo In. Dos vías de captura:
   │     │        (a) Webhook de WhatsApp Cloud API (Meta) apuntando al webhook de Make, filtrado por número del vendedor
   │     │            y por palabras clave "CONFIRMADO" / "SIN-STOCK" (regex, insensible a mayúsculas/acentos)
   │     │        (b) Fallback: respuesta por correo capturada vía Make Email > Watch Emails con parser de asunto
   │     │            "RE: Orden {order.name}"
   │     ├─(1 op) [Sleep/Timer] Make — o mejor, escenario secundario "Escalamiento" disparado por Make Scheduler cada
   │     │        5 min que consulta el Data Store: si estado sigue "esperando_confirmacion" y
   │     │        now() - timestamp > 60 min → dispara ruta de escalamiento (ver 1.2, Falla #2)
   │     └─(1 op) Router condicional según respuesta:
   │              - CONFIRMADO → continúa a Rama C
   │              - SIN-STOCK → dispara sub-flujo de cancelación (ver 1.2, Falla #1)
   │
   └── RAMA C: Logística (solo si CONFIRMADO y CP está en cobertura Lalamove CDMX/EdoMéx)
         ├─(1 op) Router por código postal: Lalamove (CDMX/EdoMéx: CP de Huixquilucan, Metepec, Naucalpan,
         │        Tlalnepantla, Texcoco, Toluca y CDMX) vs. Skydropx (resto del país, ya generado por el pedido
         │        de envío estándar de Shopify — no requiere acción de Make, ver sección 2)
         ├─(1 op) HTTP > Make a request — POST https://rest.lalamove.com/v3/quotations (cotización: origen = bodega/
         │        dirección de recolección de Mundo In, destino = dirección de envío del cliente, tipo de vehículo
         │        MOTORCYCLE/VAN según volumen del mueble)
         ├─(1 op) HTTP > Make a request — POST https://rest.lalamove.com/v3/orders (crear orden usando el
         │        quotationId de la respuesta anterior, con datos de contacto de Mundo In como remitente y del
         │        cliente como destinatario; nota especial: "recoger en Mundo In, mueble ya confirmado")
         ├─(1 op) Data Store — actualizar registro con lalamove_order_id y tracking URL
         ├─(1 op) WhatsApp Business Cloud API → notificación al cliente con link de rastreo en vivo
         │        (https://tudominio.mx/rastreo/{order.name}, ver sección 3) + ETA
         ├─(1 op) Email de respaldo al cliente con el mismo contenido (usa Shopify Notifications o Make Email)
         └─(0 op, standby) [Webhook] Make Webhook — recibe webhook de Lalamove (ORDER_STATUS_CHANGED) con estado
                  COMPLETED → dispara sub-escenario "Postventa"

[SUB-ESCENARIO] Postventa (disparado por estado COMPLETED del webhook de Lalamove)
   ├─(1 op) Delay — Make Sleep/Scheduler, espera 30 min tras entrega confirmada
   ├─(1 op) WhatsApp Business Cloud API → mensaje al cliente: "¿Tu pedido llegó en buen estado? Responde con una
   │        foto del mueble ya instalado/recibido" (plantilla de utilidad aprobada por Meta)
   ├─(1 op) Data Store — registrar timestamp de "ventana_danos_inicio" = ahora (arranca el reloj de 24h para
   │        reportes de daño, conforme a la política de devoluciones del negocio)
   └─(1 op) Router condicional a las 24h (Make Scheduler): si no hay respuesta con evidencia fotográfica →
            marcar orden como "entrega_aceptada_sin_reporte" en Data Store (tag en Shopify vía módulo nativo
            "Shopify > Update an Order"); si hay reporte con foto dentro de 24h → notificar a Alfonso/Adriana
            para iniciar proceso de garantía/reposición.
```

### 1.2 Modos de falla por paso (ninguna venta queda en limbo silencioso)

| Paso | Modo de falla | Qué pasa con el pedido |
|---|---|---|
| Trigger Shopify New Order | Make no recibe el webhook (caída de Make o de Shopify) | Mitigación: el módulo "Watch Orders" también hace polling periódico (cada 15 min) como red de seguridad nativa de Make; además, se activa un Filter en Shopify Flow (gratis, nativo) que etiqueta la orden como `automatizacion-pendiente` si no recibe tag `automatizacion-ok` en 20 min, visible en el admin de Shopify para revisión manual. |
| Notificación a Alfonso/Adriana (Rama A) | Falla WhatsApp Cloud API (rate limit, número no verificado) | Fallback automático a email (ya incluido en el diagrama, ejecución en paralelo, no en cascada, para no perder tiempo). Si ambos fallan, Make Error Handler dispara una alerta a un canal de respaldo (SMS vía Twilio, 1 op) a Alfonso. |
| Confirmación de Mundo In (Rama B) — SIN-STOCK | El vendedor confirma que no hay existencia | Sub-flujo cancela la orden en Shopify (`Shopify > Cancel an Order`, motivo "out_of_stock"), reembolsa automáticamente vía Shopify Payments (`Shopify > Create a Refund`), y notifica al cliente por WhatsApp+email con disculpa y opción de producto sustituto. Nunca se factura CFDI de una orden cancelada. |
| Confirmación de Mundo In (Rama B) — sin respuesta en 60 min | Timeout | Escalamiento automático: (1) llamada telefónica automatizada vía Twilio Voice al vendedor (mensaje pregrabado), (2) notificación urgente a Alfonso/Adriana marcada como "acción requerida — llamar a Mundo In", (3) la orden se etiqueta en Shopify como `esperando-confirmacion-manual` y NO se genera guía de Lalamove hasta confirmación explícita (evita recolección fallida). Si a las 4 horas sigue sin respuesta, Alfonso/Adriana deciden manualmente cancelar o reintentar. |
| Cotización/creación de orden Lalamove (Rama C) | La API de Lalamove devuelve error (cobertura, dirección inválida, sin conductores disponibles) | Make Error Handler captura el error HTTP; se reintenta automáticamente 1 vez tras 5 min (backoff simple). Si vuelve a fallar, se notifica a Alfonso/Adriana con el error exacto y se ofrece alternativa manual: crear la orden Lalamove desde el Partner Portal web o degradar a Skydropx nacional aunque el cliente esté en zona de cobertura Lalamove (con aviso de cambio en tiempo de entrega). La orden Shopify se etiqueta `logistica-manual-requerida`; nunca se le comunica al cliente un tracking roto. |
| Webhook de Lalamove (ORDER_STATUS_CHANGED) no llega | Lalamove reintenta 10 veces en 24h con backoff exponencial antes de deshabilitar la URL si no responde 200 (documentado en Lalamove Webhook Tutorial v1.3, https://developers.lalamove.com/files/v3_Webhook_v1.3.pdf, consultado 08-jul-2026) | El escenario responde 200 inmediatamente al recibir cualquier webhook antes de procesar lógica (patrón recomendado por Lalamove). Adicionalmente, un escenario de polling de respaldo (Make Scheduler, cada 30 min) consulta `GET /v3/orders/{orderId}` para las órdenes en estado no terminal, evitando depender 100% del webhook. |
| Mensaje post-entrega sin respuesta del cliente en 24h | Cliente no reporta daño | Se asume entrega conforme (política ya definida: reporte de daños <24h con evidencia fotográfica). Se cierra el reloj de garantía automáticamente y se registra en Data Store; no requiere intervención. |
| Facturación CFDI | Timbrado falla (datos fiscales inválidos del cliente, caída del PAC) | La app de facturación reintenta según su propia lógica (ver sección 4); si falla tras reintentos, se notifica a Alfonso/Adriana para timbrado manual dentro del plazo legal SAT (72h). El pedido se envía y entrega igualmente — la facturación nunca bloquea la logística. |

### 1.3 Estimación de créditos Make por ejecución completa

Contando solo módulos que consumen operación (cada acción de módulo = 1 crédito; routers y filtros sin acción no cuentan, triggers de webhook entrante tampoco cuentan como operación saliente pero sí al procesar):

| Bloque | Operaciones estimadas |
|---|---|
| Trigger + notificación interna (Rama A: 2 WhatsApp + 1 email) | 3–4 |
| Notificación a Mundo In + Data Store + procesamiento de respuesta (Rama B) | 4–5 |
| Escalamiento (solo si aplica, ~10–15% de los casos según riesgo documentado de Mundo In) | 3 (condicional) |
| Cotización + creación de orden Lalamove + Data Store + notificación cliente (Rama C) | 5–6 |
| Postventa (delay + mensaje + registro) | 3 |
| **Total por venta (caso sin escalamiento)** | **~15–18 operaciones** |
| **Total por venta (con escalamiento)** | **~18–21 operaciones** |

A la proyección base de 9–14 ventas/mes (mes 10–12), el consumo mensual estimado es de **135–294 operaciones**, muy por debajo de las 10,000 operaciones/mes del plan Make Core ya presupuestado ($10.59 USD/mes). Incluso con escenarios de prueba, reintentos y el polling de respaldo (cada 15–30 min, 24/7, que consume operaciones adicionales de forma constante: ~1,440–2,880 operaciones/mes solo de polling), el total se mantiene dentro del plan Core sin necesidad de upgrade. Fuente: Make.com pricing, plan Core = 10,000 operaciones/mes por $10.59 USD (consultado 08-jul-2026, https://www.make.com/en/pricing).

---

## 2. Tarifas en tiempo real: CarrierService de Shopify

### 2.1 Especificación del servicio

Se requiere un endpoint propio (no un módulo de Make, por requisito de latencia: Shopify espera respuesta en checkout) que Shopify consulte en cada cálculo de envío del carrito. Documentación verificada: Shopify CarrierService API, REST Admin API (recurso `carrier_services`, vigente en API 2026-01; nota importante: **desde el 1 de abril de 2025 las apps públicas nuevas deben construirse con GraphQL Admin API**, pero para una integración privada/custom app de una sola tienda — como es este caso — el recurso REST `CarrierService` sigue siendo funcional; alternativamente puede registrarse vía la mutación GraphQL `carrierServiceCreate`). Fuente: Shopify Dev Docs, CarrierService resource, https://shopify.dev/docs/api/admin-rest/latest/resources/carrierservice (consultado 08-jul-2026); GraphQL DeliveryCarrierService, https://shopify.dev/docs/api/admin-graphql/latest/objects/DeliveryCarrierService (consultado 08-jul-2026); changelog de nuevas Carrier Service APIs (campos `active`, `supportsServiceDiscovery`, `callbackUrl`, mutaciones `carrierServiceCreate`/`carrierServiceUpdate` desde API 2024-07), https://shopify.dev/changelog/introduction-of-new-carrier-service-apis (consultado 08-jul-2026).

**Registro del carrier service** (una sola vez, vía REST o GraphQL, requiere scope `write_shipping`):

```json
POST /admin/api/2026-01/carrier_services.json
{
  "carrier_service": {
    "name": "Entrega Mismo Día / Nacional",
    "callback_url": "https://api.tudominio.mx/shipping/rates",
    "service_discovery": true,
    "format": "json"
  }
}
```

**Request que Shopify envía al `callback_url`** en cada checkout (formato estable del recurso `CarrierService`, patrón documentado desde la introducción original de la API y confirmado vigente en la documentación 2026):

```json
{
  "rate": {
    "origin": {"country": "MX", "postal_code": "53900", "province": "MEX", "city": "Naucalpan"},
    "destination": {"country": "MX", "postal_code": "11000", "province": "CDMX", "city": "Miguel Hidalgo"},
    "items": [{"name": "Sillón Individual", "quantity": 1, "grams": 22000, "price": 450000}],
    "currency": "MXN",
    "locale": "es-MX"
  }
}
```

**Response esperado** (debe responder en menos de ~10 segundos; Shopify descarta la tarifa si el timeout se excede):

```json
{
  "rates": [
    {
      "service_name": "Entrega Mismo Día (Lalamove)",
      "service_code": "LALAMOVE_SAME_DAY",
      "total_price": "18500",
      "currency": "MXN",
      "description": "Entrega el mismo día en CDMX/EdoMéx con rastreo en vivo",
      "min_delivery_date": "2026-07-08 18:00:00 -0600",
      "max_delivery_date": "2026-07-08 22:00:00 -0600"
    }
  ]
}
```

### 2.2 Lógica interna del endpoint

1. Recibe el request de Shopify; extrae `destination.postal_code`.
2. Si el CP pertenece a la cobertura Lalamove (CDMX y municipios EdoMéx: Huixquilucan, Metepec, Naucalpan, Tlalnepantla, Texcoco, Toluca — tabla de CPs mantenida como lista estática o consulta a un mapa de códigos postales/alcaldías-municipios):
   - Llama a `POST https://rest.lalamove.com/v3/quotations` con origen fijo (dirección de recolección de Mundo In) y destino del cliente.
   - Toma `totalFee` de la respuesta, aplica **+5%** (fee cargado al cliente, ya definido en el modelo de negocio), y lo devuelve como `total_price` en centavos.
3. Si el CP no pertenece a cobertura Lalamove: llama a `POST /api/v1/quotations` de Skydropx (documentación: pro.skydropx.com/es-MX/api-docs, consultado 08-jul-2026) con el paquete/peso del mueble, y devuelve la tarifa **sin marcaje** (tal como está definido en el modelo: "tarifa API sin marcaje").
4. **Tarifa de respaldo obligatoria**: si ambas APIs fallan o tardan más del timeout, el endpoint SIEMPRE responde con una tarifa fija de respaldo pre-calculada (ej. "Envío estándar — se confirma costo exacto por WhatsApp", con un precio conservador basado en el promedio histórico) para que el checkout nunca se bloquee ni muestre "sin opciones de envío" — esto evita la pérdida de la venta completa por una falla de terceros.

### 2.3 Hospedaje recomendado (opciones serverless de bajo costo)

| Opción | Costo estimado | Notas |
|---|---|---|
| Cloudflare Workers | Gratis hasta 100,000 req/día, luego $5 USD/mes por 10M req | Latencia mínima, ideal para el volumen de esta tienda (decenas de cotizaciones/día) |
| Vercel Functions (Node/Edge) | Gratis en plan Hobby para este volumen | Fácil de integrar si Claude Code ya usa Next.js/Node para la página de rastreo (sección 3) |
| AWS Lambda + API Gateway | ~$0–1 USD/mes a este volumen (capa gratuita) | Más control, ligeramente más complejo de desplegar |

Recomendación: **Cloudflare Workers o Vercel Functions**, dado el volumen de 5–14 ventas/mes (decenas de cotizaciones de carrito/día) — el costo real será $0 dentro de capa gratuita en ambos casos.

---

## 3. Página de rastreo en vivo

### 3.1 Webhooks de Lalamove

Documentación verificada: Lalamove ofrece webhooks configurables desde la pestaña "Developers" del Partner Portal, con endpoint de sandbox `https://rest.sandbox.lalamove.com/v3/webhook`. Eventos disponibles incluyen `ORDER_STATUS_CHANGED`, `DRIVER_ASSIGNED`, `ORDER_AMOUNT_CHANGED`, `ORDER_REPLACED`, `WALLET_BALANCE_CHANGED`, `ORDER_EDITED`, `POD_STATUS_CHANGED` (proof of delivery) y `POP_STATUS_CHANGED` (proof of pickup). Fuente: Lalamove Webhook Tutorial v1.3, https://developers.lalamove.com/files/v3_Webhook_v1.3.pdf (consultado 08-jul-2026); Lalamove API Reference, https://developers.lalamove.com/ (consultado 08-jul-2026). Buena práctica documentada: responder HTTP 200 inmediatamente al recibir el webhook, antes de ejecutar lógica compleja, ya que Lalamove reintenta hasta 10 veces en 24h con backoff exponencial y deshabilita la URL si no recibe 200.

**Datos que expone el webhook `ORDER_STATUS_CHANGED`**: `orderId`, `status` (ASSIGNING_DRIVER, ON_GOING, PICKED_UP, COMPLETED, CANCELED, etc.), y con `DRIVER_ASSIGNED`: nombre del conductor, teléfono, placas, y (según plan/mercado) coordenadas de ubicación para rastreo en el mapa.

### 3.2 Implementación de la página de rastreo con marca propia

1. El endpoint serverless (mismo hosting que el CarrierService, sección 2.3) recibe los webhooks de Lalamove en `https://api.tudominio.mx/webhooks/lalamove` y los persiste (base de datos ligera: Supabase, Firebase o el mismo Data Store si se prefiere mantenerlo en Make, aunque para una página pública en vivo es preferible una base de datos con lectura rápida fuera de Make).
2. Página pública `https://tudominio.mx/rastreo/{order_name}` (Next.js/similar) que consulta el estado más reciente y renderiza: estado del pedido (línea de tiempo: confirmado → recolectado → en camino → entregado), nombre y foto del conductor si están disponibles, mapa embebido (Google Maps o Mapbox) con la ubicación del conductor si Lalamove expone coordenadas en el webhook para el mercado MX, y branding propio (logo, colores) — el cliente nunca ve la marca de Lalamove directamente, refuerza la promesa de "entrega el mismo día" como diferenciador propio.
3. Fallback si no hay datos de ubicación en vivo para México: mostrar únicamente el estado por etapas (sin mapa en tiempo real) — deja la funcionalidad de mapa como mejora incremental condicionada a verificación técnica durante el sandbox de Lalamove (ver riesgo en backlog, sección 6).

---

## 4. Facturación CFDI

### 4.1 Comparación de apps (Shopify App Store, precios verificados)

| App | Precio verificado | Modelo | Notas |
|---|---|---|---|
| **CFDI Express** | Desde $99 USD/mes | Facturas ilimitadas (sin costo por timbre) | Recomendada si el volumen crece rápido, ya que a 9–14 ventas/mes con timbrado, un plan de costo fijo por facturas ilimitadas puede no ser el más económico al inicio; conviene revisar el plan de entrada más bajo publicado en su página (https://cfdi.express/) |
| **FiscalPOP México** | Modelo con planes desde volumen bajo; permite auto-facturación del cliente post-checkout | Por suscripción + posible costo por timbre según plan | Ofrece flujo de "autofactura": el cliente genera su propia factura desde un portal tras la compra, sin intervención de Alfonso/Adriana — encaja bien con el requisito de automatización |
| **Facturama** (vía integración, no exclusiva de Shopify App Store) | Desde ~$13 USD/mes (plan básico) | Suscripción con cuota de timbres incluida | Opción más económica para el volumen inicial (5–14 ventas/mes); requiere conectar vía Make/Zapier o webhook propio si no hay app nativa 1-click |

Fuentes: Shopify App Store, CFDI Express, https://apps.shopify.com/cfdi-express (consultado 08-jul-2026); Shopify App Store, FiscalPOP México, https://apps.shopify.com/fiscalpop (consultado 08-jul-2026); comparativa de precios de mercado (Facturapi, Facturama, Bind ERP, Alegra) reportada en Panamerik, "Facturación CFDI 4.0 en Shopify México", https://www.panamerik.com/cfdi-shopify-mexico (consultado 08-jul-2026) — **nota de calidad de fuente**: esta comparativa es de un tercero (no oficial), por lo que los precios de Facturapi/Facturama/Bind/Alegra ahí citados deben confirmarse directamente en cada proveedor antes de contratar; se declara como supuesto no verificado en fuente primaria.

**Recomendación**: dado el volumen inicial (5–14 ventas/mes) y el requisito RESICO de automatización por venta, iniciar con **FiscalPOP o Facturama** (menor costo fijo, flujo de autofactura del cliente) y migrar a CFDI Express solo si el volumen de facturas hace más económico el plan de "ilimitadas".

### 4.2 Flujo de autofactura

1. Tras entrega confirmada (o inmediatamente tras pago, según preferencia fiscal de Alfonso/Adriana — RESICO permite facturar al momento del cobro), Shopify dispara el evento de la app de facturación.
2. La app envía al cliente un correo/link a un portal de autofactura donde captura su RFC, razón social, uso de CFDI y régimen fiscal.
3. La app timbra el CFDI 4.0 automáticamente ante el PAC y envía el XML+PDF al cliente y a Alfonso/Adriana.
4. Modo de falla: si el cliente no completa sus datos fiscales, la app reintenta el recordatorio (ej. a las 24h y 72h); si no hay respuesta, se emite un CFDI genérico a "público en general" antes del cierre del periodo (requisito SAT), gestionado por la propia app o manualmente si la app no lo automatiza.

---

## 5. Sincronización de existencias con Mundo In

**Hallazgo de investigación**: no se encontró evidencia de que Mundo In (mundoin.mx) ofrezca una API pública, portal B2B con feed de inventario, o catálogo descargable estructurado. Su modelo de contacto B2B es manual: el catálogo se solicita por WhatsApp. Esto confirma y refuerza el riesgo ya documentado en el negocio (Mundo In como proveedor único, con fricción operativa) y obliga a diseñar la sincronización en el escalón de menor automatización disponible, con upgrade condicionado a negociación comercial directa con Mundo In.

| Nivel | Descripción | Fricción | Cuándo usarlo |
|---|---|---|---|
| **Nivel 0 (arranque, MVP)** | Lista compartida manual: Alfonso/Adriana solicitan semanalmente por WhatsApp la disponibilidad de los SKUs del catálogo activo en la tienda y actualizan manualmente el inventario en Shopify | Alta (proceso humano recurrente) | Meses 1–3, mientras se valida el modelo y el volumen es bajo (2–4 ventas/mes) |
| **Nivel 1 (recomendado para escalar)** | Hoja de cálculo compartida (Google Sheets) que el vendedor de Mundo In actualiza 1x/día con existencias; un escenario de Make (Google Sheets > Watch Rows, trigger por cambio) sincroniza automáticamente el campo `inventory_quantity` en Shopify vía `Shopify > Update a Product Variant` | Media (requiere que el vendedor adopte el hábito; mitigar con recordatorio automático diario por WhatsApp a las 9am) | A partir de que el volumen justifique reducir la carga manual (mes 4+, 5–7 ventas/mes) |
| **Nivel 2 (ideal, condicionado)** | Si Mundo In desarrolla o ya tiene (a confirmar directamente con ellos, no encontrado en fuentes públicas) una API o feed EDI/XML de inventario, integrarlo directamente vía Make (HTTP module) o webhook, eliminando el paso humano por completo | Baja | Solo si Mundo In lo ofrece — **debe confirmarse en conversación comercial directa**, no asumir su existencia |

Independientemente del nivel, el paso de "confirmación de existencia por SKU específico antes de comprometer la venta al cliente" (Rama B del escenario Make, sección 1.1) se mantiene siempre activo como validación en tiempo real de la orden puntual, ya que ninguna sincronización periódica (diaria) garantiza que el mueble siga disponible en el momento exacto de la compra — es la salvaguarda contra vender inventario que ya no existe.

---

## 6. Mapa de credenciales y costos

### 6.1 Credenciales / API keys necesarias

| Servicio | Credencial | Dónde se obtiene | Variable de entorno sugerida |
|---|---|---|---|
| Shopify | Admin API access token (custom app) | Admin de Shopify > Apps > Desarrollo de apps | `SHOPIFY_ADMIN_ACCESS_TOKEN`, `SHOPIFY_SHOP_DOMAIN` |
| Lalamove | API Key + API Secret (HMAC-SHA256) | Partner Portal de Lalamove, pestaña "Developers" | `LALAMOVE_API_KEY`, `LALAMOVE_API_SECRET`, `LALAMOVE_MARKET` (MX), `LALAMOVE_ENV` (sandbox/production) |
| Skydropx | API Key | Panel Skydropx Pro | `SKYDROPX_API_KEY` |
| WhatsApp Business Cloud API (Meta) | Access Token + Phone Number ID + WABA ID | Meta Business Manager / Meta for Developers | `WHATSAPP_ACCESS_TOKEN`, `WHATSAPP_PHONE_NUMBER_ID`, `WHATSAPP_BUSINESS_ACCOUNT_ID` |
| Twilio (respaldo SMS/voz para escalamiento) | Account SID + Auth Token | Consola Twilio | `TWILIO_ACCOUNT_SID`, `TWILIO_AUTH_TOKEN`, `TWILIO_PHONE_NUMBER` |
| App de facturación CFDI (FiscalPOP/Facturama) | API Key propia de la app | Panel del proveedor elegido | `CFDI_APP_API_KEY` |
| Make.com | Webhook URLs internas (no requieren key externa) | Panel de escenario Make | N/A (URLs de webhook por escenario) |
| Hosting serverless (CarrierService + rastreo) | Token de despliegue (Cloudflare/Vercel) | Panel del proveedor elegido | `CF_API_TOKEN` o `VERCEL_TOKEN` |

### 6.2 Costos mensuales incrementales de la automatización

*(Adicional a los $9,300 MXN de costos fijos ya presupuestados en el business case, que incluyen Shopify Grow+CCS, Claude Max 5x, Make Core, facturación CFDI base $200 MXN y Meta Ads.)*

| Concepto | Costo estimado | Fuente / supuesto |
|---|---|---|
| WhatsApp Business Cloud API — mensajes de utilidad (notificación interna, confirmación Mundo In, tracking, postventa) | ~$0.08–0.20 MXN por conversación de utilidad de 24h (a volumen bajo-medio); a ~14 ventas/mes con 3–4 conversaciones por venta ≈ 42–56 conversaciones/mes ≈ **$3–12 MXN/mes** en tarifa Meta pura | Meta WhatsApp Business Platform pricing, tarifa "utility" para México, reportado por go4whatsup.com (consultado 08-jul-2026, https://www.go4whatsup.com/mexico/whatsapp-business-api-pricing/); modelo de precio por mensaje vigente desde 1-jul-2025 según Twilio, https://help.twilio.com/articles/30304057900699 (consultado 08-jul-2026) |
| Proveedor BSP de WhatsApp (si se usa Twilio en vez de Meta Cloud API directo) | +$0.005 USD (~$0.09 MXN) por mensaje sobre la tarifa de Meta | Twilio WhatsApp Pricing, https://www.twilio.com/en-us/whatsapp/pricing (consultado 08-jul-2026) |
| Twilio Voice (llamada de escalamiento, solo si aplica timeout de 60 min) | Variable, uso esporádico (~10–15% de órdenes) | Twilio, tarifa estándar de voz saliente a México, no cuantificada aquí por ser de bajo uso — declarar como supuesto a validar en consola Twilio |
| Facturación CFDI (Facturama, plan básico) | ~$13 USD/mes (~$226 MXN/mes) o el costo ya presupuestado de $200 MXN si se usa un proveedor de menor costo por timbre | Facturama, referenciado en comparativa de mercado (fuente terciaria, no oficial — validar directamente), consultado 08-jul-2026 |
| Hosting CarrierService + página de rastreo (Cloudflare Workers / Vercel) | $0 (dentro de capa gratuita al volumen actual) | Cloudflare Workers pricing / Vercel Hobby plan, límites gratuitos muy superiores al tráfico esperado (decenas de requests/día) |
| Make — operaciones adicionales si se excede el plan Core | $0 esperado (ver cálculo sección 1.3, muy por debajo de 10,000 ops/mes) | Make.com pricing (consultado 08-jul-2026) |
| **Total incremental estimado** | **~$150–450 MXN/mes** adicionales sobre lo ya presupuestado (principalmente por CFDI si excede el monto ya incluido, y mensajería WhatsApp) | Estimación propia a partir de las fuentes citadas |

---

## 7. Backlog de implementación priorizado (orden de construcción para Claude Code)

1. **Custom app de Shopify + credenciales base** — crear la app privada, obtener Admin API token, configurar scopes (`write_shipping`, `write_orders`, `read_orders`, `write_products`). Prerrequisito de todo lo demás.
2. **Escenario Make — Ramas A y B (notificación interna + confirmación Mundo In)** — es el corazón operativo del negocio: sin esto, cada venta depende 100% de que Alfonso/Adriana revisen Shopify manualmente. Incluye el Data Store y el mecanismo de timeout/escalamiento de 60 min.
3. **Integración Lalamove sandbox** — registrar cuenta developer, probar quotations y orders en `rest.sandbox.lalamove.com`, validar cobertura real por CP en los municipios de EdoMéx listados (Huixquilucan, Metepec, Naucalpan, Tlalnepantla, Texcoco, Toluca) antes de pasar a producción — **riesgo técnico más importante a validar primero**: confirmar en sandbox que Lalamove efectivamente opera con datos en vivo en esos municipios de EdoMéx y no únicamente en CDMX.
4. **CarrierService propio (cotización en tiempo real)** — desplegar el endpoint serverless con la lógica de ruteo Lalamove/Skydropx + tarifa de respaldo; registrar el carrier service en Shopify. Bloquea el checkout correcto si no está listo (sin esto, el checkout mostraría tarifas genéricas o nulas).
5. **Rama C del escenario Make (creación de orden Lalamove + notificación de tracking)** — depende de 3 y 4 completados.
6. **Página de rastreo en vivo con marca propia** — depende de tener los webhooks de Lalamove funcionando en producción (paso 3).
7. **Sub-escenario de postventa (reloj de 24h de daños)** — depende de 5.
8. **Integración de facturación CFDI (autofactura)** — puede construirse en paralelo a partir del paso 2, no bloquea logística.
9. **Sincronización de existencias Nivel 1 (Google Sheets → Shopify)** — mejora incremental, se implementa después de validar el Nivel 0 manual durante los primeros meses de operación.
10. **Monitoreo y polling de respaldo (Make Scheduler para huérfanos de webhook)** — capa de resiliencia, última en construirse pero antes de cualquier campaña de marketing pagado que incremente el volumen.

---

## Riesgo técnico más importante detectado

**Mundo In no tiene API ni catálogo B2B digital**: toda "sincronización de inventario" real depende de mensajería humana (WhatsApp/hoja de cálculo), lo que hace que la promesa de "entrega el mismo día" dependa en cada venta de que una persona en Mundo In responda en menos de 60 minutos — exactamente el riesgo de proveedor único ya documentado en el business case. El blueprint mitiga esto con doble canal de notificación (WhatsApp + email), timeout de 60 min con escalamiento a llamada telefónica, y la regla dura de que ninguna orden avanza a creación de guía Lalamove sin confirmación explícita — pero no elimina la dependencia operativa de fondo, que solo se resuelve con negociación comercial directa con Mundo In para una API o feed de inventario propio.

---

## Fuentes consultadas (08-jul-2026)

- Lalamove API Reference: https://developers.lalamove.com/
- Lalamove Webhook Tutorial v1.3 (PDF): https://developers.lalamove.com/files/v3_Webhook_v1.3.pdf
- Shopify CarrierService (REST Admin API): https://shopify.dev/docs/api/admin-rest/latest/resources/carrierservice
- Shopify DeliveryCarrierService (GraphQL Admin API): https://shopify.dev/docs/api/admin-graphql/latest/objects/DeliveryCarrierService
- Shopify Changelog — Introduction of new Carrier Service APIs: https://shopify.dev/changelog/introduction-of-new-carrier-service-apis
- Shopify Carrier Service API guía técnica (Shopplaza): https://shopplaza.io/blog/shopify-carrier-service-api.html
- Skydropx API docs: https://pro.skydropx.com/es-MX/api-docs
- Twilio WhatsApp Pricing: https://www.twilio.com/en-us/whatsapp/pricing
- Twilio — cambios de precios WhatsApp julio 2025: https://help.twilio.com/articles/30304057900699-Notice-Changes-to-WhatsApp-s-Pricing-July-2025
- WhatsApp Business API Pricing México 2026 (go4whatsup): https://www.go4whatsup.com/mexico/whatsapp-business-api-pricing/
- Make.com Pricing: https://www.make.com/en/pricing
- Shopify App Store — CFDI Express: https://apps.shopify.com/cfdi-express
- Shopify App Store — FiscalPOP México: https://apps.shopify.com/fiscalpop
- Comparativa de mercado CFDI Shopify México (fuente terciaria, validar antes de contratar): https://www.panamerik.com/cfdi-shopify-mexico
- Mundo In (sitio del proveedor, sin API pública identificada): https://mundoin.mx/
