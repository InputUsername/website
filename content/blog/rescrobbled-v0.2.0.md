+++
title = "rescrobbled v0.2.0"
date = 2020-08-12
+++

[Rescrobbled](https://github.com/InputUsername/rescrobbled) is a universal music scrobbling daemon
for MPRIS-enabled music players. It listens for active music players and submits songs to services
like Last.fm and ListenBrainz as they play.

## Changelog

- Improved usage instructions
- Renamed config options (old names still supported)
    - `api-key` => `lastfm-key`
    - `api-secret` => `lastfm-secret`
    - `lb-token` => `listenbrainz-token`
- Added music player whitelisting (by MPRIS identity or D-Bus bus name)
- Made Last.fm scrobbling optional

[Release](https://github.com/InputUsername/rescrobbled/releases/tag/v0.2.0)
