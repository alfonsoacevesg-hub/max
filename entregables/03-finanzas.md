# 03. Finanzas — E-commerce de Mobiliario (CDMX/EdoMéx + Nacional)

**Fecha de elaboración:** 8 de julio de 2026
**Tipo de cambio de referencia:** 17.39 MXN/USD
**Metodología:** todos los cálculos de este documento se generan con un modelo en Python (apéndice al final, reproducible). No hay aritmética manual en el cuerpo del documento.

## Resumen ejecutivo — los 3 números que mandan

1. **Punto de equilibrio operativo: 5.35 ventas/mes** (multiplicador 1.75x, fijos $9,300 MXN/mes, ISR de primer tramo RESICO). Esto valida con precisión el rango "5–6 ventas/mes" que el fundador ya tenía como supuesto cerrado: el modelo financiero completo (con comisión de pasarela, tabla real de ISR RESICO y fee de envío) confirma la cifra dentro de ±0.65 unidades.
2. **Escenario base: recupera la ronda ($57,000) en el mes 10** y cierra el mes 12 con una posición de caja de **$73,957 MXN** (+30% sobre el capital inicial), tras un valle de caja de $44,019 MXN en el mes 4 — nunca toca cero. El equilibrio mensual (resultado ≥ $0) llega en el mes 5, exactamente cuando las ventas alcanzan 6/mes.
3. **Escenario pesimista: NO hay quiebre de caja en los 12 meses, pero el runway se consume casi por completo.** La caja cae de $57,000 a un mínimo/cierre de **$8,603 MXN al mes 12** (colchón equivalente a ~0.9 meses de costos fijos), sin llegar nunca a resultado mensual positivo. Es un escenario de sobrevivencia técnica, no de negocio viable: confirma que la señal de salida declarada ("<3 ventas/mes sostenidas al mes 5") es la variable crítica a monitorear — en este escenario las ventas llegan a exactamente 3/mes en el mes 5 (el límite, no por debajo de él), lo que en la práctica significa que el fundador debe vigilar el dato real del mes 5 con un margen de tolerancia cero.

| Escenario | Mes de equilibrio operativo | Mes de recuperación de la ronda | Mes de quiebre de caja | Caja al mes 12 | Ingreso total 12m |
|---|---|---|---|---|---|
| Pesimista | No ocurre en 12m | No ocurre en 12m | No ocurre (mínimo $8,603 al mes 12) | $8,603 | $166,953 |
| Base | Mes 5 | Mes 10 | No ocurre (mínimo $44,019 al mes 4) | $73,957 | $383,541 |
| Optimista | Mes 4 | Mes 6 | No ocurre (mínimo $49,841 al mes 3) | $115,837 | $496,348 |

## 1. Supuestos del modelo (no se cambian sin instrucción explícita)

| Parámetro | Valor | Fuente/nota |
|---|---|---|
| Ticket promedio | $4,500 MXN | Dato cerrado del negocio |
| Multiplicador base | 1.75x | Costo proveedor = precio / 1.75 = **$2,571.43 MXN/unidad** |
| Margen bruto por unidad | $1,928.57 MXN | Precio − costo proveedor |
| Comisión pasarela (Shopify Payments) | 3.4% + $3 MXN fijo por transacción | Dato cerrado del negocio |
| ISR RESICO persona física | Tabla real por tramo de ingreso acumulado (ver sección 1.1) | Art. 113-E LISR |
| Costos fijos mensuales (meses 1–7, todos los escenarios; y meses 8–12 en pesimista) | $9,300 MXN | Incluye $5,000 MXN de pauta Meta Ads |
| Costos fijos mensuales (meses 8–12, solo base y optimista) | $12,300 MXN | Pauta escala de $5,000 a $8,000 MXN |
| Capital inicial (ronda pre-seed) | $57,000 MXN | Sin buffer hasta mes 9 (decisión del fundador) |
| Fee de envío local (ingreso) | 5% sobre tarifa Lalamove promedio $350 MXN, en ~70% de las órdenes (CDMX/EdoMéx) | = **$12.25 MXN de ingreso ponderado por orden**, margen puro (no genera costo de ventas adicional porque la tarifa base se paga íntegra a Lalamove) |
| Envío foráneo (Skydropx) | Tarifa API sin marcaje | Margen $0 → no se modela como línea de ingreso |
| Recompra anual | 15% | Dato cerrado del negocio, usado para LTV |

**Ingreso total por orden usado en el modelo:** $4,500 + $12.25 = **$4,512.25 MXN** (ponderando la mezcla local/foráneo).

### 1.1 Tabla real RESICO persona física (ISR mensual)

La tasa de ISR en RESICO no es un porcentaje plano: se determina por el **ingreso acumulado desde enero del ejercicio a la fecha**, y esa tasa se multiplica por el ingreso **del mes**. Tabla vigente (Art. 113-E LISR, sin cambios 2025→2026):

