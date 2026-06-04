FROM rust:slim-bookworm AS builder

WORKDIR /build
COPY . .

RUN cargo build --release -p iii && cp target/release/iii /build/iii

FROM busybox:1.36.1-uclibc AS busybox

FROM debian:bookworm-slim

RUN apt-get update && apt-get install -y --no-install-recommends \
    ca-certificates \
    && rm -rf /var/lib/apt/lists/*

COPY --from=busybox /bin/busybox /bin/busybox
COPY --from=builder /build/iii /app/iii
RUN ln -s /app/iii /usr/local/bin/iii

ENV III_EXECUTION_CONTEXT=docker
ENV III_ENV=development

EXPOSE 49134 3111 3112 9464
ENTRYPOINT ["/app/iii"]
CMD ["--config", "/app/config.yaml"]

LABEL org.opencontainers.image.title="III Engine" \
      org.opencontainers.image.vendor="iiidev" \
      org.opencontainers.image.source="https://github.com/iii-hq/iii"
