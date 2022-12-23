+++
title = "rescrobbled v0.4.0"
date = 2021-05-07
description = "rescrobbled v0.4.0 changelog"
+++

[Rescrobbled](https://github.com/InputUsername/rescrobbled) is a universal music scrobbling daemon
for MPRIS-enabled music players. It listens for active music players and submits songs to services
like Last.fm and ListenBrainz as they play.

## Changelog

- Added ignore functionality for filter scripts:
    - Filter scripts that return nothing will cause the current track to be ignored/not scrobbled
    - This can be used to, for example, filter certain artists or songs entirely

[Release](https://github.com/InputUsername/rescrobbled/releases/tag/v0.4.0)