| Ingreso acumulado (desde) | Ingreso acumulado (hasta) | Tasa |
|---|---|---|
| $0.01 | $25,000.00 | 1.00% |
| $25,000.01 | $50,000.00 | 1.10% |
| $50,000.01 | $83,333.33 | 1.50% |
| $83,333.34 | $208,333.33 | 2.00% |
| $208,333.34 | $3,500,000.00 | 2.50% |

Fuente: LISR Art. 113-E, consultada a través de fuentes especializadas de actualización fiscal — facturama.mx/blog/tablas-resico, resicocalc.com/blog/tablas-isr-resico-2026, herramientasfiscales.mx/blog/calcular-isr-resico-2026 (consultado 08-jul-2026); tasas confirmadas sin cambio respecto a 2025.

**Supuesto declarado:** se asume que el mes 1 de operación de cada escenario coincide con enero del ejercicio fiscal, de forma que el acumulado para efectos de tramo RESICO reinicia junto con el arranque del modelo a 12 meses. En la operación real, si el negocio arranca a mitad de año calendario, el acumulado relevante para el tramo de ISR sería el del ejercicio fiscal en curso (que podría incluir ingresos previos si los hubiera) — este supuesto debe revisarse con el contador al momento de dar de alta el régimen.

**Nota de comparabilidad:** el documento de referencia del negocio declara una contribución neta de "~$1,700 MXN por venta" usando una tasa ISR plana de 1.5%. El modelo aquí construido reproduce esa cifra con precisión ($1,716.72 MXN, ver sección 4 KPIs) cuando se usa la misma tasa plana de comparabilidad; el P&L mensual detallado usa en cambio la tasa real por tramo, que sube conforme crece el ingreso acumulado del año.

## 2. Escenarios de ventas (unidades/mes, 12 meses)

| Mes | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Pesimista | 1 | 1 | 2 | 2 | 3 | 3 | 3 | 4 | 4 | 4 | 5 | 5 |
| Base | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 8 | 9 | 10 | 11 | 12 |
| Optimista (base ×1.3, redondeado) | 3 | 4 | 5 | 6 | 8 | 9 | 10 | 10 | 12 | 13 | 14 | 16 |

## 3. P&L mensual y flujo de caja por escenario

#### Escenario Pesimista — P&L mensual (MXN)

| Mes | Unidades | Ingreso total | Costo de ventas | Margen bruto | Comisión pasarela | Tasa ISR | ISR RESICO | Costos fijos | Resultado del mes | Resultado acumulado | Caja acumulada |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 1 | 1 | $4,512 | $2,571 | $1,941 | $156 | 1.00% | $45 | $9,300 | $-7,561 | $-7,561 | $49,439 |
| 2 | 1 | $4,512 | $2,571 | $1,941 | $156 | 1.00% | $45 | $9,300 | $-7,561 | $-15,121 | $41,879 |
| 3 | 2 | $9,024 | $5,143 | $3,882 | $313 | 1.00% | $90 | $9,300 | $-5,821 | $-20,943 | $36,057 |
| 4 | 2 | $9,024 | $5,143 | $3,882 | $313 | 1.10% | $99 | $9,300 | $-5,830 | $-26,773 | $30,227 |
| 5 | 3 | $13,537 | $7,714 | $5,822 | $469 | 1.10% | $149 | $9,300 | $-4,096 | $-30,869 | $26,131 |
| 6 | 3 | $13,537 | $7,714 | $5,822 | $469 | 1.50% | $203 | $9,300 | $-4,150 | $-35,019 | $21,981 |
| 7 | 3 | $13,537 | $7,714 | $5,822 | $469 | 1.50% | $203 | $9,300 | $-4,150 | $-39,169 | $17,831 |
| 8 | 4 | $18,049 | $10,286 | $7,763 | $626 | 2.00% | $361 | $9,300 | $-2,523 | $-41,692 | $15,308 |
| 9 | 4 | $18,049 | $10,286 | $7,763 | $626 | 2.00% | $361 | $9,300 | $-2,523 | $-44,215 | $12,785 |
| 10 | 4 | $18,049 | $10,286 | $7,763 | $626 | 2.00% | $361 | $9,300 | $-2,523 | $-46,739 | $10,261 |
| 11 | 5 | $22,561 | $12,857 | $9,704 | $782 | 2.00% | $451 | $9,300 | $-829 | $-47,568 | $9,432 |
| 12 | 5 | $22,561 | $12,857 | $9,704 | $782 | 2.00% | $451 | $9,300 | $-829 | $-48,397 | $8,603 |

**Lectura:** no hay mes de resultado positivo ni de recuperación de la ronda dentro de los 12 meses. No se llega a un quiebre de caja formal (la caja nunca cruza cero), pero al mes 12 el colchón restante ($8,603 MXN) equivale a menos de un mes de costos fijos — el negocio queda sin margen de maniobra para absorber cualquier imprevisto (falta de stock en Mundo In, devolución, gasto no presupuestado). **Decisión que gatilla:** dado que las ventas llegan a exactamente 3/mes en el mes 5 (el límite exacto de la señal de salida "<3 ventas/mes sostenidas al mes 5", no por debajo de él), el fundador debe tratar el dato real del mes 5 como punto de decisión de alta sensibilidad: un solo mes por debajo de 3 ventas activa la señal de salida declarada y debería detener el escalamiento de pauta antes del mes 8.

