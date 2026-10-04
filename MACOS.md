# macOS compatibility (`macos-compat` branch)

> **Experimental, not fully tested.** This branch exists to explore getting
> Orrery working fully on macOS. Upstream Orrery is Linux-native; nothing here
> is supported by upstream yet.

## Purpose

Find and fix what keeps Orrery from working as a first-class macOS app, while
keeping Linux behavior unchanged. Fixes that prove out may be offered upstream
to [Hankanman/Orrery](https://github.com/Hankanman/Orrery).

## Status

Tested so far on Apple Silicon (aarch64-apple-darwin): build, launch, repo
scan, general UI, and opening a PR from the PR view. Everything else is
untested.

| Area | State |
|---|---|
| Build (`cargo build --release`) | Works unmodified; GPUI uses its macOS/Metal backend |
| Launch, repo scan, general UI | Works |
| Open URL / folder (PR clicks, etc.) | **Fixed** — `launch::open` uses `open` on macOS instead of `xdg-open` |
| Open in IDE (`ideCommand`) | Works with an editor on PATH (e.g. `subl`) |
| Agent launch (`agentCommand`) | Default is `xterm -e …` — needs a macOS terminal configured |
| Tray, desktop notifications, global hotkey, KRunner | Linux D-Bus only; inert on macOS (KRunner logs "disabled" at startup) |
| Appearance / accent detection | Uses XDG portal + kdeglobals; untested on macOS |
| Local AI (llama.cpp / Ollama) | Untested |
| Packaging (`.app` bundle) | None; run the binary from `target/release/` |

## Build

Needs Rust (rustup selects the toolchain pinned in `rust-toolchain.toml`),
Xcode Command Line Tools, and `brew install cmake pkg-config`.

```sh
cargo build --release
./target/release/orrery
```

Config lives at `~/Library/Application Support/orrery/config.toml`.

## Keeping in sync

`origin` is the fork, `upstream` is Hankanman/Orrery:

```sh
git fetch upstream && git rebase upstream/main
```
