# GridCraft 0.8.0 for AROS — ABIv11 test 4

GridCraft is a spreadsheet program. It opens and saves Excel workbooks (XLSX), imports XLS, XLSB, ODS, CSV and JSON, and exports CSV, HTML and PDF. The archive includes two programs for x86_64 AROS:

- **GridCraft**: the spreadsheet.
- **gridcraft-cli**: a command-line tool. It shows a workbook's sheets and cells, evaluates formulas, converts between formats, runs editing commands, checks files and serves an MCP automation interface over standard input and output.

This is an early test build. The target is AROS x86_64 ABIv11. The interface is drawn by the CPU, so it reacts slowly, more in a large window than in a small one.

## Requirements

- AROS x86_64 ABIv11.
- About 75 MB of disk space. RAM use depends on the document.
- No extra libraries. Both programs use only libraries that come with AROS, and nothing in the system is replaced or added.

## Installation

Unpack the archive into any drawer, for example `Work:`:

    UnZip GridCraft-0.8.0-test4.x86_64-aros-v11.zip -d Work:

This creates the drawer `Work:GridCraft` and its icon. To remove GridCraft, delete that drawer and its icon. GridCraft keeps its settings in `GridCraftData` inside its own drawer; set the variable `GRIDCRAFT_CONFIG_DIR` to keep them elsewhere.

## Starting the spreadsheet

Double-click the **GridCraft** icon. The icon gives the program the stack it needs (4,000,000 bytes).

From a Shell, set the stack first:

    Stack 4000000
    Work:GridCraft/GridCraft
    Work:GridCraft/GridCraft "Work:Accounts/budget.xlsx"
    Work:GridCraft/GridCraft --sample budget

Files named on the command line are opened at start; `--sample` opens one of the samples (`budget`, `sales`, `grades`). Open, Save As and Insert Picture use the standard file requester.

**Save before you close the window.** As in GridCraft 0.8.0 on other systems, the window's close gadget closes it without asking about unsaved changes; File › Close does ask.

## Command line

`gridcraft-cli` sets up the stack it needs itself:

    gridcraft-cli info budget.xlsx
    gridcraft-cli cat budget.xlsx --range A1:D20
    gridcraft-cli eval "=SUM(1,2)**3"
    gridcraft-cli convert budget.xlsx budget.csv
    gridcraft-cli --help

In the AROS Shell an asterisk inside quotes starts an escape, so write `**` for a `*` in a formula, as above.

`gridcraft-cli mcp` runs an MCP server on standard input and output for an MCP client that starts it as a child process. It works on its own workbook, not on the one open in the GridCraft window.

## Known limitations

- Printing is not available; export to PDF instead.
- Copy and paste use plain text; formatting and pictures do not go through the clipboard.
- Files dropped on the window are not opened.
- The control port (`--control`) and `gridcraft-cli send`, which drive a running window, are not available.
- The interface uses the fonts built into GridCraft; fonts named in a workbook (such as Calibri) are shown with them.
- Letters typed with a keymap for another 8-bit charset (such as the Polish one) come out as the wrong characters; text in workbooks is shown correctly.

## Licenses

GridCraft is licensed under the MIT license or the Apache License 2.0, at your option. The `Licenses` drawer holds these licenses and GridCraft's asset attributions, the notices for the AROS and Rust libraries linked into the programs, and the licenses of every Rust package they contain. `SOURCE.txt` names the exact sources; the source archive of this build holds the recipe and every change made for AROS.
