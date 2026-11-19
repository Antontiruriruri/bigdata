# 4. Modelo

El proyecto incluye tres modelos en la capa Gold. Todos se entrenaron en Databricks con Spark ML sobre `silver_bakery_sales`.

## 4.1 Pronóstico de demanda (GBT)

**Objetivo.** Predecir las unidades diarias de cada uno de los 10 productos más vendidos: Nankhatai, Paav, Puff, Cookies, Milk, Biscuit, Bread, Tea Powder, Pastry y Butter.

**Datos.** Rejilla completa fecha por producto, donde los días sin venta registrada cuentan como 0 unidades.

**Variables.**

| Variable | Descripción |
|---|---|
| `units_lag1` | Unidades del día anterior |
| `units_lag7` | Unidades de hace 7 días |
| `units_roll7` | Promedio de los 7 días previos |
| `dow`, `month`, `is_weekend` | Calendario |

**Modelo.** `GBTRegressor` (`maxIter=60`, `maxDepth=5`, `seed=42`).

**Validación.** Holdout temporal: se entrena con los datos anteriores al 2024-10-31 y se evalúa en los últimos 61 días (hasta el 2024-12-31). Se usa un corte temporal y no aleatorio para evitar usar información del futuro.

**Resultados.** MAE global de **3,45 unidades**.

| Producto | MAE | Unidades promedio | MAE / promedio |
|---|---|---|---|
| Paav | 6,94 | 6,6 | 105 % |
| Nankhatai | 5,61 | 6,8 | 83 % |
| Cookies | 5,35 | 3,8 | 141 % |
| Puff | 4,08 | 4,2 | 97 % |
| Biscuit | 2,89 | 3,1 | 93 % |
| Milk | 2,31 | 3,0 | 77 % |
| Bread | 1,92 | 2,9 | 66 % |
| Pastry | 1,90 | 1,5 | 127 % |
| Butter | 1,76 | 1,9 | 93 % |
| Tea Powder | 1,71 | 1,9 | 90 % |

**Pronóstico.** Recursivo a 30 días (del 2025-01-01 en adelante): cada predicción alimenta los rezagos del día siguiente. Resultado: 300 filas (10 productos por 30 días) en `gold_demand_forecast`.

**Interpretación y limitaciones.**

- Las ventas diarias por producto son pequeñas (entre 1,5 y 6,8 unidades) y ruidosas, por lo que el error absoluto parece bajo, pero relativo al promedio es alto en todos los productos. El mejor caso es Bread (66 %) y el peor Cookies (141 %).
- El error crece en el pronóstico recursivo porque cada predicción arrastra el error de las anteriores.
- No se comparó contra un modelo base (por ejemplo, repetir el valor de hace 7 días). Sin esa comparación no es posible afirmar cuánto aporta el GBT.
- **Siguientes pasos:** agregar un modelo base, probar agregación semanal y añadir variables como clima, festivos y promociones.

## 4.2 Segmentación de clientes (RFM + K-Means)

**Variables.** Recencia (días desde la última compra hasta la fecha más reciente del dataset), frecuencia (pedidos distintos) y monetario (suma de ventas), escaladas a media 0 y desviación 1.

**Selección de k.** Se probó k de 2 a 8 y se eligió el de mayor silhouette.

| k | Silhouette |
|---|---|
| **2** | **0,511** |
| 3 | 0,498 |
| 4 | 0,450 |
| 5 | 0,475 |
| 6 | 0,435 |
| 7 | 0,408 |
| 8 | 0,409 |

**Resultado.** k = 2, con silhouette de 0,511, una separación razonable entre grupos.

| Clúster | Clientes | Recencia | Frecuencia | Monetario promedio | Monetario total | Nombre |
|---|---|---|---|---|---|---|
| 0 | 268 | 74,8 | 10,5 | 3.878,89 | 1.039.543,81 | Menor valor (riesgo de churn) |
| 1 | 232 | 36,8 | 16,2 | 7.117,51 | 1.651.262,24 | Alto valor |

**Limitación.** Con dos grupos la segmentación es general. Un k mayor daría más detalle, pero tiene menor silhouette.

## 4.3 Reglas de asociación (FP-Growth)

- **Canastas:** productos distintos por `Order_ID`.
- **Parámetros:** `minSupport = 0,01`, `minConfidence = 0,25`, se conservan las reglas con lift mayor a 1 y las 50 primeras por lift.
- **Resultado principal:** Butter + Milk → Bread, con soporte 0,02, confianza 0,47 y lift 2,83.

| Métrica | Significado |
|---|---|
| Soporte | Proporción de pedidos que contienen todos los productos de la regla |
| Confianza | Probabilidad de comprar el consecuente dado el antecedente |
| Lift | Cuánto más probable es la compra conjunta frente al azar (mayor a 1 indica asociación) |

**Limitación.** Los soportes son bajos (0,02 a 0,06): son asociaciones reales pero en pocos pedidos.

## 4.4 Reproducibilidad

El tablero y estas métricas provienen de la ejecución en Databricks (Spark). El notebook `notebooks/entregable_final.ipynb` es la versión local en pandas y scikit-learn, que puede dar valores ligeramente distintos. El original está en `notebooks/databricks/`.
