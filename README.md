# build-artifacts

Pre-built binary artifacts for [MaMpf](https://github.com/MaMpf-HD/mampf) dependencies.

## Contents

### OR-Tools (Debian 13 / trixie)

Pre-built [OR-Tools](https://github.com/google/or-tools) shared libraries for Debian 13 (trixie),
for use with the [`or-tools` Ruby gem](https://github.com/ankane/or-tools-ruby) on platforms not
yet officially supported by the gem.

| Asset | OR-Tools version | Platform |
|---|---|---|
| `or-tools-9.15-trixie-amd64.tar.gz` | 9.15 | Debian 13 (trixie), x86_64 |

#### Bundled libraries

The tarball contains `lib/` with OR-Tools and all its dependencies:
`abseil`, `protobuf`, `re2`, `HiGHS`, `SCIP`, `Coin-OR CBC/Clp/CoinUtils/Osi/Cgl/OsiClp`, `bzip2`, `zlib`.

#### How it's built

The tarball is built from [`or-tools-trixie/Dockerfile`](or-tools-trixie/Dockerfile),
which clones OR-Tools `v9.15` from Google's official repository and builds it with CMake
on `debian:trixie-slim`. The build is **manual** and run rarely — only when bumping the
OR-Tools or Debian version — with the exact build → extract → verify → upload steps
documented in that Dockerfile's header.

Consumers pin the asset by SHA256 (MaMpf's `docker/production/Dockerfile` and
`.devcontainer/boot.sh`), so a rebuilt tarball must either keep the same checksum or land
together with an update to those pins.

#### Licenses

- OR-Tools, abseil, SCIP (v9+): [Apache 2.0](https://www.apache.org/licenses/LICENSE-2.0)
- protobuf, re2: BSD 3-Clause
- HiGHS: MIT
- Coin-OR (CBC, Clp, CoinUtils, Osi, Cgl): [Eclipse Public License 2.0](https://www.eclipse.org/legal/epl-2.0/)
- bzip2: BSD-like
- zlib: zlib/libpng

All source code is available at the respective upstream repositories linked above.
