# GridCraft 0.8.0 for AROS — test 4

For AROS x86_64 ABIv11. Experimental and not optimised; the window is drawn by the CPU.

## Changes since test 3

- Typing into a selected cell no longer loses keys when they come faster than the window is redrawn. Before, only the first character reached the cell, and Enter, Tab, Escape, Backspace or an arrow key typed in between could act out of order. This may also explain keys reported lost while typing quickly, which has not been confirmed.
- A document opened from a Shell with a name relative to the current drawer (`GridCraft budget.xlsx`) is now remembered with its full path, so it opens from the Recent list after a start from Workbench or from another drawer. Entries that earlier versions stored with a short name stay as they were and may show as missing; open such a document once through the file requester.
- If GridCraft cannot lock the drawer it was started from, it now stops at once with return code 20 instead of starting with only a warning. This is a safeguard; no case is known where it happens.

The crash after closing that the test 3 notes describe as fixed was found in PdfCraft. In GridCraft no way for it to happen is known, so that change is a safeguard too.

## Installation

Unpack the archive into any drawer and double-click the GridCraft icon. From a Shell, run `Stack 4000000` before starting `GridCraft`. With apkg: `apkg install gridcraft`.

## Known limitations

- Closing the window does not ask about unsaved changes (as in GridCraft 0.8.0 elsewhere); save first, or use File › Close.
- The interface reacts slowly, most in a large window.
- No printing (export to PDF instead), no formatted copy and paste, no file drops.
- Letters typed with a keymap for another 8-bit charset (such as the Polish one) come out as the wrong characters.
- A file name with accented letters typed in a Shell is refused with "argument is not valid text"; use names without them.