#### Escenario Base — P&L mensual (MXN)

| Mes | Unidades | Ingreso total | Costo de ventas | Margen bruto | Comisión pasarela | Tasa ISR | ISR RESICO | Costos fijos | Resultado del mes | Resultado acumulado | Caja acumulada |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 1 | 2 | $9,024 | $5,143 | $3,882 | $313 | 1.00% | $90 | $9,300 | $-5,821 | $-5,821 | $51,179 |
| 2 | 3 | $13,537 | $7,714 | $5,822 | $469 | 1.00% | $135 | $9,300 | $-4,082 | $-9,904 | $47,096 |
| 3 | 4 | $18,049 | $10,286 | $7,763 | $626 | 1.10% | $199 | $9,300 | $-2,361 | $-12,265 | $44,735 |
| 4 | 5 | $22,561 | $12,857 | $9,704 | $782 | 1.50% | $338 | $9,300 | $-716 | $-12,981 | $44,019 |
| 5 | 6 | $27,074 | $15,429 | $11,645 | $938 | 2.00% | $541 | $9,300 | $865 | $-12,116 | $44,884 |
| 6 | 7 | $31,586 | $18,000 | $13,586 | $1,095 | 2.00% | $632 | $9,300 | $2,559 | $-9,557 | $47,443 |
| 7 | 8 | $36,098 | $20,571 | $15,527 | $1,251 | 2.00% | $722 | $9,300 | $4,253 | $-5,304 | $51,696 |
| 8 | 8 | $36,098 | $20,571 | $15,527 | $1,251 | 2.00% | $722 | $12,300 | $1,253 | $-4,050 | $52,950 |
| 9 | 9 | $40,610 | $23,143 | $17,467 | $1,408 | 2.50% | $1,015 | $12,300 | $2,744 | $-1,306 | $55,694 |
| 10 | 10 | $45,122 | $25,714 | $19,408 | $1,564 | 2.50% | $1,128 | $12,300 | $4,416 | $3,110 | $60,110 |
| 11 | 11 | $49,635 | $28,286 | $21,349 | $1,721 | 2.50% | $1,241 | $12,300 | $6,088 | $9,198 | $66,198 |
| 12 | 12 | $54,147 | $30,857 | $23,290 | $1,877 | 2.50% | $1,354 | $12,300 | $7,759 | $16,957 | $73,957 |

**Lectura:** el valle de caja ocurre en el mes 4 ($44,019 MXN, 77% del capital inicial), coincidiendo con el punto de mínimo resultado acumulado (-$12,981). El resultado mensual se vuelve positivo en el mes 5 (6 ventas), confirmando el punto de equilibrio calculado de 5.35 unidades/mes. La ronda se recupera (resultado acumulado ≥ $0) en el mes 10. El escalamiento de pauta a $8,000 en el mes 8 reduce momentáneamente el resultado del mes (de $4,253 a $1,253) pero no revierte la tendencia.

#### Escenario Optimista — P&L mensual (MXN)

| Mes | Unidades | Ingreso total | Costo de ventas | Margen bruto | Comisión pasarela | Tasa ISR | ISR RESICO | Costos fijos | Resultado del mes | Resultado acumulado | Caja acumulada |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 1 | 3 | $13,537 | $7,714 | $5,822 | $469 | 1.00% | $135 | $9,300 | $-4,082 | $-4,082 | $52,918 |
| 2 | 4 | $18,049 | $10,286 | $7,763 | $626 | 1.10% | $199 | $9,300 | $-2,361 | $-6,443 | $50,557 |
| 3 | 5 | $22,561 | $12,857 | $9,704 | $782 | 1.50% | $338 | $9,300 | $-716 | $-7,159 | $49,841 |
| 4 | 6 | $27,074 | $15,429 | $11,645 | $938 | 1.50% | $406 | $9,300 | $1,000 | $-6,159 | $50,841 |
| 5 | 8 | $36,098 | $20,571 | $15,527 | $1,251 | 2.00% | $722 | $9,300 | $4,253 | $-1,906 | $55,094 |
| 6 | 9 | $40,610 | $23,143 | $17,467 | $1,408 | 2.00% | $812 | $9,300 | $5,947 | $4,042 | $61,042 |
| 7 | 10 | $45,122 | $25,714 | $19,408 | $1,564 | 2.00% | $902 | $9,300 | $7,642 | $11,683 | $68,683 |
| 8 | 10 | $45,122 | $25,714 | $19,408 | $1,564 | 2.50% | $1,128 | $12,300 | $4,416 | $16,099 | $73,099 |
| 9 | 12 | $54,147 | $30,857 | $23,290 | $1,877 | 2.50% | $1,354 | $12,300 | $7,759 | $23,858 | $80,858 |
| 10 | 13 | $58,659 | $33,429 | $25,231 | $2,033 | 2.50% | $1,466 | $12,300 | $9,431 | $33,289 | $90,289 |
| 11 | 14 | $63,172 | $36,000 | $27,172 | $2,190 | 2.50% | $1,579 | $12,300 | $11,102 | $44,392 | $101,392 |
| 12 | 16 | $72,196 | $41,143 | $31,053 | $2,503 | 2.50% | $1,805 | $12,300 | $14,446 | $58,837 | $115,837 |

