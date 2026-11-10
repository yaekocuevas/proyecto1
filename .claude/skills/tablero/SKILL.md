---
name: tablero
description: Kit de la PO. Resume el tablero en lenguaje simple y dice qué necesita tu atención hoy (tickets para probar, preguntas de Diego, bloqueos). Solo lectura.
disable-model-invocation: true
allowed-tools: Read, Bash(gh project:*), Bash(gh issue:*)
---

# /tablero — ¿Qué tengo que hacer hoy?

Esta skill es **solo de lectura**: no cambia nada en GitHub.

## Cómo hablar con la PO

- Usa español simple, sin jerga técnica.
- Empieza por lo que la necesita a ella. Lo demás va después, resumido.

## Paso 0: configuración

Lee la sección `## Tablero` del `CLAUDE.md` del proyecto: repo, dueño del Project y número del Project. Si no existe, pide esos datos y detente.

## Paso 1: leer el tablero

```
gh project item-list <número> --owner <dueño> --format json --limit 200
```

De cada item toma: número de issue, título, `status`, `priority`, `size` y labels. Si un item no tiene status, cuéntalo como "Sin columna".

## Paso 2: buscar lo que necesita a la PO

1. **Para probar:** items en `Prueba (QA)`.
2. **Preguntas de Diego:** para items en `Backlog` o `Especificación`, lee los comentarios:
   ```
   gh issue view <N> -R <repo> --json comments
   ```
   Si el último comentario que contiene `Preguntas para la PO` no tiene después un comentario con `Respondí`, cuenta el ticket como "pregunta sin responder".
3. **Bloqueados:** items con el label `bloqueado`.

Para no hacer demasiadas llamadas, revisa los comentarios de como máximo 15 tickets, empezando por los de mayor prioridad.

## Paso 3: responder

Usa este formato:

```
## 👋 Lo que necesita tu atención

**Para probar (QA):**
- #12 Ver los proyectos en la página principal → corre /qa 12

**Preguntas de Diego sin responder:**
- #15 Formulario de contacto → corre /historia #15

**Bloqueados:**
- #9 ... (ver el último comentario)

(Si no hay nada: "Nada urgente hoy. ✨")

## 📋 Resumen del tablero

| Columna | Cantidad | Tickets |
|---|---|---|
| Backlog | 3 | #15, #16, #17 |
| Especificación | 1 | #14 (Aldrin) |
| Listos para implementar | 0 | — |
| En implementación | 1 | #13 (Diego) |
| Revisión | 0 | — |
| Prueba (QA) | 1 | #12 |
| Cerrado | 4 | — |

## 👉 Siguiente paso sugerido
<una sola acción concreta, con el comando exacto>
```

Para elegir el siguiente paso, usa esta prioridad:
1. Probar lo que está en QA.
2. Responder preguntas de Diego.
3. Si el Backlog tiene menos de 3 historias, crear la siguiente historia pendiente de `docs/producto.md` con `/historia`.

En la columna Tickets, muestra el responsable (label `Diego` o `Aldrin`) entre paréntesis cuando exista.

## Reglas

- No modifiques nada: ni issues, ni columnas, ni archivos.
- Si un comando falla, explica el error en simple y sugiere avisarle a Diego.
