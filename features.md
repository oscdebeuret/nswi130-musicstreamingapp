# Music Streaming System

## Features overview

| #   | Feature                          | Core |
| --- | -------------------------------- | ---- |
| 1   | Song upload (artist portal)      | x    |
| 2   | Catalogue search                 | x    |
| 3   | Audio streaming                  | x    |
| 4   | Playlists                        | x    |
| 5   | User accounts & login            |      |
| 6   | Payment & Premium                |      |
| 7   | Group listening                  |      |
| 8   | Offline download (Premium)       |      |
| 9   | Notifications                    |      |
| 10  | Music recommendations            |      |
| 11  | Artist statistics & promotion    |      |
| 12  | Music player & queue             |      |
| 13  | User library & following artists |      |
| 14  | Settings & privacy               |      |

## Why these core features

For the MVP, we chose the smallest set of features that gives the main value of the
platform from start to end: **an artist publishes a song, a listener finds it, and the
listener plays it**.

- **Song upload** is core because without songs there is nothing to listen to.
- **Catalogue search** is core because listeners must be able to find songs.
- **Audio streaming** is core because playing music is the main purpose of the product.
- **Playlists** are core because they are the main way for listeners to organize their music
  and come back to it, which makes them use the platform again.

All other features are auxiliary for the MVP. User accounts and login are needed, but they
are a common service that we expect to exist. Payment, group listening and offline download
add value or bring money, but the platform can work without them.

## Core features and responsibilities

### Feature: Song Upload

As an artist, I want to upload my songs and their information so that people can listen to them on the platform.

#### Feature breakdown

1. The artist opens the artist portal and chooses an audio file and a cover image.
2. The artist fills in the song information (title, genre, release date, contributors, ISRC code).
3. The system checks that the user is an artist and that the files are valid (format, size, not damaged).
4. The system puts the upload in a queue to process it in the background.
5. The system converts the audio into streaming formats, adjusts the volume level, and creates several sizes of the cover image.
6. The system saves the processed files and adds the song to the catalogue.
7. The system tells the artist the result.

#### Responsibilities

##### Upload responsibilities (steps 1, 3)

- Show the upload form and the upload progress in the artist portal
- Check that the user is an artist
- Receive the audio file and the cover image
- Continue an upload after a connection problem (for large files)
- Check the audio file format, size and that the file is not damaged
- Check the cover image format and size

##### Song information responsibilities (step 2)

- Collect and check the song information (title, genre, release date, contributors, explicit content)
- Check the ISRC code (international song identifier)
- Link the song to the artist and the album

##### Audio processing responsibilities (steps 4, 5)

- Put uploads in a queue and process them in the background
- Convert the audio into streaming formats and qualities
- Measure the volume level and calculate a volume adjustment
- Create several sizes of the cover image

##### Storage responsibilities (step 6)

- Save the audio files and images in the media storage
- Save the song in the catalogue

##### Publishing responsibilities (step 7)

- Tell the artist if the upload succeeded or failed
- Record uploads and changes to song information in the audit log

---

### Feature: Catalogue Search

As a listener, I want to search for songs, artists and albums so that I can quickly find the music I want.

#### Feature breakdown

1. The listener types a search text (song title, artist name, album).
2. The system suggests possible searches while the listener is typing.
3. The system cleans the search text and looks for matches in the search index, even with spelling mistakes.
4. The system orders the results by match quality and popularity and groups them by type.
5. The system gets extra information for the results (cover image, song length).
6. The system shows the results to the listener.
7. The listener chooses a result, and the system records this choice.
8. In the background, the system keeps the search index up to date when the catalogue changes.

#### Responsibilities

##### Search input responsibilities (steps 1, 2)

- Receive the search text from the listener
- Suggest possible searches while the listener is typing

##### Search processing responsibilities (step 3)

- Clean the search text (lowercase, remove accents and special characters)
- Find results even when the search text has spelling mistakes
- Look for matching songs, artists and albums in the search index

##### Ordering & filtering responsibilities (step 4)

