# Caso RGM — Planner Comercial

Caso de Excel para candidatos a Planner Comercial en consumo masivo. El candidato limpia 978 filas de venta semanal con errores sembrados, calcula elasticidades y evalúa promociones y alzas de precio de una cadena ficticia (13 SKUs, 3 formatos, 26 semanas de 2026). Luego responde 18 preguntas teóricas de SQL, BigQuery, data warehouse, bases de datos y Excel.

Los casos traen solo contexto y preguntas, sin pistas: la data, los supuestos y lo que falta los descubre el candidato. Lo que se espera de cada respuesta está en la guía del evaluador.

## Archivos

| Archivo | Para quién |
|---|---|
| `participante/caso_completo.pdf` | Candidato, versión completa: contexto, 7 preguntas (3 horas) y preguntas teóricas (40 min) |
| `participante/caso_corto.pdf` | Candidato, versión corta: contexto, 4 preguntas (90 min) y preguntas teóricas (40 min) |
| `participante/preguntas_teoricas.md` | Candidato: 18 preguntas de SQL, BigQuery, data warehouse, bases de datos y Excel; van al final de ambos PDFs |
| `participante/caso_rgm_datos.xlsx` | Candidato: la data sucia, igual para ambas versiones |
| `evaluador/guia_evaluador.pdf` | Evaluador: 14 errores sembrados, respuestas del caso y de las 18 preguntas teóricas, rúbrica |
| `evaluador/datos_limpios.xlsx`, `evaluador/respuestas.json` | Evaluador: data sin errores y cifras de referencia |
| `generador/generar_datos.py` | Regenera los datos |

Cada PDF tiene su `.md` fuente al lado. Para enviar el caso basta con la carpeta `participante/`.

## Regenerar

```bash
python3 generador/generar_datos.py          # requiere openpyxl
for c in caso_completo caso_corto; do
  pandoc participante/$c.md participante/preguntas_teoricas.md -o participante/$c.pdf \
    --pdf-engine=xelatex -V geometry:margin=2cm -V mainfont="Arial Unicode MS"
done
```

La semilla fija (`random.seed(2026)`) produce siempre el mismo archivo. Cambiarla crea otra versión con las mismas trampas; las cifras nuevas quedan en `evaluador/respuestas.json` y hay que actualizarlas a mano en la guía.

## Aviso

Este repo es público y contiene las respuestas. Un candidato que lo encuentre las verá.
