---
title: "Guía del evaluador — Caso Planner Comercial"
subtitle: "Confidencial: no compartir con participantes"
lang: es
---

# 1. Qué mide el caso

La plaza de Planner Comercial (Walmart Centroamérica, Santa Ana, Costa Rica) pide Excel avanzado, construir indicadores para "detectar riesgos de manera proactiva" y dar seguimiento a iniciativas comerciales por categoría. El caso reproduce ese trabajo a escala: 13 SKUs, 3 formatos, 26 semanas y 978 filas.

El trabajo diario de un analista de Revenue Growth Management (RGM) en retail de consumo masivo gira sobre cinco palancas: precio, promoción, mix, arquitectura de precio y empaque, y condiciones comerciales. El caso siembra un hallazgo en cada una de las cuatro primeras y deja la quinta como hueco de información (no hay datos de fondos del proveedor).

Las instrucciones son cortas a propósito. Un participante promedio las sigue y entrega cifras correctas. Uno bueno encuentra que tres de las preguntas tienen una trampa que las instrucciones no mencionan.

| Pregunta | Habilidad que mide | Trampa sembrada |
|---|---|---|
| Parte 1 | Limpieza, trazabilidad | 10 duplicados solo aparecen tras estandarizar texto |
| P1 | Tablas dinámicas, normalización | Venta total por formato sin dividir entre tiendas |
| P2 | Elasticidad, lectura del calendario | Semana 13 es Semana Santa |
| P3 | Evaluación de promociones | Venta sube, margen cae a ₡0 por unidad |
| P4 | Evaluación de precios | Café pierde venta pero gana margen |
| P5 | Definir la métrica antes de medir | La pregunta no dice si éxito es unidades, venta o margen |
| P6 | Priorizar con impacto en ₡ | Hay un problema de arquitectura de empaque no preguntado |
| P7 | Pensamiento crítico | Faltan quiebres de stock, fondos del proveedor y año anterior |

# 2. Errores sembrados en la data

Los datos salen de `generador/generar_datos.py` con semilla fija; correrlo de nuevo produce el mismo archivo. La versión sin errores está en `evaluador/datos_limpios.xlsx`.

| # | Error | Filas | Tratamiento esperado |
|---|---|---|---|
| 1 | Duplicados exactos | 14 | Eliminar |
| 2 | Duplicados con descripción y formato en mayúsculas | 10 | Estandarizar texto primero, luego eliminar |
| 3 | Categoría escrita distinto (`Cafe`, `GASEOSAS `, `Aceite`) | 60 | Reemplazar por la categoría del maestro |
| 4 | Formato escrito distinto (`HIPER`, `Supermercado`, `bodega `) | 45 | Mapear a Hiper, Súper, Bodega |
| 5 | Descripción con espacios o mayúsculas | 40 | `ESPACIOS()` + tomar descripción del maestro |
| 6 | Fecha como texto (`30/03/2026`, `2026-03-30`) | 40 | Convertir a fecha |
| 7 | Precio de venta como texto (`₡1.450`, `1450,00`) | 25 | Convertir a número |
| 8 | Precio de venta vacío | 12 | Recalcular como Venta ÷ Unidades |
| 9 | Unidades con un cero de más | 3 | Detectar porque Unidades × Precio ≠ Venta; corregir a Venta ÷ Precio |
| 10 | Unidades negativas (devoluciones) | 6 | Separar o excluir del análisis de promociones y documentarlo |
| 11 | Descuento real sin bandera de promo (Cola Tropical 2L, semana 20, Bodega) | 1 | Marcar promo por precio, no por bandera |
| 12 | Bandera de promo "Descuento 10 %" sin descuento en el precio | 4 | Corregir la bandera |
| 13 | SKU `GAS-004` (Cola Tropical Zero 2L) sin registro en el maestro | 15 | Reportarlo; sin costo no se calcula su margen (₡4,18 millones de venta) |
| 14 | Faltan filas de Precio Justo Aceite en Súper, semanas 18 a 20 | 3 ausentes | Leerlo como quiebre de stock, no como caída de demanda |