- Order the results by how well they match and by popularity
- Hide songs that are not available (not published or blocked in the listener's country)
- Group the results by type (best result, songs, artists, albums)

##### Result display responsibilities (steps 5, 6)

- Get extra information and cover images for the results
- Split long result lists into pages
- Show the search results on the screen

##### Search history responsibilities (step 7)

- Record search texts and the results that listeners choose

##### Search index responsibilities (step 8)

- Detect new songs in the catalogue and add them to the search index
- Remove deleted or unpublished songs from the search index
- Update song popularity using play statistics
- Save the search index and keep it available for searches

---

### Feature: Audio Streaming

As a listener, I want to play songs directly from the internet so that I can listen to music immediately without downloading it.

#### Feature breakdown

1. The listener chooses a song to play.
2. The application asks the system for the song's audio.
3. The system checks that the listener is logged in and that the song is available in the listener's country.
4. The system finds the song's audio files and creates a temporary access link.
5. The application downloads the audio in small parts and changes the quality depending on the internet speed.
6. The application plays the audio, loads the next parts in advance, and lets the listener control the playback.
7. The system records the play for statistics.

#### Responsibilities

##### Access check responsibilities (steps 2, 3)

- Check that the listener is logged in
- Check that the song is published and available in the listener's country
- Allow playback on only one device at a time per account

##### Audio delivery responsibilities (steps 4, 5)

- Find the song's audio files in the media storage
- Create temporary, secure links to the audio files
- Send the audio in small parts in several qualities
- Change the audio quality depending on the listener's internet speed
- Keep often played songs in a cache close to the listeners

##### Player responsibilities (steps 5, 6)

- Load audio parts in advance to avoid pauses
- Decode and play the audio on the listener's device
- Apply the volume adjustment during playback
- Show the player with playback controls (play, pause, jump forward or back, skip)
- Continue playing after short connection problems

##### Play statistics responsibilities (step 7)

- Save each play (listener, song, time, duration)
- Count a play only after 30 seconds of listening
- Calculate play counts and send them to the catalogue and the search

---

### Feature: Playlists

As a listener, I want to create playlists, add or remove songs and choose who can see them so that I can organize my music and share it if I want.

#### Feature breakdown

1. The listener creates a playlist and gives it a name, and optionally a description and an image.
2. The system creates the playlist and sets the listener as its owner.
3. The listener adds, removes or reorders songs in the playlist.
4. The system checks the changes and saves the new list of songs.
5. The listener chooses who can see the playlist (private, public or link only).
6. The system updates the playlist on all the listener's devices.
7. The listener can delete the playlist.

#### Responsibilities

##### Playlist creation responsibilities (steps 1, 2)

- Show the playlist screens (create, edit, view)
- Check the playlist name and description
- Save the playlist and its owner
- Save the playlist image in several sizes

##### Song list responsibilities (steps 3, 4)

- Add one or more songs to a playlist
- Remove songs and warn about duplicate songs
- Change the order of the songs
- Check that added songs exist and are available in the listener's country
- Get song information (title, artist, length) from the catalogue

##### Access & visibility responsibilities (step 5)

- Allow only the owner to change the playlist
- Manage the playlist visibility (private, public, link only)
- Create share links for link-only playlists

##### Sync responsibilities (step 6)

- Send playlist changes to all the listener's devices
- Save playlist changes so that they are the same on every device

##### Deletion responsibilities (step 7)

- Delete playlists and allow restoring them for a short time

---

## Auxiliary features

### Feature: User Accounts & Login

As a user, I want to create an account, log in and manage my profile so that I can use the platform safely and keep my personal information up to date.

### Feature: Payment & Premium

As a listener, I want to pay for a Premium subscription so that I can use extra features on the platform.

### Feature: Group Listening

As a listener, I want to create a listening room with other users so that we can listen to the same music at the same time from different places.

### Feature: Offline Download (Premium)

As a Premium user, I want to download songs to my device so that I can listen to them without an internet connection.

### Feature: Notifications

As a user, I want to get notifications about new songs from my favourite artists so that I can discover their latest music.

### Feature: Music Recommendations

As a listener, I want to get song suggestions based on what I listen to so that I can discover new music.

### Feature: Artist Statistics & Promotion

As an artist, I want to manage my releases, see how they perform and promote them so that I can reach more listeners.

### Feature: Music Player & Queue

As a listener, I want to control the playback and manage the list of next songs so that I can listen to music without interruption.

### Feature: User Library & Following Artists

As a listener, I want to save songs, albums and playlists to my library and follow artists so that I can easily find my favourite music.

### Feature: Settings & Privacy

As a user, I want to change playback and privacy settings so that the platform works the way I like.
