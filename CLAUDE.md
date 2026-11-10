# CLAUDE.md — Página personal de Yaeko (artist-page)

## Proyecto

- Producto de Yaeko. Por ahora, su página web personal. El proyecto crecerá con frontend, backend, Docker y más.
- Esta carpeta es temporal. Al pasar al repo definitivo, se copian `CLAUDE.md` (ajustando `## Tablero`), `.github/`, `.claude/` y `docs/`.
- Diseño de referencia: artifact "Mi página web" (`https://claude.ai/artifact/KpfyS8NfbHgvWNiSXR3Cd7`). Es una referencia visual; el código sale de los specs, no de copiar el canvas.
- Visión y plan de producto: `docs/producto.md`, lo mantiene la PO con `/producto`.
- Guía de la PO: `docs/guia-po.md`.
- Stack: **pendiente de decisión** (lo define Diego). No asumir un framework hasta que esté escrito aquí.

## Tablero

- Repo: `yaekocuevas/proyecto1`
- Dueño del Project: `yaekocuevas`
- Número del Project: pendiente (todavía no hay tablero)
- URL del tablero: pendiente
- URL de preview para QA: ninguna todavía
- Cómo abrir la app en local: pendiente (lo define Diego cuando haya código)

## Equipo y roles

| Persona | Rol | Qué hace en el flujo | Sus comandos |
|---|---|---|---|
| Yaeko | PO + QA | Define el producto, escribe historias de usuario (qué y para qué), prueba en QA y cierra tickets. | `/producto`, `/historia`, `/tablero`, `/qa` |
| Diego | Tech Lead, fullstack | Hace triage y asigna. Aprueba los specs de Aldrin y se autoaprueba los suyos. Revisa los PR de Aldrin y toma lo difícil o delicado. | `/triage`, `/spec`, `/spec-impl` |
| Aldrin | Dev frontend (nuevo) | Toma tareas livianas de frontend, escribe su spec, implementa y atiende la revisión de Diego. Revisa los PR de Diego. | `/spec`, `/spec-impl` |

## Columnas y cómo moverse

| # | Columna | Entra cuando… | Quién mueve | Sale cuando… |
|---|---|---|---|---|
| 1 | **Backlog** | La PO publica la historia con `/historia` | Yaeko | Diego hace triage |
| 2 | **Especificación** | Diego asignó responsable, tamaño y áreas con `/triage` | Diego | Se mergea en `main` el PR del spec con `Estado: Aprobado` |
| 3 | **Listos para implementar** | El spec aprobado ya está en `main` | Diego | El dev arranca `/spec-impl` |
| 4 | **En implementación** | El dev trabaja en la rama `spec-NN-slug` | Dev asignado | Abre el PR de código |
| 5 | **Revisión** | PR de código abierto con `Refs #N` | Dev | Se aprueba y se hace merge a `main` |
| 6 | **Prueba (QA)** | El PR se mergeó en `main` | Quien hizo merge | La PO prueba con `/qa N` |
| 7 | **Cerrado** | La PO aceptó en QA | Yaeko | — |

Regresos:
- **Revisión pide cambios:** el ticket vuelve a 4.
- **QA falla:** `/qa` deja el reporte de bug y lo devuelve a 4. El arreglo va en un PR nuevo con `Refs #N`.
- **El spec debe cambiar durante la implementación:** se actualiza en un PR de spec y el ticket vuelve a 2.

## Asignación de tickets

- **Diego:** backend, base de datos o migraciones, Docker o infraestructura, CI/CD, autenticación, seguridad, pagos, datos personales, cambios transversales, o tamaño M o más.
- **Aldrin:** frontend acotado (UI, estilos, componentes), tamaño XS o S, sin lógica delicada.
- **Si hay duda:** Diego. Puede reasignar a Aldrin con guía.
- Por ahora el responsable se marca con el label `Diego` o `Aldrin`. Cuando haya usuarios de GitHub, será el **assignee**.
- Labels de área: `frontend`, `backend`, `infra`, `seguridad`. Label de estado: `bloqueado`, que marca un bloqueo sin mover el ticket de columna.

