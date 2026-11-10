---
name: qa
description: Kit de la PO. Te guía paso a paso para probar un ticket en la columna "Prueba (QA)". Si todo pasa, lo cierra; si algo falla, deja un reporte de bug claro y lo devuelve a "En implementación".
disable-model-invocation: true
argument-hint: '<número del issue, por ejemplo 12>'
allowed-tools: Read, Glob, Grep, Write, AskUserQuestion, Bash(gh issue:*), Bash(gh project:*), Bash(git pull:*), Bash(git status:*), Bash(git branch:*)
---

# /qa — Probar un ticket y decidir

## Cómo hablar con la PO

- Usa español simple. Cada prueba debe poder hacerla alguien sin conocimientos técnicos.
- Explica qué hacer, qué deberías ver y cómo responder.
- **Nunca** arregles código ni intentes resolver errores técnicos. Si algo no arranca, redacta un mensaje para Diego.
- Pide confirmación con un "sí" antes de publicar algo en GitHub.

## Paso 0: configuración

Lee la sección `## Tablero` del `CLAUDE.md` del proyecto. De ahí salen:
- El repo, el dueño del Project y el número del Project.
- La **URL de preview para QA** y **cómo abrir la app en local**, si están definidas.

Si falta el número de issue en `$ARGUMENTS`, pídelo.

## Paso 1: verificar el ticket

1. Lee el tablero con `gh project item-list <número> --owner <dueño> --format json --limit 200`. Busca el item por la URL del issue.
2. Si su `status` no es `Prueba (QA)`, explica en qué columna está y detente.
3. Lee el issue con `gh issue view N -R <repo> --json title,body,url,comments`.

## Paso 2: tener la versión más reciente

Pregunta: "¿Actualizo tu copia del proyecto con lo último de main?"

Si dice que sí:
1. Ejecuta `git status --short`. Si hay cambios locales, no hagas pull: explícale que hay archivos modificados y sugiérele preguntarle a Diego.
2. Revisa la rama actual con `git branch --show-current`. Si no es `main`, detente y sugiérele preguntarle a Diego cómo cambiar a `main`.
3. Si todo está bien, ejecuta `git pull --ff-only`. Si falla, muestra el error en simple y detente.

## Paso 3: preparar las pruebas

1. Toma los **criterios de aceptación** de la historia (sección `### Criterios de aceptación` del issue).
2. Busca el spec aprobado: el archivo de `specs/` que contiene la línea `> **Issue:** #N`, buscando con Grep. Si existe, úsalo para entender detalles concretos como textos exactos o tamaños.
3. Escribe **una prueba por criterio de la historia**, en este formato:

```
Prueba 1 de 3 — <nombre corto>
  Haz esto:     <pasos concretos: abre…, haz clic en…, achica la ventana…>
  Deberías ver: <resultado esperado, con los textos exactos si los hay>
```

Los criterios del spec que solo se pueden comprobar mirando el código (por ejemplo "no hay colores fuera de :root") **no** se convierten en pruebas para la PO. Anótalos como: "Esto lo revisó Diego en el código".

Para pruebas de celular, explica cómo simularlo en el navegador. En Chrome: clic derecho → "Inspeccionar" → icono del celular, y elegir un ancho de 375.

## Paso 4: abrir la app

- Si hay **URL de preview**, dásela.
- Si hay **instrucciones locales** en `CLAUDE.md` o en el README, muéstralas paso a paso.
- Si no hay nada, explica que falta, propón este mensaje para Diego: "¿Cómo abro la app para probar el #N?", y detente.

Pregunta si logró abrirla. Si no lo logró, ofrece el mensaje para Diego y detente.

## Paso 5: probar una por una

Muestra cada prueba y pregunta con `AskUserQuestion`: **Pasó** / **Falló** / **No pude probarla**.

- Si **falló**: pregunta qué vio exactamente. Pídele una captura si puede, y que la adjunte después en el issue.
- Si **no pudo probarla**: pregunta por qué.

## Paso 6: decidir y publicar

Muestra el resumen y el comentario que vas a publicar, y pide un "sí".

**Si todas pasaron:**
1. Comenta en el issue:
   ```
   ✅ QA aprobado (<fecha>)
   - [x] Prueba 1: <nombre>
   - [x] Prueba 2: <nombre>
   ```
2. Mueve el item a `Cerrado` con `gh project item-edit`. Obtén los IDs con `gh project field-list` y `gh project view`, nunca de memoria.
3. Cierra el issue: `gh issue close N -R <repo>`.

**Si alguna falló:**
1. Comenta en el issue el reporte de bug:
   ```
   ❌ QA: hay que corregir (<fecha>)

   **Prueba que falló:** <nombre>
   **Qué hice:** <pasos>
   **Qué esperaba ver:** <resultado esperado>
   **Qué vi:** <lo que contó la PO>

   Las demás pruebas: <pasaron / no se pudieron probar>
   ```
2. Mueve el item a `En implementación`. No cierres el issue.

**Si hubo pruebas que no pudo hacer, y ninguna falló:**
- Comenta qué no se pudo probar y por qué. Deja el ticket en `Prueba (QA)` y sugiere avisarle a Diego.

## Paso 7: reportar

```
<✅ Ticket #N cerrado | ❌ Ticket #N devuelto a En implementación | ⏸️ Ticket #N sigue en QA>
<url>

Siguiente: corre /tablero para ver qué más te toca.
```

## Reglas

- No modifiques código ni archivos del proyecto.
- No uses otros comandos de git además de `status`, `branch --show-current` y `pull --ff-only`.
- No cierres un ticket si alguna prueba falló o no se pudo hacer.
