# GridCraft for AROS

An AROS port of [GridCraft](https://github.com/storytold/gridcraft), a spreadsheet. It opens and saves XLSX workbooks, imports XLS, XLSB, ODS, CSV and JSON, and exports CSV, HTML and PDF.

## Download and run

[Download v0.8.0-test2](https://github.com/tomaszstaniak/aros-gridcraft/releases/tag/v0.8.0-test2) for **AROS x86_64 ABIv11**, or find `gridcraft` in the apkg catalogue. The download includes GridCraft and `gridcraft-cli`.

These builds are experimental and not optimised. They use CPU rendering and can be slow, especially in large windows.

Unpack the archive into a drawer and double-click the GridCraft icon. From a Shell, run `Stack 4000000` before starting `GridCraft`.

## Known limitations

**Save before closing the window:** the close gadget does not ask about unsaved changes. File › Close does. Printing, formatted clipboard exchange and file drops are unavailable. Non-Latin-1 keyboard layouts can produce incorrect characters.

## Build from source

This repository contains source revisions, AROS patches and build scripts. See [Building](docs/development.md), [Patch workflow](docs/patching.md) and [Source revisions](upstreams.json). The window and renderer use [egui-aros](https://github.com/tomaszstaniak/egui-aros), pinned in `upstreams.json`.

The repository stores build recipes rather than a full upstream source tree. Licenses and author notices are included in the source and application archives.
