# GridCraft for AROS

A port of [GridCraft](https://github.com/storytold/gridcraft), a spreadsheet
program written in Rust, to AROS x86_64 ABIv11, with mainline v1 retained as a
secondary target.

The repository holds the source pins, the patch series and the build tooling,
not a source checkout. The pinned source is GridCraft `823c5658` (tag v0.8.0).
The spreadsheet runs in a native Intuition window (`apps/gridcraft-aros`, added by
the series) on [egui-aros](https://github.com/tomaszstaniak/egui-aros), the shared AROS window, input and renderer layer of
the ArtCraft ports, pinned in `upstreams.json`.

Status: the spreadsheet (`GridCraft`) and `gridcraft-cli` (with the MCP server over
stdio) build for x86_64 ABIv11 and have been run on an ABIv11 guest. Mainline
v1 is not built. Details and the exact tested scope:
[development](docs/development.md). Patch handling: [patching](docs/patching.md).

[upstreams.json](upstreams.json) identifies the sources;
[patches/gridcraft/series](patches/gridcraft/series) the changes to GridCraft;
`patches/crates`, `patches/libc-aros` and `patches/aros-pthread` the changes to
dependencies. `packaging/` holds the test packages' recipes. Generated
checkouts and outputs are ignored.
