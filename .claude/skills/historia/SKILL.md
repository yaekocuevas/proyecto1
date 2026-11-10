---
name: historia
description: Para la PO (Yaeko). Con una idea, la convierte en una historia de usuario clara y la publica en el Backlog del tablero. Con "#N", ayuda a responder las preguntas que dejó el equipo en ese ticket y mejora la historia.
disable-model-invocation: true
argument-hint: '<idea> | #<número del issue>'
allowed-tools: Read, Glob, Grep, AskUserQuestion, Write, Bash(gh issue:*), Bash(gh project:*)
---

# /historia — Escribir o mejorar una historia

Hablas con la PO. No uses jerga técnica (nada de "endpoint", "componente", "base de datos"). Si una palabra técnica es inevitable, explícala en una frase.

## Paso 0: configuración

Lee de `CLAUDE.md` la sección `## Tablero` (repo, dueño y número del Project) y la `## Definition of Ready`. Lee también `docs/producto.md`, si tiene contenido, para entender el producto, el MVP y las historias ya planeadas.

Según el argumento:
- Empieza con `#` seguido de un número → **Modo B: responder preguntas**.
- Cualquier otro texto → **Modo A: historia nueva**.
- Vacío → pregunta qué quiere pedir. Si `docs/producto.md` tiene historias planeadas sin publicar, ofrécelas como opciones.

---

## Modo A: historia nueva

### A1. Revisar duplicados

Busca historias parecidas: `gh issue list -R <repo> --state all --search "<palabras clave>" --json number,title,state`. Si hay una muy parecida, muéstrala y pregunta si es la misma, si es una mejora de esa, o si es distinta.

### A2. Entrevista

Haz pocas preguntas a la vez (máximo 3 por turno), con `AskUserQuestion` cuando haya opciones claras. Lo que necesitas saber:

1. **Quién:** ¿para qué tipo de persona es? (visitante, cliente, la artista misma…).
2. **Qué:** ¿qué quiere poder hacer o ver?
3. **Para qué:** ¿qué gana con eso?
4. **Cómo lo vas a comprobar:** situaciones concretas que la PO podrá probar mirando la página. Pide números y textos reales ("3 tarjetas", "el botón dice *Escríbeme*").
5. **Qué NO incluye:** lo que se deja para otra historia.
6. **Prioridad:** P0 (urgente, bloquea otras cosas), P1 (importante para el MVP) o P2 (puede esperar).
7. **Referencias:** si hay diseño, capturas o ejemplos. El diseño de referencia del proyecto está en `CLAUDE.md`.

Reglas de la entrevista:
- **No inventes textos, precios ni datos.** Si la historia necesita un texto real (una bio, un precio, un correo) y la PO no lo tiene, anótalo en el contexto como pendiente.
- **No decidas el cómo técnico.** Si la PO propone una solución técnica ("usar WordPress", "una base de datos"), reformúlala como necesidad y deja la decisión al equipo.
- Si la idea es muy grande (más de 5 criterios o varias cosas distintas), propón dividirla en varias historias y escribe una a la vez.

### A3. Redactar

Arma la historia con este formato exacto (los títulos `###` son los de la plantilla y `/triage` los busca):

```
### Historia

Como <tipo de usuario>, quiero <lo que necesita>, para <el beneficio>.

### Contexto

<por qué importa ahora; pendientes de textos o datos, si los hay>

### Criterios de aceptación

- [ ] Dado <situación>, cuando <acción>, entonces <resultado visible>.

### Fuera de alcance

- <lo que no incluye>

### Diseño o referencias

<enlaces, o "Diseño de referencia: <URL de CLAUDE.md>", o "_No response_">

### Prioridad

<P0 — Urgente, bloquea otras cosas | P1 — Importante para el MVP | P2 — Puede esperar>
```

### A4. Revisar con la Definition of Ready

