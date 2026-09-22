# Carpeta Clases

Esta carpeta es la entrada y salida del proceso de generación de material de clase.

## Cómo agregar una clase nueva

Coloca aquí el documento Word con el contenido de la clase, en formato `.docx`. No hace falta ninguna otra preparación: el skill `generar-clase` (ver [README.md de la raíz del proyecto](../README.md)) toma ese archivo como fuente.

## Qué se genera

Por cada `<nombre-clase>.docx` puesto en esta carpeta, el proceso genera tres archivos con el mismo nombre base:

- `<nombre-clase>.pptx` — la presentación de PowerPoint de la clase, siguiendo la plantilla institucional (`Plantilla.pptx` en la raíz del proyecto).
- `<nombre-clase>-autoevaluacion.gift` — banco de ~25 preguntas (formato GIFT, importable en Moodle) para que el estudiante se autoevalúe.
- `<nombre-clase>-evaluacion.gift` — banco de ~25 preguntas (formato GIFT) para la evaluación formal, con mayor rigor.

Por ejemplo, `Clase-1-Introduccion-SIG-Transporte.docx` produce `Clase-1-Introduccion-SIG-Transporte.pptx`, `Clase-1-Introduccion-SIG-Transporte-autoevaluacion.gift` y `Clase-1-Introduccion-SIG-Transporte-evaluacion.gift`.

Los materiales intermedios de cada clase (contenido extraído, contenido organizado, imágenes, scripts de construcción) se guardan aparte, en `../ClasesTmp/<nombre-clase>/`, para poder regenerar una clase sin rehacer todo el trabajo desde cero.
