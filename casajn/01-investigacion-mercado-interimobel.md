# Investigación de Mercado — Interimobel
## Contenido de redes sociales, rebranding 2027+ y segmentación multigeneracional

**Cliente:** Interimobel (INTER ESPACIOS COMERCIALIZADORA, S.A. DE C.V.), mueblería mexicana con más de 25 años de operación, sitio oficial `interimobel.mx`, presencia en Facebook, Instagram, TikTok y Mercado Libre.
**Fecha de elaboración:** 11 de julio de 2026.
**Alcance:** Este documento es un proyecto de benchmarking/consultoría independiente. No utiliza ni contradice datos del business case de e-commerce de mobiliario documentado en `CLAUDE.md` y `/entregables/` de este repositorio — son clientes/proyectos distintos.

---

## Nota metodológica y limitación de acceso (léase antes de lo demás)

Para esta investigación se intentó acceso directo a `interimobel.mx` y a la tienda oficial de Interimobel en Mercado Libre (`tienda.mercadolibre.com.mx/interimobel`) mediante fetch directo de páginas. **Ambos dominios devolvieron bloqueo a nivel de red de la herramienta de navegación** (rechazo de conexión / HTTP 403 antes de llegar al contenido), por lo que no fue posible renderizar o leer el HTML de esas páginas de forma directa en esta sesión. Esto es una limitación de la herramienta, no evidencia de que el sitio esté caído o sea de baja calidad.

Para compensar, toda la evidencia de este documento sobre el sitio web, el catálogo, precios y redes sociales se reconstruyó mediante **búsqueda web (snippets indexados, resultados de motor de búsqueda y fragmentos de páginas de producto/colección cacheados)**. Esto permite reconstruir con razonable confianza: nombres de colecciones, nombres de producto, precios puntuales indexados, y metadatos públicos de redes sociales (bios, conteos de seguidores/posts visibles en resultados). **No permite** verificar de primera mano: diseño visual del checkout, velocidad de carga, responsividad móvil real, experiencia UX completa de navegación, o el feed completo y cronológico de Facebook/Instagram (estas plataformas exhiben muro de login incluso a través de búsqueda). Cada sección marca explícitamente qué es observación directa (ninguna, en sentido estricto de renderizado) vs. reconstrucción por búsqueda, y qué es supuesto declarado.

Todas las cifras externas incluyen fuente (URL) y la fecha de consulta es 11 de julio de 2026 salvo que se indique otra cosa (algunas fuentes ya traen fecha de publicación propia, citada donde aplica).

---

## 1. Auditoría del sitio web (interimobel.mx)

### 1.1 Estructura de catálogo (reconstruida vía búsqueda de colecciones indexadas)

El sitio corre sobre plataforma Shopify (URLs con patrón `/collections/` y `/products/`, típico de Shopify). Las colecciones indexadas y encontradas públicamente son:

- **Salas** (`/collections/salas`) — con subcategorías: sofás cama, salas 3-2, salas esquineras, salas modulares, salas reclinables, centros de TV, mesas de centro.
- **Salas Modulares** (`/collections/salas-modulares`) — colección propia, separada de "Salas" genérica, lo que sugiere que es un subsegmento con suficiente volumen de SKUs para justificar su propia página.
- **Recámaras** (`/collections/recamaras`) — con subcategorías: armarios, cabeceros, cómodas, bases de cama, burós, baúles, literas, tocadores, zapateras.
- **Comedores** (`/collections/comedores`) — con subcategorías: mesas, sillas, "paquete comedores" (mesa + sillas en set).
- **Colchones** (`/collections/colchones`) — línea propia con tecnología de marca ("Comfort Tech"), en tamaños matrimonial, queen y king.
- **Sillas** (`/collections/sillas`) — colección transversal (aplica a comedor y espacios de trabajo).
- **Home Office** — mencionado en resultados de búsqueda (escritorios, incluyendo modelos de cristal).
- **Jardín** — mobiliario de exterior (confirmado también por una reseña de producto "Set de Jardín Beirut" en Amazon México).
- **"Los Vendidos"** (`/collections/los-vendidos`) — colección curada explícitamente como catálogo de más vendidos. Su sola existencia como colección dedicada (y no solo como filtro de ordenamiento) es la señal más fuerte de qué productos la marca misma identifica como líderes de venta.
- **"De México a tu Hogar"** (`/collections/de-mexico-a-tu-hogar`) — colección con narrativa de manufactura nacional, alineada con el mensaje "100% Hecho en México" que aparece también en su contenido de TikTok (ver sección 3).

El motor de colecciones permite ordenar por "Más vendidos", "Más relevante", alfabético, precio y fecha — funcionalidad estándar de Shopify que confirma una capa de e-commerce transaccional real (no solo catálogo informativo).

**Fuente:** resultados de búsqueda web sobre `interimobel.mx/collections/*`, consulta 11-jul-2026.

### 1.2 Rango de precios visible (por producto, vía snippets indexados)

No fue posible extraer una tabla de precios completa por colección (ver limitación arriba), pero sí se recuperaron precios puntuales de productos específicos indexados, que dan una imagen razonable del rango:

