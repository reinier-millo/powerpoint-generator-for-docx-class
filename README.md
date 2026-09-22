# Generador de Presentaciones para clases docentes

Proyecto para generar el material didáctico de las clases de la asignatura (presentaciones de PowerPoint y evaluaciones) a partir de los documentos Word con el contenido de cada clase.

## Skill: `generar-clase`

Definido en [`.claude/skills/generar-clase/SKILL.md`](.claude/skills/generar-clase/SKILL.md). A partir del contenido de una clase (en un `.docx` o pegado directamente en la conversación), genera:

- La presentación de la clase en PowerPoint, siguiendo el estilo de `Plantilla.pptx` (máximo 20 diapositivas, con portada, objetivos, contenido, conclusiones y orientaciones para el estudio independiente).
- Dos bancos de preguntas en formato GIFT (importables en Moodle): una autoevaluación para el estudiante y una evaluación formal más exigente.

Los entregables se guardan en [`Clases/`](Clases/README.md) — ver ese README para el detalle de cómo nombrar los archivos de entrada y qué se genera para cada uno. Los materiales intermedios (contenido extraído, contenido organizado, imágenes) se guardan en `ClasesTmp/<nombre-clase>/`, lo que permite regenerar una clase sin rehacer el trabajo ya hecho si el proceso se interrumpe o si solo cambia una parte puntual.

## Cómo usarlo

1. Coloca el documento Word de la clase en `Clases/`.
2. Pídele a Claude que genere la clase, mencionando el archivo o el tema. El skill se activa automáticamente al detectar una solicitud de este tipo.
3. Los archivos `.pptx` y `.gift` aparecen en `Clases/` con el mismo nombre base que el `.docx` de origen.

### Ejemplos de prompt

- "Genera la presentación de la Clase 7 (Seguridad Vial) a partir de Clases/Clase-7-Seguridad-Vial.docx"
- "Para cada archivo docx dentro de la carpeta Clases quiero generar la presentación de PowerPoint de la clase"
