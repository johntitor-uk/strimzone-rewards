# StrimZone Rewards Catalog

Estructura oficial del catalogo remoto de recompensas.

## Regla principal

manifest.json es el indice principal.

Los catalogos reales viven separados por tipo:

- catalog/movies/<tmdbId>.json
- catalog/series/<tmdbId>.json
- catalog/anime/<tmdbId>.json
- catalog/collections/<packId>.json

## Colecciones

Cuando varias peliculas, series o animes comparten recompensas, se usa:

catalog/collections/<packId>.json

Ejemplo:

catalog/collections/pack_back_to_future.json

Ese archivo puede ser usado por:

- movie:105
- movie:165
- movie:196

## Assets

Los archivos visuales viven separados en:

assets/<tipo>/<tmdbId>/<categoria>/<rewardId>/<archivo>

Ejemplo:

assets/movies/105/trophies/bttf_movie_1_delorean_trophy/trophy.png
assets/movies/165/posters/bttf_movie_2_poster/poster.png