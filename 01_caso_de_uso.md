# 1. Caso de uso

## Contexto del negocio

Una panadería con varias tiendas registra cada venta: producto, categoría, cantidad, precios, descuento, método de pago, cliente, hora, clima, temporada y promociones. Los datos cubren de enero de 2023 a diciembre de 2024 y provienen del dataset público *Historical Bakery Sales* (Kaggle).

En total hay **6,58 mil pedidos**, **43,03 mil unidades vendidas** y **2,69 M en ingresos**, repartidos entre 500 clientes.

## Problema

Los datos existen, pero la operación comercial no los aprovecha para decidir. Se identifican tres problemas:

1. **Producción sin pronóstico.** Sin una estimación de la demanda por producto, se produce con base en la intuición, con riesgo de faltantes o de merma.
2. **Clientes tratados por igual.** No se distingue a quienes compran seguido y gastan más de quienes llevan meses sin volver.
3. **Ventas cruzadas sin evidencia.** No se sabe qué productos se compran juntos, así que combos y promociones se arman sin respaldo.

## Preguntas analíticas

| Pregunta | Técnica | Resultado |
|---|---|---|
| ¿Cuántas unidades se venderán por producto en los próximos 30 días? | Regresión GBT con variables de rezago | `gold_demand_forecast` |
| ¿Qué tipos de cliente existen según su comportamiento de compra? | RFM + K-Means | `gold_customer_segments` |
| ¿Qué productos se compran juntos? | Reglas de asociación (FP-Growth) | `gold_association_rules` |
| ¿Cómo está el desempeño comercial? | Tablero en Databricks | Ver `03_visualizacion.md` |

## Alcance

- **Incluye:** ingesta, limpieza y modelado sobre arquitectura Medallion; pronóstico a 30 días para los 10 productos más vendidos en unidades; segmentación de toda la base de clientes; reglas de asociación por pedido.
- **No incluye:** integración con un sistema de producción ni optimización de inventario. El pronóstico es una entrada para esa decisión, no la decisión misma.

## Por qué la arquitectura Medallion

Separar bronce, silver y gold deja trazabilidad: el dato crudo se conserva intacto, la limpieza queda documentada en una sola capa y los modelos consumen tablas ya curadas. Si cambia una regla de limpieza, solo se regenera lo que depende de ella.