Filas de control: 978 entregadas → 954 tras quitar duplicados → 948 sin devoluciones. Un participante que termina con 964 filas quitó solo los duplicados exactos (error 2 no detectado).

Las filas con unidades ×10 son: Precio Justo Café 250g, Hiper, 2 de marzo; Aceite Girasol Dorado, Bodega, 11 de mayo; Precio Justo Cola 2L, Bodega, 4 de mayo.

# 3. Respuestas de referencia

Cifras calculadas sobre la data limpia. Se aceptan diferencias de ±5 % por decisiones razonables de limpieza o de periodo base, siempre que estén escritas en la bitácora.

## P1. Venta y margen por categoría y formato

| Categoría | Venta (₡ M) | Margen (₡ M) | Margen % |
|---|---|---|---|
| Detergentes | 261,6 | 74,6 | 28,5 % |
| Gaseosas | 249,9 | 86,0 | 34,4 % |
| Café | 232,0 | 84,1 | 36,3 % |
| Aceites | 231,4 | 74,3 | 32,1 % |
| **Total** | **975,0** | **318,9** | **32,7 %** |

El margen de Gaseosas incluye GAS-004; sin su costo el participante verá un margen un poco mayor.

| Formato | Tiendas | Venta (₡ M) | Venta por tienda por semana | Participación de marca propia |
|---|---|---|---|---|
| Hiper | 8 | 202,9 | ₡975 mil | 11,9 % |
| Súper | 25 | 295,7 | ₡455 mil | 18,7 % |
| Bodega | 60 | 476,4 | ₡305 mil | 35,6 % |

**Qué separa a los participantes:** Bodega vende el 49 % del total y parece el mejor formato. Por tienda, Hiper vende 3,2 veces más que Bodega. La hoja `Tiendas` está en el archivo pero ninguna instrucción pide usarla. Quien normaliza por tienda sin que se lo pidan muestra el criterio que busca la plaza.

## P2. Elasticidad de Cola Tropical 2L

Precio: ₡1.450 → ₡1.090 (−24,8 %). Periodo base recomendado: semanas 16 a 19 (sin promo, sin efecto posterior a promo).

| Semana | Lift de unidades | Elasticidad simple (%Δu ÷ %Δp) | Elasticidad log-log |
|---|---|---|---|
| 20 | +93,6 % | −3,8 | −2,3 |
| 13 | +157,6 % | −6,4 | −3,3 |

La semana 13 (30 de marzo) es Semana Santa: todas las gaseosas venden 35 % más por temporada y la semana 12 ya muestra +10 %. Con el mismo precio, la semana 13 vendió 8.705 unidades y la 20 vendió 6.543. La elasticidad válida es la de la semana 20.

Por formato (semana 20, elasticidad simple): Hiper −3,1; Súper −3,9; Bodega −4,1. El cliente de Bodega es el más sensible al precio. Quien filtra promociones por la columna `Promo` pierde la fila de Bodega de la semana 20 (error 11) y no puede calcular este dato.

**Nivel esperado:**

- Básico: calcula una elasticidad, sin comparar las dos semanas.
- Bueno: nota que la semana 13 da un número mucho mayor y lo atribuye a Semana Santa.
- Sobresaliente: además, ve la caída de 15 % en las semanas 14 y 21 (el cliente se abasteció en la promo) y la caída de Precio Justo Cola y Cola Tropical 3L durante la promo (canibalización).

## P3. Promoción de Detergente Blanco Max

Precio: ₡2.500 → ₡1.750 (−30 %) en semanas 16 a 19. Costo: ₡1.750. **El margen unitario durante la promo es ₡0.**

| Concepto (4 semanas vs. base semanas 10 a 15) | Monto |
|---|---|
| Lift de unidades | +91,8 % |
| Venta incremental de Blanco Max | +₡6,07 M |
| Margen incremental de Blanco Max | −₡5,31 M |
| Margen perdido semanas 20 y 21 (caída post promo) | −₡0,42 M |
| Margen perdido en Espuma y Precio Justo (canibalización) | −₡1,56 M |
| **Margen neto de la categoría** | **−₡7,30 M** |

