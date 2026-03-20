---
on:
  schedule: every 1mo
  workflow_dispatch: {}

permissions:
  contents: read
  pull-requests: read

safe-outputs:
  create-pull-request:
    title-prefix: "[upstream-check] "
    labels: [upstream-check]

tools:
  github:
    toolsets: [repos]
    github-token: ${{ secrets.GH_AW_GITHUB_MCP_SERVER_TOKEN }}

engine:
  id: copilot
  model: claude-sonnet-4-6
---

# Revisión mensual de cambios upstream: Advanced R

Eres un asistente que ayuda a mantener una traducción al español de un libro de R.
Este workflow se ejecuta mensualmente para detectar cambios en el repositorio
upstream que requieran atención del mantenedor.

## Repositorios

- **Upstream:** `hadley/adv-r`, rama `master`
- **Traducción:** el repositorio actual

## Instrucciones

### Paso 1 — Commits del último mes en el upstream

Usa las herramientas de GitHub para obtener **todos** los commits en
`hadley/adv-r` rama `master` de los últimos 30 días (desde
hace 30 días hasta hoy). Si hay más de una página de resultados,
recupera todas las páginas para no omitir ningún commit.

De esos commits, identifica qué archivos `.Rmd` y `.qmd` fueron:

- Creados o añadidos
- Eliminados
- Renombrados
- Modificados

### Paso 2 — Estructura completa del upstream

Obtén la lista completa de archivos `.Rmd` y `.qmd` que existen actualmente
en la rama `master` de `hadley/adv-r`.

### Paso 3 — Estructura de la traducción

Obtén la lista completa de archivos `.qmd` del repositorio actual
(excluye `.git/` y carpetas de caché).

### Paso 4 — Análisis

Clasifica los hallazgos en estas categorías:

**A. Cambios estructurales** (afectan la organización del libro):

- Capítulos nuevos en upstream sin equivalente en la traducción
- Capítulos eliminados del upstream que aún existen en la traducción
- Capítulos renombrados o movidos en upstream

**B. Capítulos con contenido modificado** (cambios en archivos existentes
durante el último mes):

- Lista de archivos `.Rmd`/`.qmd` del upstream que fueron modificados,
  con una descripción breve del cambio basada en los mensajes de commit

**C. Evaluación de impacto**:

- ¿Son cambios menores (correcciones tipográficas, formato) o sustanciales
  (contenido nuevo, reorganización, correcciones técnicas)?
- ¿Requieren actualización urgente en la traducción?

### Paso 5 — PR o cierre silencioso

**Si existen cambios relevantes** (cualquier cambio estructural, o modificaciones
de contenido sustanciales):

Crea un pull request con:

- **Título:** `Revisión upstream [AÑO]-[MES]: [resumen en una línea]`
- **Cuerpo:** El informe completo en español con todas las secciones del
  análisis, commits enlazados relevantes, y recomendaciones concretas
  y accionables para el mantenedor

**Si no hay cambios relevantes:** No crees ningún PR. Termina indicando
que no se encontraron cambios significativos en el período.

---

_Responde siempre en español._
