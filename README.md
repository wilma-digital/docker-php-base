# PHP Base Image (WIP)

This Repo contains the build instructions for the PHP base image (bullseye and bookworm). All PHP images should inherit from this image.

## build image

cd src/trixie/src &&  docker buildx create --use &&  docker buildx build --progress=plain --platform linux/amd64,linux/arm64 --push -t docker.io/openmage/php-base:trixie .


## Documentation

This image is automatically build with following cron expression `0 0 * * *`.

## Releases

- [JPEG](https://github.com/mozilla/mozjpeg/releases)
- [TIFF](https://libtiff.gitlab.io/libtiff/releases/index.html)
- [WEBP](http://downloads.webmproject.org/releases/webp/index.html)
- [JPEG](https://github.com/uclouvain/openjpeg/releases/)

### Environment vars

| Argument | Default Value | Description |
|:---------|:--------------|:------------|

## Contributing

## Changelog

All notable changes to this project will be documented in this section.

### 2025-11-20

- updated jpeg to 2.5.4
- updated Tiff to 4.7.1
- updated WebP to 1.6.0
- updated Imagick to 7.1.2-8

### 2020-02-24

- removed unused if statement

### 2020-02-20

- added source prepare script to get php src by version argument
- removed duplicate if statement in docker-entrypoint
- updated MozJPEG to 1d2320994dd0d293d39cfaea3d13060b60f32c45
- updated Tiff to 4.1.0
- updated WebP to 1.1.0
- updated OpenJPEG to 563ecfb55ca77c0fc5ea19e4885e00f55ec82ca9
- updated ImageMagick to 7.0.9-24

### 2020-02-18

- added workflow to trigger dockerhub autobuild

### 2020-12-27
- updated MozJPEG to 4.0.0
- updated Tiff to 4.2.0
- updated OpenJPEG to 2.3.1
- updated ImageMagick to 7.0.10-53