Elasticidad: −3,1 simple, −1,8 log-log.

**Qué separa a los participantes:** el promedio responde "sí, las unidades casi se duplicaron". El bueno calcula el margen y concluye que la promo destruyó ₡5,3 M. El sobresaliente suma canibalización y efecto posterior, y pregunta si el proveedor financió parte del descuento: sin ese dato no se puede cerrar el juicio, y es la pregunta correcta en RGM.

## P4. Alzas de precio

| | Aceite Girasol Dorado | Café Volcán 250g |
|---|---|---|
| Alza | ₡2.200 → ₡2.420 (+10,0 %), semana 8 | ₡2.800 → ₡3.030 (+8,2 %), semana 10 |
| Unidades por semana | −2,4 % | −9,2 % |
| Venta por semana | +7,4 % | −1,8 % |
| Margen por semana | +28,3 % (+₡330 mil) | +12,7 % (+₡169 mil) |
| Margen extra acumulado | ₡6,26 M en 19 semanas | ₡2,87 M en 17 semanas |
| Elasticidad | −0,24 (inelástica) | −1,13 (casi unitaria) |

Café: tras el alza, Café Volcán 500g sube 7,0 % en unidades y Precio Justo Café 8,7 %. El 250g quedó en ₡12,12 por gramo contra ₡10,80 del 500g, así que parte del cliente migró de tamaño y parte se fue a marca propia.

**Qué separa a los participantes:** quien mira solo venta dice que el alza de café fracasó (−1,8 %). En margen ganó ₡2,87 M. El sobresaliente nota que la venta perdida se quedó en la tienda (500g y marca propia) y lo usa para argumentar que el alza fue segura.

## P5. Acción más exitosa

Depende de la métrica, y el participante debe decir cuál usa antes de responder:

| Métrica | Ganador |
|---|---|
| Unidades | Promo de Detergente Blanco Max (+91,8 % en 4 semanas) |
| Venta | Promo de Detergente Blanco Max (+₡6,07 M) |
| Margen | Alza de Aceite Girasol Dorado (+₡6,26 M) |

La respuesta correcta para un negocio es margen: el alza de aceite. Las dos promociones perdieron margen (detergente −₡7,30 M, gaseosa semana 20 −₡1,55 M). Para que la promo de gaseosa cubriera su costo, las unidades debían subir 257 % (margen unitario de ₡500 a ₡140); subieron 94 %.

## P6. Acciones para el segundo semestre

Respuestas fuertes, con impacto de referencia:

1. **No repetir la promo de detergente a ₡1.750** sin fondos del proveedor. Evita perder ₡7,3 M por evento. Alternativa: 15 % de descuento o mecánica 2 × ₡4.500.
2. **Revisar precio de otros SKUs inelásticos**, empezando por el resto de Aceites. Aceite Girasol Dorado muestra elasticidad −0,24.
3. **Corregir la arquitectura de Cola Tropical.** El 3L cuesta ₡783 por litro y el 2L ₡725: el empaque grande sale más caro por litro. Bajar el 3L a ₡2.050 (₡683/L) alinea la escalera de precios. Nadie pregunta por esto; aparece si el participante calcula el precio por unidad de medida que pide la Parte 2.
4. **Promociones de gaseosa solo en Bodega**, donde la elasticidad es mayor (−4,1), o con un descuento menor.
5. **Evitar quiebres de marca propia.** Precio Justo Aceite en Súper no vendió 3 semanas: ₡1,47 M de venta y ₡0,41 M de margen perdidos (parte lo recuperó Palma Rica, que subió 20 % en Súper esas semanas).
6. **Dar de alta GAS-004 en el maestro** con su costo antes de medir su lanzamiento.

## P7. Información faltante

Datos que un buen participante pide, con la decisión que cambian:

