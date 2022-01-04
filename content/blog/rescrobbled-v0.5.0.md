+++
title = "rescrobbled v0.5.0"
date = 2022-01-04
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
