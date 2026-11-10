---
name: qa
description: Para la PO (Yaeko). Guía paso a paso la prueba de un ticket en "Prueba (QA)". Si todo pasa, deja "✅ QA aprobado", lo mueve a Cerrado y cierra el issue. Si algo falla, deja un reporte de bug claro y lo devuelve a "En implementación".
disable-model-invocation: true
argument-hint: '<número del issue>'
allowed-tools: Read, Glob, Grep, AskUserQuestion, Write, Bash(gh issue:*), Bash(gh project:*), Bash(gh pr:*), Bash(git pull:*), Bash(git log:*)
---

# /qa — Probar un ticket

Hablas con la PO, que no programa. Instrucciones cortas y concretas: qué abrir, dónde hacer clic, qué mirar.

## Paso 0: configuración

Lee de `CLAUDE.md` la sección `## Tablero` (repo, dueño, número del Project, **URL de preview para QA** y **cómo abrir la app en local**) y la `## Definition of Done`.

## Paso 1: comprobar que está listo para QA

1. `gh issue view N -R <repo> --json title,body,url,state,labels,comments`.
2. Busca el item en `gh project item-list <número> --owner <dueño> --format json --limit 200`.
3. Si no está en "Prueba (QA)", di en qué columna está y detente. Probar antes de tiempo hace perder tiempo a todos.
4. Comprueba que el código está en `main`: `gh pr list -R <repo> --state merged --search "Refs #N" --json number,title,url,mergedAt`. Si no hay PR mergeado, avisa y detente.

## Paso 2: preparar la lista de pruebas

1. `git pull` en `main` para tener los specs al día.
2. Busca el spec del ticket: el archivo en `specs/` cuyo encabezado contiene `> **Issue:** #N`.
3. La lista de pruebas sale de:
   - los **criterios de la historia** (sección `### Criterios de aceptación` del issue), que son los que la PO escribió;
   - los **criterios de aceptación del spec**, si agregan algo que la PO puede ver. Tradúcelos a lenguaje simple y omite los que solo puede comprobar un dev.
4. Agrega siempre estas pruebas generales, si el ticket cambia algo visible:
   - Se ve bien en un celular (o con la ventana angosta), sin tener que mover la página hacia los lados.
   - Los botones y enlaces nuevos funcionan con clic, y también con la tecla Tab (se ve un borde o marca en el que está seleccionado) y Enter.
5. Si el spec no existe o no tiene criterios, usa solo los de la historia y avísalo en el reporte.

## Paso 3: abrir la app

- Si hay **URL de preview**, dásela.
- Si no, y hay instrucciones de **cómo abrir la app en local**, guíala paso a paso.
- Si no hay ninguna de las dos, detente: explica que no hay dónde probar y sugiere pedirle a Diego que lo defina en `CLAUDE.md`. No muevas el ticket.

## Paso 4: probar criterio por criterio

Por cada prueba:
1. Explica en 1 a 3 pasos qué hacer y qué debería ver.
2. Pregunta con `AskUserQuestion`: **Funciona** / **No funciona** / **No pude probarlo**.
3. Si **No funciona**, pregunta (de a una cosa, sin presionar):
   - ¿Qué viste en vez de lo esperado?
   - ¿En qué dispositivo y navegador? (por ejemplo, "celular Android, Chrome")
   - ¿Tienes una captura? Puede pegarla en el issue después.
4. Si **No pude probarlo**, pregunta por qué (no encontró dónde, la página no carga…) e intenta ayudar. Si no se resuelve, cuenta como pendiente.

Si la PO nota algo que **no está en los criterios** (le gustaría otro color, falta una sección), no es un fallo de QA: anótalo como **idea para una historia nueva**.

## Paso 5: resultado

Muestra el resumen (✅ / ❌ / ⏸ por prueba) y el comentario que se va a publicar. **Pide confirmación antes de publicar o mover nada.**

### Si todo funciona

1. Comentario:
   ```
   ✅ QA aprobado — <fecha>

   Probado en: <dispositivos y navegadores>

   - [x] <criterio 1>
   - [x] <criterio 2>
   ```
2. Mueve el item a "Cerrado" con `gh project item-edit`. Lee los IDs con `gh project field-list` y `gh project view`, nunca de memoria.
3. Cierra el issue: `gh issue close N -R <repo> --reason completed`.

### Si algo falla

1. Comentario:
   ```
   ❌ QA falló — <fecha>

   Probado en: <dispositivos y navegadores>

   **Funciona:**
   - [x] <criterio>

   **No funciona:**
   - [ ] <criterio>
     - **Pasos:** <qué hizo la PO>
     - **Esperado:** <lo que dice el criterio>
     - **Obtenido:** <lo que vio>

   **Siguiente paso (@<responsable según el label>):** arreglar en un PR nuevo con `Refs #N` y volver a pasarlo a "Prueba (QA)".
   ```
   No escribas `@` delante del nombre si el responsable no tiene usuario de GitHub conocido. Usa el label (`Diego` o `Aldrin`).
2. Mueve el item a "En implementación". El issue queda abierto.

### Si hay pruebas pendientes y ninguna falla

No apruebes ni muevas. Publica un comentario `⏸ QA pendiente` con lo que falta y por qué, y explica que puede volver a correr `/qa N` después.

## Paso 6: reportar

Muestra la URL del issue y la columna nueva. Si salieron ideas nuevas, ofrécele escribirlas con `/historia <idea>`.

## Reglas

- Nunca marques como aprobado algo que la PO no probó.
- No pidas a la PO que lea código ni que use la terminal, salvo para abrir la app en local si no hay preview.
- No cierres tickets fuera de este comando. Solo la PO cierra tickets.
