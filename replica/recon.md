# Mapa de recon: Spotify – Música y pódcasts (iOS)

Alcance: el ciclo principal de escucha en iPhone. Encontrar audio (inicio, búsqueda, explorar), reproducirlo (minirreproductor, pantalla de reproducción, cola) y guardarlo (biblioteca, playlists, descargas). Los pódcasts entran como segundo tipo de contenido. Quedan fuera de esta parte: audiolibros, vídeo, eventos en directo, DJ/IA, Wrapped, el hardware de Connect y la gestión de planes Familiar/Duo.
Para: **supuesto, por confirmar.** Una app de música o audio para un catálogo que el usuario puede emitir legalmente: su propio sello, artistas independientes, música libre de derechos, una iglesia, un colegio o una red de pódcasts. Un clon no puede tener un catálogo comercial (ver Fuera de alcance).
Fecha: 2026-10-27

Alcance propuesto, por confirmar: **iOS · ciclo principal de escucha · catálogo propio o con licencia.**

## Fuentes

| # | fuente | URL | notas |
| --- | --- | --- | --- |
| 1 | Ficha del App Store (ES) | https://apps.apple.com/es/app/spotify-m%C3%BAsica-y-p%C3%B3dcasts/id324684580 | 4,7★ con ~3,4 M de valoraciones, 325 MB, iOS 16.1+, v9.1.94. Cuenta la propuesta: Descubrimiento Semanal, Radar de Novedades, playlists colaborativas, Connect, Jam, DJ, Wrapped. Las capturas solo aparecen como imágenes de relleno, así que no se obtuvieron |
| 2 | Página de precios (ES) | https://www.spotify.com/es/premium/ | Individual 11,99 €, Estudiantes 6,49 €, Duo 16,99 €, Familiar 20,99 €. De pago: sin anuncios, sin conexión, saltos ilimitados, reproducir en orden, sin pérdida, 12 h de audiolibros |
| 3 | Centro de ayuda (ES) | https://support.spotify.com/es/ | Taxonomía completa de funciones: playlists (carpetas, colaborativas, portada, mixtas), cola, aleatorio, radio, letras, archivos locales, ocultar canciones, ordenar y filtrar, sesión privada, calidad de audio, fundido, ecualizador, normalización, reproducción automática, sin conexión |
| 4 | Documentación pública de la Web API | https://developer.spotify.com/documentation/web-api | Modelo de datos: Album, Artist, Track, Playlist, Show, Episode, Audiobook, Chapter, Category, User, Player (estado, dispositivos, cola, escuchado recientemente), guardar/quitar en biblioteca |
| 5 | Free vs Premium (terceros) | https://www.soundguys.com/free-spotify-vs-spotify-premium-36632/ | Free en móvil: solo aleatorio con ~6 saltos por hora, anuncios, menor calidad. No es fuente primaria |
| 6 | Cambio en Free a la carta (prensa) | https://primetel.com.cy/spotify-finally-lets-free-users-pick-and-play-songs-7824 | Dice que los usuarios Free ya pueden elegir y reproducir canciones a la carta. Sin confirmar por fuente primaria |
| 7 | Rediseño de Tu biblioteca (comunidad + prensa) | https://community.spotify.com/t5/Your-Library/New-Your-Library-view-iOS-amp-Android/m-p/5196161 · https://www.androidcentral.com/spotify-your-library-ui-update-dynamic-filters | Una sola lista con chips de filtro, orden (recientes, añadido recientemente, A–Z, creador, personalizado), hasta 4 fijados. De 2021, hay que comprobarlo en la app actual |
| 8 | la cuenta del propio usuario | — | **Aún no usada.** Siguiente pasada: capturas en `replica/screens/` desde el teléfono del propio usuario, a ritmo humano |

Nota sobre los términos: los Términos para desarrolladores de Spotify prohíben usar la API para crear un servicio de streaming competidor. Aquí la documentación de la API solo se usa para leer el modelo de datos. No llames a la API para llenar el clon.

## Ciclo principal

Abres la app, recibes una portada personalizada con cosas para escuchar, tocas una, suena en segundo plano con un minirreproductor siempre visible, y guardas lo que te gusta en tu biblioteca o en una playlist para volver a ello.

## Pantallas

