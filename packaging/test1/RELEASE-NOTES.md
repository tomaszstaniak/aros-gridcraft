# GridCraft 0.8.0 for AROS — test 1

First test build for x86_64 AROS ABIv11.

## What is included

- The GridCraft spreadsheet in a window on the Workbench screen: cells,
  formulas with recalculation, formatting, sheets, tables, charts and the
  formula bar.
- Opening and saving Excel workbooks (XLSX); import of XLS, XLSB, ODS, CSV and
  JSON; export to CSV, HTML and PDF.
- `gridcraft-cli` for viewing, formula evaluation, conversions, scripted edits,
  file checks and MCP automation.
- The standard AROS file requester for Open, Save As and Insert Picture.

## Installation

Unpack the archive into any drawer and double-click the GridCraft icon. From a
Shell, run `Stack 4000000` before starting the spreadsheet.

## Known limitations

- Closing the window does not ask about unsaved changes (as in GridCraft 0.8.0
  elsewhere); save first, or use File › Close.
- The interface reacts slowly, most in a large window.
- No printing (export to PDF instead), no formatted copy and paste, no file
  drops.
- Letters typed with a keymap for another 8-bit charset (such as the Polish
  one) come out as the wrong characters.
