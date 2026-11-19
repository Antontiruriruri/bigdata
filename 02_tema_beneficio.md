# 2. Tema del beneficio

Cada análisis alimenta una decisión concreta. Esta tabla resume el beneficio esperado y cómo medirlo.

| Análisis | Decisión que habilita | Beneficio esperado | Indicador para medirlo |
|---|---|---|---|
| Pronóstico de demanda | Cuánto producir de cada producto | Menos merma y menos faltantes | % de merma, ventas perdidas por agotado |
| Segmentación RFM | A quién dirigir campañas de retención | Recuperar clientes en riesgo de abandono | Tasa de recompra del segmento |
| Reglas de asociación | Qué combos y ubicaciones usar | Mayor ticket promedio | Unidades por pedido, ticket promedio |
| Tablero | Seguimiento de ventas | Decisiones más rápidas | Tiempo para detectar caídas de venta |

No se estiman ahorros en dinero: los datos disponibles no incluyen costos de merma ni resultados de campañas. Por eso se proponen indicadores a medir antes y después de aplicar las acciones.

## Evidencia en los datos

**Segmentación.** El K-Means separó a los clientes en dos grupos:

| Segmento | Clientes | Recencia promedio (días) | Frecuencia promedio (pedidos) | Monetario promedio | % del monetario total |
|---|---|---|---|---|---|
| Menor valor (riesgo de churn) | 268 (53,6 %) | 74,8 | 10,5 | 3.878,89 | 38,6 % |
| Alto valor | 232 (46,4 %) | 36,8 | 16,2 | 7.117,51 | 61,4 % |

El segmento de menor valor es la mayoría de los clientes y tarda el doble en volver a comprar. Es el objetivo natural de una campaña de reactivación. El segmento de alto valor, con menos de la mitad de los clientes, genera casi dos tercios del monetario, así que conviene protegerlo con beneficios de fidelización.

**Ventas cruzadas.** Las reglas con mayor lift indican productos que se compran juntos más de lo esperado por azar:

| Regla | Confianza | Lift |
|---|---|---|
| Butter + Milk → Bread | 0,47 | 2,83 |
| Butter + Bread → Milk | 0,46 | 2,52 |
| Bread + Milk → Butter | 0,28 | 2,47 |
| Rusk → Tea Powder | 0,38 | 2,39 |
| Nankhatai → Tea Powder | 0,31 | 1,90 |

Acciones posibles: un combo desayuno (pan, mantequilla y leche) y exhibir Tea Powder junto a Rusk y Nankhatai.

**Demanda.** El pronóstico entrega unidades por producto y día durante 30 días, que sirven como referencia para planear la producción semanal. Su precisión es limitada (ver `04_modelo.md`), por lo que se recomienda usarlo como guía y no como cifra exacta.

## Limitaciones del beneficio

- Las reglas muestran asociación, no causalidad: un combo debe probarse antes de generalizarlo.
- El pronóstico cubre los 10 productos más vendidos en unidades. Los de mayor ingreso (por ejemplo Chocolate Cake) no están incluidos.