| ID | pantalla | ruta / cómo se llega | propósito | componentes clave | estados vistos |
| --- | --- | --- | --- | --- | --- |
| S01 | Bienvenida / iniciar sesión | primer arranque | registrarse o entrar | logo, botones primario/secundario, botones de inicio con redes | por defecto, cargando, error |
| S02 | Registro | S01 → Regístrate gratis | email, contraseña, fecha de nacimiento, género, nombre, términos | campo de texto, selector de fecha, elección segmentada, casilla, pasos | en cada paso: vacío, inválido, enviando |
| S03 | Onboarding de gustos | tras S02 | elegir artistas para alimentar las recomendaciones | campo de búsqueda, cuadrícula de círculos de artista (selección múltiple), Hecho | vacío, seleccionados (n), cargando |
| S04 | Inicio | pestaña 1 | portada personalizada | barra superior (avatar, chips Todo/Música/Pódcasts), cuadrícula 2×4 de recientes, estantes horizontales de tarjetas, cabeceras de sección | esqueleto de carga, lleno, sin conexión (solo descargas), usuario nuevo (estantes genéricos) |
| S05 | Buscar / Explorar | pestaña 2 | entrada a la búsqueda y a explorar por género | campo de búsqueda, cuadrícula de mosaicos de color "Explorar todo" | por defecto |
| S06 | Resultados de búsqueda | S05 → escribir | encontrar canciones, artistas, álbumes, playlists, pódcasts | campo de consulta, chips de tipo, tarjeta de resultado principal, filas de canción, búsquedas recientes | búsquedas recientes, escribiendo (autocompletado), resultados, sin resultados, sin conexión |
| S07 | Página de género / categoría | S05 → mosaico | playlists seleccionadas de un género | cabecera, estantes de tarjetas de playlist | cargando, lleno |
| S08 | Tu biblioteca | pestaña 3 | todo lo guardado | chips de filtro (Playlists, Artistas, Álbumes, Pódcasts, Descargado), hoja de orden, cambio lista/cuadrícula, fijados, entrada "Canciones que te gustan", + crear | vacía (usuario nuevo), llena, filtro vacío, sin conexión |
| S09 | Detalle de playlist | tarjeta o fila de biblioteca | ver y reproducir una playlist | portada, título, propietario, duración, botón reproducir/aleatorio, interruptor de descarga, añadir/más, filas de canción, recomendaciones "Añadir a esta playlist" | cargando, llena, playlist vacía (propia), sin conexión (en gris), descargando |
| S10 | Detalle de álbum | tarjeta, página de artista | ver y reproducir un álbum | portada, artista, año, filas numeradas, corazón de guardar, reproducir/aleatorio | lleno, etiquetas de explícito |
| S11 | Página de artista | tocar un artista | centro del artista | imagen principal, botón seguir, oyentes mensuales, Populares (top 5), estantes de discografía, información, "a los fans también les gusta" | lleno, seguir / dejar de seguir |
| S12 | Canciones que te gustan | S08 → Canciones que te gustan | playlist automática de canciones guardadas | como S09 más chips de orden y de filtro por género | vacía, llena |
| S13 | Reproduciendo (reproductor completo) | tocar el minirreproductor | controlar la reproducción | portada o canvas, título/artista, me gusta, barra de progreso, anterior/reproducir/siguiente, aleatorio, repetir, selector de dispositivo, compartir, cola, tarjeta de letra, tarjeta del artista, créditos | reproduciendo, en pausa, cargando, anuncio (free), error |
| S14 | Cola | S13 → icono de cola | ver y editar lo siguiente | fila "Reproduciendo", "Siguiente en la cola" (añadido por el usuario), "Siguiente de: <contexto>", asas de arrastre, quitar, barra aleatorio/repetir | cola vacía, llena, reordenando |
| S15 | Letra a pantalla completa | S13 → tarjeta de letra | letra sincronizada | fondo de color, línea resaltada, desplazamiento | sincronizada, sin sincronizar, no disponible |
| S16 | Menú contextual de canción (hoja) | ••• en cualquier fila | acciones sobre una canción | hoja inferior: me gusta, añadir a playlist, añadir a la cola, ir al álbum/artista, compartir, ocultar, créditos, temporizador (desde el reproductor) | — |
| S17 | Selector de playlist | S16 → Añadir a playlist | elegir destino | botón Nueva playlist, búsqueda, lista de playlists propias, selección múltiple, Hecho | — |
| S18 | Crear / editar playlist | S08 + o S17 → Nueva | nombre, luego editar | campo de nombre, vista de edición: cambiar portada, título, descripción, reordenar, quitar | creando, guardada |
| S19 | Página de pódcast | búsqueda o tarjeta | centro del programa | portada, seguir, información, lista de episodios, orden | lleno |
| S20 | Página de episodio | programa → episodio | reproducir un episodio | descripción, reproducir, barra de progreso, descargar, marcar como escuchado | sin escuchar, a medias, escuchado |
| S21 | Selector de dispositivo (Connect) | icono de dispositivo en S13 | salida de audio | lista de dispositivos, este teléfono, Bluetooth/AirPlay | un dispositivo, varios |
| S22 | Ajustes | avatar → Ajustes | preferencias | listas agrupadas: cuenta, contenido (explícito, reproducción automática), reproducción (fundido, sin pausas, normalización), calidad de audio, almacenamiento, ahorro de datos, privacidad, notificaciones, cerrar sesión | — |
| S23 | Perfil | avatar → Ver perfil | la cara pública | avatar, nombre, seguidores/seguidos, playlists públicas, artistas escuchados recientemente | propio, de otro usuario |
| S24 | Venta / muro de pago | acción bloqueada (free) | convertir a premium | tarjetas de plan, lista de ventajas, llamada a la acción | — |
| — | Minirreproductor (persistente) | sobre la barra de pestañas en toda la app | reproducción siempre a mano | portada, título/artista, dispositivo, me gusta, reproducir/pausa, progreso fino | oculto (nada reproducido), reproduciendo, en pausa |

