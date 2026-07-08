# 06. Plan Go-to-Market (90 días) — E-commerce de Mobiliario (CDMX/EdoMéx + Nacional)

**Fecha de elaboración:** 8 de julio de 2026
**Responsable:** Head de Growth D2C (rol funcional cubierto por Alfonso y Adriana, sin agencia)
**Presupuesto pagado:** $5,000 MXN/mes (meses 1–7) → escala a $8,000 MXN/mes desde el mes 8, condicionado a que el escenario base de ventas se cumpla (ver `03-finanzas.md`, sección 3, fila "Costos fijos mensuales meses 8–12")
**Equipo:** 2 personas. Alfonso: producción creativa (Higgsfield + Freepik), dirección de cuenta Meta. Adriana: comunidad, WhatsApp Business, atención a comentarios/DMs, checklist de cumplimiento de entrega mismo día.
**Relación con el modelo financiero:** este plan no promete ROAS. El hallazgo central de `03-finanzas.md` (sección 4.2) es que **la conversión del sitio, no el CPC, es la variable de mayor apalancamiento sobre el punto de equilibrio** (5.35 ventas/mes). Todo lo que sigue está diseñado para (a) proteger el CPC con segmentación quirúrgica de zonas premium y (b) empujar la conversión con prueba social específica (rastreo en vivo, entrega mismo día) más que para perseguir volumen de tráfico barato.

**Nota de reconciliación de supuestos:** `03-finanzas.md` usa un CPC de trabajo de $4.00 MXN (declarado como supuesto no verificado a nivel de nicho) para la sensibilidad financiera. Este documento de GTM usa un rango más conservador de **$5–8 MXN** para la planeación operativa de campaña, por tratarse específicamente de audiencias premium de alto valor de vivienda (Polanco/Lomas, Santa Fe, Interlomas/Bosque Real, Pedregal), que suelen tener CPMs más altos que el promedio de México. Ambos son supuestos de partida, no benchmarks confirmados; se validan con datos reales de Meta Ads Manager desde la semana 1 y se reconcilian entre ambos documentos al cierre del mes 1.

---

## 1. Estructura de campaña Meta Ads (mes 1–3)

### 1.1 Arquitectura

| Nivel | Configuración |
|---|---|
| Campaña | 1 sola: "Ventas — Mobiliario Premium CDMX/EdoMéx". Objetivo: Ventas (conversiones), optimización a evento *Purchase*. Presupuesto a nivel de campaña (CBO/Advantage Campaign Budget) para que el algoritmo reparta entre conjuntos si se abre un segundo más adelante. |
| Conjunto de anuncios | 1 solo en meses 1–3, con **pines agrupados** en un único público geográfico: Polanco/Lomas de Chapultepec (Miguel Hidalgo), Santa Fe (Cuajimalpa/Álvaro Obregón), Interlomas/Bosque Real (Huixquilucan), Pedregal (Coyoacán/Tlalpan/Álvaro Obregón). Segmentación por **"personas que viven en"** (Location targeting → *People who live in this location*), radios de **1–8 km** por pin, ajustados al polígono real de cada colonia (radios más chicos en Polanco/Pedregal por densidad, radios más amplios en Interlomas por dispersión). |
| Edad / género | 28–58 años, sin restricción de género (decisor de compra de mobiliario en el hogar es mixto). |
| Intereses (capa adicional, no excluyente) | Diseño de interiores, remodelación de casa, mobiliario de lujo, marcas ancla de referencia (aspiracional, no exclusiva), arquitectura residencial. |
| Placements | Advantage+ Placements (automático) — Meta ha demostrado consistentemente mejor costo por resultado que placements manuales cuando el conjunto es único y el catálogo creativo es limitado (6–8 piezas/mes). |
| Creativos activos | 3–4 simultáneos, rotación semanal según performance (ver sección 2). |
| Píxel / medición | Meta Pixel de Shopify + **Conversions API (CAPI) desde el día 1**, vía la integración nativa Shopify–Meta (Conversions API Gateway), con deduplicación por `event_id` entre Pixel y CAPI. Esto es innegociable dado el deterioro de señal post-iOS 14.5/ATT: sin CAPI, el CPA reportado en Ads Manager se subestima en calidad de dato y la optimización del algoritmo se degrada. |

### 1.2 Exclusión explícita — recintos laborales

