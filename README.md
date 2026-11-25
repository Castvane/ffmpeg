# FFmpeg Base

FFmpeg 9.1 container images for Alpine, Raspberry Pi, CUDA and VAAPI.

[![alpine](https://github.com/Castvane/ffmpeg/actions/workflows/build_base_alpine.yaml/badge.svg)](https://github.com/Castvane/ffmpeg/actions/workflows/build_base_alpine.yaml)
[![alpine-rpi](https://github.com/Castvane/ffmpeg/actions/workflows/build_base_alpine-rpi.yaml/badge.svg)](https://github.com/Castvane/ffmpeg/actions/workflows/build_base_alpine-rpi.yaml)
[![base:ubuntu-ffmpeg-vvapi](https://github.com/Castvane/ffmpeg/actions/workflows/build_base_ubuntu-vaapi.yaml/badge.svg)](https://github.com/Castvane/ffmpeg/actions/workflows/build_base_ubuntu-vaapi.yaml)
[![ubuntu-cuda](https://github.com/Castvane/ffmpeg/actions/workflows/build_base_ubuntu-cuda.yaml/badge.svg)](https://github.com/Castvane/ffmpeg/actions/workflows/build_base_ubuntu-cuda.yaml)

FFmpeg 9.1

## Config:

```sh
--enable-libv4l2
--enable-libfreetype
--enable-alsa
--enable-libsrt
--enable-libx264
--enable-libx265
--enable-libvpx
--enable-libmp3lame
--enable-libopus
--enable-libvorbis
```

_Additional informations can be found in the Dockerfiles._

## Patches ([contrib](contrib/)):

- JSON-Stats (expands progress data per file in json format)
- HLS Bitrate (calculates bitrate estimate for HLS master playlist)

## Container images

All tags below use `ghcr.io/castvane/ffmpeg`.

| Tag | Platforms |
| --- | --- |
| `ffmpeg9.1-alpine3.23` | amd64, arm64, arm/v7 |
| `ffmpeg9.1-rpi-alpine3.23` | arm64, arm/v7 |
| `ffmpeg9.1-cuda-ubuntu24.04-cuda12.9.1` | amd64 |
| `ffmpeg9.1-cuda-ubuntu26.04-cuda13.3.0` | amd64 |
| `ffmpeg9.1-vaapi-ubuntu26.04` | amd64 |

## Build & test

```sh
$ git clone https://github.com/Castvane/ffmpeg.git
$ cd ffmpeg
$ ./Build.sh {arg}
```

Build targets are `default`, `default_native`, `rpi`, `cuda12`, `cuda13` and `vaapi`.

## Known problems:

The libraries are currently not compiled due to errors caused by Docker virtualisation.

## Feature requests:

Please create an issue with your use case and all the requirements.

## Licence

LGPL-licensed with optional components licensed under GPL. Please refer to the LICENSE file for detailed information.

## Publishing container images

Pushes to the `main` branch publish the Alpine, Raspberry Pi, CUDA and
VAAPI images to `ghcr.io/castvane/ffmpeg`. Each image workflow
can also be run manually from the repository's Actions tab.

For example, pull the Alpine image with:

```sh
docker pull ghcr.io/castvane/ffmpeg:ffmpeg9.1-alpine3.23
```

GitHub creates new container packages with private visibility. To allow
anonymous pulls, change the package visibility in its GitHub settings.

### Enable Docker Hub publishing later

In the GitHub repository's **Settings → Secrets and variables → Actions**:

1. Add the `DOCKERHUB_USERNAME` and `DOCKERHUB_TOKEN` secrets. Use a Docker
   Hub access token with write access to the destination repository.
2. Set the `DOCKERHUB_IMAGE` repository variable to a lowercase
   `namespace/repository` value for a Docker Hub repository you control.
3. Run the image workflows again. They publish the same tags to both registries.
