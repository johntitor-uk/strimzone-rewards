\# StrimZone Rewards Catalog Guide



Este repositorio contiene el catalogo remoto de recompensas de StrimZone.



La app lee `manifest.json` desde GitHub Pages y despues carga uno o varios archivos JSON de catalogo desde las rutas indicadas dentro de `catalogs`.



Este archivo existe para que cualquier ChatGPT, Codex o desarrollador que toque este repositorio entienda como agregar packs, sagas, eventos y premios individuales sin romper el sistema.



\---



\# Reglas de oro



\## 1. No renombrar IDs existentes



No cambies IDs que ya hayan sido usados en produccion.



No renombrar:



\- pack ids

\- reward ids

\- collection ids

\- content ids



Cloudflare guarda el estado de usuario usando estos IDs.  

Si cambias un ID, una recompensa ya reclamada puede aparecer como una recompensa nueva.



Ejemplos de IDs que no se deben cambiar:



```txt

pack\_back\_to\_future

bttf\_movie\_1\_xp

bttf\_movie\_2\_poster

bttf\_movie\_1\_delorean\_trophy

bttf\_mega\_delorean\_trophy

bttf\_mythic\_saga\_poster

```



Si necesitas cambiar el titulo, descripcion, assets o rareza, puedes hacerlo.  

Pero el `id` debe mantenerse estable.



\---



\# Estructura principal del repo



Usar esta estructura:



```txt

catalog/

&#x20; collections/

&#x20;   movie\_sagas/

&#x20;   series\_sagas/

&#x20;   anime\_sagas/

&#x20;   events/



&#x20; standalone/

&#x20;   movies/

&#x20;   series/

&#x20;   anime/



assets/

&#x20; collections/

&#x20; movies/

&#x20; series/

&#x20; anime/

```



\---



\# Diferencia entre collections y standalone



\## Collections



Usar `catalog/collections` para:



\- packs

\- sagas

\- colecciones grandes

\- eventos

\- recompensas finales de saga

\- recompensas que comparten un mismo `collectionId`



Ejemplos:



```txt

catalog/collections/movie\_sagas/pack\_back\_to\_future.json

catalog/collections/movie\_sagas/pack\_harry\_potter.json

catalog/collections/series\_sagas/pack\_breaking\_bad.json

catalog/collections/anime\_sagas/pack\_attack\_on\_titan.json

catalog/collections/events/event\_halloween\_2026.json

```



Los rewards que pertenecen a un pack deben tener `collectionId`.



Ejemplo:



```json

{

&#x20; "id": "bttf\_mega\_delorean\_trophy",

&#x20; "type": "mega\_trophy",

&#x20; "title": "Mega Trofeo DeLorean Legendario",

&#x20; "description": "Mira Volver al Futuro I, II y III para desbloquear el trofeo legendario de la saga.",

&#x20; "rarity": "legendary",

&#x20; "collectionId": "pack\_back\_to\_future",

&#x20; "unlockRule": {

&#x20;   "type": "saga\_completed",

&#x20;   "contentIds": \[

&#x20;     "movie:105",

&#x20;     "movie:165",

&#x20;     "movie:196"

&#x20;   ],

&#x20;   "collectionId": "pack\_back\_to\_future",

&#x20;   "requiresClaim": true

&#x20; },

&#x20; "assets": {

&#x20;   "image": "https://johntitor-uk.github.io/strimzone-rewards/assets/movies/105/trophies/bttf\_mega\_delorean\_trophy/trophy.png",

&#x20;   "background": "https://johntitor-uk.github.io/strimzone-rewards/assets/movies/105/trophies/bttf\_mega\_delorean\_trophy/background.png",

&#x20;   "model": "https://johntitor-uk.github.io/strimzone-rewards/assets/movies/105/trophies/bttf\_mega\_delorean\_trophy/model.glb"

&#x20; },

&#x20; "sortOrder": 30,

&#x20; "mediaType": "movie",

&#x20; "tmdbId": 105

}

```



\---



\## Standalone



Usar `catalog/standalone` para recompensas individuales que no pertenecen a un pack.



Ejemplos:



```txt

catalog/standalone/movies/105.json

catalog/standalone/movies/671.json

catalog/standalone/series/1396/base.json

catalog/standalone/series/1396/season\_1.json

catalog/standalone/series/1396/s01e01.json

catalog/standalone/anime/1429/base.json

catalog/standalone/anime/1429/season\_1.json

catalog/standalone/anime/1429/s01e01.json

```



