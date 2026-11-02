# Especificación: Juego base (estado actual)

**Rama**: `main` · **Estado**: Implementado · **Fecha**: 2026-11-02

Este documento registra lo que el juego ya hace. Sirve de punto de partida para las próximas especificaciones y para `/speckit-converge`.

## Historias de usuario

### H1 · Juego libre (P1)
Como jugador, abro la página y juego sin instalar nada, nivel tras nivel, hasta perder las 3 vidas.

**Aceptación**
1. Pongo una olla y explota en cruz a los 2,2 s, rompe las cajas que alcanza y elimina a las ratas.
2. Si me toca una llama o una rata, pierdo una vida y el nivel vuelve a empezar. Con 0 vidas veo "Se quemó la cena" con el nivel alcanzado, las ratas eliminadas y los puntos.
3. Cuando no quedan ratas y llego a la puerta que encontré, gano 500 puntos y paso al siguiente nivel.

### H2 · Ingredientes (P1)
Al romper cajas aparecen maíz (+1 olla a la vez), ají (+1 de alcance) y café (más velocidad). Hay uno de cada tipo por nivel.

### H3 · Menú del día (P1)
Cada día (medianoche UTC−5) hay 5 niveles iguales para todos y un solo intento por navegador.

**Aceptación**
1. Dos dispositivos, uno en horizontal y otro en vertical, reciben el mismo mapa ese día.
2. Al terminar veo el ticket con la fila 🍲/🔥/⬜, los puntos, las ratas y la cuenta regresiva para el próximo Menú.
3. Puedo compartir en X, copiar el texto o compartir/guardar una imagen.
4. Si vuelvo a entrar el mismo día, veo mi resultado y no puedo jugarlo otra vez.

### H4 · Ranking del día (P2)
Después del Menú del día escribo mi @ de X y aparezco en el top 10, ordenado por platos, luego puntos y luego por quién llegó primero.

**Aceptación**
1. Un @ que no cumple `^@?\w{1,15}$` muestra un aviso. Si el @ ya está en el ranking del día (409), dice que ya está.
2. Si quedo fuera del top 10, mi resultado sale al final, marcado y sin número de puesto.
3. Si Supabase no responde en 8 s, sale "no disponible" y el juego sigue.

### H5 · Conteo anónimo (P3)
Cada partida envía `start` y `end` (modo, platos, ratas) con un código al azar por navegador. No se guardan datos personales.

### H6 · Idioma y accesibilidad (P1)
El juego está en ES/EN según el dispositivo y se puede cambiar con botones. Tiene etiquetas ARIA y respeta "reducir movimiento".

## Requisitos funcionales
- **RF-001** Tablero de 15×13 con paredes fijas en casillas pares y cajas al azar (50 %), girado a 13×15 en vertical.
- **RF-002** Hay `min(2 + nivel, 10)` ratas con velocidad `min(1.5 + 0.15·nivel, 3)` casillas/s.
- **RF-003** Puntos: +100 por rata y +500 por nivel.
- **RF-004** Controles: flechas/WASD + Espacio. En táctil, cruceta con zona muerta central + botón OLLA.
- **RF-005** Menú del día con semilla `día·1000 + nivel` (mulberry32) y resultado en `localStorage["chef-daily"]`.
- **RF-006** Supabase REST: `GET scores` (top 10), `POST scores`, `POST plays`, todos con un tope de 8 s.
- **RF-007** Todo el texto visible está en `TEXT.es` y `TEXT.en`.

## Entidades
- **scores**: day, handle, plates (0–5), score, rats, created_at.
- **plays**: day, player (16 hex), mode (free|daily), event (start|end), plates, rats, created_at.

## Fuera de alcance (por ahora)
Cuentas de usuario, pagos o compras, multijugador, sonido.
