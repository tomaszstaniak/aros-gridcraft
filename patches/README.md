# Preserved source changes

| Directory | Applies to | Applied by |
|---|---|---|
| `gridcraft/` | storytold/gridcraft at the pin in `upstreams.json`; order in `series` | `scripts/gc.py materialize` (`git am`) |
| `crates/` | crates.io crates named in `crates/crates.json` (registry checksum and patched-tree hash recorded) | `scripts/gc.py build` |
| `libc-aros/` | rust-aros `std-groundwork/libc-aros` at the commit in `scripts/gcconfig.py` | `scripts/gc.py build` |
| `aros-pthread/` | deadwood2/AROS `compiler/pthread` at the commit in `scripts/gcconfig.py` | `scripts/gc.py build` |

Every patch carries its problem, rationale, scope, upstream status, verification and removal condition in its header (for `gridcraft/`, the commit message). `crates/`, `libc-aros/` and `aros-pthread/` are the PhotoCraft and WordCraft AROS ports' patches, copied unchanged; their headers name those ports. The window, input and renderer patches (winit, egui-winit, eframe, glutin, aros-gui-sys) belong to egui-aros, pinned in `upstreams.json`, and are applied by its own `ea.py vendor`. See [the patch workflow](../docs/patching.md).
