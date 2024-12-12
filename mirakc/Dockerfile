ARG TOOLS_TAG=debian
ARG DEBIAN_CODENAME=bookworm

FROM mirakc/tools:$TOOLS_TAG AS mirakc-tools

FROM --platform=$BUILDPLATFORM mirakc/buildenv:debian-$TARGETOS-$TARGETARCH$TARGETVARIANT AS mirakc-build
# COPY . ./
RUN git clone --depth 1 -b fix-web-openapi https://github.com/mirakc/mirakc.git /mirakc
WORKDIR /mirakc
ARG BUILDPLATFORM
ARG TARGETPLATFORM
ARG PROFILE=release
RUN sh ./docker/build-scripts/mirakc.sh debian $BUILDPLATFORM $TARGETPLATFORM $PROFILE

# FROM debian:$DEBIAN_CODENAME-slim AS mirakc
# LABEL maintainer="Contributors of mirakc"
# COPY --from=mirakc-tools /usr/local/bin /usr/local/bin/
# COPY --from=mirakc-build /usr/local/bin/mirakc /usr/local/bin/
# COPY --from=mirakc-build /build/resources/strings.yml /etc/mirakc/strings.yml
# ARG PROFILE=release
# RUN set -eux \
#  && export DEBIAN_FRONTEND=noninteractive \
#  && apt-get update \
#  && apt-get install -y --no-install-recommends ca-certificates curl dvb-tools jq socat \
#  && if [ "$PROFILE" = perf ]; then apt-get install -y --no-install-recommends heaptrack valgrind; fi \
#  # cleanup
#  && apt-get clean \
#  && rm -rf /var/lib/apt/lists/* \
#  && rm -rf /var/tmp/* \
#  && rm -rf /tmp/*
# ENV MIRAKC_CONFIG=/etc/mirakc/config.yml
# ENV MIRAKC_BUILD_PROFILE=$PROFILE
# EXPOSE 40772
# ENTRYPOINT ["/usr/local/bin/mirakc"]
# CMD []


FROM debian:bookworm-slim

RUN apt-get update && apt-get install -y --no-install-recommends \
    ca-certificates \
    curl \
    libdvbv5-0 \
    libpcsclite-dev \
    pkg-config \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# TODO: ARM64 support
RUN curl -sSLO https://github.com/kazuki0824/recisdb-rs/releases/download/1.2.2/recisdb_1.2.2-1_amd64.deb && \
    dpkg -i recisdb_1.2.2-1_amd64.deb && rm -f recisdb_1.2.2-1_amd64.deb

# COPY --from=docker.io/mirakc/mirakc:debian /usr/local/bin/* /usr/local/bin/
# COPY --from=docker.io/mirakc/mirakc:debian /etc/mirakc/strings.yml /etc/mirakc/strings.yml

COPY --from=mirakc-tools /usr/local/bin /usr/local/bin/
COPY --from=mirakc-build /usr/local/bin/mirakc /usr/local/bin/
COPY --from=mirakc-build /mirakc/resources/strings.yml /etc/mirakc/strings.yml


ENV MIRAKC_CONFIG=/etc/mirakc/config.yml

ENTRYPOINT ["/usr/local/bin/mirakc"]
