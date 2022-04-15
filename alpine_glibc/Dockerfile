FROM alpine:latest
LABEL maintainer="dreamkaylee@outlook.com"

ENV LANG=C.UTF-8
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/${TZ} /etc/localtime && echo ${TZ} > /etc/timezone

RUN set -eux && sed -i 's/dl-cdn.alpinelinux.org/mirrors.ustc.edu.cn/g' /etc/apk/repositories

RUN apk add --no-cache --virtual=.build-dependencies ca-certificates wget zstd && \
    GLIBC_VERSION="2.34-r0"  && \
    PROXY_URL="https://github.91chi.fun" && \
    ALPINE_GLIBC_REPO="https://github.com/sgerrand/alpine-pkg-glibc/releases/download" && \
    wget -q -O /etc/apk/keys/sgerrand.rsa.pub https://alpine-pkgs.sgerrand.com/sgerrand.rsa.pub  && \
    wget \
      ${PROXY_URL}/${ALPINE_GLIBC_REPO}/${GLIBC_VERSION}/glibc-${GLIBC_VERSION}.apk \
      ${PROXY_URL}/${ALPINE_GLIBC_REPO}/${GLIBC_VERSION}/glibc-bin-${GLIBC_VERSION}.apk \
      ${PROXY_URL}/${ALPINE_GLIBC_REPO}/${GLIBC_VERSION}/glibc-i18n-${GLIBC_VERSION}.apk && \
    apk add --no-cache \
      glibc-${GLIBC_VERSION}.apk \
      glibc-bin-${GLIBC_VERSION}.apk \
      glibc-i18n-${GLIBC_VERSION}.apk && \
    \
    wget "https://archive.archlinux.org/packages/z/zlib/zlib-1%3A1.2.11-4-x86_64.pkg.tar.xz" -O libz.tar.xz && \
    mkdir -p libz && tar -xf libz.tar.xz -C libz && \
    mv libz/usr/lib/libz.so* /usr/glibc-compat/lib && \
    \
    /usr/glibc-compat/bin/localedef --force --inputfile POSIX --charmap UTF-8 "$LANG" || true && \
    echo "export LANG=$LANG" > /etc/profile.d/locale.sh && \
    \
    apk del glibc-i18n && \
    rm "/root/.wget-hsts" && \
    rm -rf /var/cache/apk/* && rm -rf libz/* && rm -rf libz.tar.xz && \
    apk del .build-dependencies && \
    rm glibc-${GLIBC_VERSION}.apk && rm glibc-bin-${GLIBC_VERSION}.apk && rm glibc-i18n-${GLIBC_VERSION}.apk

RUN apk add --update bash font-adobe-100dpi ttf-dejavu fontconfig tzdata unixodbc-dev