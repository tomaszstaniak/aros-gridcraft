# GridCraft 0.8.0 for AROS — test 3

For AROS x86_64 ABIv11. Experimental and not optimised; the window is drawn by the CPU.

## Changes since test 1

- Fixed a possible crash after closing GridCraft when it was started from Workbench. A background thread could make the C library release the starting drawer's lock that Workbench releases again on exit; the program now keeps that lock for itself.
- The programs are built with whole-program optimisation and stripped of unneeded symbols, so they are smaller.
- GridCraft is available as `gridcraft` in the apkg catalogue. It installs with apkg 0.5.0 and later.

Test 2 was withdrawn: it exited at startup with "cannot preserve the startup directory". This build fixes that.

## Installation

Unpack the archive into any drawer and double-click the GridCraft icon. From a Shell, run `Stack 4000000` before starting `GridCraft`. With apkg: `apkg install gridcraft`.

## Known limitations

- Closing the window does not ask about unsaved changes (as in GridCraft 0.8.0 elsewhere); save first, or use File › Close.
- The interface reacts slowly, most in a large window.
- No printing (export to PDF instead), no formatted copy and paste, no file drops.
- Letters typed with a keymap for another 8-bit charset (such as the Polish one) come out as the wrong characters.
