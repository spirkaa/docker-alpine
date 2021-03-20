FROM alpine:3.13
LABEL maintainer="Ilya Pavlov <piv@devmem.ru>"

ENV S6_OVERLAY_RELEASE=v2.2.0.3
ARG DOWNLOAD_URL=https://github.com/just-containers/s6-overlay/releases/download/${S6_OVERLAY_RELEASE}/s6-overlay-amd64.tar.gz

ADD ./rootfs /

RUN set -eux \
    && wget -nv -O /tmp/s6overlay.tar.gz "${DOWNLOAD_URL}" \
    && tar xzf /tmp/s6overlay.tar.gz -C / \
    && rm /tmp/s6overlay.tar.gz

ENTRYPOINT [ "/init" ]
