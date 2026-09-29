---
title: "Caso práctico — Planner Comercial"
subtitle: "Precios, promociones y mix en consumo masivo"
lang: es
---

# 1. Contexto

Supermercados del Istmo (cadena ficticia) opera 93 tiendas en Costa Rica en tres formatos:

| Formato | Perfil de cliente |
|---|---|
| Hiper | Compra grande, ticket alto |
| Súper | Compra de reposición en zona urbana |
| Bodega | Formato de descuento, cliente sensible al precio |

Durante el primer semestre de 2026 (5 de enero a 29 de junio, 26 semanas) el equipo comercial movió precios y corrió promociones en cuatro categorías: Gaseosas, Café, Aceites y Detergentes. Cada categoría tiene una marca líder, en algunos casos una marca retadora y la marca propia **Precio Justo**.

La Gerencia Comercial se reúne el próximo lunes para planear el segundo semestre y te pide una respuesta basada en datos: qué funcionó, qué no, y qué hacer.

# 2. Archivo

Recibes `caso_rgm_datos.xlsx` con cuatro hojas:

| Hoja | Contenido |
|---|---|
| `Ventas` | Venta semanal por formato y SKU, extraída del sistema tal cual |
| `Maestro_Productos` | Marca, tipo de marca, contenido y costo unitario por SKU |
| `Tiendas` | Número de tiendas por formato |
| `Diccionario` | Definición de cada campo |

Montos en colones (₡). La extracción no fue revisada por nadie antes de llegar a ti.

# 3. Reglas

- Tiempo: 3 horas.
- Herramienta: Excel o Google Sheets. Fórmulas, tablas dinámicas, Power Query y gráficos están permitidos.
- No se permite usar inteligencia artificial (ChatGPT, Copilot, Gemini u otras).
- Cuando una instrucción no alcance para decidir, toma un supuesto, escríbelo y sigue.

# 4. Tareas

## Parte 1. Limpieza

1. Crea una hoja `Ventas_Limpias` con la data lista para analizar. No borres la hoja `Ventas` original.
2. Crea una hoja `Bitácora` con una fila por cada problema que encontraste: qué era, cuántas filas afectó y qué hiciste con ellas.

## Parte 2. Clasificación

En `Ventas_Limpias` agrega, con fórmulas que se puedan auditar:

1. `Semana` (1 a 26), `Marca`, `Tipo_Marca` y `Costo_Unitario` desde el maestro.
2. `Margen_Bruto` en colones y en porcentaje.
3. `Descuento_%` contra el precio regular.
4. Precio por unidad de medida: ₡ por litro, ₡ por kilo o ₡ por 100 g.

## Parte 3. Análisis

Responde cada pregunta con cifras. Una respuesta sin número no cuenta.

1. ¿Cómo se reparten la venta y el margen entre categorías y formatos? ¿Qué formato rinde más?
2. Cola Tropical 2L tuvo un descuento de 25 % en dos semanas distintas. Estima la elasticidad precio de la demanda con cada una. ¿Dan el mismo resultado? Explica por qué.
3. Detergente Blanco Max estuvo 30 % más barato durante cuatro semanas. ¿Fue una buena promoción?
4. Aceite Girasol Dorado subió de precio en la semana 8 y Café Volcán 250g en la semana 10. ¿Qué pasó con cada uno? ¿Repetirías esas alzas?
5. ¿Cuál fue la acción comercial más exitosa del semestre? Define primero cómo mides el éxito.
6. Propón tres acciones para el segundo semestre. Para cada una estima el impacto en colones y di qué supuesto usaste.
7. ¿Qué información te hace falta para confiar en tus conclusiones? Nombra datos concretos y explica qué decisión cambiaría con cada uno.

# 5. Entregables

Un solo archivo de Excel con estas hojas:

| Hoja | Qué debe tener |
|---|---|
| `Ventas` | Original, sin cambios |
| `Ventas_Limpias` | Data limpia con las columnas de la Parte 2 |
| `Bitácora` | Problemas encontrados y decisiones tomadas |
| `Análisis` | Tablas dinámicas y cálculos que respaldan cada respuesta |
| `Resumen` | Máximo una página: respuestas a las 7 preguntas, cada una con su cifra |

Al final tendrás 10 minutos para presentar el `Resumen` a la Gerencia Comercial y 10 minutos de preguntas.

# 6. Cómo se evalúa

| Criterio | Peso |
|---|---|
| Limpieza y trazabilidad de la data | 25 % |
| Técnica en Excel (fórmulas, tablas dinámicas, orden) | 20 % |
| Calidad del análisis y cálculos | 30 % |
| Pensamiento crítico: supuestos, límites de la data, lo que el caso no dice | 15 % |
| Comunicación del resumen | 10 % |

# 7. Referencia rápida

**Revenue Growth Management (RGM).** Disciplina que busca crecer venta y margen con cinco palancas: precio, promociones, mix de productos, arquitectura de precio y empaque (qué tamaños se ofrecen y a qué precio por unidad de medida) y condiciones comerciales con proveedores.

**Elasticidad precio de la demanda.** Cuánto cambia la cantidad vendida cuando cambia el precio:

$$E = \frac{\%\ \text{cambio en unidades}}{\%\ \text{cambio en precio}}$$

Una elasticidad de −2 significa que al bajar el precio 10 % las unidades suben cerca de 20 %. Si |E| > 1 la demanda es elástica; si |E| < 1 es inelástica. Para calcular el cambio necesitas un periodo de comparación sin la acción: tú decides cuál y lo justificas.

**Margen bruto.** Venta − (Unidades × Costo_Unitario).
