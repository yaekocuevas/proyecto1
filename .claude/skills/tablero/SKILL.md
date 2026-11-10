---
name: tablero
description: Para la PO (Yaeko). Resume el estado del tablero y dice qué necesita su atención hoy: tickets para probar, preguntas sin responder, bloqueos y si faltan historias en el Backlog. Solo lee, no cambia nada.
disable-model-invocation: true
allowed-tools: Read, Bash(gh project:*), Bash(gh issue:*), Bash(gh pr:*)
---

# /tablero — ¿Qué necesita mi atención?

Hablas con la PO, en palabras simples. Este comando **solo lee**: no mueve tickets, no comenta, no edita.

## Paso 0: configuración

Lee la sección `## Tablero` de `CLAUDE.md` (repo, dueño, número y URL del Project) y la regla de WIP en `## Reglas del flujo`.

## Paso 1: leer el tablero

1. `gh project item-list <número> --owner <dueño> --format json --limit 200`.
   Por cada item toma: número del issue, título, `status`, `size` y labels.
2. `gh issue list -R <repo> --state open --json number,title,labels,comments,updatedAt --limit 200`.
3. Si hay tickets en "Revisión", busca su PR: `gh pr list -R <repo> --state open --search "Refs #N" --json number,title,url,reviewDecision`.

Si un issue abierto no está en el tablero, anótalo para avisar.

## Paso 2: detectar lo que necesita a la PO

En este orden de importancia:

1. **🧪 Para probar:** tickets en "Prueba (QA)". Por cada uno, sugiere `/qa N`. Si llevan más de 2 días ahí (según `updatedAt`), dilo: el dev está esperando.
2. **❓ Preguntas sin responder:** tickets con un comentario `**Preguntas para la PO:**` que no tiene después un `**Respuestas de la PO:**`. Sugiere `/historia #N`.
3. **⛔ Bloqueados:** tickets con el label `bloqueado`. Muestra el último comentario que explica el bloqueo, si lo hay, y si la PO puede ayudar.
4. **📥 Backlog:** cuántas historias hay esperando triage. Si hay menos de 3, sugiere escribir más con `/historia <idea>`. Si `docs/producto.md` tiene historias planeadas sin número de issue, nómbralas como ideas.
5. **↩️ Devueltos por QA:** tickets en "En implementación" con un comentario `❌ QA falló` reciente. Es solo informativo.

## Paso 3: avisos del equipo

Solo informativos, en una línea cada uno:
- Más de 1 ticket "En implementación" para el mismo responsable (label `Diego` o `Aldrin`), o más de 2 en "Especificación": incumple el límite de WIP.
- Issues abiertos que no están en el tablero.
- Tickets en "Cerrado" que siguen abiertos, o issues cerrados fuera de "Cerrado".

## Paso 4: mostrar

Formato:

```
## Tu tablero — <fecha>

### Te toca a ti
<lista de los puntos 1 a 3 del Paso 2, con número, título y el comando sugerido.
 Si no hay nada: "Nada urgente. 🎉">

### Resumen
| Columna | Tickets |
|---|---|
| Backlog | #N título, … |
| … (las 7 columnas, en orden) |

### Sugerencias
<puntos 4 y 5 del Paso 2, y los avisos del Paso 3>

Tablero: <URL del tablero>
```

Mantén el reporte corto. Si una columna tiene más de 5 tickets, muestra 5 y "y N más".