| Dato faltante | Decisión que afecta |
|---|---|
| Fondos o rebates del proveedor por promo | Si la promo de detergente perdió margen o lo cubrió el proveedor |
| Inventario o quiebres por tienda | Distinguir caída de demanda de falta de producto (Precio Justo Aceite, Súper) |
| Venta del mismo periodo de 2025 | Separar estacionalidad de efecto precio (Semana Santa) |
| Precios de la competencia | Si el cliente que se fue compró en otra cadena |
| Costo de GAS-004 y fecha de lanzamiento | Margen del lanzamiento |
| Tráfico o tickets por tienda | Si la promo atrajo clientes nuevos (efecto que justificaría perder margen) |
| Espacio en góndola, exhibiciones, publicidad | Si el lift se debe al precio o a la exhibición |
| Cambios de costo en el semestre | El caso asume costo fijo; en aceite el costo suele moverse con el commodity |

Una lista genérica ("más datos", "datos de mercado") sin decisión asociada vale la mitad.

# 4. Rúbrica

Puntaje de 1 a 4 por criterio; ponderado sobre 100.

| Criterio (peso) | 1 | 2 | 3 | 4 |
|---|---|---|---|---|
| Limpieza y trazabilidad (25) | Menos de 6 errores detectados, sin bitácora | 6 a 9 errores, bitácora incompleta | 10 a 12 errores, bitácora con conteos; llega a 954 o 948 filas | 13 o 14 errores, incluido el quiebre de stock y los duplicados tras estandarizar |
| Técnica en Excel (20) | Valores pegados a mano | Fórmulas básicas, sin búsquedas | `BUSCARX` o `BUSCARV`, tablas dinámicas, fórmulas auditables | Además: Power Query o validaciones de control (conteo de filas, Venta = Unidades × Precio) |
| Análisis (30) | Cifras incorrectas o sin respaldo | P1 a P4 correctas en unidades y venta | Incluye margen en P3 y P4 y define la métrica en P5 | Detecta Semana Santa, canibalización, caída post promo y normaliza por tienda |
| Pensamiento crítico (15) | No declara supuestos | Supuestos genéricos | Menciona 3 o más datos faltantes con su decisión | Identifica la arquitectura de empaque del 3L o pregunta por fondos del proveedor sin que se lo pidan |
| Comunicación (10) | Resumen sin cifras | Cifras sin conclusión | Una conclusión por pregunta con su cifra | La Gerencia podría decidir leyendo solo el resumen |

Referencia: 80 o más puntos, avanza; 65 a 79, entrevista de desempate; menos de 65, no avanza.

# 5. Preguntas para los 10 minutos de discusión

1. ¿Qué periodo usaste como base para la elasticidad y por qué ese?
2. Si el proveedor de Blanco Max hubiera pagado ₡400 por unidad vendida en promo, ¿cambia tu recomendación? (Respuesta: con 3.396 unidades por semana en promo, Blanco Max gana ₡5,43 M en 4 semanas contra ₡5,31 M de base: +₡0,12 M. La categoría sigue en −₡1,87 M por canibalización y caída post promo. El punto de equilibrio es un aporte de unos ₡540 por unidad. Quien responde solo con Blanco Max y dice "ahora sí conviene" no integró la categoría.)
3. ¿Qué tablero de 4 indicadores le pondrías a la Gerencia para seguir estas categorías cada semana?
4. ¿Cómo automatizarías esta limpieza si la extracción llega cada lunes? (Buena respuesta: Power Query o una consulta SQL que aplique los mismos pasos de la bitácora.)

# 6. Archivos

| Archivo | Para quién |
|---|---|
| `caso_rgm_participante.pdf` + `datos/caso_rgm_datos.xlsx` | Participante |
| `guia_evaluador.pdf` | Evaluador |
| `evaluador/datos_limpios.xlsx` | Evaluador: data sin errores para comparar |
| `evaluador/respuestas.json` | Evaluador: todas las cifras de esta guía |
| `generador/generar_datos.py` | Regenerar data (`python3 generador/generar_datos.py`); cambiar la semilla crea una versión nueva del caso con las mismas trampas |
