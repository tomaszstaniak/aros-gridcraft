# GridCraft 0.19.1 for AROS

The AROS port of GridCraft 0.19.1, a spreadsheet program that opens and saves XLSX workbooks, imports XLS, XLSB, ODS, CSV and JSON, and exports CSV, HTML and PDF. It is built for three AROS systems.

## Downloads

| File | System |
|---|---|
| `GridCraft-0.19.1-aros-x86_64-abiv11-pc.zip` | AROS x86_64, ABIv11 (AROS One 1.3) |
| `GridCraft-0.19.1-aros-x86_64-abiv1-pc.zip` | AROS x86_64, mainline ABIv1 (nightly 2026-11-10) |
| `GridCraft-0.19.1-aros-aarch64-abiv1-raspi.zip` | AROS on Raspberry Pi, AArch64 ABIv1 (mainline nightly 2026-11-10) |
| `GridCraft-0.19.1-source.zip` | Port sources, patches, pinned inputs and build recipe |

Each package runs only on the system it was built for. `SHA256SUMS` lists the checksums. The packages can also be installed with the AROS package manager: `apkg install gridcraft`.

## What is new

This release moves from GridCraft 0.8.0 to GridCraft 0.19.1. For the changes in the spreadsheet itself, see the release notes of GridCraft upstream. On the AROS side:

- The interface renderer is chosen at run time with the `GUI_RENDERER` ToolType or the `--gui-renderer` Shell argument: `CPU` (default), `AUTO` or `GPU`. The reason for the choice is written to the log.
- Closing the window with unsaved changes asks whether to save them.
- A picture on the clipboard (an ILBM from Capture, MultiView or a paint program) can be pasted into a sheet (not tested in this release).
- Packages for mainline x86_64 AROS and for the Raspberry Pi, next to the ABIv11 one.
- The log file `GridCraft.log` is kept in the settings folder.
- Updates come through the AROS package manager.
- Recent files from 0.8.0 are kept.

Fixed on the way: saving over an existing file failed on AROS with "File exists" in GridCraft 0.19.1 as it comes from upstream; and the program did not start on mainline AROS.

## Installation

Unpack the archive for your system anywhere writable and double-click GridCraft. From a Shell, run `GridCraft [--gui-renderer CPU|AUTO|GPU] [file]`. Settings, recent files and `GridCraft.log` are kept in `GridCraftData` inside the program drawer. When you upgrade from 0.8.0, unpack the new version to a new place and move `GridCraftData` from the old drawer into it. The archive also holds a `.arospkg` drawer with the package manager's description; after unpacking by hand it can be deleted.

`GridCraft --version` and the first line of `GridCraft.log` name the build; please include them when you report a problem.

Tested on AROS One 1.3 (ABIv11), on mainline x86_64 AROS nightly 2026-11-10 and on the Raspberry Pi nightly 2026-11-10 in an emulator, not on a physical Raspberry Pi. On all three: starting from a Shell, opening a workbook, editing cells, saving, saving again over the same file, undo and redo, closing, and the command-line program. On mainline x86_64 also typing into cells, copy and paste, the Save As requester and the question about unsaved changes.

## Known limitations

- Every change redraws the whole window, so on slow machines and in emulators the interface follows with a delay.
- No printing (export to PDF instead); files dropped on the window are not opened.
- All workbooks share one window.
- No formatted (HTML) clipboard content. The control server (`--control`) and `gridcraft-cli send` are not available.
- No automatic updates; use the AROS package manager.
- OpenGL: where only Mesa's software renderer is present, `AUTO` uses the CPU. `GPU` has not been tested in this release.
- Not tested on AROS in this release: pasting pictures, importing XLS, XLSB and ODS files, charts, PDF export, keyboard layouts other than the US one.
- Chinese, Japanese, Korean and emoji characters are kept in workbooks but may be shown as boxes.

Licences and notices for GridCraft and everything built into it are in the `Licenses` drawer of each package.