**Lectura:** el valle de caja es el más superficial de los tres escenarios ($49,841 MXN en el mes 3, 87% del capital inicial). Equilibrio operativo en el mes 4 (6 ventas) y recuperación de la ronda en el mes 6 — cuatro meses antes que el escenario base. Al mes 12, la caja casi duplica el capital inicial ($115,837 MXN).

## 4. Análisis de sensibilidad

### 4.1 Multiplicador: 1.75x (base) vs. 1.50x

| Multiplicador | Costo unitario | Margen bruto/unidad | Unidades/mes para equilibrio |
|---|---|---|---|
| 1.75x (base) | $2,571.43 | $1,928.57 | **5.35** |
| 1.50x (sensibilidad) | $3,000.00 | $1,500.00 | **7.10** |

**Impacto:** bajar el multiplicador de 1.75x a 1.50x eleva el punto de equilibrio en +1.75 unidades/mes (+33%), de 5.35 a 7.10 ventas/mes. En el escenario base, esto retrasaría el mes de equilibrio operativo de mes 5 (6 ventas) a aproximadamente mes 6–7 (7–8 ventas), y empujaría la recuperación de la ronda más allá del mes 10. Un multiplicador de 1.50x es el límite inferior operativo declarado ("1.5–2.0x"); el modelo confirma que operar en ese extremo bajo compromete materialmente la velocidad de breakeven.

### 4.2 CPC +30% y sensibilidad de conversión (0.3% vs. 0.8%)

**Supuesto declarado:** no se localizó, en esta consulta, un benchmark público verificado de CPC de Meta Ads específico para el nicho de mobiliario premium dirigido a las zonas objetivo (Polanco/Lomas, Santa Fe, Interlomas/Huixquilucan, Pedregal). Se declara explícitamente un CPC de trabajo de **$4.00 MXN** como supuesto de partida —sin fuente puntual verificable a nivel de nicho—, sujeto a validación con datos reales de Meta Ads Manager desde el mes 1 de operación. Todo lo que sigue en esta sección es sensibilidad sobre ese supuesto, no un dato de mercado confirmado.

**Embudo presupuesto → clics → ventas:**

| Presupuesto pauta | Escenario CPC | Clics estimados | Conversión | Ventas estimadas del mes (solo canal pauta) |
|---|---|---|---|---|
| $5,000 | CPC base $4.00 | 1,250 | 0.8% | 10.00 |
| $5,000 | CPC base $4.00 | 1,250 | 0.3% | 3.75 |
| $5,000 | CPC +30% ($5.20) | 962 | 0.8% | 7.69 |
| $5,000 | CPC +30% ($5.20) | 962 | 0.3% | 2.88 |
| $8,000 | CPC base $4.00 | 2,000 | 0.8% | 16.00 |
| $8,000 | CPC base $4.00 | 2,000 | 0.3% | 6.00 |
| $8,000 | CPC +30% ($5.20) | 1,538 | 0.8% | 12.31 |
| $8,000 | CPC +30% ($5.20) | 1,538 | 0.3% | 4.62 |

**Presupuesto de pauta requerido para alcanzar el equilibrio (5.35 unidades/mes):**

| Escenario CPC | Conversión | Clics necesarios | Presupuesto requerido (MXN/mes) |
|---|---|---|---|
| CPC base $4.00 | 0.8% | 668 | $2,674 |
| CPC base $4.00 | 0.3% | 1,783 | $7,131 |
| CPC +30% ($5.20) | 0.8% | 668 | $3,476 |
| CPC +30% ($5.20) | 0.3% | 1,783 | $9,270 |

**Lectura:** con conversión de 0.8% (banda alta), el presupuesto de pauta actual ($5,000) es **más que suficiente** para el equilibrio incluso con un CPC 30% más alto (se requerirían solo $3,476). El riesgo real está en la conversión: si la conversión real cae a 0.3% (banda baja), el presupuesto necesario para el equilibrio ($7,131 a CPC base, $9,270 con CPC +30%) **supera el presupuesto de pauta actual de $9,300 total en fijos** y obligaría a: (a) aumentar el presupuesto de pauta por encima de lo modelado en el escenario base, (b) mejorar la tasa de conversión del sitio (oferta, checkout, prueba social), o (c) depender más del canal orgánico/creativo (la ventaja competitiva declarada del negocio) para cerrar la brecha. **La conversión, no el CPC, es la variable de mayor apalancamiento sobre el punto de equilibrio.**