## Flujo SDD (specs)

1. **Historia (PO):** la historia es el *qué*, escrita con la plantilla `.github/ISSUE_TEMPLATE/historia-usuario.yml` o con `/historia`.
2. **Spec (dev asignado):** primero `git pull` en `main`, luego `/spec #N <título>`.
   - Lee la historia con `gh issue view N` y úsala como fuente: cada criterio de la historia debe quedar cubierto por criterios de aceptación booleanos del spec.
   - El spec se guarda en `specs/NN-slug.md` y su encabezado incluye `> **Issue:** #N`.
   - Las preguntas de negocio que surjan se hacen en el issue, empezando el comentario con `**Preguntas para la PO:**`.
3. **PR del spec:** título `spec: #N <título>`, y solo contiene `specs/NN-slug.md`.
   - Aprobar consiste en pasar el estado de `Borrador` a `Aprobado` y hacer merge.
   - Diego aprueba los specs de Aldrin y se autoaprueba los suyos (merge con rol de admin).
4. **Implementación:** `git pull` en `main` y luego `/spec-impl NN-slug`. Se crea la rama `spec-NN-slug`.
5. **PR de código:** usa la plantilla `.github/pull_request_template.md` y lleva `Refs #N`.
   - Diego aprueba los PR de Aldrin, y Aldrin aprueba los de Diego.

Notas:
- Hacer pull de `main` antes de `/spec` evita que dos specs reciban el mismo número `NN`. Si aun así chocan, se renumera en el PR del spec.
- Estados de spec: `Borrador` → `En revisión` → `Aprobado` → `Implementado` (o `Obsoleto`).
- `specs/.spec-config.yml` define `AutoCreateBranch`. No modificarlo sin avisar al equipo.

## Reglas del flujo

1. **PR con `Refs #N`, nunca `Closes #N`.** El ticket lo cierra la PO después de QA.
2. **No se implementa sin spec `Aprobado` en `main`.** `/spec-impl` lo valida.
3. **No hay merge sin aprobación de otra persona.** La única excepción es el PR de spec de Diego.
4. **Un ticket = una historia = un spec = una rama.**
5. **WIP:** 1 ticket "En implementación" por dev, y como máximo 2 en "Especificación".
6. **Solo la PO cierra tickets.**
7. **El agente no hace commits automáticos.** Solo commitea cuando el desarrollador lo pide.
8. **Lo que está fuera de alcance va a una historia nueva**, nunca "de paso".

## Definition of Ready (historia lista para triage)

- Formato Como / quiero / para.
- Criterios Dado / cuando / entonces, observables, máximo 5.
- Fuera de alcance explícito.
- Prioridad P0, P1 o P2.
- Sin palabras vagas ni decisiones técnicas.

## Definition of Done (ticket listo para cerrar)

- El spec y el código están en `main` con aprobación.
- Todos los criterios de aceptación del spec están marcados.
- QA de la PO aprobado (comentario `✅ QA aprobado` en el issue).

## Convenciones de código y git

- Idioma: **español** para historias, specs, commits y PRs.
- Commits con prefijo de tipo: `feat:`, `fix:`, `style:`, `docs:`, `chore:`. Ejemplo: `feat: agrega grid de proyectos`.
- Un paso del plan de implementación equivale a un commit, o a pocos commits pequeños.
- Nunca subir credenciales, tokens ni archivos `.env`.
- UI: botones y enlaces reales, contraste ≥ 4.5:1, foco visible, y sin scroll horizontal a 375px.

## Qué no hacer

- No hacer merge sin aprobación (salvo el PR de spec de Diego).
- No usar `Closes #N` en PRs.
- No implementar fuera del alcance del spec activo.
- No inventar textos, precios ni datos: preguntarlos en el issue.
