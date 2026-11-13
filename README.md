# GridCraft for AROS

An AROS port of [GridCraft](https://github.com/storytold/gridcraft), a spreadsheet. It opens and saves XLSX workbooks, imports XLS, XLSB, ODS, CSV and JSON, and exports CSV, HTML and PDF.

## Download and run

[Download 0.19.1 (revision 2)](https://github.com/tomaszstaniak/aros-gridcraft/releases/tag/v0.19.1-r2) for one of these systems, or install `gridcraft` with arospkg:

| Package | System |
|---|---|
| `GridCraft-0.19.1-r2-aros-x86_64-abiv11-pc.zip` | AROS x86_64, ABIv11 distributions |
| `GridCraft-0.19.1-r2-aros-x86_64-abiv1-pc.zip` | AROS x86_64, mainline ABIv1 (nightly 2026-11-06 or later) |
| `GridCraft-0.19.1-r2-aros-aarch64-abiv1-raspi.zip` | AROS on Raspberry Pi, AArch64 ABIv1 (mainline nightly 2026-11-06 or later) |

Each package runs only on the system it was built for and includes GridCraft and `gridcraft-cli`. Unpack the archive anywhere writable and double-click the GridCraft icon; from a Shell, run `GridCraft [file]`. The program takes the stack it needs itself. `ReadMe.txt` in the drawer describes installation, the command line, the clipboard and the interface renderer.

## Known limitations

Changes of the whole window (scrolling, zooming) redraw all of it and follow with a delay on slow machines and in emulators. There is no printing (export to PDF instead), no file drops and no formatted clipboard content, and all workbooks share one window. OpenGL was not tested in this release, and the Raspberry Pi build was tested only in an emulator. The [release notes](docs/releases/v0.19.1-r2.md) list the rest.

## Build from source

This repository contains source revisions, AROS patches and build scripts. See [Building](docs/development.md), [Patch workflow](docs/patching.md) and [Source revisions](upstreams.json). The window, clipboard and start-up code use [egui-aros](https://github.com/tomaszstaniak/egui-aros), pinned in `upstreams.json`, which also holds the build tools shared by the Craft ports; the Rust standard library for AROS comes from [rust-aros](https://github.com/tomaszstaniak/rust-aros).

The repository stores build recipes rather than a full upstream source tree. The source archive of each release adds the pinned inputs. Licenses and author notices are included in the source and application archives.
