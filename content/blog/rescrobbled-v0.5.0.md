+++
title = "rescrobbled v0.5.0"
date = 2022-01-04
description = "rescrobbled v0.5.0 changelog"
+++

[Rescrobbled](https://github.com/InputUsername/rescrobbled) is a universal music scrobbling daemon
for MPRIS-enabled music players. It listens for active music players and submits songs to services
like Last.fm and ListenBrainz as they play.

## Changelog

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

[Release](https://github.com/InputUsername/rescrobbled/releases/tag/v0.5.0)

## Patches

### v0.5.1 (2022-01-23)

- Fixed the way player D-Bus bus names are checked against the player whitelist

[Release](https://github.com/InputUsername/rescrobbled/releases/tag/v0.5.1)

### v0.5.2 (2022-03-04)

- Improved error handling
  - More consistent error messages
  - Causes of errors are now always included
- Fixed `basic.py` and `ignore_artists.py` filter script examples
- Updated dependencies

[Release](https://github.com/InputUsername/rescrobbled/releases/tag/v0.5.2)

### v0.5.3 (2022-06-16)

- Entered Last.fm passwords are no longer displayed in plaintext
- Updated dependencies

[Release](https://github.com/InputUsername/rescrobbled/releases/tag/v0.5.3)