Antes de mostrarla, comprueba:
- Formato Como / quiero / para.
- Entre 1 y 5 criterios Dado / cuando / entonces, y cada resultado se puede ver o comprobar.
- Al menos un punto en Fuera de alcance.
- Tiene prioridad.
- Sin palabras vagas: "bonito", "rápido", "moderno", "fácil", "intuitivo", "etc.", "y eso", "mejorar", "optimizar". Si aparece alguna, pregunta qué significa concretamente.
- Sin decisiones técnicas.

Si algo falla, vuelve a preguntar solo lo que falta.

### A5. Confirmar y publicar

1. Muestra el título propuesto (corto, en español, empieza con verbo o sustantivo: "Ver proyectos en la página principal") y la historia completa. Pide confirmación o cambios.
2. Guarda el cuerpo en un archivo temporal y publica:
   ```
   gh issue create -R <repo> --title "<título>" --body-file <archivo>
   ```
3. Agrega el issue al tablero y ponlo en Backlog. Lee los IDs, nunca de memoria:
   ```
   gh project item-add <número> --owner <dueño> --url <url del issue> --format json
   gh project field-list <número> --owner <dueño> --format json
   gh project view <número> --owner <dueño> --format json
   gh project item-edit --id <item> --project-id <proyecto> --field-id <Status> --single-select-option-id <Backlog>
   ```
4. Si la historia venía de la lista de `docs/producto.md`, anota el número del issue junto a ella en ese archivo y dile a la PO que ese cambio queda sin subir.

### A6. Reportar

Muestra el número y la URL del issue, y explica el siguiente paso: "Diego la revisará con `/triage`. Si tiene preguntas, las verás en `/tablero` y las respondes con `/historia #N`."

---

## Modo B: responder preguntas (`/historia #N`)

### B1. Leer el ticket

1. `gh issue view N -R <repo> --json title,body,url,state,labels,comments`.
2. Busca la columna del ticket en `gh project item-list <número> --owner <dueño> --format json --limit 200`.
3. Busca los comentarios que empiezan con `**Preguntas para la PO:**`. Toma los que no tienen después un comentario `**Respuestas de la PO:**`.
4. Si no hay preguntas pendientes, dilo y pregunta si quiere cambiar algo de la historia. Si quiere, sigue con B3.

### B2. Responder

Para cada pregunta:
1. Explícala en palabras simples, y por qué el equipo la necesita.
2. Si hay opciones claras, ofrécelas con `AskUserQuestion`. Si la respuesta depende de un dato que la PO no tiene, se puede responder "lo averiguo" y queda pendiente.
3. No inventes la respuesta por ella.

### B3. Actualizar

1. Si las respuestas cambian la historia (criterios, alcance, contexto), reescribe el cuerpo con el formato de A3 y revísalo con A4.
2. Muestra a la PO el comentario de respuestas y, si aplica, la historia nueva. Pide confirmación.
3. Publica:
   ```
   gh issue edit N -R <repo> --body-file <archivo>     (solo si cambió)
   gh issue comment N -R <repo> --body-file <archivo>
   ```
   El comentario:
   ```
   **Respuestas de la PO:**

   1. <respuesta a la pregunta 1>
   2. <respuesta a la pregunta 2>

   <"Actualicé la historia: <qué cambió>." o "La historia no cambia.">
   ```

### B4. Reportar

Muestra la URL del issue y explica qué pasa ahora:
- En **Backlog:** Diego vuelve a hacer `/triage`.
- En **Especificación** o más adelante: el dev sigue con su trabajo. Si cambiaron los criterios, avisa que el spec tendrá que ajustarse.

---

## Reglas

- No cambies la columna del ticket, salvo ponerlo en Backlog al crearlo.
- No asignes responsable, tamaño ni áreas. Eso es `/triage`.
- No cierres tickets aquí. Eso es `/qa`.
- Nunca publiques sin que la PO confirme el texto.