| Categoría | Producto (ejemplo indexado) | Precio |
|---|---|---|
| Salas — modular premium | Sala Modular Excellence Elite Especial | $27,080 MXN (precio regular $54,707) |
| Salas — modular premium | Sala Modular Excellence Premium Especial | $35,890 MXN (precio regular $72,505) |
| Salas — modular premium | Sala Modular Terranova Elite Especial | $33,730 MXN (precio regular $68,141) |
| Salas — modular premium | Sala Modular Terranova Premium Especial | $42,420 MXN (precio regular $85,697) |
| Salas — entrada (Mercado Libre) | Sala Fantasy Interimobel | $2,750 MXN (antes $3,396) |
| Comedores — sillas | Silla Oslo Vento | $590 MXN |
| Comedores — sillas | Silla De Madera Capri (estilo Eames Hood) | $990 MXN |
| Comedores — sillas | Silla Bolonia Black | $2,555 MXN |
| Comedores — sillas | Silla Malta (pata nogal) | $3,150 MXN |
| Comedores — mesas | Mesa Comedor Erza | $1,750 MXN |
| Comedores — mesas | Mesa Comedor Kiara | $4,990 MXN |
| Comedores — mesas | Mesa Comedor Amelia Merlot | $5,350 MXN |
| Comedores — set | Mesa Lucca Negra + Sillas Capri | $6,941 MXN |
| Comedores — mesas | Mesa Comedor Roma | $11,090 MXN |
| Recámaras — armarios | Armario Lima (Natural/Blanco) | $6,655.56 MXN |
| Recámaras — armarios | Armario Chile (Natural/Blanco) | $7,766.67 MXN |
| Recámaras — armarios | Armario Panamá (Natural/Blanco) | $9,989 MXN |
| Colchones | Colchón Freedom - América (matrimonial/queen/king) | $15,166.67 MXN |

**Lectura del rango:** el catálogo de Interimobel abarca desde piezas de entrada de ~$590–$2,750 MXN (sillas sueltas, salas básicas vía Mercado Libre) hasta piezas premium de $35,000–$42,000+ MXN (salas modulares Elite/Premium) y colchones individuales de gama alta por arriba de $15,000 MXN. Esto es un **rango de precio unitario mucho más amplio** que el de un solo SKU de comparación: hay al menos un orden de magnitud (~15–20x) entre la silla más barata indexada y la sala modular más cara. Es un catálogo de mueblería generalista de línea completa, no un catálogo curado de ticket único.

**Supuesto declarado:** estos precios son puntuales (productos específicos encontrados por el motor de búsqueda en el momento de la consulta) y no una muestra estadísticamente representativa de todo el catálogo; los precios de e-commerce de mueblería cambian con frecuencia por promociones (se observaron descuentos de 50%+ vs. precio de lista en varios SKU). No se debe leer esta tabla como "precio promedio real" del catálogo, sino como evidencia de rango.

**Fuente:** snippets de búsqueda sobre productos individuales en `interimobel.mx/products/*` y listados de Mercado Libre, consulta 11-jul-2026.

### 1.3 Señales de "productos líderes"

Tres señales convergen para identificar el núcleo del catálogo:

1. **Colección "Los Vendidos" dedicada** — la marca misma cura y expone sus más vendidos como colección de primer nivel en el menú, no solo como filtro. Esto es la señal más directa disponible.
2. **Densidad de subcategorías**: Salas y Recámaras son las únicas categorías con suficiente profundidad de catálogo para justificar sub-colecciones especializadas (salas: cama, esquinera, modular, reclinable, 3-2; recámaras: armario, cabecero, cómoda, base de cama, buró, baúl, litera, tocador, zapatera). Comedores tiene subcategorías (mesas, sillas, paquetes) pero visiblemente menos variantes de forma/tipología que Salas o Recámaras. Colchones es una línea angosta pero con marca propia de tecnología ("Comfort Tech"), lo que sugiere inversión de marketing dedicada a esa categoría aun con menos SKUs.
3. **Existencia de una colección "Salas Modulares" separada de "Salas"** — cuando una tienda Shopify separa un subtipo en su propia colección de nivel de menú (en vez de dejarlo como filtro dentro de la colección padre), normalmente es porque ese subtipo genera tráfico/conversión suficiente para justificarlo editorialmente. Esto es una lectura razonada, no una cifra confirmada por Interimobel.

**Conclusión razonada (no cifra oficial):** Salas (particularmente modulares/esquineras) y Recámaras (particularmente armarios/closets) aparentan ser el núcleo de catálogo por profundidad de SKU, mientras que Colchones aparenta ser una apuesta de marca diferenciada (tecnología propia) más que de volumen. Comedores parece un tercer pilar de tráfico más transaccional (piezas sueltas de precio bajo — sillas desde $590 MXN — que probablemente funcionan como producto de entrada/tráfico más que como generador de ticket alto).

### 1.4 Calidad de experiencia de e-commerce (lo que se puede y no se puede verificar)

