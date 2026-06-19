# StrimZone Rewards Assets

Estructura oficial para assets publicos de recompensas StrimZone.

## Regla principal

assets/<tipo>/<tmdbId>/<categoria>/<rewardId>/<archivo>

## Tipos permitidos

- movies: peliculas
- series: series
- anime: anime

## Categorias permitidas

- trophies
- posters
- packs

## Archivos recomendados dentro de cada recompensa

### Trofeos

assets/movies/105/trophies/bttf_movie_1_delorean_trophy/trophy.png
assets/movies/105/trophies/bttf_movie_1_delorean_trophy/background.png
assets/movies/105/trophies/bttf_movie_1_delorean_trophy/badge.png
assets/movies/105/trophies/bttf_movie_1_delorean_trophy/model.glb

### Posters

assets/movies/105/posters/bttf_movie_1_poster/poster.png
assets/movies/105/posters/bttf_movie_1_poster/background.png
assets/movies/105/posters/bttf_movie_1_poster/badge.png

### Packs

assets/movies/105/packs/bttf_saga_pack/cover.png
assets/movies/105/packs/bttf_saga_pack/background.png
assets/movies/105/packs/bttf_saga_pack/badge.png

## Regla importante

No usar carpetas globales como:

assets/trophies
assets/posters
assets/models
assets/badges

Eso mezcla peliculas, series y anime.

Cada asset debe vivir dentro de su contenido:

assets/<tipo>/<tmdbId>/<categoria>/<rewardId>/

## URLs publicas

GitHub Pages sirve los archivos asi:

https://johntitor-uk.github.io/strimzone-rewards/assets/<tipo>/<tmdbId>/<categoria>/<rewardId>/<archivo>

## Ejemplo manifest para trofeo

"assets": {
  "image": "https://johntitor-uk.github.io/strimzone-rewards/assets/movies/105/trophies/bttf_movie_1_delorean_trophy/trophy.png",
  "background": "https://johntitor-uk.github.io/strimzone-rewards/assets/movies/105/trophies/bttf_movie_1_delorean_trophy/background.png",
  "badge": "https://johntitor-uk.github.io/strimzone-rewards/assets/movies/105/trophies/bttf_movie_1_delorean_trophy/badge.png",
  "model": "https://johntitor-uk.github.io/strimzone-rewards/assets/movies/105/trophies/bttf_movie_1_delorean_trophy/model.glb"
}

## Ejemplo manifest para poster

"assets": {
  "poster": "https://johntitor-uk.github.io/strimzone-rewards/assets/movies/105/posters/bttf_movie_1_poster/poster.png",
  "background": "https://johntitor-uk.github.io/strimzone-rewards/assets/movies/105/posters/bttf_movie_1_poster/background.png",
  "badge": "https://johntitor-uk.github.io/strimzone-rewards/assets/movies/105/posters/bttf_movie_1_poster/badge.png"
}