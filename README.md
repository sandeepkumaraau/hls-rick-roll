# HLS Adaptive Video Streaming Player

This is a project to demonstrate the process of encoding a single video file into multiple resolutions for adaptive bitrate streaming using HLS, hosting the static files on GitHub Pages, and playing the stream using hls.js.

## Live Demo

You can view the live project here: **[https://sandeepkumaraau.github.io/hls-rick-roll/](https://sandeepkumaraau.github.io/hls-rick-roll/)**

## About The Project

The goal of this project was to learn and execute a complete video delivery pipeline. The key steps included:

* Taking a single source video (`input.mp4`).
* Encoding it into three different resolutions (720p, 480p, 360p).
* Creating 2-second video segments for each resolution.
* Packaging the output into the HLS (HTTP Live Streaming) format.
* Uploading all generated files to a static website host (GitHub Pages).
* Using the `hls.js` JavaScript library to create a browser-based player that adaptively streams the video.

## Technologies Used

* **[FFmpeg](https://ffmpeg.org/):** For video encoding and HLS packaging.
* **[hls.js](https://github.com/video-dev/hls.js/):** For client-side HLS playback in the browser.
* **[Git & GitHub](https://github.com/):** For version control and hosting.
* **[GitHub Pages](https://pages.github.com/):** For static web hosting.

## FFmpeg Command Used

This was the command used to encode the 720p source video into the three HLS variants.

```bash
ffmpeg -i input.mp4 \
-vf "scale=w=640:h=360"  -c:v h264 -b:v 800k  -c:a aac -b:a 96k \
-vf "scale=w=854:h=480" -c:v h264 -b:v 1400k -c:a aac -b:a 128k \
-vf "scale=w=1280:h=720" -c:v h264 -b:v 2800k -c:a aac -b:a 128k \
-map 0:v:0 -map 0:a:0 -map 0:v:0 -map 0:a:0 -map 0:v:0 -map 0:a:0 \
-f hls \
-hls_time 2 \
-hls_playlist_type vod \
-hls_segment_filename "output/v%v/segment%03d.ts" \
-master_pl_name master.m3u8 \
-var_stream_map "v:0,a:0,name:360p v:1,a:1,name:480p v:2,a:2,name:720p" output/v%v/stream.m3u8