# ---------- BUILDER ----------
FROM debian:bookworm AS builder
ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
      build-essential \
      cmake \
      ca-certificates && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /src
COPY . .

RUN mkdir -p build && \
    cmake -Bbuild -H. -DDNP3_EXAMPLES=ON && \
    make -j$(nproc) -Cbuild

# ---------- RUNTIME ----------
FROM debian:bookworm-slim
ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
      ca-certificates && \
    rm -rf /var/lib/apt/lists/*

COPY --from=builder /src/build/cpp/lib/*.so* /usr/local/lib/
COPY --from=builder /src/build/cpp/examples/*/*-demo /usr/local/bin/
RUN chmod +x /usr/local/bin/*-demo || true && ldconfig

WORKDIR /usr/local/bin
