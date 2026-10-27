# Recon map: Spotify – Música y pódcasts (iOS)

Scope: the core listening loop on iPhone. Find audio (home, search, browse), play it (mini player, Now Playing, queue), and keep it (library, playlists, downloads). Podcasts are included as a second content type. Audiobooks, video, live events, DJ/AI, Wrapped, Connect hardware and Family/Duo plan admin are out of this slice.
For: **assumed**, please confirm. A music or audio app for a catalogue the user is allowed to stream: their own label, indie artists, royalty-free music, a church, a school or a podcast network. A clone can't hold a mainstream catalogue (see Out of scope).
Date: 2026-10-27

Proposed scope, to confirm: **iOS · core listening loop · user's own or licensed catalogue.**

## Sources

| # | source | URL | notes |
| --- | --- | --- | --- |
| 1 | App Store listing (ES) | https://apps.apple.com/es/app/spotify-m%C3%BAsica-y-p%C3%B3dcasts/id324684580 | 4.7★ from ~3.4M ratings, 325 MB, iOS 16.1+, v9.1.94. Lists the pitch: Discover Weekly, Release Radar, collab playlists, Connect, Jam, DJ, Wrapped. Screenshots only as image placeholders, so they weren't captured |
| 2 | Pricing page (ES) | https://www.spotify.com/es/premium/ | Individual €11.99, Student €6.49, Duo €16.99, Family €20.99. Gated: no ads, offline, unlimited skips, play in order, lossless, 12 h of audiobooks |
| 3 | Help centre (ES) | https://support.spotify.com/es/ | Full feature taxonomy: playlists (folders, collaborative, cover, mixed), queue, shuffle, radio, lyrics, local files, hide songs, sort and filter, private session, audio quality, crossfade, EQ, normalization, autoplay, offline |
| 4 | Public Web API docs | https://developer.spotify.com/documentation/web-api | Data model: Album, Artist, Track, Playlist, Show, Episode, Audiobook, Chapter, Category, User, Player (state, devices, queue, recently played), Library save/remove |
| 5 | Free vs Premium (third party) | https://www.soundguys.com/free-spotify-vs-spotify-premium-36632/ | Free mobile: shuffle-only with ~6 skips per hour, ads, lower bitrate. Not from a primary source |
| 6 | Free on-demand change (press) | https://primetel.com.cy/spotify-finally-lets-free-users-pick-and-play-songs-7824 | Reports that Free users can now pick and play songs on demand. Not confirmed by a primary source |
| 7 | Your Library redesign (community + press) | https://community.spotify.com/t5/Your-Library/New-Your-Library-view-iOS-amp-Android/m-p/5196161 · https://www.androidcentral.com/spotify-your-library-ui-update-dynamic-filters | One list with filter chips, sort (recents, recently added, A–Z, creator, custom), up to 4 pins. From 2021, so check against the current app |
| 8 | the user's own account | — | **Not used yet.** Next pass: screenshots into `replica/screens/` from the user's own phone, at human speed |

Terms note: Spotify's Developer Terms forbid using the API to build a competing streaming service. The API docs are used here only to read the data model. Don't call the API to fill the clone.

## Core loop

Open the app, get a personalised shelf of things to play, tap one, it plays in the background with a persistent mini player, and you save what you like to your library or a playlist so you can come back to it.

## Screens