Capturas de cada pantalla: pendientes de hacer desde el dispositivo del propio usuario en `replica/screens/S##.png`.

## Flujos

```
F01 Usuario nuevo llega a la primera reproducción
    S01 -> S02 (4 pasos) -> S03 -> S04 -> tocar tarjeta -> S09 -> reproducir
    clics del camino feliz: ~10 (sin contar los campos del registro)
    casos límite: email ya usado, fecha de nacimiento de menor, saltarse los gustos, sin red

F02 Buscar una canción y reproducirla
    S04 -> S05 -> S06 (escribir) -> tocar fila de canción -> suena, aparece el minirreproductor
    clics del camino feliz: 3 + escribir
    casos límite: errata / sin resultados, sin conexión, explícito filtrado, en free se abre un contexto aleatorio

F03 Guardar una canción y encontrarla después
    S13 me gusta (+) -> S08 -> S12 -> tocar canción
    clics del camino feliz: 1 para guardar, 3 para volver a ponerla
    casos límite: quitar el me gusta desde otra pantalla, me gusta sin conexión (sincroniza después)

F04 Crear una playlist y llenarla
    S08 + -> S18 nombre -> S09 (vacía) -> añadir canciones buscando dentro de la playlist / S16 -> S17
    clics del camino feliz: 4 para crear, 3 por canción añadida
    casos límite: aviso de canción duplicada, nombre vacío, reordenar, borrar playlist (aviso con deshacer)

F05 Controlar lo que suena después
    ••• en cualquier fila -> S16 Añadir a la cola -> S13 -> S14 reordenar / quitar
    clics del camino feliz: 2 para encolar, 4 para reordenar
    casos límite: la cola se vacía al empezar otro contexto, aleatorio activado, repetir una

F06 Escuchar sin conexión (premium)
    interruptor de descarga en S09 -> progreso -> modo avión -> filtro Descargado en S08 -> reproducir
    clics del camino feliz: 1 para descargar, 2 para encontrarlo
    casos límite: almacenamiento lleno, descarga con datos móviles desactivada, recomprobación de sesión a los 30 días, free -> S24

F07 Seguir y escuchar un pódcast, retomarlo después
    S06 -> S19 seguir -> S20 reproducir -> salir a medias -> S04 "seguir escuchando" -> retoma en la posición
    clics del camino feliz: 4
    casos límite: episodio marcado como escuchado, aviso de episodio nuevo, velocidad de reproducción

F08 Explorar un artista
    nombre del artista en S13 -> S11 -> seguir -> reproducir Populares / abrir álbum S10
    clics del camino feliz: 2-3

F09 Compartir una playlist o canción
    S16 Compartir -> hoja de compartir de iOS / copiar enlace -> el destinatario abre el enlace -> S09
    casos límite: playlist privada, destinatario sin sesión iniciada

F10 Cambiar ajustes de reproducción
    avatar en S04 -> S22 -> fundido / calidad / interruptor de explícito
    clics del camino feliz: 3
```

## Componentes