- **Verificable indirectamente:** la plataforma es Shopify, tiene motor de colecciones con ordenamiento, páginas de producto individuales con URL única, y opciones de envío con costo (~$250–$350 MXN en CDMX/EdoMéx/Pachuca/Toluca) y tiempos de entrega declarados (se encontró referencia a 15–30 días hábiles según producto, notablemente más lento que la promesa de entrega inmediata de otros jugadores del mercado). También existe un dominio alterno `interimobel.shop` indexado, lo que podría indicar migración de plataforma en curso o duplicidad de tiendas — **se declara como hallazgo a verificar directamente por el cliente**, no como hecho confirmado.
- **No verificable en esta sesión** (limitación de acceso, sección arriba): calidad real de fotografía de producto, fricción real del checkout, responsividad móvil, velocidad de carga. **Recomendación operativa:** el equipo de Interimobel o un tercero con acceso de navegador estándar debe correr una auditoría UX/CRO manual (PageSpeed Insights, prueba de checkout end-to-end, revisión de fotografía en al menos 10 SKU de cada categoría líder) antes de comprometer el plan de rebranding a supuestos de UX no verificados en este documento.
- **Reseñas de clientes** (vía Amazon México, Indeed, sitios de reseñas de mueblerías) muestran un patrón mixto: elogios consistentes a calidad de producto y relación precio-valor, pero quejas recurrentes sobre **tiempos de entrega y logística/montaje** (ej. patas desalineadas en mobiliario de jardín, lentitud de entrega). Esto es relevante para el contenido de redes: cualquier narrativa de rebranding que prometa "inmediatez" debe ser honesta con esta fricción operativa conocida, o el contenido creará expectativas que el fulfillment actual no cumple.

**Fuente:** Indeed.com/cmp/Interimobel/reviews; muebleriaalexandra.com.mx reseñas; apestan.com caso Interimobel; Amazon México reseñas de producto — consulta 11-jul-2026.

### 1.5 Presencia en Mercado Libre (mezcla de canal)

**Confirmado:** Interimobel opera una **Tienda Oficial en Mercado Libre** (`tienda.mercadolibre.com.mx/interimobel`, también espejada en `eshops.mercadolibre.com.mx/interimobel` y `interimobel.mercadoshops.com.mx`), categorizada en Hogar, Muebles y Jardín, con subcategorías de sillas/sillones/bancos, mobiliario de cocina, sets de muebles y mobiliario de exterior. Ofrece meses sin intereses y envío gratis en varios productos.

**Implicación estratégica (relevante para el rebranding, no para el business case de dropshipping de este repositorio, que es un proyecto distinto):** operar simultáneamente en marketplace (Mercado Libre) y en tienda propia (Shopify) crea una realidad de **mezcla de canal y precio** que un e-commerce puro no enfrenta — los mismos SKU (o muy similares) pueden aparecer a distinto precio en cada canal, lo cual: (a) fragmenta la narrativa de marca porque Mercado Libre impone su propio layout/UX sobre la experiencia, diluyendo la identidad visual que el rebranding busca construir; (b) puede generar percepción de inconsistencia de precio entre consumidores que comparan canales; (c) es una fuente adicional (más allá de redes sociales) de "voz de marca" que el plan de contenido debe considerar, porque el consumidor puede formarse una primera impresión de Interimobel en Mercado Libre, no en el sitio propio ni en redes. **Se declara como hallazgo de investigación, no como recomendación de cambiar de canal** — eso excede el alcance de esta investigación de contenido/redes.

**Fuente:** listado.mercadolibre.com.mx/hogar-muebles-jardin/\_Tienda_interimobel; tienda.mercadolibre.com.mx/interimobel — consulta 11-jul-2026 (contenido reconstruido vía snippets de búsqueda; acceso directo bloqueado, ver nota metodológica).

---

## 2. Productos líderes y su efecto en el ticket de e-commerce

### 2.1 Mezcla de categorías líderes

Con base en la sección 1.3, el catálogo tiene al menos tres "polos" de precio-volumen distintos:

- **Polo de tráfico/entrada** (precio bajo, alta frecuencia de búsqueda): sillas sueltas ($590–$3,150 MXN), mesas de comedor de gama media ($1,750–$5,350 MXN), salas de entrada vía Mercado Libre (~$2,750 MXN).
- **Polo de ticket medio** (mobiliario funcional de recámara): armarios/closets ($6,655–$9,989 MXN), sets de comedor completos (~$6,941–$11,090 MXN).
- **Polo de ticket alto** (mobiliario de diseño/estatus): salas modulares premium ($27,080–$42,420 MXN con precio de lista hasta $85,697 MXN) y colchones individuales de gama alta (~$15,166 MXN).

### 2.2 Razonamiento explícito sobre el efecto en el ticket promedio de e-commerce

Si —como sugiere la evidencia de la sección 1.3— **Salas y Recámaras concentran la mayor profundidad de SKU** (más variantes, más sub-colecciones) y dentro de Salas existe una sub-línea "Modular Premium/Elite" con precios de $27,000–$42,000+ MXN, el efecto razonado sobre el ticket promedio de e-commerce es el siguiente:

- **Si el volumen de unidades vendidas está sesgado hacia el polo de entrada** (sillas, mesas sueltas, salas de Mercado Libre de bajo precio) — que es plausible dado que estas piezas tienen menor barrera de decisión de compra y son las que aparecen con descuentos agresivos en TikTok (ver sección 3: cupones, "compra mínima de $10,000" para acceder a 50%+15% de descuento) — el **ticket promedio ponderado por unidad quedaría empujado a la baja**, mucho más cerca de los $2,000–$7,000 MXN que de los $30,000+ MXN de las piezas premium.
- **Si en cambio el valor total de venta (GMV) está concentrado en pocas unidades de alto precio** (salas modulares premium, sets de recámara completos, colchones), el ticket promedio en términos de valor —aunque con menos transacciones— podría acercarse o superar los $10,000–$15,000 MXN por transacción, especialmente si el mueble se vende como set (mesa + sillas, armario + cómoda + buró) en vez de pieza suelta.
- La existencia de una **promoción activa observada en TikTok** ("50% de descuento + 15% adicional... compra mínima de $10,000 MXN") es la señal más concreta y reciente (video indexado, campaña vigente) de que **Interimobel mismo está incentivando activamente el ticket hacia arriba de los $10,000 MXN** mediante el umbral de descuento — es decir, la marca ya está usando su propio mix de producto para empujar el ticket promedio al alza combinando SKU de distintas categorías en un solo carrito.