| ID | screen | route / how to reach | purpose | key components | states seen |
| --- | --- | --- | --- | --- | --- |
| S01 | Welcome / sign in | first launch | sign up or log in | logo, primary/secondary buttons, social login buttons | default, loading, error |
| S02 | Sign up | S01 → Sign up free | email, password, date of birth, gender, name, terms | text input, date picker, segmented choice, checkbox, stepper | per step: empty, invalid, submitting |
| S03 | Taste onboarding | after S02 | pick artists to seed recommendations | search field, artist circle grid (multi-select), Done | empty, selected (n), loading |
| S04 | Home | tab 1 | personalised feed | top bar (avatar, filter chips All/Music/Podcasts), recents 2×4 grid, horizontal shelves of cards, section headers | loading skeleton, filled, offline (downloads only), new user (generic shelves) |
| S05 | Search / Browse | tab 2 | entry to search and genre browse | search field, "Browse all" colour tiles grid | default |
| S06 | Search results | S05 → type | find tracks, artists, albums, playlists, podcasts | query field, type chips, top result card, track rows, recent searches list | recent searches, typing (autocomplete), results, no results, offline |
| S07 | Genre / category page | S05 → tile | curated playlists for a genre | header, shelves of playlist cards | loading, filled |
| S08 | Your Library | tab 3 | everything saved | filter chips (Playlists, Artists, Albums, Podcasts, Downloaded), sort sheet, list/grid toggle, pinned items, "Liked Songs" entry, + create | empty (new user), filled, filtered empty, offline |
| S09 | Playlist detail | card or library row | view and play a playlist | cover, title, owner, duration, play/shuffle FAB, download toggle, add/more, track rows, "Add to this playlist" recs | loading, filled, empty playlist (own), offline (greyed), downloading |
| S10 | Album detail | card, artist page | view and play an album | cover, artist, year, track rows with numbers, save heart, play/shuffle | filled, explicit badges |
| S11 | Artist page | tap artist | artist hub | hero image, follow button, monthly listeners, Popular (top 5), discography shelves, about, "fans also like" | filled, follow / unfollow |
| S12 | Liked Songs | S08 → Liked Songs | auto-playlist of saved tracks | like S09 plus sort and filter chips by genre | empty, filled |
| S13 | Now Playing (full player) | tap mini player | control playback | art or canvas, title/artist, like, scrubber, prev/play/next, shuffle, repeat, device picker, share, queue, lyrics card, about-artist card, credits | playing, paused, buffering, ad playing (free), error |
| S14 | Queue | S13 → queue icon | see and edit what's next | Now playing row, "Next in queue" (user-added), "Next from: <context>", drag handles, remove, shuffle/repeat bar | empty queue, filled, reordering |
| S15 | Lyrics full screen | S13 → lyrics card | time-synced lyrics | coloured background, highlighted line, scrub | synced, unsynced, unavailable |
| S16 | Track context menu (sheet) | ••• on any row | actions on a track | bottom sheet: like, add to playlist, add to queue, go to album/artist, share, hide, credits, sleep timer (from player) | — |
| S17 | Add to playlist picker | S16 → Add to playlist | choose a destination | New playlist button, search, list of own playlists, multi-select, Done | — |
| S18 | Create / edit playlist | S08 + or S17 → New | name, then edit | name input, edit view: cover change, title, description, reorder, remove | creating, saved |
| S19 | Podcast show page | search or card | show hub | cover, follow, about, episode list, sort | filled |
| S20 | Episode page | show → episode | play one episode | description, play, progress bar, download, mark played | unplayed, in progress, played |
| S21 | Device picker (Connect) | S13 device icon | output device | list of devices, this phone, Bluetooth/AirPlay | one device, many |
| S22 | Settings | avatar → Settings | preferences | grouped lists: account, content (explicit, autoplay), playback (crossfade, gapless, normalization), audio quality, storage, data saver, privacy, notifications, log out | — |
| S23 | Profile | avatar → View profile | public face | avatar, name, followers/following, public playlists, recently played artists | own, other user |
| S24 | Upsell / paywall | gated action (free) | convert to premium | plan cards, benefit list, CTA | — |
| — | Mini player (persistent) | above the tab bar everywhere | always-on playback | art, title/artist, device, like, play/pause, thin progress | hidden (nothing played), playing, paused |

Screenshots for every screen: still to capture from the user's own device into `replica/screens/S##.png`.

## Flows

```
F01 New user gets to first play
    S01 -> S02 (4 steps) -> S03 -> S04 -> tap card -> S09 -> play
    happy path clicks: ~10 (signup fields aside)
    edge: email taken, under-age DOB, skip taste picker, no network

F02 Search for a song and play it
    S04 -> S05 -> S06 (type) -> tap track row -> plays, mini player appears
    happy path clicks: 3 + typing
    edge: typo / no results, offline, explicit filtered, free tier opens shuffle context instead

F03 Save a song and find it later
    S13 like (+) -> S08 -> S12 -> tap track
    happy path clicks: 1 to save, 3 to replay
    edge: unlike from a different screen, liked while offline (sync later)

F04 Make a playlist and fill it
    S08 + -> S18 name -> S09 (empty) -> add songs via search inside playlist / S16 -> S17
    happy path clicks: 4 to create, 3 per song added
    edge: duplicate song warning, empty name, reorder, delete playlist (undo toast)

F05 Control what plays next
    any row ••• -> S16 Add to queue -> S13 -> S14 reorder / remove
    happy path clicks: 2 to queue, 4 to reorder
    edge: queue cleared when starting new context, shuffle on, repeat one

F06 Listen offline (premium)
    S09 download toggle -> progress -> airplane mode -> S08 Downloaded filter -> play
    happy path clicks: 1 to download, 2 to find
    edge: storage full, download over cellular off, logged-out 30-day re-check, free tier -> S24

F07 Follow and play a podcast, resume later
    S06 -> S19 follow -> S20 play -> leave mid-way -> S04 "continue listening" -> resumes at position
    happy path clicks: 4
    edge: episode marked played, new episode notification, playback speed

F08 Explore an artist
    S13 artist name -> S11 -> follow -> play Popular / open album S10
    happy path clicks: 2-3

F09 Share a playlist or track
    S16 Share -> iOS share sheet / copy link -> recipient opens link -> S09
    edge: private playlist, recipient not logged in

F10 Change playback settings
    S04 avatar -> S22 -> crossfade / quality / explicit toggle
    happy path clicks: 3
```