**Regla dura:** no se segmenta ningún pin de recinto laboral (oficinas de gobierno, cámaras empresariales, corporativos) como ubicación geográfica. Si en algún momento se desea alcanzar a funcionarios públicos o directivos como público adicional, se hace exclusivamente por **capa de intereses/cargo** (ej. "servicio público", "política pública", títulos de puesto en Detailed Targeting), nunca por pin de domicilio laboral. Esta exclusión es tanto una decisión de eficiencia (el domicilio de vivienda predice mejor la decisión de amueblar un hogar que el domicilio de trabajo) como una salvaguarda reputacional para una marca D2C premium.

### 1.3 Presupuesto de testeo creativo

- Techo: **6–8 creativos probados por mes**, con **$500–800 MXN de gasto de señal por variante** antes de decidir pausar o escalar. Con un presupuesto total de $5,000 MXN/mes, esto implica correr los 6–8 creativos de forma escalonada (no los 8 simultáneos desde el día 1), reemplazando 1–2 por semana según el ciclo de aprendizaje de Meta (mínimo 3–4 días por variante antes de leer resultados, para salir de la fase de aprendizaje).
- Regla de pausa: cualquier creativo que no alcance CTR de link >0.8% ni genere al menos 1 ViewContent por cada $150 MXN gastados hacia el día 4 se pausa y se reemplaza — no se "deja correr" por inercia dado el techo de $5,000/mes.

---

## 2. Retargeting (mes 2 en adelante)

Se activa en el mes 2 una vez que el conjunto de prospección acumula tráfico suficiente para poblar públicos personalizados (mínimo ~500–1,000 visitantes al sitio).

| Público | Ventana | Exclusión |
|---|---|---|
| ViewContent | 14 días | Compradores confirmados (evento Purchase) |
| AddToCart (sin compra) | 14 días | Compradores confirmados |

**Creativo héroe de retargeting:** video corto (formato 9:16) que muestra la pantalla de rastreo en vivo del pedido —estilo Uber/Didi, con el mapa, el ícono del repartidor moviéndose y el ETA— superpuesto a un clip de la pieza de mobiliario ya instalada en una sala. Este es el activo más importante de todo el plan: convierte la promesa "llega hoy" de un texto a una prueba visual verificable, que es precisamente la objeción de mayor fricción para comprar mobiliario en línea sin verlo físicamente.

**Secuencia de objeciones (3 anuncios, rotación por frecuencia de exposición):**

1. **Medidas** — carrusel con overlay de dimensiones reales sobre la pieza en contexto (ancho/alto/profundidad), para resolver la duda #1 de compra de muebles en línea.
2. **Materiales** — macro shots de textura (madera, tapizado, herrajes), con copy centrado en calidad/durabilidad.
3. **"Llega hoy de verdad"** — el video de rastreo en vivo (creativo héroe) + testimonios/reseñas reales o UGC simulada de clientes recibiendo el pedido el mismo día.

**Presupuesto:** se distribuye dentro del mismo techo de $5,000 MXN/mes. Regla de reparto sugerida mes 2–3: ~70% prospección / ~30% retargeting, ajustando semanalmente según el volumen real de tráfico (si el volumen de ViewContent/AddToCart es bajo, no tiene sentido sobre-invertir en retargeting con público insuficiente).

---

## 3. Test A/B fundacional

**Diseño:** mismo creativo base (ej. sala estilo con la pieza ancla del catálogo), dos ángulos de mensaje corriendo en paralelo dentro del mismo conjunto de anuncios:

- **Variante A — Logística/urgencia:** "Llega HOY a tu casa" (headline y copy centrados en la entrega mismo día + rastreo en vivo).
- **Variante B — Diseño/estética:** "Diseño que transforma tu espacio" (headline y copy centrados en estilo, curaduría, aspiracionalidad).

**Operación:** presupuesto de señal igual entre ambas variantes ($750–800 MXN c/u), corriendo mínimo 5–7 días o hasta acumular ~1,000 impresiones por variante (lo que ocurra después), para salir de la fase de aprendizaje antes de leer el resultado.

**Criterio de decisión (ambos deben cumplirse, no uno solo):**
- **CTR de link > 1%** — umbral de calidad de creativo/mensaje.
- **CPA por debajo de $1,700 MXN** — el techo absoluto es la contribución neta por venta calculada en `03-finanzas.md` (sección 5); un CPA por encima de esa cifra significa que el negocio pierde dinero en cada venta atribuible a esa variante, sin importar cuántas se generen.

