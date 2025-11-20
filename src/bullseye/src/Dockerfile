FROM openmage/debian:bullseye-latest

COPY root /

## configure default environment stuff and file permissions
## block packages from being installed
RUN set -xe; \
    chmod 755 /usr/local/bin/*; \
    mkdir /home/www-data; \
    chmod 711 /home/www-data; \
    chown www-data:www-data /home/www-data; \
    usermod -d /home/www-data www-data; \
    \
    	{ \
    		echo 'Package: libjpeg*'; \
    		echo 'Pin: release *'; \
    		echo 'Pin-Priority: -1'; \
    	} > /etc/apt/preferences.d/no-libjpeg; \
    	{ \
    		echo 'Package: libtiff*'; \
    		echo 'Pin: release *'; \
    		echo 'Pin-Priority: -1'; \
    	} > /etc/apt/preferences.d/no-libtiff; \
    	{ \
    		echo 'Package: libwebp*'; \
    		echo 'Pin: release *'; \
    		echo 'Pin-Priority: -1'; \
    	} > /etc/apt/preferences.d/no-libwebp;

ARG IMAGICK_RUNTIME_REQUIREMENTS="libpng16-16 liblcms2-2 libgomp1 libltdl7 bzip2 gosu brotli"
ARG IMAGICK_RUNTIME_REQUIREMENTS_EXTRA=""
ARG IMAGICK_BUILD_REQUIREMENTS="curl cmake gcc libtool libedit-dev liblcms2-dev build-essential autoconf automake pkg-config libpng-dev libltdl-dev nasm"
ARG IMAGICK_BUILD_REQUIREMENTS_EXTRA=""
ARG IMAGICK_EXTRA_CONFIGURE_ARGS=""

ARG MOZJPEG_VERSION="4.1.1"
ARG MOZJPEG_EXTRA_CONFIGURE_ARGS=""

ARG TIFF_VERSION="4.7.1"
ARG TIFF_EXTRA_CONFIGURE_ARGS=""

ARG WEBP_VERSION="1.6.0"
ARG WEBP_EXTRA_CONFIGURE_ARGS=""

ARG IMAGICK_VERSION="7.1.2-8"
ARG IMAGICK_EXTRA_CONFIGURE_ARGS=""

ARG OPENJPEG_VERSION="2.5.4"

## configure imagick and the dependencies
RUN set -xe; \
    \
    /usr/local/bin/docker-install-requirements imagick; \
    mkdir -p /tmp/mozjpeg; \
    mkdir -p /tmp/tiff; \
    mkdir -p /tmp/libwebp; \
    mkdir -p /tmp/openjpeg;
################################################
## install mozjpeg
################################################
WORKDIR /tmp/mozjpeg
RUN docker-package-download -o mozjpeg.tar.gz -s https://github.com/mozilla/mozjpeg/archive/refs/tags/v${MOZJPEG_VERSION}.tar.gz; \
    tar --strip 1 -xzf mozjpeg.tar.gz; \
    cmake -G"Unix Makefiles" -DCMAKE_INSTALL_PREFIX=/usr -DCMAKE_INSTALL_LIBDIR=/usr/lib -DWITH_JPEG8=true; \
    make install prefix=/usr libdir=/usr/lib64;
################################################
## install tiff
################################################
WORKDIR /tmp/tiff
RUN docker-package-download -o tiff.tar.gz -s http://download.osgeo.org/libtiff/tiff-${TIFF_VERSION}.tar.gz; \
    tar --strip 1 -xzf tiff.tar.gz; \
    ./configure \
            --prefix=/usr \
            ${TIFF_EXTRA_CONFIGURE_ARGS:-} \
            ; \
    make -j"$(nproc)"; \
    make install;
################################################
## install webp
################################################
WORKDIR /tmp/libwebp
RUN docker-package-download -o libwebp.tar.gz -s https://storage.googleapis.com/downloads.webmproject.org/releases/webp/libwebp-${WEBP_VERSION}.tar.gz; \
    tar --strip 1 -xzf libwebp.tar.gz; \
    ./configure \
            --prefix=/usr \
            ${WEBP_EXTRA_CONFIGURE_ARGS:-} \
            ; \
    make -j"$(nproc)"; \
    make install;
################################################
## install openjpeg
################################################
WORKDIR /tmp/openjpeg
RUN docker-package-download -o openjpeg.tar.gz -s https://github.com/uclouvain/openjpeg/archive/refs/tags/v${OPENJPEG_VERSION}.tar.gz; \
    tar --strip 1 -xzf openjpeg.tar.gz; \
    mkdir build;
WORKDIR /tmp/openjpeg/build
RUN cmake .. -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=/usr; \
    make -j"$(nproc)"; \
    make install;
################################################
## install imagick
################################################
WORKDIR /tmp/imagemagick
RUN docker-package-download -o imagemagick.tar.gz -s https://github.com/ImageMagick/ImageMagick/archive/refs/tags/${IMAGICK_VERSION}.tar.gz; \
    tar --strip 1 -xzf imagemagick.tar.gz; \
    ./configure \
            --prefix=/usr \
            --with-webp \
            --without-perl \
            --without-x \
            --without-xml \
            --without-pango \
            --without-jbig \
            --without-wmf \
            --with-perl=no \
            --with-modules \
            ${IMAGICK_EXTRA_CONFIGURE_ARGS:-} \
            ; \
    make -j"$(nproc)"; \
    make install; \
    /usr/local/bin/docker-layer-clean
