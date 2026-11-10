# SPEC 01 — Inicio: nombre, fecha y lugar del evento

> **Issue:** #1
> **Estado:** Aprobado
> **Depende de:** ninguna
> **Fecha:** 2026-11-10
> **Objetivo:** Mostrar en la página de Inicio el nombre, la frase, la fecha y la ciudad de MIZUKI Japan Experience con HTML y CSS estáticos, sin JavaScript.

---

## Alcance

**Dentro:**

- Crear `index.html` en la raíz del repo, con el contenido de la página de Inicio.
- Crear `styles.css` en la raíz, enlazado desde `index.html`.
- Actualizar `CLAUDE.md`: la sección `## Proyecto` (Stack) y la fila "Cómo abrir la app en local", en el mismo PR de código.
- Textos exactos y orden del artifact de referencia: línea de presentación, nombre, subtítulo, frase, fecha y ciudad.

**Fuera de alcance (para historias futuras):**

- Cuentas regresivas del evento y de la preventa.
- Menú y los cuatro caminos (entradas, Mizuki, tarima, patrocinio).
- Nombre del local, hasta que se confirme.
- Aviso "Sitio de prueba · información no oficial".
- Diseño completo para celular. En #1 solo se valida que no haya scroll horizontal a 375px.
- Copiar colores, tipografía y disposición exactas del canvas del artifact.
- Imágenes, logo, favicon y JavaScript.

---

## Modelo de datos

Esta funcionalidad no introduce estructuras de datos. Es contenido estático en HTML.

---

## Plan de implementación

1. Crear `index.html` con el esqueleto HTML5: `lang="es"`, meta viewport, `<title>MIZUKI Japan Experience</title>` y el contenido textual en su orden, sin estilos. Prueba manual: abrir el archivo con doble clic y leer los textos.
2. Crear `styles.css`, enlazarlo desde `index.html` y centrar el contenido. Definir colores de fondo y de texto con contraste ≥ 4.5:1. Prueba manual: ver la página en escritorio y con la ventana a 375px.
3. Actualizar `CLAUDE.md`: escribir el stack elegido (HTML + CSS + JS puro) y cómo abrir la app en local (doble clic en `index.html`).

Cada paso deja la página abrible en el navegador.

---

## Criterios de aceptación

- [ ] Abrir `index.html` con doble clic en Chrome carga sin errores en la consola.
- [ ] Hay un solo `h1` con el texto "MIZUKI".
- [ ] La línea "Z Beats Production presenta" aparece encima de "MIZUKI".
- [ ] El subtítulo "Japan Experience" aparece debajo de "MIZUKI".
- [ ] La frase "Una tarde de baile entre Japón y Bolivia" aparece debajo del nombre.
- [ ] La fecha "Domingo 22 de agosto de 2027" aparece en la página, con esas palabras exactas.
- [ ] El lugar aparece como "Cochabamba" y no aparece ningún nombre de local.
- [ ] No aparecen cuentas regresivas, menú, cuatro caminos ni el aviso "Sitio de prueba".
- [ ] Con la ventana del navegador a 375px de ancho no hay scroll horizontal.
- [ ] El texto tiene contraste ≥ 4.5:1 con su fondo, verificado con una herramienta de contraste.
- [ ] El repo tiene solo dos archivos nuevos: `index.html` y `styles.css`. Además se modifica `CLAUDE.md`.
- [ ] `CLAUDE.md` indica el stack elegido y cómo abrir la app en local.

---

## Decisiones

- **Sí:** HTML + CSS + JS puro. Es una sola pantalla, sin backend, y se abre con doble clic.
- **No:** Vite o Astro ahora. Son más herramientas para una sola página. Se puede migrar cuando haya varias páginas o backend.
- **Sí:** Sin JavaScript en #1. Ninguno de los criterios necesita interacción. `script.js` se crea en la historia que lo necesite.
- **Sí:** Archivos en la raíz (`index.html`, `styles.css`). **No:** carpeta `src/`, por ser una sola página. Se reorganiza cuando crezca.
- **Sí:** Prueba manual con checklist en el navegador. **No:** Playwright ahora, para no agregar dependencias. Se revisa cuando haya más páginas.
- **Sí:** Tomar del artifact solo textos, orden y jerarquía. Los colores y la tipografía son libres dentro de los criterios de contraste. **No:** copiar el canvas, porque el código sale de los specs.
- **Sí:** Validar solo que no haya scroll horizontal a 375px. **No:** layout completo para celular, que va en su propia historia.
- **Sí:** Actualizar `CLAUDE.md` en el mismo PR de código, porque cierra el pendiente de stack y de cómo abrir la app.

---

## Riesgos

| Riesgo | Mitigación |
|---|---|
| El stack cambia pronto cuando llegue backend o Docker | El HTML estático se puede servir después desde cualquier framework o servidor. |
| Los colores no cumplen el contraste | El criterio de contraste ≥ 4.5:1 se verifica antes de cerrar. |
| El nombre de un local aparece por error | El criterio explícito verifica que no aparezca ningún nombre de local. |

---

## Lo que **no** está en este spec

- Cuentas regresivas del evento y de la preventa.
- Menú y los cuatro caminos.
- Nombre del local.
- Aviso "Sitio de prueba · información no oficial".
- Layout completo para celular.
- JavaScript e imágenes.

Cada una, si se hace, va en su propia historia y su propio spec.