Los rewards standalone normalmente NO deben tener `collectionId`.



Usar `unlockRule.contentId` para conectar el premio con la pelicula, serie, temporada o episodio.



Ejemplo:



```json

{

&#x20; "id": "movie\_105\_watch\_xp",

&#x20; "type": "xp",

&#x20; "title": "XP del primer viaje temporal",

&#x20; "description": "Mira Volver al Futuro para ganar XP.",

&#x20; "rarity": "common",

&#x20; "mediaType": "movie",

&#x20; "tmdbId": 105,

&#x20; "xpAmount": 150,

&#x20; "unlockRule": {

&#x20;   "type": "watch\_progress",

&#x20;   "minimumProgress": 0.9,

&#x20;   "contentId": "movie:105",

&#x20;   "requiresClaim": true

&#x20; },

&#x20; "assets": {},

&#x20; "sortOrder": 10

}

```



\---



\# Formato de contentId



Usar estos formatos:



```txt

movie:<tmdbId>

series:<tmdbId>

series:<tmdbId>:s<seasonNumber>

series:<tmdbId>:s<seasonNumber>:e<episodeNumber>

anime:<tmdbId>

anime:<tmdbId>:s<seasonNumber>

anime:<tmdbId>:s<seasonNumber>:e<episodeNumber>

```



Ejemplos:



```txt

movie:105

series:1396

series:1396:s1

series:1396:s1:e1

anime:1429

anime:1429:s1

anime:1429:s1:e1

```



Reglas:



\- Usar `movie` para peliculas.

\- Usar `series` para series normales.

\- Usar `anime` para anime si se quiere separar visualmente o por catalogo.

\- Para temporadas usar `s1`, `s2`, `s3`.

\- Para episodios usar `s1:e1`, `s1:e2`, etc.



\---



\# manifest.json



`manifest.json` es el indice principal.



La app lee este archivo primero y despues carga los catalogos indicados.



Cada entrada de pelicula, serie o anime puede apuntar a un solo catalogo o a varios catalogos.



El formato antiguo con string es valido:



```json

{

&#x20; "catalogs": {

&#x20;   "movies": {

&#x20;     "105": "catalog/collections/movie\_sagas/pack\_back\_to\_future.json"

&#x20;   }

&#x20; }

}

```



Pero el formato recomendado es array:



```json

{

&#x20; "catalogs": {

&#x20;   "movies": {

&#x20;     "105": \[

&#x20;       "catalog/collections/movie\_sagas/pack\_back\_to\_future.json",

&#x20;       "catalog/standalone/movies/105.json"

&#x20;     ]

&#x20;   }

&#x20; }

}

```



Usar array para todo contenido nuevo.



\---



\# Ejemplo completo de manifest.json



```json

{

&#x20; "schemaVersion": 1,

&#x20; "updatedAt": "2026-07-02T22:04:27Z",

&#x20; "catalogs": {

&#x20;   "movies": {

&#x20;     "105": \[

&#x20;       "catalog/collections/movie\_sagas/pack\_back\_to\_future.json",

&#x20;       "catalog/standalone/movies/105.json"

&#x20;     ],

&#x20;     "165": \[

&#x20;       "catalog/collections/movie\_sagas/pack\_back\_to\_future.json"

&#x20;     ],

&#x20;     "196": \[

&#x20;       "catalog/collections/movie\_sagas/pack\_back\_to\_future.json"

&#x20;     ]

&#x20;   },

&#x20;   "series": {

&#x20;     "1396": \[

&#x20;       "catalog/collections/series\_sagas/pack\_breaking\_bad.json",

&#x20;       "catalog/standalone/series/1396/base.json"

&#x20;     ]

&#x20;   },

&#x20;   "anime": {

&#x20;     "1429": \[

&#x20;       "catalog/collections/anime\_sagas/pack\_attack\_on\_titan.json",

&#x20;       "catalog/standalone/anime/1429/base.json"

&#x20;     ]

&#x20;   },

&#x20;   "collections": {

&#x20;     "pack\_back\_to\_future": "catalog/collections/movie\_sagas/pack\_back\_to\_future.json",

&#x20;     "pack\_breaking\_bad": "catalog/collections/series\_sagas/pack\_breaking\_bad.json",

&#x20;     "pack\_attack\_on\_titan": "catalog/collections/anime\_sagas/pack\_attack\_on\_titan.json"

&#x20;   }

&#x20; }

}

```



