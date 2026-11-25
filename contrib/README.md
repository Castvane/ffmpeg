# FFmpeg patches

The Dockerfiles download FFmpeg source and apply these patches before running `configure`.

| Patch | Behavior |
| --- | --- |
| [`ffmpeg-hls.patch`](ffmpeg-hls.patch) | Estimates bandwidth for HLS master playlists and logs a variant-to-stream map as `hls.streammap:`. |
| [`ffmpeg-jsonstats.patch`](ffmpeg-jsonstats.patch) | Adds JSON input, mapping, output, and per-stream progress records to the FFmpeg CLI. |
| [`posix-ioctl.patch`](posix-ioctl.patch) | Defines the POSIX ioctl macro for libv4l2 where needed on musl. The Alpine and Raspberry Pi container Dockerfiles apply this patch. |

## JSON statistics

JSON statistics are enabled by default in the patched CLI. Use `-jsonstats` to enable them explicitly, or `-nojsonstats -stats` to use FFmpeg's conventional progress display.

```sh
ffmpeg -jsonstats -i input.mp4 -f null -
```

JSON records are prefixed with `ffmpeg.inputs:`, `ffmpeg.mapping:`, `ffmpeg.outputs:`, or `ffmpeg.progress:`. Strip the prefix from each matching line before parsing the JSON. Applying the patch also adds `README.PROGRESS.md` to the FFmpeg source tree with examples.