| componente | variantes | estados | se usa en |
| --- | --- | --- | --- |
| Botón | píldora primaria (color de acento), contorno secundario, texto, icono | por defecto, pulsado, desactivado, cargando | S01, S02, S11, S24 |
| Botón flotante de reproducir | reproducir, pausa, con marca de aleatorio | por defecto, pulsado, cargando | S09, S10, S11, S12, S19 |
| Chip de filtro | simple, múltiple, con cerrar (×) | apagado, encendido | S04, S06, S08, S12 |
| Tarjeta | cuadrada (álbum/playlist), círculo (artista), ancha (episodio), mosaico de recientes | por defecto, pulsada, indicador de reproduciendo | S04, S07, S11, S06 |
| Estante | fila de desplazamiento horizontal con cabecera + "Mostrar todo" | cargando, lleno | S04, S07, S11 |
| Fila de canción | con portada / numerada (álbum) / fila de episodio | por defecto, reproduciendo (texto de acento + barras de ecualizador), en gris (no disponible/sin conexión), etiqueta de explícito, punto de descargada | S06, S09–S12, S14 |
| Menú de hoja inferior | lista de acciones con cabecera (portada+título) | abierta, cerrándose | S16, hoja de orden, S21 |
| Minirreproductor | — | reproduciendo, en pausa, cargando, en otro dispositivo | global |
| Barra de progreso | fina (mini), completa (S13) | quieta, arrastrando, en búfer | S13, mini, S20 |
| Controles de transporte | aleatorio, anterior, reproducir/pausa, siguiente, repetir (no/todo/una) | activo, desactivado (free) | S13 |
| Corazón / añadir | — | apagado, encendido (acento), animando | S10, S13, filas |
| Interruptor de descarga | — | apagado, en cola, descargando (anillo), hecho | S09, S10, S20 |
| Campo de búsqueda | fijo (S05), activo con cancelar | vacío, escribiendo, lleno | S05, S06, S17 |
| Barra de pestañas | Inicio, Buscar, Tu biblioteca (+ Premium/Crear) | seleccionada | global |
| Aviso (toast) | informativo, con acción Deshacer / "Cambiar" | visible, desapareciendo | añadir a playlist, me gusta, borrar |
| Esqueleto de carga | tarjeta, fila, cabecera | — | S04, S09 |
| Cabecera grande | degradado del color de la portada, se pliega en el título de navegación al desplazar | expandida, plegada | S09–S12, S19 |
| Fila de ajustes | interruptor, flecha, deslizador (fundido), segmentado (calidad) | — | S22 |
| Estado vacío | icono + titular + llamada a la acción | — | S08, S09, S12, S06 |
| Modal / alerta | confirmar borrado, venta | — | S18, S24 |

## Modelo de datos inferido

Los nombres de entidades y campos se dejan en inglés porque pasan tal cual al esquema de replica-architect.

```
User         id, email, display_name, avatar_url, birthdate, country, plan (free | premium | family | duo | student),
             explicit_allowed, created_at
             evidencia: campos de S02, S22, ayuda "Gestionar tu cuenta", API Users
             confianza: alta

Artist       id, name, image_url, bio, genres[], monthly_listeners (derivado), verified
             evidencia: S11, API Artists
             confianza: alta

Album        id, title, artist_ids[], cover_url, release_date, type (album | single | ep | compilation), label, copyright
             evidencia: S10, API Albums
             confianza: alta

Track        id, album_id, title, artist_ids[], duration_ms, track_number, disc_number, explicit, isrc, audio_url,
             lyrics_id?, popularity
             evidencia: filas de S10, S13, API Tracks
             confianza: alta

Lyrics       track_id, lines [{start_ms, text}], synced bool
             evidencia: S15, ayuda "Letras"
             confianza: media

Playlist     id, owner_id, name, description, cover_url (propia o mosaico 2x2), is_public, is_collaborative,
             folder_id?, created_at, updated_at, snapshot_id
             evidencia: S09, S18, ayuda "Playlists colaborativas", API Playlists (snapshot_id)
             confianza: alta

PlaylistItem playlist_id, position, item_type (track | episode), item_id, added_by, added_at
             evidencia: filas de S09 ("añadida por" en colaborativas), API playlist items
             confianza: alta

Folder       id, owner_id, name, parent_id?
             evidencia: ayuda "Carpetas de playlists"
             confianza: media

LibraryItem  user_id, item_type (track | album | artist | playlist | show | episode), item_id, saved_at, pinned bool
             evidencia: filtros/orden/fijados de S08, S12, API Library
             confianza: alta

Show         id, title, publisher, description, cover_url, rss_url?
Episode      id, show_id, title, description, duration_ms, release_date, audio_url
             evidencia: S19, S20, API Shows/Episodes
             confianza: alta

PlaybackPosition user_id, episode_id, position_ms, fully_played, updated_at
             evidencia: progreso de S20, retomar en F07
             confianza: alta

PlaybackState user_id, device_id, context_uri, item_id, position_ms, is_playing, shuffle, repeat (off|context|track)
QueueItem    user_id, position, item_id, source (user | context)
             evidencia: S13, cola en dos secciones de S14, API Player
             confianza: alta (estado), media (cola guardada en el servidor)

Device       id, user_id, name, type, is_active
             evidencia: S21, API Player devices
             confianza: media

ListenEvent  user_id, item_id, context_uri, played_at, ms_played, skipped
             evidencia: "Escuchado recientemente", recomendaciones, Wrapped, API recently played
             confianza: alta (que existe), suposición (campos exactos)

Download     user_id, item_type, item_id, device_id, status, downloaded_at
             evidencia: F06, ayuda "Escuchar sin conexión" (límite de descargas por dispositivo)
             confianza: media (se guarda por dispositivo, sobre todo en el cliente)

Follow       follower_id, target_type (user | artist | show | playlist), target_id
             evidencia: S11, S19, S23, API follow
             confianza: alta

Category     id, name, color, icon_url; CategoryPlaylist category_id, playlist_id
             evidencia: mosaicos de S05, S07, API Categories
             confianza: alta

HomeSection  id, user_id?, title, kind, items[] (generados por usuario)
             evidencia: estantes de S04
             confianza: suposición (probablemente se generan al vuelo, no se guardan)
```

