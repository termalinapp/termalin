# Termalin Connector, official container image.
#
# Built from the static (musl) release binaries, nothing else: no shell, no
# package manager, no SSH server. Put the binaries next to this file first:
#
#   bin/termalin-agent-x86_64     (linux/amd64)
#   bin/termalin-agent-aarch64    (linux/arm64)
#
#   docker buildx build --platform linux/amd64,linux/arm64 -t ghcr.io/<owner>/termalin-connector connector-image
#
# The Connector runs as uid 10001 and keeps its config in the /data volume.
# First start with TERMALIN_TOKEN enrols; later starts just connect. See
# docs/CONNECTOR_DOCKER.md.

# Runs on the build machine's own platform (no emulation needed): CA
# certificates, a passwd entry for uid 10001 and the /data folder it owns.
FROM --platform=$BUILDPLATFORM alpine:3.20 AS prep
ARG TARGETARCH
COPY bin/ /bin-in/
RUN set -e; \
    apk add --no-cache ca-certificates >/dev/null; \
    case "$TARGETARCH" in \
      amd64) f=termalin-agent-x86_64 ;; \
      arm64) f=termalin-agent-aarch64 ;; \
      *) echo "no Connector build for $TARGETARCH" >&2; exit 1 ;; \
    esac; \
    install -D -m 0755 "/bin-in/$f" /out/usr/local/bin/termalin-agent; \
    mkdir -p /out/etc/ssl/certs /out/data /out/tmp; \
    cp /etc/ssl/certs/ca-certificates.crt /out/etc/ssl/certs/; \
    echo 'termalin:x:10001:10001:Termalin Connector:/data:/sbin/nologin' > /out/etc/passwd; \
    echo 'termalin:x:10001:' > /out/etc/group; \
    chown 10001:10001 /out/data; chmod 0700 /out/data; chmod 1777 /out/tmp

FROM scratch
COPY --from=prep /out/ /
ENV PATH=/usr/local/bin \
    HOME=/data \
    XDG_CONFIG_HOME=/data \
    SSL_CERT_FILE=/etc/ssl/certs/ca-certificates.crt \
    TERMALIN_CONTAINER=1
USER 10001:10001
VOLUME ["/data"]
WORKDIR /data
LABEL org.opencontainers.image.title="Termalin Connector" \
      org.opencontainers.image.description="Reach servers, internal sites and desktops on this network from Termalin, without opening a port" \
      org.opencontainers.image.source="https://github.com/termalinapp/termalin" \
      org.opencontainers.image.url="https://termal.in"
ENTRYPOINT ["/usr/local/bin/termalin-agent"]
CMD ["run"]