La variante ganadora se queda con ~80% del presupuesto del conjunto; el 20% restante se usa para retar con un nuevo challenger (mismo formato de test, nuevo ángulo), manteniendo el aprendizaje continuo sin reabrir el debate completo cada mes.

---

## 4. Sistema orgánico (sin techo de presupuesto)

**Cadencia:** 1–2 piezas por día combinadas entre Instagram y TikTok (7–14 piezas/semana), producidas por Alfonso; publicación, respuesta a comentarios/DMs y cierre por WhatsApp Business a cargo de Adriana.

**WhatsApp Business como canal de cierre:** catálogo de Shopify sincronizado (misma fuente de precios/existencias que la tienda), respuestas rápidas configuradas para las 5 objeciones más comunes (medidas, materiales, tiempo de entrega real, política de devolución, formas de pago), y todo tráfico de bio-link de IG/TikTok dirigido a un botón directo de WhatsApp además del sitio — el objetivo es capturar la conversación de cierre en el canal donde el fundador puede resolver dudas de estilo/personalización en tiempo real, algo que el checkout de Shopify solo no logra.

### 4.1 Los 5 formatos repetibles

| # | Formato | Objetivo |
|---|---|---|
| F1 | **Ambientación en 4 estilos** — la misma pieza mostrada en 4 tratamientos de decoración distintos (minimalista, cálido/madera, contemporáneo de lujo, editorial/boho) | Demostrar versatilidad y calidad de dirección de arte — la ventaja competitiva declarada del negocio |
| F2 | **Antes/después** — espacio vacío o desactualizado → mismo espacio amueblado | Prueba de transformación, alto potencial de guardados/compartidos |
| F3 | **POV entrega en vivo** — punto de vista del repartidor de Lalamove llegando, unboxing, reacción del cliente | Prueba social directa de la promesa "mismo día" |
| F4 | **Pieza en contexto Polanco/Interlomas** — la pieza en un entorno lifestyle reconocible de la zona objetivo (terraza, sala con vista, roof) | Aspiracionalidad + relevancia geográfica directa con el segmento pagado |
| F5 | **Respuesta a comentario con render** — alguien pregunta "¿lo tienen en otro color/tamaño?" y se responde con un render personalizado de esa variante | Conversión de la ventaja de producción creativa ilimitada en engagement y prueba de capacidad de personalización |

---

## 5. Kits de prompts por formato

