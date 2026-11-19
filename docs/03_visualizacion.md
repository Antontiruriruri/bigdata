# 3. Visualización

El tablero **"Tablero de Seguimiento de Ventas y Análisis Comercial"** se construyó en Databricks sobre las tablas de la capa Gold y Silver.

- PDF completo: [`reports/Tablero_de_Seguimiento_de_Ventas.pdf`](../reports/Tablero_de_Seguimiento_de_Ventas.pdf)
- Captura general: `images/dashboard_completo.png`

![Tablero completo](../images/dashboard_completo.png)

## Indicadores principales

| Indicador | Valor |
|---|---|
| Ingresos totales | 2,69 M |
| Pedidos totales | 6,58 mil |
| Unidades vendidas | 43,03 mil |

## Gráficos y la pregunta que responde cada uno

| Gráfico | Qué muestra | Para qué sirve |
|---|---|---|
| Evolución de ingresos diarios | Serie diaria de ingresos de 2023 a 2024 | Detectar picos, caídas y estacionalidad |
| Días con más pedidos | Fechas con mayor número de pedidos | Identificar días de alta demanda para reforzar la producción |
| Ingresos por producto | Ranking de ingresos por producto | Saber qué productos sostienen el negocio |
| Distribución de clientes por segmento de valor | Proporción entre alto valor y menor valor (riesgo de churn) | Dimensionar la base de clientes por segmento |
| Segmentación RFM de clientes | Dispersión de recencia contra monetario por clúster | Ver cómo se separan los dos grupos |
| Pronóstico de demanda por producto | Unidades predichas por día para enero de 2025 | Planear la producción de los próximos 30 días |
| Reglas de asociación de productos | Soporte, confianza y lift de cada regla | Diseñar combos y ventas cruzadas |

## Lectura rápida del tablero

- Los ingresos diarios son muy variables, con picos puntuales que superan los 10 mil.
- Chocolate Cake y Cookies lideran los ingresos por producto, seguidos de Black Forest Cake.
- Los dos segmentos de clientes tienen un tamaño parecido, con una leve mayoría de clientes de menor valor.
- En la dispersión RFM, el clúster de alto valor se concentra en recencias bajas (compras recientes) y montos altos.

## Decisiones de diseño

- Cada gráfico responde una sola pregunta de negocio.
- Se usaron indicadores grandes arriba para dar contexto antes del detalle.
- Los nombres de los segmentos ("Alto Valor", "Menor Valor (Riesgo de Churn)") traducen los clústeres numéricos a un lenguaje que entiende el área comercial.
