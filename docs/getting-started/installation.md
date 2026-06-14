# Installation

> **Status:** The repository currently builds a minimal Rust binary. The collectors and
> APIs described elsewhere in these docs are not implemented yet. These steps cover
> building and running what exists today.

## Requirements

- **Rust** 1.85+ (edition 2024) — install via [rustup](https://rustup.rs/)
- A Linux host (developed and tested on Arch Linux)

## Build from source

```bash
git clone https://github.com/CK-Technology/halo.git
cd halo
cargo build --release
```

The release binary is written to `target/release/halo`.

## Run

```bash
./target/release/halo
```

## Development build

```bash
# Fast iterative build + run
cargo run

# Format and lint before committing
cargo fmt
cargo clippy --all-targets
```

## Next steps

- [Configuration](configuration.md) — config file layout (planned)
- [Deployment](deployment.md) — running on a server or in a container
- [Architecture Overview](../architecture/overview.md) — where this is headed