\---



\# Formato de archivo de catalogo



Cada archivo JSON de catalogo debe tener esta forma:



```json

{

&#x20; "schemaVersion": 1,

&#x20; "catalogType": "collection",

&#x20; "id": "pack\_example",

&#x20; "title": "Pack Example",

&#x20; "description": "Description here.",

&#x20; "updatedAt": "2026-07-02T22:04:27Z",

&#x20; "contentIds": \[

&#x20;   "movie:105"

&#x20; ],

&#x20; "packs": \[],

&#x20; "rewards": \[]

}

```



Usar:



```txt

catalogType: "collection"

```



para packs, sagas, colecciones y eventos.



Usar:



```txt

catalogType: "standalone"

```



para recompensas individuales fuera de packs.



\---



\# Reglas para packs



Un pack debe tener:



```json

{

&#x20; "id": "pack\_example",

&#x20; "title": "Pack Example",

&#x20; "description": "Completa el contenido requerido para desbloquear recompensas.",

&#x20; "type": "movie\_saga",

&#x20; "requiredContentIds": \[

&#x20;   "movie:105"

&#x20; ],

&#x20; "finalRewardIds": \[

&#x20;   "example\_final\_reward"

&#x20; ],

&#x20; "assets": {

&#x20;   "image": "",

&#x20;   "background": ""

&#x20; },

&#x20; "sortOrder": 10

}

```



\## Campos importantes de pack



\### id



Debe ser unico y estable.



Ejemplos:



```txt

pack\_back\_to\_future

pack\_harry\_potter

pack\_breaking\_bad

pack\_attack\_on\_titan

event\_halloween\_2026

```



\### type



Valores recomendados:



```txt

movie\_saga

series\_saga

anime\_saga

event

collection

limited\_event

```



\### requiredContentIds



Contenido necesario para completar el pack.



Ejemplo:



```json

"requiredContentIds": \[

&#x20; "movie:105",

&#x20; "movie:165",

&#x20; "movie:196"

]

```



\### finalRewardIds



Rewards finales del pack.



Ejemplo:



```json

"finalRewardIds": \[

&#x20; "bttf\_mega\_delorean\_trophy",

&#x20; "bttf\_mythic\_saga\_poster"

]

```



\---



\# Reglas para rewards de pack



Los rewards de pack deben tener:



```json

"collectionId": "pack\_back\_to\_future"

```



Y si el reward final depende de completar toda la saga:



```json

"unlockRule": {

&#x20; "type": "saga\_completed",

&#x20; "contentIds": \[

&#x20;   "movie:105",

&#x20;   "movie:165",

&#x20;   "movie:196"

&#x20; ],

&#x20; "collectionId": "pack\_back\_to\_future",

&#x20; "requiresClaim": true

}

```



Si el reward depende de ver una pelicula, episodio o temporada:



```json

"unlockRule": {

&#x20; "type": "watch\_progress",

&#x20; "minimumProgress": 0.9,

&#x20; "contentId": "movie:105",

&#x20; "requiresClaim": true

}

```



\---



\# Reglas para standalone rewards



Los rewards standalone:



\- no deben tener `collectionId`

\- deben tener `mediaType`

\- deben tener `tmdbId`

\- deben tener `unlockRule.contentId`

\- deben estar dentro de `catalog/standalone`



Ejemplo de XP individual:



```json

{

&#x20; "id": "movie\_105\_watch\_xp",

&#x20; "type": "xp",

&#x20; "title": "XP del primer viaje temporal",

&#x20; "description": "Mira Volver al Futuro para ganar XP.",

&#x20; "rarity": "common",

&#x20; "mediaType": "movie",

&#x20; "tmdbId": 105,

&#x20; "xpAmount": 150,

&#x20; "unlockRule": {

&#x20;   "type": "watch\_progress",

&#x20;   "minimumProgress": 0.9,

&#x20;   "contentId": "movie:105",

&#x20;   "requiresClaim": true

&#x20; },

&#x20; "assets": {},

&#x20; "sortOrder": 10

}

```



Ejemplo de trofeo individual:



