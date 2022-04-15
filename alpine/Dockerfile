FROM alpine:latest
LABEL maintainer="dreamkaylee@outlook.com"

ENV LANG=C.UTF-8
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/${TZ} /etc/localtime && echo ${TZ} > /etc/timezone

RUN set -eux && sed -i 's/dl-cdn.alpinelinux.org/mirrors.ustc.edu.cn/g' /etc/apk/repositories

RUN apk add --no-cache --virtual=.build-dependencies ca-certificates wget zstd && \
    wget -q -O /etc/apk/keys/sgerrand.rsa.pub https://alpine-pkgs.sgerrand.com/sgerrand.rsa.pub  && \
    \
    wget "https://archive.archlinux.org/packages/z/zlib/zlib-1%3A1.2.11-4-x86_64.pkg.tar.xz" -O libz.tar.xz && \
    mkdir -p libz && tar -xf libz.tar.xz -C libz && \
    mv libz/usr/lib/libz.so* /usr/glibc-compat/lib && \
    \
    /usr/glibc-compat/bin/localedef --force --inputfile POSIX --charmap UTF-8 "$LANG" || true && \
    echo "export LANG=$LANG" > /etc/profile.d/locale.sh && \
    \
    rm "/root/.wget-hsts" && \
    rm -rf /var/cache/apk/* && rm -rf libz/* && rm -rf libz.tar.xz && \
    apk del .build-dependencies

RUN apk add --update bash font-adobe-100dpi ttf-dejavu fontconfig tzdata unixodbc-dev