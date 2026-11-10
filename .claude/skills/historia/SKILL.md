---
name: historia
description: Kit de la PO. Crea una historia de usuario clara y sin ambigüedades y la publica como issue en la columna Backlog del tablero. Con "#N", revisa y mejora una historia existente (por ejemplo, para responder preguntas de Diego).
disable-model-invocation: true
argument-hint: '<idea en una frase> | #N'
allowed-tools: Read, Edit, Write, Glob, AskUserQuestion, Bash(gh issue:*), Bash(gh project:*), Bash(date:*)
---

# /historia — Crear o mejorar una historia de usuario

## Cómo hablar con la PO

- Es su primer proyecto y no tiene perfil técnico. Habla en **español simple**, con frases cortas.
- Explica cada término la primera vez. Ejemplos:
  - "Historia de usuario = qué necesita alguien y para qué".
  - "Criterio de aceptación = algo que podrás comprobar tú misma mirando la app".
- **Nunca** pidas decisiones técnicas. Si aparece una (cómo guardar datos, qué tecnología usar), escríbela en el Contexto como `Pregunta para Diego: ...` y sigue.
- Pregunta de **3 en 3** con `AskUserQuestion`. Da opciones concretas, con tu recomendación primero y marcada "(Recomendado)".
- Pide confirmación con un "sí" antes de cualquier cambio en GitHub.

## Paso 0: configuración

Lee la sección `## Tablero` del `CLAUDE.md` del proyecto. De ahí salen el **repo**, el **dueño del Project** y el **número del Project**. Si no existe esa sección, pregunta esos tres datos y ofrece agregarla a `CLAUDE.md`. Nunca los inventes.

Lee también `docs/producto.md`, si existe. Úsalo para entender el producto y no repetir preguntas que ya tienen respuesta.

## Paso 1: elegir el modo

- **`$ARGUMENTS` es `#N` (modo mejorar):**
  - Lee el issue con `gh issue view N -R <repo> --json title,body,comments,state`.
  - Resume en simple qué dice la historia y qué preguntas o comentarios dejó Diego (busca `Preguntas para la PO`).
  - Ve al paso 2, solo con lo que falta o lo que hay que aclarar.
- **`$ARGUMENTS` es una idea (modo crear):** úsala como punto de partida.
- **Está vacío:** si `docs/producto.md` tiene historias pendientes (`- [ ]`), muestra las primeras 5 y pregunta cuál hacer. Si no hay, pide la idea en una frase.

## Paso 2: preguntas de negocio

Ve bloque por bloque y espera la respuesta antes de seguir:

1. **Quién y para qué:** qué tipo de usuario, qué quiere hacer y qué gana con eso.
2. **Cómo lo vas a comprobar:** describe qué verías en la pantalla cuando funcione. Pide ejemplos concretos: textos exactos, cuántos elementos, qué pasa si algo sale mal (por ejemplo, un campo vacío).
3. **Límites:** qué NO incluye esta historia, si hay un diseño o referencia, y la prioridad.
   - **P0:** urgente, bloquea otras cosas.
   - **P1:** importante para el MVP.
   - **P2:** puede esperar.

Si detectas palabras vagas ("rápido", "bonito", "fácil", "intuitivo", "etc.", "y más"), pregunta qué significan con un ejemplo observable.

## Paso 3: escribir la historia

Convierte cada cosa comprobable en un criterio con este formato:
`- [ ] Dado <situación>, cuando <acción>, entonces <resultado que se ve>.`

**Título:** corto, desde el punto de vista del usuario, empezando con un verbo, de hasta 60 caracteres. Ejemplo: `Ver los proyectos en la página principal`.

**Cuerpo:** usa exactamente estos encabezados, que son los mismos de la plantilla del repo:

```markdown
### Historia

Como **<usuario>**, quiero **<necesidad>**, para **<beneficio>**.

### Contexto

<por qué importa; preguntas para Diego si las hay>

### Criterios de aceptación

- [ ] Dado ..., cuando ..., entonces ...

### Fuera de alcance

- ...

### Diseño o referencias

<enlaces o "Ninguna">

### Prioridad

<P0 — Urgente, bloquea otras cosas | P1 — Importante para el MVP | P2 — Puede esperar>
```

## Paso 4: revisión de calidad

Comprueba cada punto y explícale a la PO, en simple, lo que no cumpla:

- **Tiene valor:** se entiende qué gana el usuario.
- **Es comprobable:** cada criterio describe algo que se ve o se hace en la app. Nada como "que funcione bien".
- **Es pequeña:** tiene como máximo 5 criterios. Si tiene más, o mezcla dos objetivos, propone dividirla en varias historias y crea una por una.
- **Es independiente:** si depende de otra, lo dice en Contexto con el número `#N`.
- **No dice cómo construirla:** describe el qué, no el cómo. Si menciona tecnología, muévelo a "Pregunta para Diego".

## Paso 5: confirmar

Muestra el título y el cuerpo completos, y pregunta:

```
¿Publico esta historia en el tablero (columna Backlog)? (sí / cambiar algo)
```

En modo mejorar, pregunta: `¿Actualizo el issue #N con esta versión?`

## Paso 6: publicar

**Modo crear:**
1. Busca duplicados: `gh issue list -R <repo> --state all --search "<título>" --json number,title`. Si hay uno igual, muéstralo y pregunta si mejoramos ese en lugar de crear otro.
2. Guarda el cuerpo en un archivo temporal, en el scratchpad de la sesión, nunca dentro del proyecto. Luego crea el issue: `gh issue create -R <repo> --title "<título>" --body-file <archivo>`.
3. Agrégalo al tablero: `gh project item-add <número> --owner <dueño> --url <url> --format json`. Guarda el `id`.
4. Lee los IDs actuales, **nunca de memoria**:
   - `gh project field-list <número> --owner <dueño> --format json`
   - `gh project view <número> --owner <dueño> --format json` (este da el id del proyecto)
5. Pon Status = `Backlog` y Priority = la elegida, con `gh project item-edit --id <item> --project-id <proyecto> --field-id <campo> --single-select-option-id <opción>`.

**Modo mejorar:**
- Actualiza el cuerpo con `gh issue edit N -R <repo> --body-file <archivo>`. Si el título cambió, usa también `--title`.
- Si respondiste preguntas de Diego, deja un comentario corto: `gh issue comment N -R <repo> --body "Respondí las preguntas: <resumen>"`.
- No cambies la columna.

## Paso 7: actualizar el plan y reportar

Si la historia venía de `docs/producto.md`, cambia su línea de `- [ ]` a `- [x] ... → #N`. Es el único cambio que haces en ese archivo.

Responde:

```
✅ Historia <publicada | actualizada>: #N <título>
   <url>

Qué sigue: Diego hará el triage (asigna quién la hace).
Puedes ver cómo va con /tablero.
```

## Reglas

- No asignes dev, tamaño ni labels de área: eso lo decide Diego en `/triage`.
- No muevas tickets de columna, salvo Backlog al crear.
- No inventes textos, precios ni datos del negocio: pregúntalos.
- Si un comando de `gh` falla, muestra el error en simple ("GitHub respondió: ...") y detente.