**Conclusión razonada:** la mezcla de catálogo de Interimobel no apunta a un ticket promedio único y estable, sino a una **estructura bimodal**: alto volumen de unidades de bajo precio (tráfico, sillas, piezas sueltas) coexistiendo con menor volumen de unidades de precio alto (salas modulares, sets completos) que generan la mayor parte del GMV. Para el plan de contenido de rebranding, esto tiene una implicación directa: el contenido no puede optimizar solo por "el producto más barato que convierte fácil en redes" (riesgo: erosionar percepción de marca premium) ni solo por "la pieza de diseño más cara" (riesgo: reducir alcance/descubrimiento). El calendario de contenido (sección 6) balancea deliberadamente ambos polos.

**Supuesto explícito y obligatorio:** Interimobel no publica públicamente su ticket promedio real de e-commerce, GMV por categoría, ni volumen de unidades por SKU. Todo lo anterior es un razonamiento inferido a partir de: (a) profundidad relativa de catálogo por categoría, (b) rango de precios de productos indexados individualmente, y (c) una promoción activa observada. **No se debe citar como cifra real de ticket promedio de Interimobel** — es un análisis direccional para orientar el plan de contenido, no un dato financiero verificado. Si el cliente tiene acceso a su propio Shopify Analytics / reporte de Mercado Libre, ese dato interno debe reemplazar este razonamiento antes de tomar decisiones de inversión en pauta.

---

## 3. Auditoría de redes sociales actuales

**Limitación declarada de entrada:** Facebook e Instagram muestran muro de login casi total incluso para contenido público cuando se intenta acceder directamente (confirmado: la búsqueda de la propia página de Facebook devuelve como primer resultado "Inicia sesión o regístrate para verlo"). Todo lo que sigue proviene de metadatos indexados por motores de búsqueda (bios, conteos, títulos de post, y transcripciones parciales de texto de TikTok, que sí es indexable con mayor libertad). Se documenta como reconstrucción indirecta, no observación directa del feed.

### 3.1 Facebook

- Página activa desde el 1 de marzo de 2016 (antigüedad de casi 10 años), categorizada como "Shopping & Retail", con ubicación física referenciada en Héroes del 47, Ciudad de México.
- Contenido histórico indexado incluye: anuncios de renovación/remodelación de tienda, mensajes de "somos fabricantes", promoción de sucursales por zona ("Localiza tu sucursal más cercana"), y videos de sucursal (ej. video de sucursal Coacalco con métricas modestas: 1.2K vistas, 12 likes, 2 "me encanta", 0 comentarios, 7 compartidos en el ejemplo encontrado).
- **Lectura:** el patrón de contenido histórico en Facebook se orienta a mensajes institucionales/de sucursal más que a contenido de descubrimiento de producto o de marca aspiracional — coherente con un uso de Facebook como canal de "generación mayor" (Gen X/Boomers), que es exactamente el patrón de uso de plataforma reportado para ese segmento en México (ver sección 4).

**Fuente:** resultados de búsqueda sobre facebook.com/interimobel y es-la.facebook.com/interimobel — consulta 11-jul-2026.

### 3.2 Instagram

- Cuenta @interimobel, bio "Muebles y decoración", con **4,391 seguidores y 345 publicaciones** (cifra indexada en el momento de la consulta — puede variar; no se debe tratar como en tiempo real).
- Tiene sección de Reels activa con contenido de producto/decoración.
- **Lectura:** para una marca con 25+ años y catálogo tan amplio, 4,391 seguidores es una base relativamente pequeña — sugiere que Instagram ha sido un canal secundario/no priorizado hasta ahora, lo cual es tanto un riesgo (poca base construida) como una oportunidad (mucho espacio de crecimiento con una estrategia de contenido más deliberada, que es justamente el objetivo de este encargo).

**Fuente:** instagram.com/interimobel/ — consulta 11-jul-2026.

### 3.3 TikTok

Esta es la plataforma con **mayor volumen de contenido indexado y accesible** en esta investigación (TikTok es más permisivo con indexación de texto/hashtags de video que Meta). Hallazgos de tono y formato, basados en transcripciones de texto de al menos 7 videos indexados:

- **Tono:** casual, coloquial, con emojis abundantes, llamadas a la acción directas ("¿Vas a dejar que se te escape?", "¡No dejes pasar esta oportunidad!"), fuerte uso de humor/urgencia tipo "FOMO" — un tono claramente orientado a audiencias jóvenes (Gen Z/millennial joven), no institucional.
- **Formatos observados:** promoción de descuentos con cupón ("PRIMAVERA10"), recorridos de sucursal ("Gabriel Mancera y División del Norte", "Revolución"), producto destacado individual (Sala Esquinera Zurich con función sofá-cama), narrativa de manufactura nacional ("100% hechos en México... Orgullo Mexicano"), y contenido de categoría específica (persianas, comedores a meses sin intereses).
- **Hashtags recurrentes:** #fyp #paratii #mexico #cdmx #muebles #hogar — hashtags genéricos de descubrimiento, no hashtags de nicho o de campaña propia consistente (no se detectó un hashtag de marca propio usado de forma sistemática más allá de #Interimobel).
- **Señal de segmentación por edad:** **no se detectó segmentación explícita por edad/generación en el contenido de TikTok revisado** — el tono es uniformemente "joven-general" incluso cuando el producto (ej. comedores a 12 meses sin intereses) podría hablarle mejor a un público de mayor edad/poder adquisitivo familiar con un tono distinto. Este es precisamente el vacío que el encargo del cliente busca cerrar.

**Fuente:** tiktok.com/@interimobel, videos indexados con IDs 7329649942111227141, 7480582710398831927, 7507286072678796550, 7497322073090510086, 7441691310467124487, 7436111494653250871, 7493914349757598982, 7493923610231442693, 7489134488245521670 — consulta 11-jul-2026.

### 3.4 Síntesis de la auditoría de redes

| Plataforma | Seguidores/actividad (indexado) | Tono dominante | Público aparente |
|---|---|---|---|
| Facebook | Página desde 2016; engagement bajo en ejemplos encontrados | Institucional / de sucursal | Gen X / Boomer (por uso de plataforma, no por segmentación deliberada) |
| Instagram | 4,391 seguidores, 345 posts | Producto/decoración, formato Reels | Mixto, subdesarrollado |
| TikTok | Actividad reciente y frecuente (múltiples videos en 2025-2026) | Urgencia/descuento, casual, joven | Gen Z / millennial joven, sin segmentación etaria explícita |

**Hallazgo central de esta sección:** Interimobel **no tiene actualmente una estrategia de contenido diferenciada por generación** — cada plataforma tiene un tono determinado más por el hábito de uso de esa plataforma que por una decisión deliberada de hablarle a un segmento etario específico con un mensaje adaptado. Esto confirma la premisa del encargo del cliente y es la oportunidad central que el plan de la sección 6 debe capturar.

---

## 4. Contexto de mercado para el rebranding 2027+

### 4.1 Mercado de mueblería y e-commerce en México

