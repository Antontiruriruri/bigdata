# Datos

El dataset no se incluye en el repositorio por su tamaño.

1. Descárgalo desde Kaggle: https://www.kaggle.com/datasets/sdcloud/historical-bakery-sales-dataset
2. Guarda el archivo `bakery_sales_dataset.csv` en esta carpeta (`data/`).
3. Ejecuta el notebook `notebooks/entregable_final.ipynb`.

Alternativa con la librería `kagglehub`:

```python
import kagglehub
path = kagglehub.dataset_download("sdcloud/historical-bakery-sales-dataset")
print(path)
```
