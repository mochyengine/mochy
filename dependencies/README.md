# Dependencies

This folder contains the external libraries that MochyEngine is built on.
They are included directly in the repository, so you can build the engine
without downloading anything else.

All of them are free, open-source libraries. Their licenses allow them to be
used in MochyEngine and shipped together with the editor. The full copyright
and license texts are in [`COPYRIGHT.txt`](../COPYRIGHT.txt).

## What is included

| Library | What it does in MochyEngine | Version | Added | License |
|---|---|---|---|---|
| [AngelScript](https://www.angelcode.com/angelscript/) | Scripting language for game logic | 2.39.0 | 2026-11-12 | Zlib |
| [Box2D](https://github.com/erincatto/box2d) | 2D physics | 3.2.0+ | 2026-11-12 | MIT |
| [glad](https://glad.dav1d.de/) | Loads OpenGL functions | OpenGL 4.6 core (glad 0.1.36) | 2026-11-12 | MIT / Apache-2.0 |
| [GLFW](https://github.com/glfw/glfw) | Window, keyboard, mouse and gamepad input | 3.5.1 | 2026-11-12 | Zlib |
| [GLM](https://github.com/g-truc/glm) | Math (vectors, matrices) | 1.0.3+ | 2026-11-12 | MIT / Happy Bunny |
| [Dear ImGui](https://github.com/ocornut/imgui) | Editor user interface | 1.93.1 WIP (docking) | 2026-11-12 | MIT |
| [miniaudio](https://github.com/mackron/miniaudio) | Sound and music playback | 0.11.26 | 2026-11-12 | Public domain / MIT-0 |
| [Quill](https://github.com/odygrd/quill) | Logging | 13.1.0 | 2026-11-12 | MIT |
| [stb_image](https://github.com/nothings/stb) | Loading images (PNG, JPG, ...) | 2.31 | 2026-11-12 | Public domain / MIT |
| [toml++](https://github.com/marzer/tomlplusplus) | Reading and writing settings files (TOML) | 3.4.0+ | 2026-11-12 | MIT |

## Good to know

- Each library is in its own folder with a simple lowercase name, for example
  `glfw/` or `imgui/`. The version is listed in this file, not in the folder name.
- Only the parts needed to build the engine are included. Documentation,
  tests and examples of the libraries were removed to keep the repository small.
  You can find them on each library's website.
- We do not change the source code of the libraries. If a library needs a fix,
  it should go to the original project first.

## Updating a library

1. Download the new version from the library's website.
2. Replace the contents of its folder. Keep the folder name.
3. Remove documentation, tests and examples again (see the list below).
4. Update the version and date in the table above.
5. If the copyright or license changed, update [`COPYRIGHT.txt`](../COPYRIGHT.txt).
6. Build the engine on Linux and Windows to make sure everything still works.

---

<details>
<summary><b>Technical details for developers</b></summary>

### Exact sources

- **Dear ImGui:** `docking` branch, commit `cf53467ab0db7a783f04b32dd9160e6aa5b720ef` (2026-11-10).
- **toml++:** `master` branch, commit `c828e4d4d131eacaad1ff129d73385cb79d46fbf` (2026-11-11).
  This commit is newer than the 3.4.0 release, although `VERSION.txt` still says 3.4.0.
- **glad:** generated with
  `--profile="core" --api="gl=4.6" --generator="c" --spec="gl" --extensions=""`
  ([open in the online generator](https://glad.dav1d.de/#profile=core&language=c&specification=gl&loader=on&api=gl%3D4.6)).
- **Box2D:** `main` branch, commit `dd8bd3fb525f0f33f1079f8ca3c5770783c44a53` (2026-10-30).
  This commit is newer than the 3.2.0 release, although `CMakeLists.txt` still says 3.2.0.
- **GLM:** `master` branch, commit `6f14f4792a0cde5d0cf2c910506724d61cb95834` (2026-04-07).
  This commit is newer than the 1.0.3 release. `glm/detail/setup.hpp` already says 1.1.0,
  but that version has not been released yet.
- **AngelScript:** official GitHub repository [anjo76/angelscript](https://github.com/anjo76/angelscript),
  tag `v2.39.0`, commit `4857f0b48a3122967478417c1270ecef1f4fe590` (2026-11-10).
  The ZIP on angelcode.com has the same content, only with Windows line endings.
- **stb_image:** `master` branch, commit `562641d1e825ed40414ffa099ce56a24e672a166` (2026-11-11).
  `LICENSE` was copied from the root of the stb repository.
- **GLFW, miniaudio, Quill:** the release tags `3.5.1`, `0.11.26` and `v13.1.0`.

### Removed upstream content

- All libraries: `.github/`, `.gitignore`, `.gitattributes`, `.gitmodules`,
  `.git-blame-ignore-revs` and other repository tooling files.
- angelscript: `sdk/docs/`, `sdk/samples/`, `sdk/tests/`, all IDE projects in
  `sdk/angelscript/projects/` except `cmake/`.
- box2d: `samples/`, `test/`, `benchmark/`, `docs/`, `shared/`, `extern/`.
- glfw: `docs/`, `tests/`, `examples/`, and everything in `deps/` except `deps/wayland/`
  (those files are only used by the GLFW examples and tests).
- glm: `doc/`, `test/`, `util/`.
- imgui: `examples/`, `docs/`, `misc/fonts/` (optional font files under other licenses;
  the default fonts are embedded in `imgui_draw.cpp`).
- miniaudio: `extras/`, `data/`, `tests/`, `external/`, `examples/`, `website/`, `tools/`, `research/`,
  `resources/` (branding images).
- quill: `docs/`, `test/`, `benchmarks/`, `examples/`, `fuzz/`.
- tomlplusplus: `tests/`, `docs/`, `vendor/`, `examples/`, `toml-test/`, `fuzzing/`, `benchmarks/`,
  `tools/` (only used when toml++ is built on its own from a git checkout;
  `tomlplusplus_REGENERATE` is always OFF when it is added as a subproject).

### CMake integration

| Folder | How it is built |
|---|---|
| `angelscript/` | `add_subdirectory(angelscript/sdk/angelscript/projects/cmake)`, `AS_DISABLE_INSTALL=ON`; add-ons in our own static target |
| `box2d/` | `add_subdirectory`, link `box2d::box2d` |
| `glad/` | own static target from `src/glad.c`, include dir `include/` |
| `glfw/` | `add_subdirectory`, `GLFW_BUILD_EXAMPLES/TESTS/DOCS=OFF` (**DOCS must be OFF**, `docs/` was removed), `GLFW_INSTALL=OFF` |
| `glm/` | `add_subdirectory`, `GLM_BUILD_LIBRARY=OFF`, `GLM_BUILD_INSTALL=OFF`, link `glm::glm` |
| `imgui/` | own static target: core sources + `backends/imgui_impl_glfw.cpp` + `backends/imgui_impl_opengl3.cpp` |
| `miniaudio/` | own static target from `miniaudio.c`; on Linux link `Threads::Threads`, `${CMAKE_DL_LIBS}`, `m` |
| `quill/` | `add_subdirectory`, `QUILL_DISABLE_NON_PREFIXED_MACROS=ON`, link `quill::quill` |
| `stbimage/` | own target with one source file defining `STB_IMAGE_IMPLEMENTATION` |
| `tomlplusplus/` | compiled mode: `TOML_HEADER_ONLY=0` + one source file defining `TOML_IMPLEMENTATION` |

General rules:
- Add every subproject with `add_subdirectory(... SYSTEM)`.
- Warning flags are `PRIVATE` on our own targets only.
- Set `BUILD_SHARED_LIBS=OFF` and `CMAKE_MSVC_RUNTIME_LIBRARY` as **cache** variables in the
  CMake presets. AngelScript uses the old CMP0077 behavior and clears a normal variable of the same name.
- Linux needs the X11/Wayland development packages for GLFW.

</details>
