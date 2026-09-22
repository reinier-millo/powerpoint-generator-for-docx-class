---
name: generar-clase
description: Genera presentaciones PowerPoint para clases universitarias a partir de contenido provisto en el contexto o en un documento Word, siguiendo la plantilla institucional, y produce además un documento de autoevaluación y evaluación parcial para los estudiantes. Usa este skill siempre que el usuario pida crear, armar o preparar una clase, presentación de clase, diapositivas de clase o material de clase en PowerPoint, aunque no mencione explícitamente "PowerPoint" o "pptx".
---

# Generar clase

Este skill produce dos entregables a partir del contenido de una clase: la presentación en PowerPoint y un documento de autoevaluación para los estudiantes. Usa el skill `pptx` para construir el archivo `.pptx` — no generes el PowerPoint manualmente.

## Materiales de trabajo reutilizables

Guarda los materiales intermedios que generes a lo largo del proceso (contenido extraído, contenido organizado, imágenes) en `ClasesTmp/<nombre-archivo-clase>/`, usando el mismo nombre base que el archivo de entrada. Esto evita rehacer trabajo si la clase necesita regenerarse más adelante — por ejemplo, si solo cambia el diseño no hace falta volver a extraer o reorganizar el contenido, y si solo cambia un tema puntual no hace falta rebuscar o regenerar todas las imágenes.

Antes de ejecutar cualquiera de los pasos siguientes, verifica si ya existe el material correspondiente en `ClasesTmp/<nombre-archivo-clase>/`. Haz esta verificación de la forma más barata posible:

- Comprueba primero solo la existencia y fecha de modificación de los archivos (por ejemplo con `ls -la` o `stat`), no su contenido. Compara la fecha del `.docx` de origen (o la fecha en que se te dio el contenido) contra la fecha de `contenido-extraido.md` / `contenido-preparado.md`. Si el material intermedio es más reciente que la fuente, el paso está al día y puedes saltarlo por completo — no necesitas abrir ni releer esos archivos para confirmarlo, solo pasar su ruta al siguiente paso.
- Carga el contenido de un archivo de `ClasesTmp/` únicamente cuando vayas a usarlo de verdad para generar el siguiente artefacto (por ejemplo, leer `contenido-preparado.md` recién cuando construyas la presentación), no como parte de la verificación en sí.
- Si el usuario no indicó que algo cambió, asume que los materiales existentes siguen siendo válidos y avanza directo al paso que realmente falta (normalmente el punto 4, construir el `.pptx`) en vez de repasar los pasos 1 a 3 "por las dudas".
- Solo reprocesa un paso completo (releer el Word, reorganizar el contenido, rebuscar imágenes) cuando el usuario indique explícitamente que la fuente cambió, o cuando la verificación de fechas muestre que el `.docx` de origen es más nuevo que el material guardado.

## 1. Leer el contenido de la clase

El contenido puede venir directamente en el contexto de la conversación, o en un documento Word.

- Si viene en un documento `.docx`, léelo con `pandoc -t markdown <nombre-archivo-clase.docx>` para obtener el texto en un formato que puedas procesar. Guarda el resultado en `ClasesTmp/<nombre-archivo-clase>/contenido-extraido.md`.
- El nombre base del archivo de entrada (sin extensión) determina el nombre de los archivos de salida (ver punto 4 y 5) y de la carpeta de materiales en `ClasesTmp/`. Si el contenido viene directo en el contexto sin archivo asociado, pregunta al usuario qué nombre de archivo usar antes de continuar.

## 2. Preparar el contenido

- Usa únicamente el contenido indicado — no agregues temas que no estén en el material original.
- La clase debe servirle al profesor como guía para seguir una secuencia clara durante la sesión.
- Organiza el contenido en un orden lógico y simple, pensado para que estudiantes de primer año puedan asimilarlo sin dificultad.
- Resalta los aspectos fundamentales que el estudiante debe adquirir, de forma que el material también sirva de apoyo para el estudio independiente.
- Guarda el contenido ya organizado (secuencia, objetivos, puntos clave por tema) en `ClasesTmp/<nombre-archivo-clase>/contenido-preparado.md`, para poder reutilizarlo si luego solo hace falta rehacer el diseño de las diapositivas.

## 3. Diseño y estilo de la presentación

