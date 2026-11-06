# $CHEF Explosive Kitchen · Cocina Explosiva

Juego de laberinto gratis para PC y celular. Pones ollas a presión que explotan en cruz, rompes cajas, eliminas ratas y buscas la puerta escondida. Es un juego original del token **$CHEF** (gato bengalí con lentes, hoodie negro y corona). Está publicado en <https://chefmarketw.github.io>.

## Archivos

| Ruta | Qué es |
|---|---|
| `index.html` | **Versión publicada** (GitHub Pages desde `main`). Va minificada, con las fuentes dentro y con los meta `og:`/`twitter:` en el `<head>`. No se edita a mano. |
| `src/game.html` | **Código fuente legible y comentado** (en español). Es una copia del artifact "$CHEF Cocina Explosiva" (<https://claude.ai/artifact/ELqTVq3RMRCChJMNN7AUE9>). Los cambios se hacen aquí. |
| `card.jpg` | Imagen de vista previa (1200×630) para X y otras redes. |
| `README.md` | Texto público: primero en inglés y después en español. |
| `LICENSE` | © 2026 Chefmarketw ($CHEF). Todos los derechos reservados. |
| `.specify/`, `.claude/skills/` | Spec Kit (desarrollo guiado por especificaciones). |
| `specs/` | Especificaciones. `001-juego-base` describe cómo está hoy el juego. |

### Cómo se publica un cambio
1. Edita `src/game.html` y pruébalo en el navegador (o como artifact).
2. Genera `index.html` a partir de él: minifica el JS y el CSS y pon el `<head>` público (title, description, copyright, og:*, twitter:card). Toma ese `<head>` del `index.html` actual. La primera línea de `src/game.html` es el esqueleto que agrega el artifact y no va en la versión publicada.
3. Haz commit en una rama y luego merge a `main`. GitHub Pages publica `main`.

> El script `build.py` que se usó antes (meter las fuentes woff2 y minificar) **no está en el repo**. Si hace falta, vuelve a escribirlo en `tools/build.py` y guárdalo.

## Arquitectura
- Un solo archivo HTML, sin dependencias externas ni CDN. Las fuentes Bungee y Courier Prime van en base64 (subconjunto latino).
- Se dibuja en `<canvas>` 2D con `requestAnimationFrame`. Los sprites (gato, ratas, objetos) se dibujan por código y se guardan en caché según el tamaño de la casilla.
- Tablero de 15×13. En pantallas verticales se gira a 13×15 con la misma semilla, así que todos reciben la misma cocina.
- Constantes: `FUSE = 2.2` s (mecha), `DEATH = 1.1`, `CLEAR = 1.6`, 3 vidas, `DAILY_LEVELS = 5`. El día cambia a medianoche UTC−5.
- Puntos: rata +100, nivel superado +500. Hay `2 + nivel` ratas (máximo 10) y su velocidad es `min(1.5 + 0.15·nivel, 3)`.
- Objetos dentro de cajas: maíz (+1 olla), ají (+1 de alcance), café (más velocidad). La puerta también está bajo una caja.
- **Menú del día**: 5 niveles con semilla `día·1000 + nivel` (mulberry32) y un intento por día y navegador (`localStorage` `chef-daily`). Se comparte como texto en X o como imagen.
- **Ranking y conteo** en Supabase. Proyecto `chef`, ref `frnpeqrldutmjikalmps`, se usa por REST con la llave *publishable*.
  - `scores(day, handle, plates, score, rats)`: el @ de X (`^[A-Za-z0-9_]{1,15}$`) es único por día (409 si se repite).
  - `plays(day, player, mode free|daily, event start|end, plates, rats)`: `player` es un código anónimo de 16 hex (`localStorage` `chef-id`).
  - RLS activo. Si el servicio no responde en 8 s, el juego sigue y solo falta el ranking.
- Bilingüe ES/EN (objeto `TEXT`). Elige el idioma por `navigator.language` y se puede cambiar con botones.
- Respeta `prefers-reduced-motion`.

## Convenciones
- Siempre en **inglés primero y luego español** en todo lo público: README, LICENSE, meta, link de vista previa y textos para compartir.
- Todo texto visible va en las dos lenguas dentro de `TEXT`. Nunca se escribe texto suelto en el HTML.
- Comentarios del código en español, claros y en frases completas.
- Mensajes de commit en inglés, en imperativo y diciendo qué cambia para el jugador (p. ej. "Give the board more room on short phone screens").
- Sin datos personales y sin rastreadores. Solo el código anónimo al azar.
- El juego es gratis y no exige comprar $CHEF. No se promete rentabilidad.
- Probar en PC (teclado) y en celular (cruceta y botón OLLA), y en pantallas bajas.

## Artifacts relacionados (claude.ai)
- Juego (fuente): <https://claude.ai/artifact/ELqTVq3RMRCChJMNN7AUE9>
- Bocetos del gato: <https://claude.ai/artifact/EFYGEfQ3hhgRQQgo2MSwRg>, <https://claude.ai/artifact/JkAL6po7hrhUWaunjETDfw>
- Bocetos de las ratas: <https://claude.ai/artifact/RqjsaaM8fCw34rUceM7D9G>
- Estadísticas $CHEF: <https://claude.ai/artifact/6evXStANnYRwFJLdZab3hV>
- Banner del stream: <https://claude.ai/artifact/LCCj1NTNsr2pang92NJfAJ>

## Flujo con Spec Kit
Primero lee `.specify/memory/constitution.md`. Para una funcionalidad nueva: `/speckit-specify` → (`/speckit-clarify`) → `/speckit-plan` → `/speckit-tasks` → `/speckit-implement`.
