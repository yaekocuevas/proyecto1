---
name: producto
description: Para la PO (Yaeko). Entrevista sobre el negocio y arma o actualiza el plan de producto en docs/producto.md: visión, público, objetivos, MVP, épicas e historias planeadas con prioridad.
disable-model-invocation: true
argument-hint: '[tema a actualizar, opcional]'
allowed-tools: Read, Write, Edit, AskUserQuestion, Bash(gh issue:*), Bash(git status:*), Bash(git diff:*)
---

# /producto — El plan del producto

Hablas con la PO, que conoce su negocio pero no programa. El resultado es `docs/producto.md`, el documento que el equipo lee para entender qué se construye y por qué.

## Paso 0: contexto

1. Lee `docs/producto.md`.
2. Lee de `CLAUDE.md` la sección `## Proyecto` (incluye el diseño de referencia) y la `## Tablero`.
3. `gh issue list -R <repo> --state all --json number,title,state --limit 200`, para saber qué historias ya existen.

Elige el modo:
- `docs/producto.md` está vacío o solo tiene el texto de ejemplo → **Modo A: crear el plan**.
- Ya tiene contenido → **Modo B: actualizar**. Si se pasó un tema como argumento, empieza por ese.

## Modo A: crear el plan

Entrevista en bloques, con pocas preguntas por turno (máximo 3). Usa `AskUserQuestion` cuando haya opciones claras. Resume lo entendido al final de cada bloque y confírmalo antes de pasar al siguiente.

1. **Visión:** ¿qué es esto, en una frase? ¿Qué problema resuelve o qué oportunidad aprovecha?
2. **Público:** ¿quién va a visitar la página? (clientes, galerías, fans, prensa…). ¿Qué busca cada uno?
3. **Objetivos:** ¿qué tiene que pasar para decir que funciona? Pide algo observable ("que me escriban para encargos", "vender 5 piezas al mes"), no "tener presencia".
4. **Contenido disponible:** ¿qué hay ya? (fotos, textos, bio, redes, precios). ¿Qué falta? No inventes nada: lo que falte queda como pendiente.
5. **MVP:** de todo lo que quiere, ¿qué es lo mínimo para publicar la primera versión y empezar a aprender? Ayúdala a recortar: si algo puede esperar a la segunda versión, que espere.
6. **Fuera del MVP:** lo que viene después.
7. **Restricciones:** fechas, presupuesto, cosas que no quiere. Sin decisiones técnicas: si menciona una herramienta concreta, anótala como preferencia para que el equipo la evalúe.

Después propón las **épicas** (grupos grandes, como "Portafolio" o "Contacto") y, dentro de cada una, **historias planeadas** en una línea ("Como visitante, quiero ver los proyectos, para conocer su trabajo"), con prioridad P0, P1 o P2. Las del MVP son P0 o P1.

## Modo B: actualizar

1. Resume el plan actual en 5 a 8 líneas.
2. Pregunta qué cambió: algo del negocio, el MVP, prioridades, o ideas nuevas.
3. Actualiza solo las secciones afectadas. Si una historia ya publicada cambia de prioridad o de alcance, no edites el issue aquí: sugiere `/historia #N`.
4. Anota el cambio en `## Historial de cambios`.

## Formato de `docs/producto.md`

```
# Producto

> Lo mantiene la PO con `/producto`. Última actualización: <fecha>.

## Visión
<una o dos frases>

## Público
| Quién | Qué busca |
|---|---|

## Objetivos
- <objetivo observable>

## Contenido
- **Disponible:** …
- **Pendiente:** … (quién lo consigue, si se sabe)

## MVP
<qué incluye la primera versión, en una lista corta>

## Después del MVP
- …

## Restricciones y preferencias
- …

## Épicas e historias planeadas
### <Épica>
| Historia | Prioridad | Issue |
|---|---|---|
| Como …, quiero …, para … | P1 | #N o "—" |

## Historial de cambios
- <fecha>: <qué cambió>
```

En la columna "Issue", pon el número si la historia ya está publicada (compáralo con la lista del Paso 0) o "—" si no.

## Paso final

1. Muestra el documento (o los cambios, en Modo B) y pide confirmación antes de guardarlo.
2. Guarda `docs/producto.md`.
3. Sugiere los siguientes pasos: publicar las historias P0 y P1 sin issue con `/historia <idea>`, empezando por la más importante.
4. Avisa que el archivo quedó cambiado en su computadora, sin subir. **No hagas commit ni push** salvo que la PO lo pida.

## Reglas

- No inventes datos del negocio, textos, precios ni fechas. Pregúntalos o márcalos como pendientes.
- No publiques issues desde aquí. Eso es `/historia`.
- No tomes decisiones técnicas (stack, hosting, herramientas). Eso lo define Diego.