- Sigue el estilo, layouts y colores definidos en la plantilla `Plantilla.pptx`, ubicada en el directorio raíz del proyecto. Reutiliza sus layouts en vez de crear diapositivas desde cero, para que el resultado sea visualmente consistente con el resto del material institucional.
- Cada clase tiene un máximo de 20 diapositivas. Este límite obliga a sintetizar: prioriza claridad y capacidad de síntesis sobre cobertura exhaustiva, pero el contenido completo indicado en el punto 1 debe quedar representado.
- Las clases duran aproximadamente 1 hora y media — ten esa duración en mente al decidir cuánto detalle entra en cada diapositiva.
- Agrega gráficos y figuras cuando ayuden a interpretar el contenido (diagramas, esquemas, gráficos de datos). Pueden generarse directamente o buscarse en internet; si una imagen proviene de internet, incluye la fuente (nombre y enlace) como referencia visible en la diapositiva o en las notas del orador.
- Guarda cada imagen usada (generada o descargada) en `ClasesTmp/<nombre-archivo-clase>/imagenes/`, junto con su fuente cuando aplique, para poder reutilizarlas si la clase se regenera.
- Configura el idioma de corrección ortográfica del texto en español (es-ES) al construir la presentación con el skill `pptx`, para que PowerPoint no marque el contenido como errores ortográficos al abrirlo.

## 4. Generar la presentación

- Construye el archivo usando el skill `pptx`, aplicando la plantilla y el contenido preparado en los puntos anteriores.
- Estructura fija de la presentación:
  1. **Portada**: asignatura, número de clase, tema y contenido general.
  2. **Objetivos de la clase.**
  3. **Diapositivas de contenido**, siguiendo el orden lógico definido en el punto 2.
  4. **Penúltima diapositiva**: resumen de las conclusiones.
  5. **Última diapositiva**: orientaciones para el estudio independiente (diagnóstico del territorio).
- Guarda el resultado en `Clases/<nombre-archivo-clase>.pptx`, usando el mismo nombre base que el archivo de entrada (o el nombre acordado con el usuario si el contenido vino directo en el contexto). Crea la carpeta `Clases/` si no existe.

## 5. Generar las evaluaciones en formato GIFT

Genera dos archivos en formato [GIFT](https://docs.moodle.org/en/GIFT_format) (el formato de importación de bancos de preguntas de Moodle), para que puedan subirse directamente sin retrabajo manual:

- `Clases/<nombre-archivo-clase>-autoevaluacion.gift`: ~25 preguntas para que el estudiante mida su propio nivel de aprendizaje frente a los objetivos de la clase. Usa una mezcla de preguntas de selección múltiple, cálculos, razonamiento lógico y de interpretación — variedad que empuje al autoestudio real en vez de respuestas mecánicas. El nivel debe corresponder a estudiantes de primer año de Ingeniería en Transporte.
- `Clases/<nombre-archivo-clase>-evaluacion.gift`: ~25 preguntas sobre el mismo tema pero con mayor rigor y exigencia, pensadas como evaluación formal del aprendizaje más que como autoevaluación.

Cada pregunta, en ambos archivos, debe alinearse con al menos uno de los objetivos de la clase definidos en el punto 4, de modo que la evaluación funcione como verificación real de que esos objetivos se cumplieron.

Respeta la sintaxis GIFT para que Moodle pueda importar el archivo sin errores:

```
// Comentario: se ignora al importar, útil para anotar de qué objetivo viene la pregunta
::Título breve de la pregunta::¿Enunciado de la pregunta de selección múltiple? {
=Respuesta correcta
~Respuesta incorrecta 1
~Respuesta incorrecta 2
~Respuesta incorrecta 3
}

::Pregunta de cálculo::¿Cuál es el resultado de la operación X? {#25:0.5}

::Pregunta de razonamiento::Explica brevemente por qué ocurre X. {}
```

- `=` marca la respuesta correcta en preguntas de selección múltiple, `~` las incorrectas.
- Las preguntas numéricas (cálculos) usan `{#respuesta:tolerancia}`.
- Las preguntas abiertas de razonamiento/interpretación (sin corrección automática exacta) pueden dejarse como `{}` para revisión manual en Moodle, o convertirse en selección múltiple cuando sea posible evaluarlas así.
- Cada pregunta debe tener un título único después de `::`, para poder identificarla dentro del banco de preguntas de Moodle.

## 6. Verificación final

Antes de dar por terminada la tarea, revisa:

- Que la presentación no supere las 20 diapositivas y respete la estructura del punto 4.
- Que todo el contenido indicado en el punto 1 esté representado en alguna diapositiva.
- Que el estilo visual coincida con `Plantilla.pptx`.
- Que las imágenes tomadas de internet tengan su fuente citada.
- Que el idioma de corrección de la presentación esté configurado en español.
- Que ambos archivos `.gift` tengan aproximadamente 25 preguntas cada uno, cubran los objetivos de la clase, y respeten la sintaxis GIFT (títulos únicos con `::`, `=`/`~` en selección múltiple, `{#valor:tolerancia}` en numéricas).
