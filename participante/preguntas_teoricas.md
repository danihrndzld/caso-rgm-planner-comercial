
# Preguntas teóricas

40 minutos, sin consultar. Responde aparte del Excel. Tablas en BigQuery:

| Tabla | Columnas |
|---|---|
| `istmo.comercial.ventas` | `fecha` (DATE), `formato`, `sku`, `precio_venta`, `unidades`, `venta` |
| `istmo.comercial.productos` | `sku`, `descripcion`, `categoria`, `marca`, `tipo_marca`, `costo_unitario` |
| `istmo.comercial.tiendas` | `formato`, `numero_tiendas` |

## A. SQL

1. Escribe una consulta que devuelva la venta total y el margen bruto por categoría, ordenada de mayor a menor venta.
2. Explica la diferencia entre `WHERE` y `HAVING`. Modifica tu consulta anterior para mostrar solo las categorías con venta mayor a ₡250 millones.
3. El SKU `GAS-004` está en `ventas` pero no en `productos`. ¿Qué le pasa a ese SKU en tu consulta de la pregunta 1 si usas `JOIN` y qué le pasa si usas `LEFT JOIN`? Escribe una consulta que liste los SKUs vendidos que no existen en `productos`.
4. Escribe una consulta que encuentre filas repetidas por la combinación `fecha`, `formato`, `sku`. ¿Cómo conservarías una sola fila de cada grupo?
5. Escribe una consulta que muestre, para cada SKU y semana, las unidades vendidas y la variación porcentual contra la semana anterior.

## B. BigQuery

6. En BigQuery con precio bajo demanda, ¿qué determina el costo de una consulta? ¿Por qué `SELECT *` es caro y por qué agregar `LIMIT 10` no lo abarata?
7. La tabla `ventas` crecerá a 5 años de venta diaria por tienda. ¿Por qué columna la particionarías y por cuáles la agruparías (clustering)? Justifica con una consulta típica del área comercial.
8. ¿En qué se diferencia BigQuery de una base de datos como MySQL o SQL Server? Da un uso para el que elegirías cada una.

## C. Data warehouse y modelado

9. La venta se registra en la caja de cada tienda y se analiza en un data warehouse. Explica la diferencia entre un sistema OLTP y uno OLAP, y por qué no conviene analizar directamente sobre el sistema de caja.
10. En un esquema estrella, ¿qué tabla del caso sería la tabla de hechos y cuáles las dimensiones? ¿Qué dimensión falta que te habría ayudado a responder el caso?
11. La tabla `ventas` tiene granularidad semana × formato × SKU. Nombra dos preguntas de negocio que no se pueden responder con esa granularidad. Explica también la diferencia entre ETL y ELT.

## D. Bases de datos

12. ¿Cuál sería la llave primaria de `ventas`? ¿Qué llave foránea definirías? Nombra dos errores del archivo del caso que estas restricciones habrían evitado.
13. En el archivo del caso, la columna `Categoria` viene repetida en cada fila de ventas. ¿Qué problema de diseño es ese y cómo se corrige?

## E. Excel

14. Nombra dos limitaciones de `BUSCARV` y cómo las resuelve `BUSCARX` o `INDICE` + `COINCIDIR`.
15. Usaste "Quitar duplicados" y quedaron duplicados sin detectar. ¿Por qué pasa? ¿Qué funciones usarías antes de volver a intentarlo?
16. Escribe la fórmula que devuelve la venta de Gaseosas en Bodega en la semana 20, usando la tabla `Ventas_Limpias`.
17. En una tabla dinámica, un analista calcula el margen % de cada fila y luego pide el promedio de esa columna para toda la categoría. ¿Por qué el resultado está mal y cómo se calcula bien?
18. La extracción de ventas llegará todos los lunes con los mismos errores. ¿Qué harías para no repetir la limpieza a mano cada semana?