## 5. KPIs financieros

| KPI | Valor | Nota |
|---|---|---|
| Costo unitario proveedor (1.75x) | $2,571.43 MXN | |
| Ingreso por orden (ticket + fee envío ponderado) | $4,512.25 MXN | |
| Comisión pasarela por orden | $156.42 MXN | |
| ISR por orden (tasa de comparabilidad 1.5%) | $67.68 MXN | Ver nota de comparabilidad, sección 1.1 |
| **Contribución neta por venta** | **$1,716.72 MXN** | Coincide con el supuesto cerrado del negocio (~$1,700 MXN) |
| **LTV a 12 meses** (contribución × (1+15% recompra anual)) | **$1,974.23 MXN** | |
| LTV perpetuidad geométrica (referencia, si 15% se sostiene indefinidamente año con año) | $2,019.67 MXN | Solo referencia — no usar como cifra de decisión sin validar retención multi-año |
| **Unidades/mes de equilibrio** | **5.35** | Confirma el rango declarado 5–6 ventas/mes |
| Ingreso mensual de equilibrio | $24,131.51 MXN | |
| **ROAS requerido para equilibrio** (ingreso equilibrio / pauta $5,000) | **4.83x** | Por cada $1 de pauta, se necesitan $4.83 de ingreso para cubrir fijos |

**CAC implícito por escenario** (presupuesto de pauta del mes / ventas del mes; supuesto conservador: 100% de las ventas atribuidas a pauta paga, lo que sobreestima el CAC real dado que el negocio cuenta con ventaja de producción creativa/orgánica declarada):

| Escenario | CAC promedio 12m | CAC mes 1 | CAC mes 12 |
|---|---|---|---|
| Pesimista | $2,145.92 MXN | $5,000 MXN | $1,000 MXN |
| Base | $1,056.00 MXN | $2,500 MXN | $667 MXN |
| Optimista | $798.67 MXN | $1,667 MXN | $500 MXN |

**Lectura CAC vs. LTV:** en los tres escenarios el CAC promedio a 12 meses ($799–$2,146 MXN) es inferior a la contribución neta por venta ($1,717 MXN) excepto en el escenario pesimista, donde el CAC promedio ($2,146) **supera la contribución por venta** — es decir, en el escenario pesimista el negocio pierde dinero por cada venta atribuible a pauta paga si se contabiliza el 100% del gasto publicitario contra esas ventas. Esto refuerza que el escenario pesimista solo es sostenible si una parte relevante de las ventas proviene de canal orgánico/creativo sin costo de adquisición directo, tal como plantea la ventaja competitiva declarada del negocio.

## 6. Conclusiones para el comité

1. El punto de equilibrio de 5.35 ventas/mes calculado con el modelo completo (comisiones, ISR real por tramo, fee de envío) **valida** el supuesto cerrado de 5–6 ventas/mes; no hay necesidad de ajustar la tesis de inversión en este punto.
2. El escenario base recupera la ronda en el mes 10, dentro del horizonte de 12 meses y con margen de dos meses antes del cierre del año — razonable para una ronda pre-seed sin buffer explícito.
3. El escenario pesimista es el hallazgo más relevante de este análisis: **no quiebra técnicamente la caja, pero termina el año con menos de un mes de runway** y sin haber alcanzado nunca resultado mensual positivo. La señal de salida declarada (<3 ventas/mes sostenidas al mes 5) se ubica exactamente en el borde de este escenario, lo que la convierte en el indicador de monitoreo más importante del negocio en su primer semestre.
4. La sensibilidad muestra que el negocio es más vulnerable a una caída en la tasa de conversión del sitio que a un incremento de 30% en el CPC de Meta Ads — el presupuesto actual de pauta absorbe holgadamente un CPC más alto, pero no una conversión por debajo de ~0.5%.
5. Una caída del multiplicador de 1.75x a 1.50x —dentro del rango declarado como aceptable (1.5–2.0x)— eleva el punto de equilibrio en un tercio; el multiplicador de 1.75x no debe tratarse como piso de seguridad sino como el centro de un rango donde el extremo bajo (1.50x) ya exige una velocidad de ventas sensiblemente mayor.

## Apéndice — Código reproducible

Todos los cálculos de este documento se generan con los siguientes dos scripts de Python (sin edición manual de cifras).

### A. Modelo base — P&L, flujo de caja e hitos por escenario

