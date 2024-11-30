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

COPY --from=docker.io/mirakc/mirakc:debian /usr/local/bin/* /usr/local/bin/
COPY --from=docker.io/mirakc/mirakc:debian /etc/mirakc/strings.yml /etc/mirakc/strings.yml

ENV MIRAKC_CONFIG=/etc/mirakc/config.yml

ENTRYPOINT ["/usr/local/bin/mirakc"]
