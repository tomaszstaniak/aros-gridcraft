# Preserved source changes

| Directory | Applies to | Applied by |
|---|---|---|
| `gridcraft/` | storytold/gridcraft at the pin in `upstreams.json`; order in `series` | `scripts/craft.py materialize` (`git am`) |
| `gridcraft-v0.8/` | the previous base, v0.8.0 (series 0001-0010, kept for history; not applied) | nothing |
| `crates/` | crates.io crates named in `crates/crates.json`; the patches, registry checksums and patched-tree hashes are in the shared set of the pinned Craft tools (egui-aros `tools/craft/crates`); this port has no crate patches of its own | `scripts/craft.py build` |
| `aros-pthread/` | deadwood2/AROS `compiler/pthread` at the commit named for the target in the pinned tools' `craftconfig.py` (ABIv11 target only) | `scripts/craft.py build` |

Two more sets are applied by every build and kept once in the pinned Craft tools (egui-aros
`tools/craft/platform`): `libc-aros` (rust-aros `std-groundwork/libc-aros` at `rust_aros_commit`;
currently no patch) and `aros-glue` (the pomaros-hosted C bridge, Raspberry Pi target only).

Every patch carries its problem, rationale, scope, upstream status,
verification and removal condition in its header (for `gridcraft/`, the commit
message). The crate, libc-aros and pthread sets are shared with the other
Craft ports; their headers may name the port they were written for. The window, input and
renderer patches (winit, egui-winit, eframe, glutin, aros-gui-sys) belong to egui-aros,
pinned in `upstreams.json`, and are applied by its own `ea.py vendor`. See
[the patch workflow](../docs/patching.md).