- El mercado de muebles de hogar en México se proyecta con una tasa de crecimiento anual compuesta (CAGR) de **3.4% entre 2024 y 2032**, impulsado por urbanización, mayor disponibilidad de opciones de personalización y auge de comercio electrónico. Fuente: [Mordor Intelligence — Mercado de muebles para el hogar en México](https://www.mordorintelligence.com/es/industry-reports/mexico-home-furniture-market), consulta 11-jul-2026.
- México se proyecta para **superar a Estados Unidos en penetración de e-commerce en 2026**, con el comercio electrónico mexicano creciendo ~27% y superando los USD $30,000 millones en ventas anuales. Fuente: [AmericaMalls & Retail — El panorama para 2026 sitúa a México en la vanguardia del retail digital](https://americaretail-malls.com/paises/mexico/el-panorama-para-2026-situa-a-mexico-en-la-vanguardia-del-retail-digital/), consulta 11-jul-2026.
- Tendencias de retail 2026 destacan **IA aplicada a retail media/personalización**, consolidación de **social commerce** (TikTok e Instagram como canales de descubrimiento vía livestream y contenido de creadores), y un consumidor que prioriza **conveniencia, trazabilidad, materiales responsables y políticas de empresa claras**. Fuentes: [The Logistics World — Retail Media en México: tendencias hacia 2026](https://thelogisticsworld.com/logistica-comercio-electronico/retail-media-mexico-tendencias-hacia-2026/); [AmericaMalls & Retail — Tendencias clave que redefinirán el e-commerce en 2026](https://americaretail-malls.com/paises/mexico/tendencias-clave-que-redefiniran-el-e-commerce-en-2026/), consulta 11-jul-2026.

**Implicación para el rebranding 2027+:** una marca de mueblería con 25+ años que hoy compite en gran medida con descuentos agresivos (ver TikTok, sección 3.3) tiene una oportunidad de reposicionarse hacia atributos de mayor valor percibido (manufactura nacional ya presente en su narrativa, personalización, materiales) en línea con hacia dónde se mueve el consumidor mexicano — sin abandonar la palanca de descuento, que sigue siendo relevante para el segmento de menor edad/poder adquisitivo.

### 4.2 Consumo por generación

- Gen Z ya representa aproximadamente **35% de los consumidores en México** y para 2026 suma cerca de **32 millones de personas (25% de la población total)**. Fuente: [Agente Digitalizado — Gen Z Mexicana 2026](https://agentedigitalizado.com/gen-z-mexicana-2026-perfil-de-consumo-habitos-digitales-y-lo-que-realmente-valora-este-segmento/), consulta 11-jul-2026.
- **46% de Gen Z inicia búsquedas en redes sociales**, superando a buscadores tradicionales; ejemplos citados: 40% busca en TikTok vs. 25% en Google para temas de belleza, 36% vs. 29% para marcas de moda. Fuente: [Merca2.0 — Tendencias de marketing 2026: Gen Z, la generación que consume con causa](https://www.merca20.com/tendencias-de-marketing-2026-gen-z-la-generacion-que-consume-con-causa/), consulta 11-jul-2026.
- Preferencia de plataforma por generación: **Gen Z** pasa más tiempo en Instagram (41%), **Millennials** se reparten entre Facebook e Instagram (42% cada uno), **Gen X y Boomers** se concentran en Facebook (40%). Fuente: [Sprout Social — The 2025 generational marketing playbook](https://sproutsocial.com/insights/guides/generational-marketing/), consulta 11-jul-2026.
- Generación X muestra **alta fidelidad de marca basada en confianza y experiencia** (no en tendencia), lo que la hace receptiva a campañas multicanal que combinan medios tradicionales y digitales. Fuente: [Merca2.0 — Generación X: la clave del consumo intermedio entre millennials y boomers](https://www.merca20.com/generacion-x-la-clave-del-consumo-intermedio-entre-millennials-y-boomers/), consulta 11-jul-2026.
- En México, para octubre de 2025 había **99 millones de identidades de usuario en redes sociales (74.9% de la población)**; Facebook lidera en tasa de uso (92.5%) pero WhatsApp lidera en preferencia (27.6%), con TikTok en clara alza (21.6% de preferencia, tercer lugar). El segmento de mayor uso es **millennials de 25-34 años** (15.7% mujeres, 15.5% hombres). Fuente: [Way2net — Estadísticas de Redes Sociales México 2025](https://www.way2net.com/2025/05/estadisticas-de-redes-sociales-mexico-2025/), consulta 11-jul-2026.

**Supuesto declarado:** las cifras de "preferencia de plataforma por generación" citadas (Sprout Social) son de fuente internacional/EUA, no específica de México; se usan como referencia direccional de comportamiento generacional transferible, no como dato duro mexicano. Donde existe dato mexicano específico (Way2net, INEGI/ENDUTIH), se prioriza este último.

---

## 5. Segmentación multipúblico por edad

Con base en la auditoría del sitio (sección 1-2) y de redes (sección 3), se proponen **4 segmentos generacionales**, diseñados para no contradecir lo ya observado (ej.: no se inventa una segmentación por edad que el catálogo actual no pueda sostener; se ancla cada segmento a categorías de producto reales del catálogo).

### Segmento 1 — "Primer Nido" (Gen Z tardía / Millennial joven, ~22-30 años)
Independizándose o rentando su primer departamento. Presupuesto acotado, alta sensibilidad a precio, decisión de compra rápida e impulsiva vía redes.
- **Ángulo de contenido:** piezas sueltas de entrada (sillas desde $590 MXN, mesas desde $1,750 MXN, salas de Mercado Libre ~$2,750 MXN), formato "amuebla tu depa en X presupuesto", tono TikTok ya validado por la marca (urgencia, cupones, humor).
- **Canal primario:** TikTok e Instagram Reels.
- **Coherencia con hallazgos:** este es el segmento que la marca ya sirve mejor hoy (tono TikTok actual) — el trabajo aquí es profundizar, no inventar.

### Segmento 2 — "Casa Nueva / Familia Joven" (Millennial establecido, ~30-42 años)
Compra casa o departamento más grande, amuebla varias habitaciones a la vez (sala + comedor + recámara), ticket de compra más alto y decisión más deliberada (compara, pide meses sin intereses).
- **Ángulo de contenido:** sets completos y promociones tipo "compra mínima de $10,000 MXN" ya observadas en TikTok, contenido de "antes/después" de espacios completos, mensajes de financiamiento (meses sin intereses, ya confirmado como práctica vigente en Mercado Libre e Interimobel.mx).
- **Canal primario:** Instagram (carruseles de "moodboard" por espacio) + TikTok.
- **Coherencia con hallazgos:** conecta directamente con la señal de la sección 2.2 (la marca ya empuja el ticket hacia sets combinados).

### Segmento 3 — "Renovación con Criterio" (Gen X, ~43-58 años)
Ya tiene casa amueblada; busca renovar piezas específicas (sala, colchón, comedor) por desgaste o cambio de gusto, no por mudanza. Mayor poder adquisitivo relativo, decisión menos impulsiva, valora calidad/durabilidad y confianza de marca (25+ años de trayectoria es un activo aquí, hoy subutilizado en el contenido).
- **Ángulo de contenido:** historia de marca ("25+ años, hechos en México"), garantía y durabilidad, testimonios reales de clientes (dado que las reseñas mixtas de logística son un riesgo conocido, este segmento es el más sensible a señales de confiabilidad — usar testimonios auténticos, no solo promocionales), catálogo premium (salas modulares Elite/Premium, colchones Comfort Tech).
- **Canal primario:** Facebook (ya es su hábito de plataforma) + email/WhatsApp para seguimiento post-contacto.
- **Coherencia con hallazgos:** Facebook ya tiene tono institucional; se trata de evolucionar ese tono a uno más aspiracional/de confianza sin perder el canal.

### Segmento 4 — "Nido que se Renueva" (Boomer activo / pre-jubilación, ~59-70 años)
Hijos que se van de casa, reduce espacio o rediseña una habitación para nuevo uso (home office, cuarto de huéspedes, estudio). Motivado por comodidad y practicidad más que por tendencia; alta confianza en recomendación directa y atención personalizada (sucursal física).
- **Ángulo de contenido:** contenido híbrido online-offline ("visítanos en sucursal", ya presente en Facebook/TikTok de la marca), enfoque en piezas de comodidad (recámaras, colchones, salas reclinables — categoría ya existente en el catálogo), tono cálido y sin urgencia artificial (evitar el FOMO agresivo del contenido TikTok actual, que no resuena con este segmento).
- **Canal primario:** Facebook + WhatsApp Business (el número de contacto 55 8106 5385 ya es canal activo confirmado en el sitio).
- **Coherencia con hallazgos:** este es el segmento menos servido hoy; el catálogo (reclinables, colchones premium) ya lo soporta, falta el contenido dedicado.

---

## 6. Plan de contenido digital a 90 días

**Principio rector:** cada mes mantiene los 4 segmentos activos simultáneamente (no se secuencian por mes), pero cada mes tiene un **pilar temático rotativo** que da foco a la producción sin abandonar ningún segmento. Todo el calendario es ejecutable con los formatos y canales que la marca ya opera (TikTok, Instagram, Facebook), evitando prescribir canales nuevos no validados en la auditoría.

### Mes 1 (agosto 2026) — Pilar: "Quién es Interimobel hoy" (fundamento de marca para el rebranding)
Objetivo: sentar la narrativa de marca renovada antes de escalar producto, aprovechando el activo subutilizado de "25+ años / hecho en México".

| Semana | Segmento foco | Formato | Pieza de contenido |
|---|---|---|---|
| 1 | Segmento 3 (Gen X) | Reel/TikTok 30-45s | Historia de marca: "25 años vistiendo hogares mexicanos" con planta/manufactura si disponible |
| 1 | Segmento 1 (Primer Nido) | TikTok 15-20s | Reto "amuebla tu depa con $15,000 MXN" usando catálogo de entrada |
| 2 | Segmento 2 (Familia joven) | Carrusel Instagram (6-8 slides) | "Antes/después" de una casa completa amueblada por Interimobel (sala+comedor+recámara) |
| 2 | Segmento 4 (Boomer activo) | Post Facebook + foto | Presentación de línea de salas reclinables y colchones Comfort Tech, tono cálido, CTA a WhatsApp |
| 3 | Todos | Historia Instagram/Facebook | Detrás de cámaras: "así se fabrica" (activo de manufactura nacional) |
| 3 | Segmento 3 | Testimonio en video (UGC o staged) | Cliente real hablando de durabilidad/calidad (mitigar el hallazgo de reseñas mixtas sobre logística, mostrando garantía y respaldo) |
| 4 | Segmento 1 | TikTok trend/humor | Participación en un trend vigente de TikTok adaptado a mueblería ("cosas que necesitas saber antes de amueblar tu primer depa") |
| 4 | Todos | Recap mensual (Reel) | Mosaico de las piezas más queridas del mes ("Los Vendidos" como narrativa, no solo colección de catálogo) |

**Cadencia sugerida mes 1:** TikTok 4-5x/semana, Instagram (feed+reels) 3x/semana, Facebook 2-3x/semana.

### Mes 2 (septiembre 2026) — Pilar: "Vive tu espacio" (producto + ocasión de compra por segmento)
Objetivo: convertir el fundamento de marca del mes 1 en contenido orientado a categorías específicas, ligado a ocasiones reales de compra (regreso a clases/oficina en casa, inicio de temporada de mudanzas post-verano).

| Semana | Segmento foco | Formato | Pieza de contenido |
|---|---|---|---|
| 1 | Segmento 2 | Carrusel + Reel | Línea "Home Office" para quien regresa a rutina híbrida — ligar a escritorios/sillas del catálogo |
| 1 | Segmento 1 | TikTok | "3 combos de sala bajo $5,000" (usa el polo de entrada identificado en sección 2.1) |
| 2 | Segmento 3 | Reel comparativo | "Cómo renovar solo tu comedor sin cambiar toda la casa" — ángulo de renovación puntual |
| 2 | Segmento 4 | Facebook Live o video largo | Recorrido de sucursal con enfoque en piezas de comodidad, invitación a visitar en persona |
| 3 | Segmento 2 | TikTok | Explicación simple de meses sin intereses / financiamiento, con ejemplo de set de recámara completo |
| 3 | Segmento 1 | Reel | Colaboración con micro-creador de contenido de "primer depa" (alineado con hallazgo de sección 4.1: microcreadores > macroinfluencers) |
| 4 | Todos | Encuesta/interacción Instagram Stories | "¿Qué habitación renuevas primero?" — insumo real de segmentación para mes 3 |
| 4 | Segmento 3/4 | Post Facebook | Reseña de cliente real + explicación clara de política de garantía/devoluciones (construir confianza) |

**Cadencia sugerida mes 2:** TikTok 4-5x/semana, Instagram 3-4x/semana, Facebook 2-3x/semana.

### Mes 3 (octubre 2026) — Pilar: "Prepara tu casa" (temporada alta pre-fin de año + prueba de concepto de rebranding)
Objetivo: capitalizar el arranque de temporada alta de decoración/fin de año (Buen Fin es en noviembre; octubre es ventana de "preparación") y medir qué segmento/pilar tuvo mejor desempeño para ajustar el plan del Q4.

| Semana | Segmento foco | Formato | Pieza de contenido |
|---|---|---|---|
| 1 | Segmento 2 | Reel + Carrusel | "Prepara tu comedor para las reuniones de fin de año" — sets de comedor como hilo conductor |
| 1 | Segmento 1 | TikTok | Adelanto de promociones de temporada (la marca ya tiene track record de cupones tipo "PRIMAVERA10" — versión de temporada) |
| 2 | Segmento 4 | Facebook | Contenido de "renueva el cuarto de visitas para las fiestas" — ángulo de comodidad para huéspedes |
| 2 | Segmento 3 | Reel testimonial | Casos de clientes Gen X que renovaron sala/colchón — refuerzo de confianza antes de Buen Fin |
| 3 | Todos | Teaser cross-canal | Anuncio de campaña Buen Fin (aunque la ejecución de Buen Fin cae en noviembre, fuera de esta ventana de 90 días, se declara como transición planeada) |
| 3 | Segmento 1/2 | TikTok | Contenido co-creado con micro-influencer de decoración/diseño de interiores accesible |
| 4 | Todos | Reel de cierre de trimestre | Recap de 90 días: mostrar evolución visual del contenido (de institucional a multigeneracional) como prueba social del rebranding en marcha |
| 4 | Interno/equipo | — | Revisión de métricas por segmento (alcance, guardados, CTA a WhatsApp/sitio) para ajustar pilares del Q4 2026 |

**Cadencia sugerida mes 3:** TikTok 5x/semana (escala por temporada), Instagram 3-4x/semana, Facebook 3x/semana.

### Notas de ejecución transversales

- **Todos los segmentos reciben al menos una pieza de contenido dedicada cada mes** (cumplido explícitamente en las tablas anteriores), evitando que el plan colapse de vuelta al tono único "joven-general" detectado como vacío actual en la sección 3.4.
- **No se recomienda abandonar el tono TikTok actual** (urgencia/descuento) para el Segmento 1 — es el que mejor funciona hoy según la evidencia; se recomienda en cambio **añadir** los tonos faltantes para los otros tres segmentos, en los canales donde cada uno ya tiene el hábito de consumo (Facebook para Gen X/Boomer, Instagram como puente para Millennial).
- **Riesgo a monitorear:** las quejas de logística/entrega detectadas en reseñas (sección 1.4) son más dañinas para los Segmentos 3 y 4 (que valoran confianza y compran con menos frecuencia pero mayor ticket) que para el Segmento 1. Cualquier pieza de contenido dirigida a Gen X/Boomer que prometa "tranquilidad" debe estar respaldada por una mejora operativa real o moderar la promesa — de lo contrario el contenido puede generar el efecto contrario (exponer la marca a comentarios negativos en un canal de mayor visibilidad).
- **Medición sugerida (no cuantificada por falta de datos internos):** dado que no se tuvo acceso a Analytics real de Interimobel, se recomienda que el cliente instrumente al menos alcance, guardados y clics a WhatsApp/sitio por pieza de contenido, etiquetados por segmento objetivo, para poder validar o refutar esta segmentación al cierre de los 90 días con datos propios — no con las inferencias de este documento.

---

## Resumen de fuentes citadas

- Colecciones y productos interimobel.mx (reconstrucción vía búsqueda) — consulta 11-jul-2026
- [Tienda Oficial Interimobel — Mercado Libre](https://tienda.mercadolibre.com.mx/interimobel) — consulta 11-jul-2026
- [Instagram @interimobel](https://www.instagram.com/interimobel/) — consulta 11-jul-2026
- [Facebook Interimobel](https://www.facebook.com/interimobel/) — consulta 11-jul-2026
- [TikTok @interimobel](https://www.tiktok.com/@interimobel) — consulta 11-jul-2026
- [Indeed — Reseñas Interimobel](https://mx.indeed.com/cmp/Interimobel/reviews) — consulta 11-jul-2026
- [Mordor Intelligence — Mercado de muebles para el hogar en México](https://www.mordorintelligence.com/es/industry-reports/mexico-home-furniture-market) — consulta 11-jul-2026
- [AmericaMalls & Retail — Panorama retail digital México 2026](https://americaretail-malls.com/paises/mexico/el-panorama-para-2026-situa-a-mexico-en-la-vanguardia-del-retail-digital/) — consulta 11-jul-2026
- [The Logistics World — Retail Media en México 2026](https://thelogisticsworld.com/logistica-comercio-electronico/retail-media-mexico-tendencias-hacia-2026/) — consulta 11-jul-2026
- [Agente Digitalizado — Gen Z Mexicana 2026](https://agentedigitalizado.com/gen-z-mexicana-2026-perfil-de-consumo-habitos-digitales-y-lo-que-realmente-valora-este-segmento/) — consulta 11-jul-2026
- [Merca2.0 — Tendencias de marketing 2026: Gen Z](https://www.merca20.com/tendencias-de-marketing-2026-gen-z-la-generacion-que-consume-con-causa/) — consulta 11-jul-2026
- [Merca2.0 — Generación X](https://www.merca20.com/generacion-x-la-clave-del-consumo-intermedio-entre-millennials-y-boomers/) — consulta 11-jul-2026
- [Sprout Social — 2025 generational marketing playbook](https://sproutsocial.com/insights/guides/generational-marketing/) — consulta 11-jul-2026
- [Way2net — Estadísticas de Redes Sociales México 2025](https://www.way2net.com/2025/05/estadisticas-de-redes-sociales-mexico-2025/) — consulta 11-jul-2026
