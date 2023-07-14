+++
title = "rescrobbled v0.7.1"
date = 2019-09-15
updated = 2023-07-13
description = "rescrobbled releases"
+++

[Rescrobbled](https://github.com/InputUsername/rescrobbled) is a music scrobbler daemon.
It detects active media players running on D-Bus using MPRIS, automatically updates "now playing"
status, and scrobbles songs to Last.fm or ListenBrainz-compatible services as they play.

## Changelog

### [v0.7.1] (2023-07-13)

- Made album name optional when submitting to Last.fm
- Updated dependencies

### [v0.7.0] (2023-01-20)

- Removed notification functionality
  - As an alternative, I recommend [mpris-notifier](https://github.com/l1na-forever/mpris-notifier).
- Made album name optional when submitting to ListenBrainz
  - ListenBrainz does not require album names for submissions, so they are now optional.
    This means tracks without `xesam:album` will still be submitted.
  - The Last.fm library used by rescrobbled still requires the album, but this restriction could
    be lifted in the future.
  - This does have the side effect of now treating empty album names (i.e. "")
    the same as if they were missing from the MPRIS metadata.
- Updated player finding logic to be more resilient to players that cause errors
- Moved to OpenSSL/`libssl` version 3

### [v0.6.2] (2022-11-16)

- Fixed scrobbling from applications that report a single string value for `xesam:artist`

### [v0.6.1] (2022-10-11)

- Fixed builds of version 0.6.0 breaking
  - Dependency mpris released a breaking change in version 2.0.0-rc3,
    but Rust/Cargo's semver resolution does not see this as a breaking change,
    leading to builds of rescrobbled 0.6.0 breaking that were previously fine
    with mpris 2.0.0-rc2.

### [v0.6.0] (2022-07-20)

- Fixed scrobbling behind a HTTP/HTTPS proxy
  - Replaced the rustfm-scrobble dependency with a fork that automatically picks up proxy settings
- Filter scripts now receive the `xesam:genre` (song genre) MPRIS property in addition to artist,
  title and album name
  - Note: you may have to update your filter script to take this into account. For example, the
    following Python code now raises an error because the additional line (genre) is not unpacked:
    ```python
    artist, title, album = (l.rstrip() for l in sys.stdin.readlines())
    ```
    This can be fixed by reading and ignoring the additional line:
    ```python
    artist, title, album, _ = (l.rstrip() for l in sys.stdin.readlines())
    ```
    Or, alternatively:
    ```python
    artist = sys.stdin.readline().rstrip()
    title = sys.stdin.readline().rstrip()
    album = sys.stdin.readline().rstrip()
    ```
- Moved to Rust 2021 edition

### [v0.5.3] (2022-06-16)

- Entered Last.fm passwords are no longer displayed in plaintext
- Updated dependencies

### [v0.5.2] (2022-03-04)

- Improved error handling
  - More consistent error messages
  - Causes of errors are now always included
- Fixed `basic.py` and `ignore_artists.py` filter script examples
- Updated dependencies

### [v0.5.1] (2022-01-23)

- Fixed the way player D-Bus bus names are checked against the player whitelist

### [v0.5.0] (2022-01-04)

- Added support for multiple ListenBrainz instances
  - You can now specify multiple ListenBrainz instances, supporting custom installs
    and other scrobbling services that use a ListenBrainz compatible API
- Added a number of example filter scripts
- The auto-generated config file and session token file are now created with
  more restrictive permissions (`0600`)
- Added fallback behavior when a player does not report track length:
  - Tracks will scrobble after the default minimum track length (30 seconds)
  - Tracks will only scrobble once, unless paused and then unpaused
- Internal refactoring
- Cleaned up the README
- Documented where the session token is stored

### [0.4.0] (2021-05-07)

- Added ignore functionality for filter scripts:
  - Filter scripts that return nothing will cause the current track to be ignored/not scrobbled
  - This can be used to, for example, filter certain artists or songs entirely

### [v0.3.3] (2021-05-06)

- Added `-v` (`--version`) command-line switch to get the program's version
- Released on crates.io

### [v0.3.2] (2021-04-19)

- Fixed config template typos (`min_play_time` => `min-play-time`, `player_whitelist` => `player-whitelist`)

### [v0.3.1] (2021-03-29)

- Fixed a typo in the config file template (`lastfm-token` => `lastfm-key`)

### [v0.3.0] (2021-02-18)

- Fixed a bug where a single song on repeat only scrobbled once
- Rescrobbled now creates the config file if it doesn't exist
- Added the `filter-script` config option:
    - Rescrobbled will run this script to filter metadata before
      submitting it to Last.fm and/or ListenBrainz
    - The script receives artist, song title and album name on
      consecutive lines of its standard input (in that order)
    - It should produce filtered metadata on the corresponding
      lines of its standard output
    - Format might change in future updates, eg. to provide
      additional metadata

### [v0.2.0] (2020-08-12)

- Improved usage instructions
- Renamed config options (old names still supported)
    - `api-key` => `lastfm-key`
    - `api-secret` => `lastfm-secret`
    - `lb-token` => `listenbrainz-token`
- Added music player whitelisting (by MPRIS identity or D-Bus bus name)
- Made Last.fm scrobbling optional

### [v0.1.0] (2019-09-15)

Initial release

<!---->

[v0.7.1]: https://github.com/InputUsername/rescrobbled/releases/v0.7.1
[v0.7.0]: https://github.com/InputUsername/rescrobbled/releases/v0.7.0
[v0.6.2]: https://github.com/InputUsername/rescrobbled/releases/v0.6.2
[v0.6.1]: https://github.com/InputUsername/rescrobbled/releases/v0.6.1
[v0.6.0]: https://github.com/InputUsername/rescrobbled/releases/v0.6.0
[v0.5.3]: https://github.com/InputUsername/rescrobbled/releases/v0.5.3
[v0.5.2]: https://github.com/InputUsername/rescrobbled/releases/v0.5.2
[v0.5.1]: https://github.com/InputUsername/rescrobbled/releases/v0.5.1
[v0.5.0]: https://github.com/InputUsername/rescrobbled/releases/v0.5.0
[v0.4.0]: https://github.com/InputUsername/rescrobbled/releases/v0.4.0
[v0.3.3]: https://github.com/InputUsername/rescrobbled/releases/v0.3.3
[v0.3.2]: https://github.com/InputUsername/rescrobbled/releases/v0.3.2
[v0.3.1]: https://github.com/InputUsername/rescrobbled/releases/v0.3.1
[v0.3.0]: https://github.com/InputUsername/rescrobbled/releases/v0.3.0
[v0.2.0]: https://github.com/InputUsername/rescrobbled/releases/v0.2.0
[v0.1.0]: https://github.com/InputUsername/rescrobbled/releases/v0.1.0
