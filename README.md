# Castvane FFmpeg

Build recipes for FFmpeg 9.1 container images for Alpine Linux, Raspberry Pi, NVIDIA CUDA, and VAAPI. The images run `ffmpeg` as their entrypoint and show the version when started without arguments.

## Container images

The publishing workflows target `ghcr.io/castvane/ffmpeg` with the following tags. Check [GitHub Actions](https://github.com/Castvane/ffmpeg/actions?query=branch%3Amain) for build and publication status.

| Variant | Image tag | Target platforms |
| --- | --- | --- |
| [Alpine 3.23](.github/workflows/build_base_alpine.yaml) | `ffmpeg9.1-alpine3.23` | `linux/amd64`, `linux/arm64`, `linux/arm/v7` |
| [Raspberry Pi on Alpine 3.23](.github/workflows/build_base_alpine-rpi.yaml) | `ffmpeg9.1-rpi-alpine3.23` | `linux/arm64`, `linux/arm/v7` |
| [CUDA 12.9.1 on Ubuntu 24.04](.github/workflows/build_base_ubuntu-cuda.yaml) | `ffmpeg9.1-cuda-ubuntu24.04-cuda12.9.1` | `linux/amd64` |
| [CUDA 13.3.0 on Ubuntu 26.04](.github/workflows/build_base_ubuntu-cuda.yaml) | `ffmpeg9.1-cuda-ubuntu26.04-cuda13.3.0` | `linux/amd64` |
| [VAAPI on Ubuntu 26.04](.github/workflows/build_base_ubuntu-vaapi.yaml) | `ffmpeg9.1-vaapi-ubuntu26.04` | `linux/amd64` |

Once the Alpine image is available to your account, check its version with:

```sh
docker run --rm ghcr.io/castvane/ffmpeg:ffmpeg9.1-alpine3.23 -version
```

The Raspberry Pi build currently passes OpenMAX options on `linux/arm/v7` that FFmpeg 9.1 no longer accepts. The multi-platform image build can fail before publishing either architecture.

## Build locally

Install Docker with Buildx, then build and check the default Alpine image:

```sh
git clone https://github.com/Castvane/ffmpeg.git
cd ffmpeg
./Build.sh default
docker run --rm castvane/ffmpeg:ffmpeg9.1-alpine3.23 -version
```

| `Build.sh` target | Build configuration |
| --- | --- |
| `default` | Alpine 3.23 for `linux/amd64` |
| `default_native` | Alpine 3.23 for the Docker builder's default platform |
| `rpi` | Raspberry Pi on Alpine 3.21, using the builder's default platform |
| `cuda12` | Ubuntu 24.04. Set `CUDA_VERSION=12.9.1` when invoking the script to match CI. |
| `cuda13` | Ubuntu 26.04. Set `CUDA_VERSION=13.3.0` when invoking the script to match CI. |
| `vaapi` | Ubuntu 26.04 for `linux/amd64` |

See the [build script](Build.sh) for image tags and the Dockerfile for each variant for its FFmpeg configuration. The [binary export workflow](.github/workflows/build_base_alpine_static.yaml) is manually dispatched on a self-hosted runner and is configured to export a workflow artifact.

## Patches

The builds include patches for HLS playlist bandwidth estimation and JSON stream statistics. The Alpine container builds also apply the musl V4L2 ioctl patch. See the [patch documentation](contrib/README.md) for the available log prefixes and options.

## Publishing

Pushes to `main` trigger the four container publishing workflows. You can also dispatch them from [GitHub Actions](https://github.com/Castvane/ffmpeg/actions). They authenticate to GHCR with the workflow's `GITHUB_TOKEN`. Pulling a private package requires authenticated access to that package.

For optional Docker Hub publishing, add the `DOCKERHUB_USERNAME` and `DOCKERHUB_TOKEN` Actions secrets and set the `DOCKERHUB_IMAGE` Actions variable to a lowercase `namespace/repository`. The token needs write access to that repository. When the variable is set, the workflows target both registries with the same tags.

## Licensing

See [LICENSE](LICENSE) for Castvane's MIT license. FFmpeg and its dependencies have their own license terms. These Dockerfiles use `--enable-nonfree`, which FFmpeg marks as producing an unredistributable binary. Review the build configuration before distributing images.