## Components

| component | variants | states | used on |
| --- | --- | --- | --- |
| Button | primary pill (accent), secondary outline, text, icon | default, pressed, disabled, loading | S01, S02, S11, S24 |
| Play FAB | play, pause, shuffle-badged | default, pressed, buffering | S09, S10, S11, S12, S19 |
| Filter chip | single, multi, with close (×) | off, on | S04, S06, S08, S12 |
| Card | square (album/playlist), circle (artist), wide (episode), recents tile | default, pressed, now-playing indicator | S04, S07, S11, S06 |
| Shelf | horizontal scroll row with header + "Show all" | loading, filled | S04, S07, S11 |
| Track row | with art / numbered (album) / episode row | default, now playing (accent text + eq bars), greyed (unavailable/offline), explicit badge, downloaded dot | S06, S09–S12, S14 |
| Bottom sheet menu | action list with header (art+title) | open, dismissing | S16, sort sheet, S21 |
| Mini player | — | playing, paused, buffering, device-remote | global |
| Scrubber | thin (mini), full (S13) | idle, dragging, buffered | S13, mini, S20 |
| Transport controls | shuffle, prev, play/pause, next, repeat (off/all/one) | active, disabled (free tier) | S13 |
| Heart / add toggle | — | off, on (accent), animating | S10, S13, rows |
| Download toggle | — | off, queued, downloading (ring), done | S09, S10, S20 |
| Search field | static (S05), active with cancel | empty, typing, filled | S05, S06, S17 |
| Tab bar | Home, Search, Your Library (+ Premium/Create) | selected | global |
| Toast / snackbar | info, with Undo / "Change" action | visible, dismissing | add to playlist, like, delete |
| Skeleton loader | card, row, header | — | S04, S09 |
| Large header | gradient from cover colour, collapses to nav title on scroll | expanded, collapsed | S09–S12, S19 |
| Settings row | toggle, disclosure, slider (crossfade), segmented (quality) | — | S22 |
| Empty state | icon + headline + CTA | — | S08, S09, S12, S06 |
| Modal / alert | confirm delete, upsell | — | S18, S24 |

## Inferred data model

