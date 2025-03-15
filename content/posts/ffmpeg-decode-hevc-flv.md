---
title: "Ffmpeg Decode Hevc Flv"
date: 2025-03-16T02:06:39+09:00
draft: false
---
## TL;DR

If you encountered issues with hevc encoded flv videos, try to use
latest ffmpeg from github releases.

## Story

When I tried to concatenate recorded live streaming videos by ffmpeg
as usual, it complained something like "Video codec (c) is not implemented".

Checking videos with `ffprobe` got following information:

```bash
Stream #0:0: Video: none ([12][0][0][0] / 0x000C), none`
```

However, my PotPlayer on windows machine get quite well with these videos.
So checked the codec with it and found that it was `hevc`.

LLMs suggested me use PotPlayer to convert the videos to mp4 by
replaying and recording them. NO WAY, what the he*l is this?
I have to concatenate and convert all the videos to mp4 by replaying them?

How can there not be an elegant way to get things done in my command line?

For reasons I don't give a sh*t, hevc encoded flv videos are not officially
supported by ffmpeg in a long time, and if you are using the ffmpeg downloaded
from package manager like apt or brew, they still do not work with
hevc encoded flv videos.

**However**, latest versions have added the support, as referred in this [answer](https://github.com/BililiveRecorder/BililiveRecorder/issues/653#issuecomment-2708931739).

It saved my day.