Relaciones: Artist n-n Album, Album 1-n Track, Track n-n Artist; User 1-n Playlist, Playlist 1-n PlaylistItem → Track|Episode; User 1-n LibraryItem; Show 1-n Episode; User 1-n PlaybackPosition, ListenEvent, Follow; User 1-1 PlaybackState 1-n QueueItem; Folder 1-n Playlist; Category n-n Playlist.

## Matriz de funciones

Ver `features.csv`. Must: 25, should: 18, could: 14, skip: 10.

## Fuera de alcance (no se puede o no se debe clonar)

- **El catálogo musical.** Las grabaciones y composiciones tienen licencia de sellos y editoriales. El clon solo emite audio que el usuario posee o ha licenciado: subidas propias, música independiente o libre de derechos, o un acuerdo propio.
- **Letras.** Con licencia de un proveedor de letras y de las editoriales.
- **Catálogo de audiolibros** y el modelo de horas: acuerdos con editoriales.
- **Pódcasts en vídeo y exclusivos.** Contenido con licencia o propio.
- **Recomendaciones entrenadas con los datos de escucha de Spotify** (Descubrimiento Semanal, Radar de Novedades, daylist, DJ). La *función* se puede rehacer con los datos de escucha del propio clon, pero no con la misma calidad desde el primer día.
- **La red social y los usuarios.** Seguidores, actividad de amigos, playlists colaborativas con usuarios existentes.
- **El ecosistema de dispositivos Spotify Connect.** Integrado en altavoces, televisores y coches por acuerdos con socios. Un clon solo tiene AirPlay/Bluetooth a través de iOS.
- **Wrapped como marca y su estética.** Se puede hacer un resumen anual, pero no con su nombre ni su diseño.
- **La Web API de Spotify como fuente de datos.** Sus términos prohíben crear un servicio competidor con ella.
- **Elementos de marca.** Logo, el verde, la fuente Circular, iconos, nombres de playlists editoriales y portadas. Se quedan en `replica/screens/` solo como referencia.

## Tamaño

24 pantallas (+ el minirreproductor global), 10 flujos, 18 entidades.

Partes difíciles:
1. **Motor de audio en iOS.** Audio en segundo plano, pantalla de bloqueo y Centro de control (MPNowPlayingInfoCenter, comandos remotos), reproducción sin pausas y con fundido, AirPlay, interrupciones (llamadas, Siri) y streaming HLS con búfer.
2. **Descargas sin conexión.** Almacenamiento cifrado, cola de descargas, reanudar, límites de espacio, recomprobación de la licencia (premium cada 30 días) y sincronizar la biblioteca al volver a tener conexión.
3. **Cola y estado de reproducción.** Una cola en dos partes (lo añadido por el usuario antes que el contexto), un aleatorio reversible, modos de repetición y conservar el estado al cerrar y reabrir la app (y entre dispositivos, si se llega a eso).

También tienen su complicación: el proceso de ingesta de audio (subida → transcodificar a AAC/Opus HLS en varias calidades → CDN), la búsqueda con autocompletado en varios tipos, una portada de recomendaciones sin un gran volumen de datos, y las normas de compras dentro de la app del App Store para la suscripción.

Tamaño: **L** (un trimestre) para el ciclo principal tal como está acotado, con catálogo propio en iOS. Todo Spotify es **XL**, así que hay que acotarlo. Una porción fina (subir catálogo → buscar → reproducir en segundo plano → me gusta → playlists, sin descargas ni recomendaciones) es **M**.