```json

{

&#x20; "id": "movie\_105\_delorean\_trophy",

&#x20; "type": "trophy",

&#x20; "title": "Trofeo DeLorean 1985",

&#x20; "description": "Mira Volver al Futuro para desbloquear este trofeo.",

&#x20; "rarity": "legendary",

&#x20; "mediaType": "movie",

&#x20; "tmdbId": 105,

&#x20; "unlockRule": {

&#x20;   "type": "watch\_progress",

&#x20;   "minimumProgress": 0.9,

&#x20;   "contentId": "movie:105",

&#x20;   "requiresClaim": true

&#x20; },

&#x20; "assets": {

&#x20;   "image": "https://johntitor-uk.github.io/strimzone-rewards/assets/movies/105/trophies/movie\_105\_delorean\_trophy/trophy.png",

&#x20;   "background": "https://johntitor-uk.github.io/strimzone-rewards/assets/movies/105/trophies/movie\_105\_delorean\_trophy/background.png",

&#x20;   "model": "https://johntitor-uk.github.io/strimzone-rewards/assets/movies/105/trophies/movie\_105\_delorean\_trophy/model.glb"

&#x20; },

&#x20; "sortOrder": 20

}

```



\---



\# Reward types



Tipos soportados:



```txt

xp

poster

trophy

mega\_trophy

pack

```



Usar:



```txt

xp

```



para experiencia.



Usar:



```txt

poster

```



para posters desbloqueables.



Usar:



```txt

trophy

```



para trofeos normales.



Usar:



```txt

mega\_trophy

```



para recompensas grandes/finales de saga.



\---



\# Rarity values



Rarezas soportadas:



```txt

common

rare

epic

legendary

mythic

```



Orden recomendado:



```txt

common < rare < epic < legendary < mythic

```



\---



\# Unlock rule types



Tipos de unlockRule recomendados:



```txt

watch\_progress

content\_completed

saga\_completed

event\_active

manual

```



\## watch\_progress



Para rewards que se desbloquean al ver cierto porcentaje de contenido.



```json

"unlockRule": {

&#x20; "type": "watch\_progress",

&#x20; "minimumProgress": 0.9,

&#x20; "contentId": "movie:105",

&#x20; "requiresClaim": true

}

```



\## saga\_completed



Para rewards que se desbloquean al completar varios contenidos.



```json

"unlockRule": {

&#x20; "type": "saga\_completed",

&#x20; "contentIds": \[

&#x20;   "movie:105",

&#x20;   "movie:165",

&#x20;   "movie:196"

&#x20; ],

&#x20; "collectionId": "pack\_back\_to\_future",

&#x20; "requiresClaim": true

}

```



\## manual



Para rewards que se activan manualmente o desde backend.



```json

"unlockRule": {

&#x20; "type": "manual",

&#x20; "requiresClaim": true

}

```



\---



\# Assets



Usar URLs publicas de GitHub Pages.



Base URL:



```txt

https://johntitor-uk.github.io/strimzone-rewards/

```



\## Paths recomendados para assets de packs



```txt

assets/collections/<packId>/posters/<rewardId>/poster.png

assets/collections/<packId>/trophies/<rewardId>/trophy.png

assets/collections/<packId>/backgrounds/<rewardId>/background.png

```



Ejemplo:



```txt

assets/collections/pack\_back\_to\_future/posters/bttf\_mythic\_saga\_poster/poster.png

```



\## Paths recomendados para assets individuales de peliculas



```txt

assets/movies/<tmdbId>/posters/<rewardId>/poster.png

assets/movies/<tmdbId>/trophies/<rewardId>/trophy.png

assets/movies/<tmdbId>/badges/<rewardId>/badge.png

```



Ejemplo:



```txt

assets/movies/105/trophies/movie\_105\_delorean\_trophy/trophy.png

```



\## Paths recomendados para assets individuales de series



```txt

assets/series/<tmdbId>/posters/<rewardId>/poster.png

assets/series/<tmdbId>/trophies/<rewardId>/trophy.png

assets/series/<tmdbId>/badges/<rewardId>/badge.png

```



\## Paths recomendados para assets individuales de anime



```txt

assets/anime/<tmdbId>/posters/<rewardId>/poster.png

assets/anime/<tmdbId>/trophies/<rewardId>/trophy.png

assets/anime/<tmdbId>/badges/<rewardId>/badge.png

```



\---



\# Como agregar un nuevo pack



Pasos:



1\. Crear el archivo JSON dentro de la carpeta correcta.

2\. Si es saga de peliculas, usar `catalog/collections/movie\_sagas`.

3\. Si es saga de series, usar `catalog/collections/series\_sagas`.

4\. Si es saga de anime, usar `catalog/collections/anime\_sagas`.

5\. Si es evento, usar `catalog/collections/events`.

