\# StrimZone Rewards Assets



Estructura oficial para assets publicos de recompensas StrimZone.



\## Regla principal



assets/<tipo>/<tmdbId>/rewards/<rewardId>/<archivo>



\## Tipos permitidos



\- movies: peliculas

\- series: series

\- anime: anime



\## Ejemplo pelicula



assets/movies/105/rewards/bttf\_movie\_1\_delorean\_trophy/trophy.png

assets/movies/105/rewards/bttf\_movie\_1\_delorean\_trophy/background.png

assets/movies/105/rewards/bttf\_movie\_1\_delorean\_trophy/badge.png

assets/movies/105/rewards/bttf\_movie\_1\_delorean\_trophy/model.glb



\## Ejemplo poster



assets/movies/105/rewards/bttf\_movie\_1\_poster/poster.png

assets/movies/105/rewards/bttf\_movie\_1\_poster/background.png



\## Ejemplo serie



assets/series/1399/rewards/got\_iron\_throne\_trophy/trophy.png

assets/series/1399/rewards/got\_iron\_throne\_trophy/background.png



\## Ejemplo anime



assets/anime/37854/rewards/one\_piece\_wanted\_poster/poster.png

assets/anime/37854/rewards/one\_piece\_wanted\_poster/background.png



\## Nombres recomendados dentro de cada recompensa



\- trophy.png

\- poster.png

\- background.png

\- badge.png

\- model.glb



\## Manifest



Cada reward en manifest.json debe apuntar a estas URLs publicas:



https://johntitor-uk.github.io/strimzone-rewards/assets/<tipo>/<tmdbId>/rewards/<rewardId>/<archivo>