```
User         id, email, display_name, avatar_url, birthdate, country, plan (free | premium | family | duo | student),
             explicit_allowed, created_at
             evidence: S02 fields, S22, help "Manage your account", API Users
             confidence: high

Artist       id, name, image_url, bio, genres[], monthly_listeners (derived), verified
             evidence: S11, API Artists
             confidence: high

Album        id, title, artist_ids[], cover_url, release_date, type (album | single | ep | compilation), label, copyright
             evidence: S10, API Albums
             confidence: high

Track        id, album_id, title, artist_ids[], duration_ms, track_number, disc_number, explicit, isrc, audio_url,
             lyrics_id?, popularity
             evidence: S10 rows, S13, API Tracks
             confidence: high

Lyrics       track_id, lines [{start_ms, text}], synced bool
             evidence: S15, help "Lyrics"
             confidence: medium

Playlist     id, owner_id, name, description, cover_url (custom or 2x2 mosaic), is_public, is_collaborative,
             folder_id?, created_at, updated_at, snapshot_id
             evidence: S09, S18, help "Collaborative playlists", API Playlists (snapshot_id)
             confidence: high

PlaylistItem playlist_id, position, item_type (track | episode), item_id, added_by, added_at
             evidence: S09 rows ("added by" in collab), API playlist items
             confidence: high

Folder       id, owner_id, name, parent_id?
             evidence: help "Playlist folders"
             confidence: medium

LibraryItem  user_id, item_type (track | album | artist | playlist | show | episode), item_id, saved_at, pinned bool
             evidence: S08 filters/sort/pins, S12, API Library
             confidence: high

Show         id, title, publisher, description, cover_url, rss_url?
Episode      id, show_id, title, description, duration_ms, release_date, audio_url
             evidence: S19, S20, API Shows/Episodes
             confidence: high

PlaybackPosition user_id, episode_id, position_ms, fully_played, updated_at
             evidence: S20 progress, F07 resume
             confidence: high

PlaybackState user_id, device_id, context_uri, item_id, position_ms, is_playing, shuffle, repeat (off|context|track)
QueueItem    user_id, position, item_id, source (user | context)
             evidence: S13, S14 two-section queue, API Player
             confidence: high (state), medium (queue persistence server-side)

Device       id, user_id, name, type, is_active
             evidence: S21, API Player devices
             confidence: medium

ListenEvent  user_id, item_id, context_uri, played_at, ms_played, skipped
             evidence: "Recently played", recommendations, Wrapped, API recently played
             confidence: high (exists), guess (exact fields)

Download     user_id, item_type, item_id, device_id, status, downloaded_at
             evidence: F06, help "Listen offline" (download limits per device)
             confidence: medium (stored per device, mostly client-side)

Follow       follower_id, target_type (user | artist | show | playlist), target_id
             evidence: S11, S19, S23, API follow
             confidence: high

Category     id, name, color, icon_url; CategoryPlaylist category_id, playlist_id
             evidence: S05 tiles, S07, API Categories
             confidence: high

HomeSection  id, user_id?, title, kind, items[] (generated per user)
             evidence: S04 shelves
             confidence: guess (likely generated on the fly, not stored)
```

Relationships: Artist n-n Album, Album 1-n Track, Track n-n Artist; User 1-n Playlist, Playlist 1-n PlaylistItem → Track|Episode; User 1-n LibraryItem; Show 1-n Episode; User 1-n PlaybackPosition, ListenEvent, Follow; User 1-1 PlaybackState 1-n QueueItem; Folder 1-n Playlist; Category n-n Playlist.

## Feature matrix

See `features.csv`. Must: 25, should: 18, could: 14, skip: 10.

## Out of scope (cannot or should not be cloned)

- **The music catalogue.** Recordings and compositions are licensed from labels and publishers. The clone streams only audio the user owns or has licensed: their own uploads, indie/royalty-free, or a deal of their own.
- **Lyrics.** Licensed from a lyrics provider and from publishers.
- **Audiobooks catalogue** and the hours model: publisher deals.
- **Video podcasts and exclusive shows.** Licensed or owned content.
- **Recommendations trained on Spotify's listening data** (Discover Weekly, Release Radar, daylist, DJ). The *feature* can be rebuilt on the clone's own listen data, but not the quality on day one.
- **The social graph and users.** Followers, friend activity, collaborative playlists with existing users.
- **Spotify Connect device ecosystem.** Built into speakers, TVs and cars by partner deals. A clone gets AirPlay/Bluetooth through iOS only.
- **Wrapped as a brand and its look.** An annual recap can be rebuilt, but not its name or design.
- **Spotify Web API as a data source.** Its terms forbid building a competing service on it.
- **Brand assets.** Logo, the green, the Circular font, icons, editorial playlist names and cover art. They stay in `replica/screens/` as reference only.

## Size

Screens 24 (+ the global mini player), flows 10, entities 18.

Hard parts:
1. **Audio playback engine on iOS.** Background audio, lock screen and Control Centre (MPNowPlayingInfoCenter, remote commands), gapless/crossfade, AirPlay, interruptions (calls, Siri), and HLS streaming with buffering.
2. **Offline downloads.** Encrypted storage, download queue, resume, storage limits, re-validation of the licence (premium check every 30 days), and library sync when back online.
3. **Queue and playback state.** A two-part queue (user-added before context), shuffle that is reversible, repeat modes, and keeping state the same across app kill/relaunch (and across devices if you go that far).

Also non-trivial: audio ingestion pipeline (upload → transcode to AAC/Opus HLS at several bitrates → CDN), search with autocomplete across types, a recommendation feed without a big dataset, App Store in-app purchase rules for the subscription.

Size: **L** (a quarter) for the core loop as scoped, with your own catalogue on iOS. All of Spotify is **XL**, so rescope. A thin slice (upload catalogue → search → play in background → like → playlists, no offline and no recs) is **M**.
