# Builds OR-Tools v9.15 C++ library on Debian 13 (trixie).
#
# The resulting artifact (libortools.so + headers) is used by the
# or-tools Ruby gem (0.17.1) when building the MaMpf production Docker image,
# because the gem has no pre-built binary for Debian 13.
#
# Build and extract:
#
#   docker build -t or-tools-builder \
#     or-tools-trixie/
#
#   docker run --rm or-tools-builder \
#     tar czf - -C /opt/or-tools . \
#     > or-tools-9.15-trixie-amd64.tar.gz
#
# Then upload or-tools-9.15-trixie-amd64.tar.gz as a release asset to
# this repository (build-artifacts).
#
# After uploading, verify the runtime dependencies of libortools.so:
#
#   tar xzf or-tools-9.15-trixie-amd64.tar.gz --to-stdout \
#     ./lib/libortools.so | docker run --rm -i debian:trixie-slim \
#     bash -c 'cat > /tmp/libortools.so && ldd /tmp/libortools.so'
#
# If abseil/protobuf appear as external .so dependencies, include their
# .so files in the runtime Docker image alongside libortools.so.
# If they do not appear (statically bundled), only libortools.so is needed.

FROM debian:trixie-slim

RUN apt-get update && apt-get install -y \
    cmake \
    git \
    build-essential \
    pkg-config \
    && rm -rf /var/lib/apt/lists/*

ARG OR_TOOLS_VERSION=v9.15

RUN git clone --depth 1 --branch ${OR_TOOLS_VERSION} \
    https://github.com/google/or-tools.git /tmp/or-tools

RUN cmake -S /tmp/or-tools -B /tmp/build \
      -DBUILD_DEPS=ON \
      -DBUILD_SHARED_LIBS=ON \
      -DCMAKE_BUILD_TYPE=Release \
      -DBUILD_TESTING=OFF \
      -DBUILD_EXAMPLES=OFF \
      -DBUILD_SAMPLES=OFF && \
    cmake --build /tmp/build --parallel "$(nproc)" && \
    cmake --install /tmp/build --prefix /opt/or-tools && \
    rm -rf /tmp/or-tools /tmp/build
