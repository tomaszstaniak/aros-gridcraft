---
type: guide
updated: 2026-10-20
---

# Development

## Current capabilities

A pinned GridCraft source plus an explicit patch series builds the editor
(`GridCraft`) and the command line (`gridcraft-cli`, including the MCP server
over stdio) for **x86_64 AROS ABIv11** with a private Rust std toolchain. Both
have been run on an ABIv11 guest (older AROS One 1.3 installation); see "What
has been run" below for the exact scope. Mainline ABIv1 is not built.

## Targets

| Architecture / ABI | Priority | Compile | Link | Runtime |
|---|---|---|---|---|
| x86_64 / ABIv11, older SDK (AROS One 1.3) | Primary | yes | yes | editor and CLI checks pass (see below) |
| x86_64 / ABIv11, 2026.09 SDK | Primary, separate profile | not run | not run | not run |
| x86_64 / mainline v1 | Secondary | not run | not run | not run |

Mainline v1 needs mainline's own Rust cross compiler (`configure --enable-rust`
in the AROS tree: rustc 1.98.1, `panic = abort`, `mmap` and sockets as ENOSYS
stubs) and its crate store; the ABIv11 profile below does not build for it. The
port's own changes are written for `target_os = "aros"` without ABIv11-only
assumptions, and the egui window layer (egui-aros) is built on mainline's
`aros-gui-sys`, but nothing has been compiled for v1 yet.

## Requirements

- An x86_64 ABIv11 GCC 10.5 cross toolchain and the matching SDK. The C glue,
  the patched pthread objects and the Rust link must all use the same SDK:
  ABIv1 and ABIv11 pass `SysBase` differently, and a mismatch crashes inside the
  first system call instead of failing to build.
- A private Rust std toolchain for AROS registered with rustup, reporting
  exactly `rustc 1.100.0-nightly (0dfb098f3 2026-08-31)`, built with the
  rust-aros std patch tooling.
- A rust-aros checkout (target template `x86_64-aros.json.in` and
  `std-groundwork/libc-aros`, at the commit named in `scripts/gcconfig.py`).
- A deadwood2/AROS checkout containing the commit named in
  `scripts/gcconfig.py` (its `compiler/pthread` source is read, never changed).
- An egui-aros checkout containing the commit pinned in `upstreams.json`.
- Python 3.11+, Git, `patch`, and network access once for `cargo fetch`.

## Configuration

`scripts/gcconfig.py` is the one place for paths. Each setting comes from an
environment variable, else the ignored `local.json` (copy
`local.example.json`), else a default. `python3 scripts/gc.py config` shows
where each value comes from; a missing value is reported with the variable that
sets it.

## Build

```sh
python3 scripts/gc.py fetch          # clone/verify upstream/gridcraft at its pin
python3 scripts/gc.py materialize    # work/gridcraft = pin + patches/gridcraft/series; never overwrites
CARGO_HOME=$PWD/deps/v11/cargo-home cargo fetch --manifest-path work/gridcraft/Cargo.toml   # once, online
python3 scripts/gc.py build --online # once, for the std sources' own dependencies
python3 scripts/gc.py build          # GridCraft and gridcraft-cli
python3 scripts/gc.py build -p gridcraft-cli
python3 scripts/gc.py host test -p gridcraft-aros   # host cargo with egui-aros patched in
```

Each build writes `build/v11/out/<time>-<id>/` with the binaries and
`manifest.json`: work-copy commit and dirtiness, upstream pins, crate and
libc-aros patch digests, the pthread objects, the egui-aros vendor record,
toolchain identity, the cargo command, binary SHA-256 and a
check that no symbol is left undefined. Builds use the committed work-copy tree
unless `--worktree` is given (recorded as such). Ambient `RUSTFLAGS`,
`CARGO_TARGET_DIR` and similar overrides are refused.

What the build adds around the pinned source:

- egui-aros at its pinned commit, prepared by its own `scripts/ea.py vendor`:
  the window, input, CPU renderer and ASL layer, and its crate patches (winit,
  egui-winit, eframe, glutin, raw-window-handle, aros-gui-sys) as `[patch]`
  entries.
- `patches/crates/`: rayon-core 1.13.0 (`terminate_global_thread_pool`) and
  parking_lot_core 0.9.12 (realtime condvar deadlines on AROS).
- `patches/libc-aros/`: a `memchr` declaration for rust-aros libc-aros.
- `patches/aros-pthread/`: the pthread link library's detach fix. The unpatched
  `compiler/pthread` is first rebuilt and compared with the SDK's
  `libpthread.a`, object for object; then the two patched objects go first on
  the link line.
- The unwind panic profile (`-Cpanic=unwind -Cforce-unwind-tables=yes`) by
  default; `GRIDCRAFT_PANIC=abort` selects abort.

Never strip the binaries fully; `x86_64-aros-strip --strip-debug` is safe.

## The spreadsheet on AROS

`apps/gridcraft-aros` replaces the desktop app's eframe/wgpu window with
egui-aros: an Intuition window on the default public screen, drawn in software.
Open, Save As and Insert Picture use ASL requesters with the desktop app's
filters and save types; the chosen path is absolute. Copy writes plain text to
the system clipboard and Paste reads it. Links open with `OpenURL`, limited to
http, https and mailto as on the desktop. Settings live in
`PROGDIR:GridCraftData` (or `GRIDCRAFT_CONFIG_DIR`). The desktop theme watcher
is not started (AROS has no desktop theme, and its polling thread would keep
the pthread library's exit handler waiting). The loop sleeps until input, a
repaint the app asks for, or a wake-up from another thread; there is no idle
poll.

Not available, and reported as such: printing (no printers are offered), HTML
on the clipboard (plain text is used), file drops, the control server
(`--control` is refused). As in GridCraft 0.8.0 on other systems, closing the
window does not ask about unsaved changes; File › Close does.

The spreadsheet needs a large stack: from a Shell, `Stack 4000000` first.
`gridcraft-cli` runs its command on a thread with an 8 MiB stack.

`GridCraft --screenshot FILE.png [--screenshot-frames N]` writes the window's
pixels after N frames and quits. `GRIDCRAFT_LOG=file` appends the log to a file;
`RUST_LOG=info` or `debug` raises the level (`debug` logs every frame).
`GRIDCRAFT_TRACE_INPUT=1` logs every frame's key and text events and the focused
widget.

## What has been run

On 2026-10-20, ABIv11 guest with the older AROS One 1.3 installation, QEMU TCG,
1024x768 Workbench screen. Guest scripts are in `tests/guest/`.

- CLI: `version`, `info`, `cat` (values and formulas), `eval`, `convert` to CSV,
  `run` with a cell edit saved to XLSX, a second `run` adding a formula and
  saving over the same file (recalculated: `=C2*2` gave 50), `check` (3 of 3
  formulas matched, round trip equal), MCP `initialize` and `tools/list` over
  stdio. Every command returned 0.
- Spreadsheet: opening an XLSX (path with a space) with computed formulas,
  typing a formula into a cell and into the formula bar (`=C2*10` gave 75),
  Ctrl+S over the opened file and reopening it, copying a cell and pasting it in
  a Shell (`7.5`), resizing, three unattended starts in a row with identical
  screenshots, exit code 0 each time, and the desktop usable after closing.
- Idle: with the window untouched, QEMU used 0.37 CPU-seconds per 30 seconds
  (the bare desktop 0.73).

Not run: charts, pivot tables, encrypted workbooks, XLS/XLSB/ODS import, PDF
export from the window, large workbooks, the 2026.09 ABIv11 base, real
hardware, mainline v1.
