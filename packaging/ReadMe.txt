GridCraft @VERSION@ for AROS
============================

GridCraft is a spreadsheet program. It opens and saves Excel workbooks
(XLSX), imports XLS, XLSB, ODS, CSV and JSON, and exports CSV, HTML and PDF.
This is its AROS port, based on GridCraft @UPSTREAM_VERSION@.

This package is for: @SYSTEM@.
It does not run on other AROS systems; use the package made for yours.

The drawer holds two programs:

  GridCraft       the spreadsheet
  gridcraft-cli   the command-line program: shows a workbook's sheets and
                  cells, evaluates formulas, converts between formats, runs
                  editing commands, checks files and serves an MCP
                  automation interface over standard input and output


Installation
------------

Unpack the archive anywhere writable, for example to Work:. It creates the
drawer GridCraft and its icon. Both programs use only libraries that come
with AROS; nothing in the system is replaced or added.

To upgrade, unpack the new version to a new place (not over the old drawer),
then move the drawer GridCraftData from the old GridCraft drawer into the
new one to keep your settings and recent files.

Starting
--------

Workbench: double-click GridCraft. The program takes the stack it needs
itself; it also works from a Shell with the default stack.

Shell:

  GridCraft [--gui-renderer CPU|AUTO|GPU] [--window WxH] [file ...]
  GridCraft --sample

Files named on the command line are opened at start; --sample opens a
sample workbook. All workbooks share one window. Open, Save As and
Insert Picture use the standard file requester. To close GridCraft, use the
window's close gadget; with unsaved changes it asks whether to save them.

  gridcraft-cli info budget.xlsx
  gridcraft-cli cat budget.xlsx --range A1:D20
  gridcraft-cli eval "=SUM(1,2)**3"
  gridcraft-cli convert budget.xlsx budget.csv
  gridcraft-cli --help

In the AROS Shell an asterisk inside quotes starts an escape, so write **
for a * in a formula, as above.

gridcraft-cli mcp runs an MCP server on standard input and output for a
client that starts it as a child process. It works on its own workbook, not
on the one open in the GridCraft window.

Settings and log
----------------

Settings, recent files and the log file GridCraft.log are kept in the drawer
GridCraftData inside the GridCraft drawer; it is created at the first start.
The variable GRIDCRAFT_CONFIG_DIR names another drawer instead.
GRIDCRAFT_LOG names another log file.

Clipboard
---------

Text is exchanged with other programs through the system clipboard. A
picture on the clipboard (an ILBM written by Capture, MultiView or a paint
program) can be pasted into the sheet; this has not been tested in this
release.

Interface renderer
------------------

The ToolType GUI_RENDERER in the program's icon, or the Shell argument
--gui-renderer, chooses how the interface is drawn. A Shell argument takes
priority; the icon is not read when the program is started from the Shell.
Values ignore letter case.

  AUTO  OpenGL if it opens and is not Mesa's software renderer, otherwise
        CPU; the reason is written to GridCraft.log (default)
  CPU   the processor draws the interface; OpenGL is not opened
  GPU   OpenGL (the software renderer too), or the program stops with an
        error

Known limitations
-----------------

@LIMITATIONS@

Licences
--------

GridCraft is available under the MIT license or the Apache License 2.0, at
your option. The texts, the notices for the application icon and other
bundled material, and the licences of everything linked into the programs
are in the Licenses drawer. BUILD.txt identifies this build.

GridCraft: https://github.com/storytold/gridcraft
AROS port: https://github.com/tomaszstaniak/aros-gridcraft