```python
"""
Modelo financiero — Business Case E-commerce de Mobiliario (CDMX/EdoMéx + Nacional)
Analista financiero senior. Todos los cálculos se hacen aquí, sin aritmética manual en el documento final.
"""
import pandas as pd
pd.set_option("display.float_format", lambda x: f"{x:,.0f}")

# ----------------------------------------------------------------------------------
# 1. PARÁMETROS FIJOS DEL NEGOCIO (fuente: CLAUDE.md — datos cerrados)
# ----------------------------------------------------------------------------------
TICKET = 4500.0                 # precio promedio de venta al cliente
MULTIPLICADOR = 1.75            # precio = costo_proveedor * multiplicador
COSTO_PROVEEDOR = TICKET / MULTIPLICADOR   # costo Mundo In por unidad

COMISION_PORC = 0.034           # Shopify Payments variable
COMISION_FIJA = 3.0             # Shopify Payments fija por transacción

CAPITAL_INICIAL = 57000.0
FIJOS_BASE = 9300.0             # incluye 5,000 de pauta Meta Ads
PAUTA_BASE = 5000.0
PAUTA_ESCALADA = 8000.0         # desde mes 8, solo escenarios base y optimista
FIJOS_ESCALADOS = FIJOS_BASE - PAUTA_BASE + PAUTA_ESCALADA  # 12,300

TARIFA_LALAMOVE = 350.0         # tarifa promedio de entrega local
FEE_ENVIO_PORC = 0.05           # 5% cargado al cliente sobre tarifa Lalamove
PROP_LOCAL = 0.70               # % de órdenes con entrega local (CDMX/EdoMéx, mismo día)
INGRESO_ENVIO_POR_ORDEN = PROP_LOCAL * FEE_ENVIO_PORC * TARIFA_LALAMOVE  # ponderado, MXN/orden
# Nota metodológica: el fee de envío es un margen puro (5% sobre la tarifa de Lalamove que
# el negocio ya paga en su totalidad al proveedor logístico); no genera costo de ventas
# adicional. El envío foráneo (Skydropx) se cobra a tarifa API sin marcaje → margen $0,
# por lo que no se modela como línea de ingreso.

RECOMPRA_ANUAL = 0.15           # tasa de recompra anual (para LTV)

# ----------------------------------------------------------------------------------
# 2. TABLA REAL RESICO PERSONA FÍSICA — ISR mensual (Art. 113-E LISR, vigente 2025-2026,
#    sin cambios respecto a 2025). La tasa se determina por el INGRESO ACUMULADO desde
#    enero del ejercicio a la fecha, y esa tasa se multiplica por el ingreso DEL MES.
#    Fuente: SAT / LISR Art. 113-E, consultada vía múltiples calculadoras fiscales
#    especializadas (facturama.mx/blog/tablas-resico, resicocalc.com/blog/tablas-isr-resico-2026,
#    herramientasfiscales.mx/blog/calcular-isr-resico-2026) — consultado 08-jul-2026.
#    Supuesto declarado: se asume que el mes 1 del modelo coincide con enero del ejercicio
#    fiscal (arranque de operaciones alineado al año calendario), por lo que el acumulado
#    para efectos de tramo RESICO se reinicia en el mes 1 de cada escenario.
# ----------------------------------------------------------------------------------
TRAMOS_RESICO = [
    (0.01, 25_000.00, 0.010),
    (25_000.01, 50_000.00, 0.011),
    (50_000.01, 83_333.33, 0.015),
    (83_333.34, 208_333.33, 0.020),
    (208_333.34, 3_500_000.00, 0.025),
]

def tasa_resico(acumulado_hasta_este_mes):
    for lim_inf, lim_sup, tasa in TRAMOS_RESICO:
        if lim_inf <= acumulado_hasta_este_mes <= lim_sup:
            return tasa
    return TRAMOS_RESICO[-1][2]

# ----------------------------------------------------------------------------------
# 3. ESCENARIOS DE VENTAS (unidades/mes, 12 meses)
# ----------------------------------------------------------------------------------
PESIMISTA = [1, 1, 2, 2, 3, 3, 3, 4, 4, 4, 5, 5]
BASE = [2, 3, 4, 5, 6, 7, 8, 8, 9, 10, 11, 12]
OPTIMISTA = [round(v * 1.3) for v in BASE]

ESCENARIOS = {"Pesimista": PESIMISTA, "Base": BASE, "Optimista": OPTIMISTA}

# Solo Base y Optimista escalan la pauta (y por tanto los fijos) desde el mes 8 (índice 7)
ESCALA_FIJOS = {"Pesimista": False, "Base": True, "Optimista": True}


def correr_escenario(nombre, ventas_mensuales, escala_fijos, capital_inicial=CAPITAL_INICIAL):
    filas = []
    acumulado_ingreso = 0.0
    caja = capital_inicial
    resultado_acumulado = 0.0
    for mes, unidades in enumerate(ventas_mensuales, start=1):
        ingreso_producto = unidades * TICKET
        ingreso_envio = unidades * INGRESO_ENVIO_POR_ORDEN
        ingreso_total = ingreso_producto + ingreso_envio

        costo_ventas = unidades * COSTO_PROVEEDOR

        comision = ingreso_total * COMISION_PORC + unidades * COMISION_FIJA

        acumulado_ingreso += ingreso_total
        tasa_isr = tasa_resico(acumulado_ingreso)
        isr = ingreso_total * tasa_isr

        if escala_fijos and mes >= 8:
            fijos = FIJOS_ESCALADOS
        else:
            fijos = FIJOS_BASE

        resultado = ingreso_total - costo_ventas - comision - isr - fijos
        resultado_acumulado += resultado
        caja += resultado

        filas.append({
            "Escenario": nombre,
            "Mes": mes,
            "Unidades": unidades,
            "Ingreso producto": ingreso_producto,
            "Ingreso envío (fee 5%)": ingreso_envio,
            "Ingreso total": ingreso_total,
            "Costo de ventas": costo_ventas,
            "Margen bruto": ingreso_total - costo_ventas,
            "Comisión pasarela": comision,
            "Tasa ISR aplicada": tasa_isr,
            "ISR RESICO": isr,
            "Costos fijos": fijos,
            "Resultado del mes": resultado,
            "Resultado acumulado": resultado_acumulado,
            "Caja (contra $57,000)": caja,
        })
    df = pd.DataFrame(filas)

    # Hitos
    mes_quiebre = None
    quiebre_rows = df[df["Caja (contra $57,000)"] < 0]
    if not quiebre_rows.empty:
        mes_quiebre = int(quiebre_rows.iloc[0]["Mes"])

    mes_equilibrio = None
    equilibrio_rows = df[df["Resultado del mes"] >= 0]
    if not equilibrio_rows.empty:
        mes_equilibrio = int(equilibrio_rows.iloc[0]["Mes"])

    mes_recuperacion = None
    recuperacion_rows = df[df["Resultado acumulado"] >= 0]
    if not recuperacion_rows.empty:
        mes_recuperacion = int(recuperacion_rows.iloc[0]["Mes"])

    hitos = {
        "escenario": nombre,
        "mes_quiebre_caja": mes_quiebre,
        "mes_equilibrio_operativo": mes_equilibrio,
        "mes_recuperacion_ronda": mes_recuperacion,
        "caja_final_mes12": df["Caja (contra $57,000)"].iloc[-1],
        "resultado_acumulado_mes12": df["Resultado acumulado"].iloc[-1],
        "ingreso_total_12m": df["Ingreso total"].sum(),
    }
    return df, hitos


resultados = {}
hitos_all = []
for nombre, ventas in ESCENARIOS.items():
    df, hitos = correr_escenario(nombre, ventas, ESCALA_FIJOS[nombre])
    resultados[nombre] = df
    hitos_all.append(hitos)

hitos_df = pd.DataFrame(hitos_all)
print(hitos_df.to_string(index=False))
for nombre, df in resultados.items():
    print(f"\n--- {nombre} ---")
    print(df.to_string(index=False))
```

