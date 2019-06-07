FROM php:7.2-cli

RUN apt-get update && apt-get -y upgrade

RUN apt-get install -y --no-install-recommends wget zlib1g-dev libpq-dev git \
        libicu-dev \
        libxml2-dev \
        libpng-dev \
        libjpeg-dev \
        libmcrypt-dev \
        libxslt-dev \
        libfreetype6-dev \
        libssl-dev \
        libjpeg62-turbo-dev \
        ffmpeg \
        g++ \
        gcc \
        libc6-dev \
        make \
        pkg-config \
    && docker-php-ext-install zip \
    && docker-php-ext-configure gd --with-png-dir=/usr/local/ --with-jpeg-dir=/usr/local/ --with-freetype-dir=/usr/local/ \
    && docker-php-ext-configure pcntl --enable-pcntl \
    && docker-php-ext-install gd \
    && docker-php-ext-install bcmath \
    && docker-php-ext-install mbstring \
    && docker-php-ext-install pcntl \
    && docker-php-ext-install -j$(nproc) exif iconv

RUN touch /usr/local/etc/php/php.ini \
    && echo "phar.readonly = Off" >> /usr/local/etc/php/php.ini

RUN curl --insecure https://getcomposer.org/composer.phar -o /usr/bin/composer && chmod +x /usr/bin/composer

ENV GOLANG_VERSION 1.11.10

RUN set -eux; \
  \
# this "case" statement is generated via "update.sh"
  dpkgArch="$(dpkg --print-architecture)"; \
  case "${dpkgArch##*-}" in \
    amd64) goRelArch='linux-amd64'; goRelSha256='aefaa228b68641e266d1f23f1d95dba33f17552ba132878b65bb798ffa37e6d0' ;; \
    armhf) goRelArch='linux-armv6l'; goRelSha256='29812e3443c469de6b976e4e44b5e6402d55f6358a544278addc22446a0abe8b' ;; \
    arm64) goRelArch='linux-arm64'; goRelSha256='6743c54f0e33873c113cbd66df7749e81785f378567734831c2e5d3b6b6aa2b8' ;; \
    i386) goRelArch='linux-386'; goRelSha256='619ddab5b56597d72681467810c63238063ab0d221fe0df9b2e85608c10161e5' ;; \
    ppc64el) goRelArch='linux-ppc64le'; goRelSha256='a6c7129e92fe325645229846257e563dab1d970bb0e61820d63524df2b54fcf8' ;; \
    s390x) goRelArch='linux-s390x'; goRelSha256='35f196abd74db6f049018829ea6230fde6b8c2e24d2da9f9e75ce0e6d0292b49' ;; \
    *) goRelArch='src'; goRelSha256='df27e96a9d1d362c46ecd975f1faa56b8c300f5c529074e9ea79bdd885493c1b'; \
      echo >&2; echo >&2 "warning: current architecture ($dpkgArch) does not have a corresponding Go binary release; will be building from source"; echo >&2 ;; \
  esac; \
  \
  url="https://golang.org/dl/go${GOLANG_VERSION}.${goRelArch}.tar.gz"; \
  wget -O go.tgz "$url"; \
  echo "${goRelSha256} *go.tgz" | sha256sum -c -; \
  tar -C /usr/local -xzf go.tgz; \
  rm go.tgz; \
  \
  if [ "$goRelArch" = 'src' ]; then \
    echo >&2; \
    echo >&2 'error: UNIMPLEMENTED'; \
    echo >&2 'TODO install golang-any from jessie-backports for GOROOT_BOOTSTRAP (and uninstall after build)'; \
    echo >&2; \
    exit 1; \
  fi; \
  \
  export PATH="/usr/local/go/bin:$PATH"; \
  go version

ENV GOPATH /go
ENV PATH $GOPATH/bin:/usr/local/go/bin:$PATH

RUN mkdir -p "$GOPATH/src" "$GOPATH/bin" && chmod -R 777 "$GOPATH"
