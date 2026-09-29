# Caso RGM — Planner Comercial

Caso de Excel para candidatos a Planner Comercial en consumo masivo. El candidato limpia 978 filas de venta semanal con errores sembrados, calcula elasticidades y evalúa promociones y alzas de precio de una cadena ficticia (13 SKUs, 3 formatos, 26 semanas de 2026).

## Archivos

| Archivo | Para quién |
|---|---|
| `participante/caso_completo.pdf` | Candidato, versión completa (3 horas, 7 preguntas) |
| `participante/caso_corto.pdf` | Candidato, versión corta (90 minutos, 5 tareas) |
| `participante/caso_rgm_datos.xlsx` | Candidato: la data sucia, igual para ambas versiones |
| `evaluador/guia_evaluador.pdf` | Evaluador: 14 errores sembrados, respuestas con cifras, rúbrica |
| `evaluador/datos_limpios.xlsx`, `evaluador/respuestas.json` | Evaluador: data sin errores y cifras de referencia |
| `generador/generar_datos.py` | Regenera los datos |

Cada PDF tiene su `.md` fuente al lado. Para enviar el caso basta con la carpeta `participante/`.

## Regenerar

```bash
python3 generador/generar_datos.py          # requiere openpyxl
pandoc participante/caso_completo.md -o participante/caso_completo.pdf \
  --pdf-engine=xelatex -V geometry:margin=2cm -V mainfont="Arial Unicode MS"
```

La semilla fija (`random.seed(2026)`) produce siempre el mismo archivo. Cambiarla crea otra versión con las mismas trampas; las cifras nuevas quedan en `evaluador/respuestas.json` y hay que actualizarlas a mano en la guía.

## Aviso

Este repo es público y contiene las respuestas. Un candidato que lo encuentre las verá.