### B. Sensibilidad y KPIs

```python
"""
Parte 2 — Sensibilidad y KPIs financieros
Reutiliza los parámetros y funciones del modelo base (script A)
"""
import numpy as np
import pandas as pd

TICKET = 4500.0
COMISION_PORC = 0.034
COMISION_FIJA = 3.0
TARIFA_LALAMOVE = 350.0
FEE_ENVIO_PORC = 0.05
PROP_LOCAL = 0.70
INGRESO_ENVIO_POR_ORDEN = PROP_LOCAL * FEE_ENVIO_PORC * TARIFA_LALAMOVE  # 12.25
FIJOS_BASE = 9300.0
PAUTA_BASE = 5000.0
ISR_FLAT = 0.015  # tasa usada en el KPI de "contribución por venta" para comparabilidad con el
                   # supuesto declarado en CLAUDE.md (~$1,700/venta); el P&L mensual usa tabla real.

TRAMOS_RESICO = [
    (0.01, 25_000.00, 0.010),
    (25_000.01, 50_000.00, 0.011),
    (50_000.01, 83_333.33, 0.015),
    (83_333.34, 208_333.33, 0.020),
    (208_333.34, 3_500_000.00, 0.025),
]

def tasa_resico(acumulado):
    for lim_inf, lim_sup, tasa in TRAMOS_RESICO:
        if lim_inf <= acumulado <= lim_sup:
            return tasa
    return TRAMOS_RESICO[-1][2]

def resultado_mes(unidades, multiplicador, fijos, isr_tasa):
    costo_unit = TICKET / multiplicador
    ingreso = unidades * (TICKET + INGRESO_ENVIO_POR_ORDEN)
    costo_ventas = unidades * costo_unit
    comision = ingreso * COMISION_PORC + unidades * COMISION_FIJA
    isr = ingreso * isr_tasa
    return ingreso - costo_ventas - comision - isr - fijos

def unidades_equilibrio(multiplicador, fijos=FIJOS_BASE, isr_tasa=0.010):
    """Busca (por barrido fino) las unidades/mes necesarias para resultado = 0,
    usando la tasa RESICO del primer tramo (empresa en etapa temprana, ingreso mensual bajo)."""
    for u in np.arange(0.01, 30, 0.001):
        if resultado_mes(u, multiplicador, fijos, isr_tasa) >= 0:
            return u
    return None

# SENSIBILIDAD 1 — Multiplicador 1.75x vs 1.50x
u_175 = unidades_equilibrio(1.75)
u_150 = unidades_equilibrio(1.50)

# SENSIBILIDAD 2 — CPC +30% y conversión 0.3% vs 0.8%
# Supuesto declarado: sin benchmark público verificado de CPC de Meta Ads para el nicho
# de mobiliario premium en las zonas objetivo; se usa CPC de trabajo $4.00 MXN.
CPC_BASE = 4.00
CPC_ALTO = CPC_BASE * 1.30
CONVERSIONES = [0.008, 0.003]
PRESUPUESTOS = [5000, 8000]

filas_embudo = []
for presupuesto in PRESUPUESTOS:
    for cpc, etiqueta_cpc in [(CPC_BASE, "CPC base $4.00"), (CPC_ALTO, "CPC +30% ($5.20)")]:
        clics = presupuesto / cpc
        for conv in CONVERSIONES:
            ventas = clics * conv
            filas_embudo.append({
                "Presupuesto pauta": presupuesto, "Escenario CPC": etiqueta_cpc,
                "Clics estimados": round(clics, 0), "Tasa de conversión": f"{conv*100:.1f}%",
                "Ventas estimadas del mes (pauta)": round(ventas, 2),
            })
embudo_df = pd.DataFrame(filas_embudo)

filas_presupuesto_req = []
for cpc, etiqueta_cpc in [(CPC_BASE, "CPC base $4.00"), (CPC_ALTO, "CPC +30% ($5.20)")]:
    for conv in CONVERSIONES:
        clics_necesarios = u_175 / conv
        presupuesto_necesario = clics_necesarios * cpc
        filas_presupuesto_req.append({
            "Escenario CPC": etiqueta_cpc, "Tasa de conversión": f"{conv*100:.1f}%",
            "Unidades objetivo (equilibrio)": round(u_175, 2),
            "Clics necesarios": round(clics_necesarios, 0),
            "Presupuesto de pauta requerido (MXN/mes)": round(presupuesto_necesario, 0),
        })
presupuesto_req_df = pd.DataFrame(filas_presupuesto_req)

# KPIs
costo_unit_175 = TICKET / 1.75
ingreso_por_orden = TICKET + INGRESO_ENVIO_POR_ORDEN
comision_por_orden = ingreso_por_orden * COMISION_PORC + COMISION_FIJA
isr_por_orden = ingreso_por_orden * ISR_FLAT
contribucion_neta = ingreso_por_orden - costo_unit_175 - comision_por_orden - isr_por_orden

LTV_12m = contribucion_neta * (1 + 0.15)
LTV_perpetuo = contribucion_neta / (1 - 0.15)

BASE = [2, 3, 4, 5, 6, 7, 8, 8, 9, 10, 11, 12]
PESIMISTA = [1, 1, 2, 2, 3, 3, 3, 4, 4, 4, 5, 5]
OPTIMISTA = [round(v * 1.3) for v in BASE]
ESCENARIOS = {"Pesimista": PESIMISTA, "Base": BASE, "Optimista": OPTIMISTA}
ESCALA_FIJOS = {"Pesimista": False, "Base": True, "Optimista": True}

filas_cac = []
for nombre, ventas in ESCENARIOS.items():
    for mes, unidades in enumerate(ventas, start=1):
        pauta_mes = PAUTA_BASE if (not ESCALA_FIJOS[nombre] or mes < 8) else 8000.0
        cac = pauta_mes / unidades if unidades > 0 else np.nan
        filas_cac.append({"Escenario": nombre, "Mes": mes, "Unidades": unidades,
                            "Pauta del mes": pauta_mes, "CAC implícito": round(cac, 0)})
cac_df = pd.DataFrame(filas_cac)
cac_resumen = cac_df.groupby("Escenario").agg(
    CAC_promedio_12m=("CAC implícito", "mean"),
    CAC_mes1=("CAC implícito", "first"),
    CAC_mes12=("CAC implícito", "last"),
).reset_index()

ingreso_equilibrio = u_175 * ingreso_por_orden
roas_requerido = ingreso_equilibrio / PAUTA_BASE

print(f"Unidades equilibrio 1.75x: {u_175:.2f} | 1.50x: {u_150:.2f}")
print(embudo_df.to_string(index=False))
print(presupuesto_req_df.to_string(index=False))
print(f"Contribución neta por venta: {contribucion_neta:.2f}")
print(f"LTV 12m: {LTV_12m:.2f} | LTV perpetuo: {LTV_perpetuo:.2f}")
print(f"ROAS requerido equilibrio: {roas_requerido:.2f}x")
print(cac_resumen.to_string(index=False))
```
