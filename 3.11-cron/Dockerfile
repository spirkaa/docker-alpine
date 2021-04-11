FROM alpine:3.11
LABEL maintainer="Ilya Pavlov <piv@devmem.ru>"

ENV S6_OVERLAY_RELEASE=v2.2.0.3

ADD ./rootfs /
ADD https://github.com/just-containers/s6-overlay/releases/download/${S6_OVERLAY_RELEASE}/s6-overlay-amd64.tar.gz /tmp/s6overlay.tar.gz

RUN set -eux \
    && tar xzf /tmp/s6overlay.tar.gz -C / \
    && rm /tmp/s6overlay.tar.gz

ENTRYPOINT [ "/init" ]
