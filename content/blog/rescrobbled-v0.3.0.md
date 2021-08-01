+++
title = "rescrobbled v0.3.0"
date = 2021-02-18
+++

[Rescrobbled](https://github.com/InputUsername/rescrobbled) is a universal music scrobbling daemon
for MPRIS-enabled music players. It listens for active music players and submits songs to services
like Last.fm and ListenBrainz as they play.

## Changelog

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

[Release](https://github.com/InputUsername/rescrobbled/releases/tag/v0.3.0)

## Patches

### v0.3.1 (2021-03-29)

- Fixed a typo in the config file template (`lastfm-token` => `lastfm-key`)

[Release](https://github.com/InputUsername/rescrobbled/releases/tag/v0.3.1)

### v0.3.2 (2021-04-19)

- Fixed config template typos (`min_play_time` => `min-play-time`, `player_whitelist` => `player-whitelist`)

[Release](https://github.com/InputUsername/rescrobbled/releases/tag/v0.3.2)

### v0.3.3 (2021-05-06)

- Added `-v` (`--version`) command-line switch to get the program's version
- Released on crates.io

[Release](https://github.com/InputUsername/rescrobbled/releases/tag/v0.3.3)
