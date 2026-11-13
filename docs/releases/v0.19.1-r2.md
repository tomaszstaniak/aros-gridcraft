# GridCraft 0.19.1 for AROS, revision 2

The AROS port of GridCraft 0.19.1, a spreadsheet program that opens and saves XLSX workbooks, imports XLS, XLSB, ODS, CSV and JSON, and exports CSV, HTML and PDF. It is built for three AROS systems.

## Downloads

| File | System |
|---|---|
| `GridCraft-0.19.1-r2-aros-x86_64-abiv11-pc.zip` | AROS x86_64, ABIv11 distributions |
| `GridCraft-0.19.1-r2-aros-x86_64-abiv1-pc.zip` | AROS x86_64, mainline ABIv1 (nightly 2026-11-06 or later) |
| `GridCraft-0.19.1-r2-aros-aarch64-abiv1-raspi.zip` | AROS on Raspberry Pi, AArch64 ABIv1 (mainline nightly 2026-11-06 or later) |
| `GridCraft-0.19.1-r2-source.zip` | Port sources, patches, pinned inputs and build recipe |

Each package runs only on the system it was built for. `SHA256SUMS` lists the checksums. The packages can also be installed with the AROS package manager: `apkg install gridcraft`.

## What is new in revision 2

Revision 2 is built from GridCraft's main branch of 2026-11-12 (still version 0.19.1) with changes of the AROS port. `GridCraft --version` prints `0.19.1-r2`.

- The window reacts faster with the CPU renderer. Only the parts of the window that changed are drawn again, so typing, moving the pointer and the blinking cursor no longer redraw everything. In the emulators used for testing, a typical frame of a scripted session took 0.1 to 0.25 s before and takes about 0.02 s now. Changes of the whole window (scrolling, zooming, opening a dialog) are several times faster too, and still follow with a delay on slow machines.
- The `GUI_RENDERER` ToolType in the icon is now `AUTO`, as in the other Craft programs, instead of `CPU`. With Mesa's software renderer, on mainline x86_64 and on the Raspberry Pi, `AUTO` uses the CPU as before.
- On mainline x86_64, `AUTO` now keeps to the CPU renderer. A program that had opened OpenGL there, even only to look at it, left its log file in use after it ended, and the Shell it was started from no longer redirected the output of later programs (seen on nightly 2026-11-10). `GPU` still opens OpenGL.
- The mainline x86_64 and Raspberry Pi packages need a nightly build from 2026-11-06 or later. On older ones the programs stop with a software failure at start; update the system first. This also applies to revision 1.
- Charts are kept between frames by GridCraft itself now; the port's own change for that (revision 1) is no longer needed.

With the package manager: `apkg upgrade gridcraft`. Settings and recent files in `GridCraftData` are kept.

## What was new in revision 1

- Saving no longer stops the window. Save, Save As and AutoSave write the file in the background: you can go on scrolling and typing while a large workbook is saved. What you type after choosing Save is not in that file and stays marked as unsaved. Closing the window waits until the file is written, and "Save" in the question about unsaved changes closes the workbook only after that.
- A chart is worked out once and redrawn from that until the workbook, its size or the zoom changes, instead of on every frame. This is a small gain: most of the time of a frame with a large chart is still spent drawing it.

## What was new in 0.19.1

This release moves from GridCraft 0.8.0 to GridCraft 0.19.1. For the changes in the spreadsheet itself, see the release notes of GridCraft upstream. On the AROS side:

- The interface renderer is chosen at run time with the `GUI_RENDERER` ToolType or the `--gui-renderer` Shell argument: `AUTO`, `CPU` or `GPU`. The reason for the choice is written to the log.
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

Revision 2 was tested on an ABIv11 distribution (AROS One 1.3), on mainline x86_64 AROS nightly 2026-11-10 and on the Raspberry Pi nightly 2026-11-10 in an emulator, not on a physical Raspberry Pi. On all three: an upgrade from revision 1 with the package manager (settings and a file of the user kept), then a scripted session from a Shell (editing cells, saving, saving again over the same file), a second start and removing the package. Not repeated for revision 2: the hand tests of 0.19.1 and revision 1 (typing into cells, copy and paste, the Save As requester, closing with unsaved changes).

## Known limitations

- Changes of the whole window (scrolling, zooming, opening a dialog) redraw all of it; on slow machines and in emulators these follow with a delay.
- No printing (export to PDF instead); files dropped on the window are not opened.
- All workbooks share one window.
- No formatted (HTML) clipboard content. The control server (`--control`) and `gridcraft-cli send` are not available.
- Saving shows no progress in the window.
- No automatic updates; use the AROS package manager.
- OpenGL: where only Mesa's software renderer is present, on mainline x86_64 and on the Raspberry Pi, `AUTO` uses the CPU. `GPU` has not been tested in this release.
- Not tested on AROS in this release: pasting pictures, importing XLS, XLSB and ODS files, charts, PDF export, keyboard layouts other than the US one.
- Chinese, Japanese, Korean and emoji characters are kept in workbooks but may be shown as boxes.

Licences and notices for GridCraft and everything built into it are in the `Licenses` drawer of each package.