6\. Agregar el pack dentro de `packs`.

7\. Agregar los rewards dentro de `rewards`.

8\. Asegurar que los rewards de pack tengan `collectionId`.

9\. Asegurar que los rewards finales esten en `finalRewardIds`.

10\. Agregar la ruta del catalogo en `manifest.json`.

11\. Validar JSON.

12\. Hacer commit y push.



\---



\# Como agregar premios individuales



Pasos:



1\. Crear el archivo JSON dentro de `catalog/standalone`.

2\. Dejar `packs` vacio.

3\. Agregar rewards dentro de `rewards`.

4\. No usar `collectionId` si no pertenece a un pack.

5\. Usar `unlockRule.contentId`.

6\. Agregar la ruta en `manifest.json`.

7\. Validar JSON.

8\. Hacer commit y push.



Ejemplo de standalone catalog:



```json

{

&#x20; "schemaVersion": 1,

&#x20; "catalogType": "standalone",

&#x20; "id": "movie\_105\_standalone\_rewards",

&#x20; "title": "Premios individuales de Volver al Futuro",

&#x20; "description": "Recompensas individuales para Volver al Futuro.",

&#x20; "updatedAt": "2026-07-02T22:04:27Z",

&#x20; "contentIds": \[

&#x20;   "movie:105"

&#x20; ],

&#x20; "packs": \[],

&#x20; "rewards": \[

&#x20;   {

&#x20;     "id": "movie\_105\_watch\_xp",

&#x20;     "type": "xp",

&#x20;     "title": "XP del primer viaje temporal",

&#x20;     "description": "Mira Volver al Futuro para ganar XP.",

&#x20;     "rarity": "common",

&#x20;     "mediaType": "movie",

&#x20;     "tmdbId": 105,

&#x20;     "xpAmount": 150,

&#x20;     "unlockRule": {

&#x20;       "type": "watch\_progress",

&#x20;       "minimumProgress": 0.9,

&#x20;       "contentId": "movie:105",

&#x20;       "requiresClaim": true

&#x20;     },

&#x20;     "assets": {},

&#x20;     "sortOrder": 10

&#x20;   }

&#x20; ]

}

```



Y en `manifest.json`:



```json

"movies": {

&#x20; "105": \[

&#x20;   "catalog/collections/movie\_sagas/pack\_back\_to\_future.json",

&#x20;   "catalog/standalone/movies/105.json"

&#x20; ]

}

```



\---



\# Comandos de validacion



Validar `manifest.json`:



```cmd

powershell -NoProfile -Command "Get-Content manifest.json -Raw | ConvertFrom-Json | Out-Null"

```



Validar un catalogo:



```cmd

powershell -NoProfile -Command "Get-Content 'catalog\\collections\\movie\_sagas\\pack\_back\_to\_future.json' -Raw | ConvertFrom-Json | Out-Null"

```



Validar un standalone:



```cmd

powershell -NoProfile -Command "Get-Content 'catalog\\standalone\\movies\\105.json' -Raw | ConvertFrom-Json | Out-Null"

```



Revisar espacios raros:



```cmd

git diff --check

```



Revisar estado:



```cmd

git status --short

```



\---



\# Comandos para probar GitHub Pages



Ver manifest publicado:



```cmd

powershell -NoProfile -Command "Invoke-WebRequest 'https://johntitor-uk.github.io/strimzone-rewards/manifest.json' -UseBasicParsing | Select-Object -ExpandProperty Content"

```



Ver si un catalogo existe:



```cmd

powershell -NoProfile -Command "Invoke-WebRequest 'https://johntitor-uk.github.io/strimzone-rewards/catalog/collections/movie\_sagas/pack\_back\_to\_future.json' -UseBasicParsing | Select-Object -ExpandProperty StatusCode"

```



Debe devolver:



```txt

200

```



\---



\# Estilo de commits



Usar mensajes claros:



```txt

Add Harry Potter reward pack

Add movie 105 standalone rewards

Add Breaking Bad reward pack

Add Attack on Titan anime rewards

Add Halloween 2026 reward event

Update reward manifest

Organize reward catalogs

```



\---



\# Regla final



Si el reward pertenece a una saga, pack o evento:



```txt

usar catalog/collections

usar collectionId

```



Si el reward es individual:



```txt

usar catalog/standalone

no usar collectionId

usar unlockRule.contentId

```



Usar arrays en `manifest.json` para todo contenido nuevo.

