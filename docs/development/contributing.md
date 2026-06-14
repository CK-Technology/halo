# Contributing

Halo is an experimental personal project, but contributions and ideas are welcome.

## Prerequisites

- **Rust** 1.85+ (edition 2024) via [rustup](https://rustup.rs/)
- Linux (developed on Arch Linux)

## Build & test

```bash
git clone https://github.com/CK-Technology/halo.git
cd halo

cargo build              # debug build
cargo run                # build + run
cargo test               # run tests
cargo fmt                # format
cargo clippy --all-targets   # lint
```

Run `cargo fmt` and `cargo clippy` before opening a pull request.

## Project layout

```
halo/
├── src/            # Rust source (currently a minimal binary)
├── docs/           # Documentation (this tree)
├── assets/         # Logo and branding
├── Cargo.toml
└── README.md
```

## Adding a new Eye

See [Eyes overview → Adding a new Eye](../eyes/overview.md#adding-a-new-eye). In short:
implement the common Eye interface, map the source to the normalized event, add a config
block, and write `docs/eyes/<name>.md`.

## Documentation conventions

- `docs/README.md` is the **only** capitalized README in `docs/` — it is the index.
- All other docs files use lowercase names and live in topic subfolders.
- Mark anything not yet built as **design intent** so readers know what is real.
- Keep it accurate and concise — no marketing copy.

## Scope

Before building a feature, check the [roadmap](roadmap.md) and the out-of-scope list. Halo
is a sandbox alongside the Heimdall stack, not a replacement for it.
