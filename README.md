# Analítica de ventas de panadería con arquitectura Medallion

Proyecto de Big Data: pipeline **Bronce → Silver → Gold** sobre el dataset *Historical Bakery Sales* (Kaggle: `sdcloud/historical-bakery-sales-dataset`), con pronóstico de demanda, segmentación de clientes y reglas de asociación, más un tablero de seguimiento comercial.

**Integrantes:** [Jerson Hermosa Bernal, Antonio Alvarez Muñoz]  ·  **Curso / especialización:** [Analitica de Datos]  ·  **Fecha:** [20261118]

## Criterios de evaluación y dónde encontrarlos

| Criterio | Peso | Documento |
|---|---|---|
| Caso de uso | 8 % | [`docs/01_caso_de_uso.md`](docs/01_caso_de_uso.md) |
| Tema del beneficio | 8 % | [`docs/02_tema_beneficio.md`](docs/02_tema_beneficio.md) |
| Visualización | 8 % | [`docs/03_visualizacion.md`](docs/03_visualizacion.md) |
| Modelo | 8 % | [`docs/04_modelo.md`](docs/04_modelo.md) |
| Pipeline Medallion | resto | [`notebooks/entregable_final.ipynb`](notebooks/entregable_final.ipynb) |

## Resumen de resultados

- **Volumen analizado:** 2,69 M en ingresos, 6,58 mil pedidos y 43,03 mil unidades vendidas (2023-2024), con 500 clientes.
- **Pronóstico de demanda (GBT, top 10 productos):** MAE global de 3,45 unidades sobre un holdout de 61 días.
- **Segmentación RFM (K-Means):** k = 2 (silhouette 0,511). El segmento de alto valor reúne al 46,4 % de los clientes y al 61,4 % del monetario total.
- **Reglas de asociación (FP-Growth):** la mayor es Butter + Milk → Bread, con lift 2,83.

## Arquitectura Medallion

| Capa | Tabla | Contenido |
|---|---|---|
| Bronce | `bronze_bakery_sales` | CSV crudo sin transformaciones y análisis exploratorio (EDA) |
| Silver | `silver_bakery_sales` | Datos depurados (duplicados, nulos, valores inconsistentes) y variables derivadas |
| Gold | `gold_daily_sales`, `gold_product_daily_sales` | Series diarias globales y por producto |
| Gold | `gold_demand_forecast` | Pronóstico de 30 días para 10 productos (300 filas) |
| Gold | `gold_customer_segments` | Recencia, frecuencia, monetario y clúster por cliente |
| Gold | `gold_association_rules` | Top 50 reglas por lift |

Detalle de cada decisión: ver el notebook y [`docs/04_modelo.md`](docs/04_modelo.md).

## Estructura del repositorio

```
├── README.md
├── docs/                          # documentos de los criterios de evaluación
├── notebooks/
│   ├── entregable_final.ipynb     # pipeline Medallion (VS Code, pandas + scikit-learn)
│   └── databricks/                # notebook original con Spark
├── images/                        # capturas del tablero y del modelo
├── reports/                       # PDF del tablero
├── outputs/gold/                  # resultados de la capa Gold en CSV
└── data/                          # instrucciones de descarga del dataset
```

## Cómo ejecutarlo

1. Clona el repositorio y crea un entorno virtual: `python -m venv .venv`.
2. Instala las dependencias: `pip install -r requirements.txt`.
3. Descarga el dataset siguiendo [`data/README.md`](data/README.md) y colócalo en `data/`.
4. Abre `notebooks/entregable_final.ipynb` en VS Code y ejecuta las celdas en orden.

## Nota sobre las dos versiones del código

El tablero y las métricas del modelo se generaron en **Databricks** con Spark (`GBTRegressor`, `KMeans`, `FPGrowth`). El notebook principal del repositorio es una versión equivalente en **pandas y scikit-learn** para poder ejecutarla localmente. Las cifras de este repositorio corresponden a la ejecución en Databricks; la versión local puede dar resultados ligeramente distintos porque las implementaciones de los algoritmos no son idénticas.
