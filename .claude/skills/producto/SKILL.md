---
name: producto
description: Kit de la PO. Convierte la idea de negocio en un plan simple (visión, usuarios, MVP, épicas y lista de historias) guardado en docs/producto.md. Úsalo al empezar el proyecto o cuando cambie la visión.
disable-model-invocation: true
argument-hint: '[idea o cambio, opcional]'
allowed-tools: Read, Write, Edit, Glob, AskUserQuestion, Bash(date:*), Bash(ls:*)
---

# /producto — De la idea al plan

Fecha de hoy (úsala en el archivo, nunca la inventes):
!`date +%F`

## Cómo hablar con la PO

- Es su primer proyecto y no tiene perfil técnico. Habla en **español simple**, con frases cortas.
- La primera vez que uses un término (MVP, épica, historia de usuario), explícalo en una línea.
  - Ejemplo: "MVP = la versión más pequeña que ya sirve para lanzar y aprender".
- Nunca pidas decisiones técnicas (lenguajes, bases de datos, servidores). Si aparece una, anótala en "Preguntas abiertas" como pregunta para Diego.
- Haz preguntas de **3 en 3** con la herramienta de preguntas (`AskUserQuestion`). Da opciones concretas, con tu recomendación primero y marcada como "(Recomendado)".
- Si una respuesta es vaga ("que sea fácil", "bonito", "rápido"), pide un ejemplo concreto.

## Paso 1: contexto

1. Lee `CLAUDE.md` del proyecto, si existe, para entender de qué trata.
2. Lee `docs/producto.md`, si existe.
   - Si existe, esto es una **actualización**: pregunta qué cambió y no repitas lo que ya está.
   - Si no existe, esto es una **sesión nueva**.
3. Si `$ARGUMENTS` trae una idea o un cambio, úsalo como punto de partida.

## Paso 2: preguntas (sesión nueva)

Ve bloque por bloque y espera las respuestas antes de seguir.

- **Bloque 1, el problema:**
  - Qué problema resuelve el producto.
  - Para quién es (tipos de usuario).
  - Cómo lo resuelven hoy, sin tu producto.
- **Bloque 2, la propuesta:**
  - Qué hace distinto tu producto.
  - Cuál es la acción más importante que hará un usuario.
  - Cómo sabrás que funciona (por ejemplo: personas registradas, ventas, mensajes recibidos).
- **Bloque 3, el MVP:**
  - Qué es lo mínimo para lanzar.
  - Qué queda **fuera** del MVP aunque sea buena idea.
  - Si hay alguna fecha o evento importante.

Con las respuestas, agrupa las funcionalidades en **épicas**. Una épica es un grupo grande de funcionalidades, como "Mostrar portafolio" o "Contacto". Divide cada épica en **historias pendientes** de una línea, cada una pequeña y con valor para un usuario.

Propón un orden por prioridad: primero lo que desbloquea a lo demás y lo que es imprescindible para el MVP. Muestra la lista y pregunta si el orden está bien.

## Paso 3: borrador y confirmación

Muestra el borrador completo con este formato y pregunta: "¿Lo guardo así o cambiamos algo?".

```markdown
# Producto

> Última actualización: <fecha de hoy>

## Visión
<una frase: para quién, qué problema, qué lo hace distinto>

## Problema
<2–4 líneas>

## Usuarios
- **<tipo>:** <qué necesita>

## Qué nos hace distintos
- <punto>

## MVP (lo mínimo para lanzar)
**Dentro:**
- <funcionalidad>

**Fuera (para después):**
- <funcionalidad>

## Cómo sabremos que funciona
- <señal medible>

## Épicas
1. **<Épica>:** <una línea>

## Historias pendientes (en orden de prioridad)
- [ ] <Épica> · <historia en una línea>

## Preguntas abiertas
- <pregunta> (para <Diego | la PO>)
```

En las historias pendientes:
- `- [ ]` significa que todavía no se publicó.
- `- [x] ... → #N` significa que ya es el ticket `#N`. Esa marca la pone `/historia`.

## Paso 4: guardar

Escribe `docs/producto.md`. Si ya existía, conserva las marcas `→ #N` de las historias ya publicadas, y no borres historias publicadas.

Termina con:

```
✅ Plan guardado en docs/producto.md

Siguiente paso: crea la primera historia con
  /historia <primera historia pendiente>
```

## Reglas

- Solo escribes `docs/producto.md`. No tocas GitHub ni código.
- No inventes datos del negocio (precios, cifras, nombres). Si faltan, déjalos como pregunta abierta.
