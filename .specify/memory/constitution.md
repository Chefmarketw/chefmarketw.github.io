<!--
Sync Impact Report
- Version: (plantilla) → 1.0.0
- Principios añadidos: I–VI (todos nuevos)
- Secciones añadidas: Restricciones técnicas, Flujo de trabajo, Gobierno
- Plantillas: plan/spec/tasks sin cambios necesarios ✅
- Pendiente: recrear tools/build.py (fuentes + minificado)
-->
# Constitución de $CHEF Explosive Kitchen

## Principios

### I. Un solo archivo, sin dependencias
El juego se entrega como un único HTML autocontenido: JS y CSS en línea, fuentes en base64 y sin CDN, frameworks ni pasos de instalación para jugar. Si una funcionalidad necesita una librería externa, hay que justificarlo en el plan y aprobarlo.

### II. Funciona sin red de terceros
Jugar nunca depende de un servicio externo. El ranking y el conteo (Supabase) son opcionales: si fallan o tardan más de 8 s, el juego sigue y solo muestra un aviso. Ninguna funcionalidad nueva puede bloquear el inicio ni la partida.

### III. PC y celular por igual (NO NEGOCIABLE)
Cada cambio funciona con teclado (flechas/WASD + Espacio) y con controles táctiles (cruceta + botón OLLA), en pantallas horizontales, verticales y bajas. Se respeta `prefers-reduced-motion` y se mantienen las etiquetas ARIA.

### IV. Bilingüe: inglés primero
Todo texto visible existe en inglés y en español dentro de `TEXT`. Lo público (README, meta, vistas previas, textos para compartir) va primero en inglés y después en español. El idioma inicial se toma del dispositivo.

### V. Justo y reproducible
El Menú del día es igual para todos: la misma semilla da el mismo mapa en cualquier dispositivo y orientación. Los cambios a la generación de niveles, a los puntos o a la dificultad no pueden cambiar en mitad del día un Menú que ya empezó. Si hace falta, se aplican desde el día siguiente.

### VI. Privacidad y sin promesas financieras
No se recogen datos personales: solo un código anónimo al azar y el @ de X que el jugador escribe voluntariamente. Sin rastreadores ni analítica de terceros. El juego es gratis y no exige comprar $CHEF. Ningún texto promete ganancias.

## Restricciones técnicas
- Canvas 2D y `requestAnimationFrame`. El arte se dibuja por código y se guarda en caché según el tamaño de la casilla.
- `src/game.html` es la fuente legible y `index.html` la versión publicada (minificada, con el `<head>` público).
- Supabase (`frnpeqrldutmjikalmps`): solo la llave *publishable* va en el cliente. Las tablas tienen RLS y restricciones `CHECK`. Los cambios de esquema se hacen con migraciones.
- La identidad visual del gato $CHEF (bengalí, lentes, hoodie negro, corona dorada) es fija.

## Flujo de trabajo
- Cada funcionalidad pasa por Spec Kit: spec → plan → tasks → implement, en `specs/NNN-nombre/`.
- Se trabaja en ramas y GitHub Pages publica desde `main`.
- Commits en inglés, en imperativo y desde el punto de vista del jugador.
- Antes de hacer merge se prueba en el navegador en PC y en celular (o con un viewport de celular), revisando ES y EN.

## Gobierno
Esta constitución tiene prioridad sobre cualquier otra práctica. Cada plan revisa que la cumple. Los cambios a la constitución se documentan aquí y suben la versión: MAJOR si se quita o cambia un principio, MINOR si se añade uno y PATCH si solo se aclara algo. La guía del día a día está en `CLAUDE.md`.

**Version**: 1.0.0 | **Ratified**: 2026-11-02 | **Last Amended**: 2026-11-02