**Nota de vigencia (consultado 08-jul-2026):** Higgsfield organiza su generación de video en un sistema de **capas separadas** —movimiento de cámara, comportamiento del sujeto y estilo visual no se mezclan en una sola oración— y su regla más importante es **un solo movimiento de cámara por clip** (combinar dos movimientos en un mismo prompt genera inestabilidad). La app expone más de 50 presets de Camera Controls (dolly in/out, whip pan, crash zoom, 360° orbit, dolly zoom, bullet time, FPV) y una suite ampliada "DoP" (Director of Photography) con más de 100 presets cinematográficos adicionales, además de plantillas específicas de producto-a-video. Fuentes: [Higgsfield Camera Controls](https://higgsfield.ai/camera-controls), [Higgsfield Presets](https://higgsfield.ai/viral-presets), [Kolbo.AI — Higgsfield Suite: 100+ Camera Presets](https://kolbo.ai/blog/higgsfield-suite-100-camera-presets), [Techpresso — 30 Best Higgsfield Prompts](https://academy.techpresso.co/prompts/higgsfield-prompts). **Supuesto declarado:** la duración exacta por clip (típicamente 5–10 s según preset y plan) y la disponibilidad puntual de cada preset varían por plan/actualización de producto; se debe verificar el preset exacto dentro de la app de Higgsfield antes de producir en volumen, ya que la librería se actualiza con frecuencia.

Freepik (ahora operando bajo la marca combinada **Magnific**) ofrece más de 40 modelos de generación de imagen, incluyendo **Mystic 2.5 Fluid** (fotorrealismo, curado por fotógrafos/VFX, evita el look "sobre-artificial"), **Flux.2 Pro** y **Nano Banana** (edición/variación rápida), con soporte de múltiples aspect ratios por generación. Fuentes: [Freepik Mystic — Magnific Blog](https://www.magnific.com/blog/freepik-mystic/), [Freepik API — Mystic docs](https://docs.freepik.com/api-reference/mystic/post-mystic), consultado 08-jul-2026.

Variables entre corchetes (`[pieza]`, `[ambiente]`, `[estilo]`, `[color]`) se sustituyen por el SKU y contexto real de cada publicación.

### F1 — Ambientación en 4 estilos

**Higgsfield (video, 1 clip por estilo → 4 clips, luego editar en secuencia de 4–8 s c/u):**
> Preset: Camera Controls → *Dolly In* (un solo movimiento). Sujeto: [pieza] de mobiliario en el centro del encuadre. Ambiente: sala montada en estilo [minimalista escandinavo / cálido madera y lino / contemporáneo de lujo con mármol y latón / editorial boho con textiles y plantas]. Iluminación: luz natural difusa de ventana lateral, hora dorada suave. Movimiento de cámara: dolly in lento hacia [pieza], sin zoom digital. Duración objetivo: clip corto (validar duración exacta disponible en el preset seleccionado dentro de la app).

**Freepik (imagen, mínimo 3 prompts, modelo sugerido: Mystic 2.5 Fluid):**
1. *[pieza] centered in a [minimalist Scandinavian] living room, soft diffused window light from the left, shot on a 35mm lens, shallow depth of field, warm neutral color grade, editorial interior photography style — 4:5*
2. *[pieza] styled in a [contemporary luxury] living room with marble accents and brass details, golden hour light through sheer curtains, shot on a 50mm lens, cinematic soft shadows, high-end furniture catalog style — 9:16*
3. *[pieza] in a [boho editorial] living room with woven textiles and indoor plants, natural overcast daylight, shot on a 24mm wide lens, muted earthy tones, lifestyle magazine style — 1:1*

### F2 — Antes/después

**Higgsfield (video, transición controlada — evitar mezclar movimientos):**
> Preset: DoP → transición tipo "reveal" o, si no está disponible en el plan, dos clips independientes con el mismo *Static/Locked* seguidos de corte duro (evitar solicitar dos movimientos de cámara distintos dentro de un mismo prompt). Sujeto: [ambiente] vacío o desactualizado, luego el mismo encuadre con [pieza] instalada. Iluminación: constante entre ambos clips para que el contraste sea el mueble, no la luz. Ambiente: [sala/comedor/recámara] real o generado. Duración: clip corto por segmento, unidos en edición.

**Freepik (imagen "antes" y "después", modelo sugerido: Flux.2 Pro):**
1. *Empty [living room] with bare walls and worn flooring, neutral flat daylight, shot on a 24mm lens, documentary real-estate photography style, slightly desaturated — 4:5*
2. *Same [living room] fully furnished with [pieza] as the anchor piece, styled with matching decor, warm afternoon light through the window, shot on a 35mm lens, aspirational interior photography style — 4:5*
3. *Split-composition concept image, left half empty [ambiente], right half same space furnished with [pieza], consistent lighting across both halves, editorial before/after style — 1:1*

### F3 — POV entrega en vivo

**Higgsfield (video, un movimiento por clip):**
> Preset: Camera Controls → *FPV* o *Handheld POV* (para la llegada del repartidor) en un clip, y *Static* en el clip del unboxing/reacción (clips separados, no combinados). Sujeto: repartidor con caja de [pieza] llegando a la puerta / cliente abriendo la caja. Ambiente: entrada de casa/edificio en zona [Polanco/Interlomas], luz de día natural. Iluminación: exterior natural, sin flash. Duración: clip corto por segmento, editado como secuencia con el mapa de rastreo en vivo superpuesto en post.

**Freepik (frames de apoyo para thumbnails/carrusel, modelo sugerido: Nano Banana para variaciones rápidas):**
1. *Delivery courier holding a branded box with [pieza] at a residential front door in [Polanco], natural midday light, shot on a 35mm lens, candid documentary style — 9:16*
2. *Close-up of hands unboxing [pieza], soft indoor daylight from a nearby window, shot on a 50mm macro lens, warm authentic tone, UGC-style photography — 9:16*
3. *Customer smiling next to newly unboxed [pieza] in their living room, natural window light, shot on a 35mm lens, candid lifestyle photography, unposed style — 4:5*

### F4 — Pieza en contexto Polanco/Interlomas

**Higgsfield (video, un movimiento por clip):**
> Preset: Camera Controls → *Orbit* (parcial, no 360 completo, para mantener el mueble legible) o *Crash Zoom* de salida (de detalle a plano general revelando el entorno). Sujeto: [pieza] como punto focal. Ambiente: terraza/sala con vista reconocible de [Polanco/Interlomas/Bosque Real], arquitectura de la zona visible al fondo. Iluminación: hora dorada o luz de mediodía cenital suave. Duración: clip corto, un solo movimiento.

**Freepik (imagen, modelo sugerido: Mystic 2.5 Fluid):**
1. *[pieza] on a terrace overlooking [Bosque Real] skyline, golden hour backlight, shot on a 50mm lens, shallow depth of field, aspirational real-estate style — 4:5*
2. *[pieza] in a bright living room with floor-to-ceiling windows facing [Polanco] rooftops, soft midday light, shot on a 35mm lens, high-end architectural photography style — 9:16*
3. *[pieza] styled in a modern [Interlomas] home office nook, natural side light, shot on a 40mm lens, clean minimal composition, premium lifestyle catalog style — 1:1*

### F5 — Respuesta a comentario con render

**Higgsfield (video corto de "reveal" de la variante solicitada):**
> Preset: Camera Controls → *Crash Zoom* in hacia el detalle de color/material de la variante pedida en el comentario. Sujeto: [pieza] en [color/acabado solicitado]. Ambiente: fondo neutro o el mismo set de F1 para consistencia de marca. Iluminación: luz de estudio suave, difusa. Duración: clip muy corto (formato respuesta a comentario / Reels reply).

**Freepik (imagen de la variante pedida, modelo sugerido: Flux.2 Pro o Nano Banana para variación rápida de color sobre un render base):**
1. *[pieza] in [requested color/finish] on a neutral studio background, soft diffused studio lighting, shot on a 50mm lens, clean product photography style — 1:1*
2. *[pieza] in [requested size variant] shown to scale next to a standard sofa for size reference, even daylight, shot on a 35mm lens, catalog comparison style — 4:5*
3. *[pieza] in [requested color/finish] styled in a quick lifestyle vignette (corner of a living room), natural window light, shot on a 45mm lens, Instagram-reply style, fast turnaround aesthetic — 9:16*

---

## 6. Calendario 90 días (semana a semana)

Horizonte: mes 1–3 del escenario base de `03-finanzas.md` (ventas objetivo: mes 1 = 2, mes 2 = 3, mes 3 = 4 unidades/mes).

| Semana | Hito | Acción principal | KPI a vigilar | Umbral de acción |
|---|---|---|---|---|
| 1 | Fundación técnica | Instalar Pixel + CAPI, verificar deduplicación de eventos; publicar catálogo en WhatsApp Business; lanzar campaña única con conjunto de pines agrupados y 3 creativos iniciales (F1 y F4) | Eventos Purchase/ViewContent registrados correctamente en Events Manager (calidad de coincidencia) | Si CAPI no dedupe en 48h, pausar escalamiento de gasto hasta resolver — dato sucio invalida todo lo demás |
| 2 | Primer aprendizaje de creativo | Publicar 1–2 piezas orgánicas/día (F1, F2); iniciar test A/B fundacional (Variante A vs B) | CTR de link por variante | Pausar variante con CTR <0.8% al día 4 |
| 3 | Cierre del test A/B | Analizar resultado del A/B con criterio dual (CTR>1% y CPA<$1,700); reasignar 80/20 al ganador | CPA, CTR | Si ninguna variante cumple ambos criterios, mantener 50/50 una semana más antes de decidir |
| 4 | Fin de mes 1 | Checkpoint de ventas mes 1 (objetivo: 2 unidades) | Ventas del mes, CAC del mes | CAC mes 1 de referencia (finanzas): ~$2,500 MXN; si CAC real >$3,500, revisar segmentación de pines antes de subir gasto |
| 5 | Apertura de retargeting | Activar públicos ViewContent/AddToCart (14 días); publicar creativo héroe de rastreo en vivo | Tamaño de público de retargeting | Si el público es <300 personas, mantener 100% del presupuesto en prospección una semana más |
| 6 | Secuencia de objeciones | Lanzar los 3 anuncios de retargeting (medidas, materiales, "llega hoy de verdad") | Frecuencia de exposición por público | Rotar creativo si frecuencia >3 sin conversión |
| 7 | Rotación creativa 2 | Reemplazar 1–2 creativos de prospección de menor performance (F3, F5 entran a la rotación) | Costo por ViewContent | Pausar creativo si costo por ViewContent >$150 MXN sostenido 4 días |
| 8 | Fin de mes 2 | Checkpoint de ventas mes 2 (objetivo: 3 unidades) | Ventas del mes, % mismo día cumplido | Si % mismo día cumplido <90%, escalar el hallazgo a Mundo In/Lalamove antes de seguir invirtiendo en pauta que promete esa entrega |
| 9 | Ajuste de mensaje | Introducir variante de copy retadora (challenger) sobre el ganador del A/B fundacional | CTR del challenger vs. campeón | Reemplazar campeón si challenger supera CTR y CPA en 2 semanas consecutivas |
| 10 | Consistencia orgánica | Evaluar qué formato (F1–F5) genera más guardados/compartidos y priorizarlo en cadencia | Tasa de guardados/compartidos por formato | Redirigir cadencia hacia el formato top si la diferencia es >2x vs. el resto |
| 11 | Revisión de zonas | Analizar performance por pin agregado (aunque el conjunto sea único, revisar breakdown geográfico en Ads Manager) | CPA por zona (breakdown) | Si una zona concentra >50% del gasto con <20% de las ventas, reducir su peso relativo en el radio/pin |
| 12 | Fin de mes 3 / cierre de los 90 días | Checkpoint de ventas mes 3 (objetivo: 4 unidades); consolidar aprendizajes de creativo ganador, ángulo ganador y formato orgánico ganador para el trimestre 2 | Ventas acumuladas 90 días (objetivo acumulado: 9 unidades), CAC promedio del trimestre | Si ventas acumuladas <7 unidades en 90 días, revisar oferta/conversión del sitio antes de aumentar presupuesto de pauta — recordar que la conversión, no el CPC, es la variable crítica (`03-finanzas.md`, sección 4.2) |

**Puente hacia el mes 4+:** si el escenario base se sostiene (mes 4 = 5 unidades, cruce a resultado mensual positivo según `03-finanzas.md`), el presupuesto de pauta se mantiene en $5,000 MXN hasta el mes 7 y escala a $8,000 MXN desde el mes 8 — no antes, y no automáticamente: la escalada solo procede si el CAC del trimestre 2 se mantiene por debajo de $1,700 MXN (techo de contribución neta por venta).

---

## 7. Tablero de KPIs

| KPI | Definición | Frecuencia de revisión | Rango de referencia (supuesto, no promesa) |
|---|---|---|---|
| CAC (costo de adquisición) | Gasto en pauta del mes / ventas atribuibles a pauta del mes | Semanal | $500–$2,150 MXN según escenario (`03-finanzas.md`, sección 5); **techo absoluto de decisión: $1,700 MXN** (contribución neta por venta) |
| ROAS | Ingreso atribuido a pauta / gasto en pauta | Semanal, solo como diagnóstico interno — **no se comunica como promesa a inversionistas ni se usa como meta de campaña** | Sin cifra objetivo fija; se reporta el dato real observado, contextualizado con el ROAS requerido de equilibrio (4.83x, `03-finanzas.md` sección 5) |
| CPC | Costo por clic en Ads Manager | Semanal | $5–8 MXN en zonas premium (supuesto de trabajo de este documento, ver nota de reconciliación) |
| CTR (link) | Clics en enlace / impresiones | Semanal | Umbral de decisión: >1% en test A/B; >0.8% como piso de no-pausa |
| Conversión web | Purchase / sesiones (Shopify Analytics) | Semanal | 0.3–0.8% en tráfico frío (mismo rango de sensibilidad usado en `03-finanzas.md` sección 4.2) |
| Conversión WhatsApp | Conversaciones cerradas con venta / conversaciones iniciadas | Semanal | Sin benchmark externo disponible; se establece línea base propia desde la semana 1 y se compara mes contra mes |
| % mismo día cumplido | Órdenes CDMX/EdoMéx entregadas el mismo día / total de órdenes elegibles para mismo día | Por orden, consolidado semanal | Meta operativa: ≥90%; por debajo de eso, la promesa central de venta está en riesgo y debe pausarse su comunicación en creativos activos hasta resolver la causa raíz (usualmente confirmación de existencia con Mundo In) |
| Costo por ViewContent | Gasto en prospección / eventos ViewContent | Semanal | Umbral de pausa de creativo: >$150 MXN sostenido 4 días |
| Frecuencia (retargeting) | Impresiones / alcance del público de retargeting | Semanal | Rotar creativo si frecuencia >3 sin conversión asociada |

**Regla de gobierno del tablero:** ningún KPI de este tablero se reporta de forma aislada al comité — siempre se cruza con el punto de equilibrio de 5.35 ventas/mes y la señal de salida declarada (<3 ventas/mes sostenidas al mes 5). El GTM existe para sostener esa cifra, no al revés.
