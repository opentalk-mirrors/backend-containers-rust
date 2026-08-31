# Rust Container Images

This repository contains Dockerfiles and configuration for building Rust container images.

## Images

- `alpine-3.22.5`: Alpine Linux 3.22.5 based image
- `alpine-3.24.1`: Alpine Linux 3.24.1 based image
- `alpine-edge`: Alpine Linux edge based image
- `trixie`: Debian Trixie based image
- `trixie/slim`: Debian Trixie slim based image

## Build

To build any of the images, use:

```bash
docker build -t <image-name> -f images/<image-name>/Dockerfile .
```

For example:
```bash
docker build -t rust-alpine -f images/alpine-3.22.5/Dockerfile .
```

## License

This project is licensed under the EUPL-1.2 license. See [LICENSE](LICENSES/EUPL-1.2.txt) for more information.
