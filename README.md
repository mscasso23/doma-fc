# DOMA FC — estadísticas

Sitio estático con las estadísticas del equipo de fútbol 8.
Publicado con GitHub Pages: https://mscasso23.github.io/doma-fc/

## Cómo funciona

- `index.html` — la app entera (sin dependencias, sin build).
- `data.json` — **todos los datos**: temporadas, plantel y partidos.

Cualquiera que abra el link **lee** `data.json` y ve las estadísticas, sin cuenta ni login.

Para **cargar** partidos hace falta un token de GitHub guardado en el navegador
(botón "Cargar datos" arriba a la derecha). Con el token, la app escribe `data.json`
por la API de GitHub: **cada partido cargado queda como un commit**, así que el
historial de cambios es el historial de git.

El token tiene que ser *fine-grained*, limitado a este repositorio, con
**Contents: Read and write** y nada más. Vive solo en el `localStorage` del navegador
donde lo pegaste; no está en el repo ni viaja a ningún otro lado.

## Modelo de datos

```
{
  "version": 2,
  "seasons": [{ "id", "nombre", "orden", "pjManual": {playerId: n}, "arqueros": {playerId: {pj, gc}} }],
  "players": [{ "id", "nombre", "apodo", "dorsal", "pos", "activo" }],
  "matches": [{ "id", "seasonId", "fecha", "jornada", "rival", "sede", "gf", "gc",
                "goles": [{ "autor", "ec", "asist" }],
                "video", "formacion", "titulares", "suplentes", "arquero",
                "tarjetas", "mvp", "walkover", "notas" }]
}
```

Dos detalles que importan:

- **Cada gol lleva su asistencia apareada** (`{autor, asist}`), no son dos listas
  separadas. `ec: true` = gol en contra del rival (sin autor).
- **PJ y presentismo** salen de las alineaciones (`titulares` + `suplentes`) cuando la
  temporada las tiene cargadas; si no, de `season.pjManual`. El denominador del
  presentismo excluye los partidos con `walkover: true` (el rival no se presentó).

El Apertura 2026 vino del Excel original, que no tenía alineación partido por partido:
por eso usa `pjManual` y `arqueros`.

## Notas

- Después de cargar un partido, el link del equipo tarda hasta ~1 minuto en mostrarlo:
  es el tiempo que GitHub Pages tarda en republicar el sitio. Vos lo ves al instante
  porque leés por la API.
- El repo es público, así que `data.json` es legible por cualquiera. No pongas datos
  personales ahí (no hay DNI ni teléfonos, y conviene que siga así).
