# Guía de la PO

Esta guía explica tu trabajo como **Product Owner (PO)** y **QA** en el proyecto. No necesitas saber programar.

Tu trabajo es decidir **qué** se construye y **para qué**, y comprobar que lo construido funciona. El **cómo** lo deciden Diego y Aldrin.

## Tus 4 comandos

Se escriben en Claude Code, dentro de la carpeta del proyecto.

| Comando | Cuándo usarlo | Qué hace |
|---|---|---|
| `/producto` | Al empezar, y cuando cambie tu idea | Te hace preguntas sobre tu negocio y arma el plan en `docs/producto.md`: visión, MVP y lista de historias. |
| `/historia <idea>` | Cada vez que quieras pedir algo nuevo | Te ayuda a escribir una historia clara y la publica en el tablero. |
| `/historia #N` | Cuando Diego te deja preguntas en un ticket | Te ayuda a responderlas y mejora la historia. |
| `/tablero` | Cada día, o cuando quieras saber cómo va todo | Te dice qué necesita tu atención: tickets para probar, preguntas pendientes y bloqueos. |
| `/qa N` | Cuando un ticket está en "Prueba (QA)" | Te guía paso a paso para probarlo. Si todo está bien, lo cierra; si algo falla, avisa al dev con un reporte claro. |

## Tu semana típica

1. **Lunes:** `/tablero`. Revisa qué hay pendiente y responde las preguntas de Diego con `/historia #N`.
2. **Cuando tengas una idea:** `/historia <idea>`. Mantén 3 o 4 historias listas en Backlog para que el equipo nunca se quede sin trabajo.
3. **Cuando haya algo en QA:** `/qa N`. Pruébalo lo antes posible, para que los devs no esperen.
4. **Cada tanto:** `/producto`, para ajustar el plan si cambió algo del negocio.

## Las columnas del tablero

| Columna | Qué significa | ¿Te toca a ti? |
|---|---|---|
| Backlog | Historias escritas, esperando que Diego las revise | Sí, tú las creas |
| Especificación | El dev está escribiendo el plan técnico (spec) | Solo si te hacen preguntas |
| Listos para implementar | El plan técnico está aprobado | No |
| En implementación | El dev está programando | No |
| Revisión | Otro dev revisa el código | No |
| Prueba (QA) | Ya está hecho y espera tu prueba | **Sí: `/qa N`** |
| Cerrado | Probado y aceptado por ti | — |

## Cómo se ve una buena historia

**❌ Mal escrita:**
> Quiero una sección de proyectos bonita y rápida, con fotos y eso.

Problemas: "bonita", "rápida" y "y eso" no se pueden comprobar, y no dice para quién es ni para qué.

**✅ Bien escrita:**
> Como **visitante**, quiero **ver los proyectos de la artista en la página principal**, para **conocer su trabajo antes de contactarla**.
>
> - [ ] Dado que abro la página, cuando bajo hasta "Proyectos", entonces veo 3 tarjetas con título y descripción.
> - [ ] Dado que estoy en un celular, cuando veo "Proyectos", entonces las tarjetas aparecen una debajo de otra.
>
> **Fuera de alcance:** fotos de los proyectos (otra historia).

No te preocupes por escribirla perfecta: `/historia` te ayuda con las preguntas.

## Glosario

- **Historia de usuario:** lo que necesita alguien y para qué. Es tu forma de pedir algo.
- **Criterio de aceptación:** algo concreto que podrás comprobar mirando la app.
- **MVP:** la versión más pequeña del producto que ya sirve para lanzar y aprender.
- **Épica:** un grupo grande de historias relacionadas (por ejemplo, "Contacto").
- **Issue / ticket:** una tarjeta del tablero en GitHub. Cada historia es un issue con número (`#12`).
- **Spec:** el plan técnico que escribe el dev a partir de tu historia.
- **PR (Pull Request):** la propuesta de cambio de código que otro dev revisa.
- **Merge:** cuando el cambio aprobado se une al proyecto principal.
- **main:** la versión principal del proyecto, la que pruebas en QA.
- **Label:** una etiqueta de color en el ticket (por ejemplo `frontend` o `Diego`).
- **Prioridad:** P0 = urgente, P1 = importante para el MVP, P2 = puede esperar.
