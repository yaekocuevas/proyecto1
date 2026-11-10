---
name: triage
description: Para el Tech Lead (Diego). Revisa una historia en Backlog, analiza su impacto técnico (con un subagente Opus cuando hay código), sugiere responsable, tamaño y áreas, y la mueve a "Especificación" con un comentario de triage.
disable-model-invocation: true
argument-hint: '<número del issue>'
allowed-tools: Read, Glob, Grep, Agent, AskUserQuestion, Bash(gh issue:*), Bash(gh project:*), Bash(gh label:*), Bash(git ls-files:*)
---

# /triage — Asignar una historia

## Paso 0: configuración

Lee la sección `## Tablero` de `CLAUDE.md` (repo, dueño y número del Project) y la sección `## Asignación de tickets` (reglas de responsable).

## Paso 1: leer la historia

1. Ejecuta `gh issue view N -R <repo> --json title,body,url,labels,comments`.
2. Busca el item en `gh project item-list <número> --owner <dueño> --format json --limit 200` por la URL del issue.
3. Si no está en `Backlog`, avisa en qué columna está y pregunta si continuar.

## Paso 2: Definition of Ready

La historia está lista si cumple todo esto:

- Tiene `### Historia` en formato Como / quiero / para.
- Tiene `### Criterios de aceptación` con al menos 1 criterio Dado / cuando / entonces, y cada uno es observable.
- Tiene `### Fuera de alcance` con al menos un punto.
- Tiene `### Prioridad`.
- No tiene palabras ambiguas sin aclarar, como "rápido", "bonito" o "etc.".

**Si no está lista:**
1. Redacta un comentario que empiece con `**Preguntas para la PO:**`, con preguntas numeradas y concretas, sin jerga técnica.
2. Muéstralo, pide confirmación y publícalo con `gh issue comment`.
3. Detente. El ticket se queda en Backlog. La PO lo verá en `/tablero` y responderá con `/historia #N`.

## Paso 3: análisis de impacto

Revisa si hay código: `git ls-files | head -50`.

**Si hay código:** lanza un subagente con la herramienta Agent, usando `subagent_type: "Explore"` y `model: "opus"`. Este es el prompt:

> Analiza este repositorio para estimar el impacto de esta historia de usuario: <historia y criterios>. Responde, sin proponer código:
> (1) áreas afectadas: frontend, backend, infra o seguridad;
> (2) archivos y carpetas que probablemente cambian;
> (3) riesgos (datos, seguridad, migraciones, rendimiento, efectos en otras partes);
> (4) tamaño estimado (XS, S, M, L o XL), con el motivo;
> (5) notas que el dev debería considerar al escribir el spec.

**Si no hay código:** estima a partir de la historia, con el mismo esquema de 5 puntos, e indica que es una estimación sin código.

## Paso 4: sugerencia

Aplica las reglas de `## Asignación de tickets` de `CLAUDE.md`:
- **Diego:** backend, base de datos o migraciones, Docker o infraestructura, CI/CD, autenticación, seguridad, pagos, datos personales, cambios transversales o tamaño M o más.
- **Aldrin:** frontend acotado, tamaño XS o S y sin lógica delicada.
- **Si hay duda:** Diego.

Muestra la sugerencia y pide confirmación o ajustes con `AskUserQuestion`:

```
Responsable: <Diego | Aldrin> — <motivo>
Tamaño:      <XS | S | M | L | XL>
Áreas:       <frontend, backend, infra, seguridad>
Riesgos:     <lista o "ninguno relevante">
```

## Paso 5: aplicar

1. Labels: el responsable y las áreas.
   ```
   gh issue edit N -R <repo> --add-label "<Responsable>,<área1>,<área2>"
   ```
2. Lee los IDs con `gh project field-list` y `gh project view`, nunca de memoria. Luego, con `gh project item-edit`:
   - Asigna `Size`.
   - Pon `Status` = `Especificación`.
3. Publica el comentario de triage, con `gh issue comment`:
   ```
   **Triage** — <fecha>

   - **Responsable:** <nombre>
   - **Tamaño:** <Size>
   - **Áreas:** <áreas>
   - **Riesgos:** <lista>
   - **Notas para el spec:** <notas del análisis>

   **Siguiente paso (<responsable>):** `git pull` en `main` y luego `/spec #N <título>`.
   Abre un PR solo con el spec, titulado `spec: #N <título>`.
   ```

## Paso 6: reportar

Muestra la URL del issue, el responsable, el tamaño y la columna nueva.

## Reglas

- No escribas el spec ni código. Eso lo hace el responsable con `/spec`.
- No cambies la prioridad. Es decisión de la PO. Si crees que debería cambiar, menciónalo en el comentario.
