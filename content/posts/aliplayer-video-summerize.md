---
title: "Aliplayer Video Summerize"
date: 2025-08-15T10:33:23+08:00
draft: false
---

- Some domestic companies are using `AliPlayer` as their video streaming solution.

- As some may know, there are notorious "learning" **KPI**s, wasting your time
on those videos nobody gives a shit.

- So, why not make a script automatically transcribe the video to text and
make a summary.

## Basic approach

- when click playing videos in a browser, we can fetch the `m3u8` link
in the developer mode `(F12)`
- using `ffmpeg -i <m3u8_link> -c <output>`, we can download the video
without logging in to the website
- using whisper to make a transcription and somewhat `LLM` to conclude the transcription.

## Next Steps

- How to handle a playlist?
  - Good UX: automatically iterate through the playlist and fetch all `m3u8` links
  - Bad UX: let the user to click videos and fetch `m3u8` each time

- Can we use cookie for a session inside terminal
  - no longer need to open the link to the video every time
  - just feed the `url` to the video and let the magic happen
