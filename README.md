# KytyPS5 — Mesh Shader Fallback + ASTRO BOT & NVIDIA Fixes

[![Build KytyPS5 (Windows)](https://img.shields.io/github/check-runs/felipeferro/KytyPS5/rdna1-work?nameFilter=Build%20KytyPS5%20%28Windows%29&label=Build%20KytyPS5%20%28Windows%29)](https://github.com/felipeferro/KytyPS5/actions/workflows/build.yml)
[![Build KytyPS5 (Linux)](https://img.shields.io/github/check-runs/felipeferro/KytyPS5/rdna1-work?nameFilter=Build%20KytyPS5%20%28Linux%29&label=Build%20KytyPS5%20%28Linux%29)](https://github.com/felipeferro/KytyPS5/actions/workflows/build.yml)
[![Build KytyPS5 (macOS)](https://img.shields.io/github/check-runs/felipeferro/KytyPS5/rdna1-work?nameFilter=Build%20KytyPS5%20%28macOS%29&label=Build%20KytyPS5%20%28macOS%29)](https://github.com/felipeferro/KytyPS5/actions/workflows/build.yml)
[![Releases](https://img.shields.io/github/v/release/felipeferro/KytyPS5?include_prereleases&label=Download%20Release)](https://github.com/felipeferro/KytyPS5/releases)
[![Platform](https://img.shields.io/badge/platform-Windows%20x64%20%7C%20Linux%20x64%20%7C%20macOS%20x86__64-0078D4.svg)](#system-requirements)
[![License](https://img.shields.io/badge/license-GPL--2.0-blue.svg)](LICENSE)

> [!TIP]
> **Pre-built binaries (Windows, Linux, macOS):** Download the latest release directly from [**Releases**](https://github.com/felipeferro/KytyPS5/releases).

## About This Fork (EN) / Sobre Este Fork (PT-BR)

### English
This community fork combines experimental **Mesh Shader Fallback** support with pending upstream fixes so users with older GPUs (Pascal, RDNA 1, Polaris) and NVIDIA GPUs can boot and test games like **ASTRO BOT (`PPSA21567`)** without waiting for separate PRs to be merged:

1. **Mesh Shader Fallback (`VK_EXT_mesh_shader` -> Compute + Vertex Shader)** *(based on [`AbbasBHF/KytyPS5` `rdna1-work`](https://github.com/AbbasBHF/KytyPS5/tree/rdna1-work), issue [`#1447`](https://github.com/KytyPS5/KytyPS5/issues/1447))*:
   - Emulates PS5 NGG/Mesh Shaders via Compute + Vertex shader lowering when hardware `VK_EXT_mesh_shader` is unavailable.
   - Makes `VK_KHR_fragment_shader_barycentric` optional (`fsb` fallback).
   - Allows GPUs such as **NVIDIA GeForce GTX 10-series (GTX 1060 / 1070 / 1080)**, **GTX 1650**, **AMD Radeon RX 5000 (RDNA 1)**, and **RX 400/500 (Polaris)** to get past `Fatal Error: Not implemented (!m_graphics.mesh_shader_enabled)`.
2. **ASTRO BOT (`PPSA21567`) Startup Crash Fix** *([PR `#1528`](https://github.com/KytyPS5/KytyPS5/pull/1528))*:
   - Guards against null root and nested SRT table pointers in `SrtWalker.cpp`, fixing the immediate `0x0000000000000058` access violation on startup.
3. **NVIDIA Sampler `maxAnisotropy` Fix** *([PR `#1625`](https://github.com/KytyPS5/KytyPS5/pull/1625))*:
   - Clamps `maxAnisotropy` to `[1.0f, maxSamplerAnisotropy]` in `descriptors.cpp`, preventing `vkCreateSampler` from failing with `VK_ERROR_OUT_OF_HOST_MEMORY` on NVIDIA drivers.

---

### Português (PT-BR)
Este fork comunitário reúne o **Fallback de Mesh Shader** com correções críticas que ainda estão em PRs abertos no repositório oficial, permitindo rodar e testar jogos como **ASTRO BOT (`PPSA21567`)** em placas de vídeo mais antigas (Pascal, RDNA 1, Polaris) e GPUs NVIDIA:

1. **Fallback de Mesh Shader (`VK_EXT_mesh_shader` -> Compute + Vertex Shader)** *(baseado em [`AbbasBHF/KytyPS5` `rdna1-work`](https://github.com/AbbasBHF/KytyPS5/tree/rdna1-work), issue [`#1447`](https://github.com/KytyPS5/KytyPS5/issues/1447))*:
   - Converte Mesh Shaders do PS5 em Compute + Vertex Shaders quando a GPU não possui `VK_EXT_mesh_shader` via hardware, além de tornar `fragmentShaderBarycentric` opcional.
   - Permite que placas como **NVIDIA GTX série 10 (GTX 1060 / 1070 / 1080)**, **GTX 1650**, **AMD RX série 5000 (RDNA 1)** e **RX 400/500 (Polaris)** passem do erro `Fatal Error: Not implemented (!m_graphics.mesh_shader_enabled)`.
2. **Correção do Crash de Inicialização no ASTRO BOT (`PPSA21567`)** *([PR `#1528`](https://github.com/KytyPS5/KytyPS5/pull/1528))*:
   - Ignora ponteiros nulos na tabela SRT em `SrtWalker.cpp`, corrigindo a falha de violação de acesso (`address=0x0000000000000058`) ao iniciar o jogo.
3. **Correção de Sampler `maxAnisotropy` em GPUs NVIDIA** *([PR `#1625`](https://github.com/KytyPS5/KytyPS5/pull/1625))*:
   - Limita `maxAnisotropy` ao intervalo `[1.0f, maxSamplerAnisotropy]` em `descriptors.cpp`, evitando falhas de driver (`VK_ERROR_OUT_OF_HOST_MEMORY`) em placas NVIDIA.

---

**[Weekly updates](https://github.com/KytyPS5/KytyPS5/discussions/862)** â€” game progress, recent fixes, and ongoing development.

**[Development on Discord](https://discord.gg/UNrkMqGaBg)** â€” KytyPS5 development.

KytyPS5 is a free and open-source PlayStation 5 translation layer written in C++ for Windows
and Linux, with experimental macOS support. It is based on a heavily modified version of
[Kyty](https://github.com/InoriRus/Kyty). The project is in active development, and behavior
can change significantly between builds.

> [!IMPORTANT]
> KytyPS5 is not affiliated with Sony Interactive Entertainment or PlayStation. The project does
> not distribute games or copyrighted system software. Use only game files that you have obtained
> legally.

## Test Status & Known Issues (EN) / Status dos Testes e Problemas Conhecidos (PT-BR)

> [!WARNING]
> **Experimental Build (Work in Progress):** This fork gets **ASTRO BOT (`PPSA21567` v1.018)** past the initial startup crashes and through the intro video on older/non-Mesh-Shader GPUs, but the game is **not yet playable in-game** on NVIDIA Pascal due to a post-intro `VK_ERROR_DEVICE_LOST`.

### Test Environment / Ambiente de Teste
- **Game / Jogo:** ASTRO BOT (`PPSA21567`, EU, v1.018 unmodified `Original_files`)
- **CPU:** AMD Ryzen 7 5700X (8C/16T)
- **GPU:** NVIDIA GeForce GTX 1060 6GB (Pascal — no hardware `VK_EXT_mesh_shader` or `VK_KHR_fragment_shader_barycentric`)
- **RAM:** 16 GB

### Progress Summary / Resumo do Progresso

| Stage / Etapa | Official Build (`f492027`) | This Fork (`rdna1-work` + Fixes) | Details / Detalhes |
| :--- | :---: | :---: | :--- |
| **1. Startup SRT Table Walk** (`runtimeLinker.cpp:715` / `SrtWalker.cpp`) | ❌ Crashes (`0x0000000000000058` null pointer) | ✅ **Fixed** | Fixed via null root & nested SRT pointer guard ([PR `#1528`](https://github.com/KytyPS5/KytyPS5/pull/1528)). |
| **2. Pipeline Creation without Mesh Shaders** (`pipelineCache.cpp:698`) | ❌ Fatal Error (`!m_graphics.mesh_shader_enabled`) | ✅ **Fixed** | Fixed via Compute + Vertex Shader lowering (`rdna1-work`, [Issue `#1447`](https://github.com/KytyPS5/KytyPS5/issues/1447)). |
| **3. NVIDIA Sampler Creation** (`descriptors.cpp`) | ❌ Driver crash on invalid `maxAnisotropy` | ✅ **Fixed** | Clamped `maxAnisotropy` to `[1.0f, maxSamplerAnisotropy]` ([PR `#1625`](https://github.com/KytyPS5/KytyPS5/pull/1625)). |
| **4. Boot & Intro Video (`AvPlayer`)** | ❌ Unreachable | ✅ **Working** | Compiles `14 VS \| 21 PS \| 38 CS \| 1 GS` and plays intro video to completion (`AvPlayer video stopped`, `tick=338218`). |
| **5. Post-Intro 3D Transition** (`commandScheduler.cpp:582`) | ❌ Unreachable | ⚠️ **Known Issue (`VK_ERROR_DEVICE_LOST`)** | Fails on `vkQueueSubmit` during 3D scene transition (see details below). |

---

### Documented Errors & Recommended Settings / Erros Documentados e Configurações Recomendadas

#### 1. Post-Intro `VK_ERROR_DEVICE_LOST` (`commandScheduler.cpp:582`)
```text
Shaders: VS 14 | PS 21 | CS 38 | GS 1 | LS 0 | HS 0 | TES 0
vkQueueSubmit failed: ErrorDeviceLost (-4), tick=338218 debug_op=0 debug_submit=8908 args=4096,1,1,97,0x0000000908e6aa00 shader_hash=0x0000000000000000
Not implemented (result != vk::Result::eSuccess) in src\graphics\host_gpu\renderer\commandScheduler.cpp:582
```
- **EN:** Occurs immediately after `AvPlayer video stopped` when transitioning to the 3D scene (`DispatchDirect` with `4096x1x1` thread groups). Matches upstream issues [`#1498`](https://github.com/KytyPS5/KytyPS5/issues/1498) and [`#1511`](https://github.com/KytyPS5/KytyPS5/issues/1511) on NVIDIA GPUs:
  - **Cause A (Wave64 on Subgroup-32 NVIDIA GPUs):** PS5 shaders run in `Wave64`, which is emulated with 2 lanes per invocation on NVIDIA `Warp32` hardware; LDS spinlocks (`OpAtomicExchange` on `lds_dwords`) or unbounded shader loops can deadlock a warp and trigger Windows TDR (`ErrorDeviceLost`).
  - **Cause B (Per-thread LDS private array size on NVIDIA):** Vertex/Pixel shaders emulating LDS allocate a fixed `8192 dword` (32 KB) private array per thread, which can overflow NVIDIA local memory reservations when `VS 14` / `PS 21` are dispatched.
- **PT-BR:** Ocorre logo após o término do vídeo de abertura (`AvPlayer video stopped`) na transição para a cena 3D (`DispatchDirect` de `4096x1x1` grupos). Corresponde ao comportamento documentado nas issues oficiais [`#1498`](https://github.com/KytyPS5/KytyPS5/issues/1498) e [`#1511`](https://github.com/KytyPS5/KytyPS5/issues/1511) em placas NVIDIA (deadlock na emulação de *Wave64* sobre *Warp32* em spinlocks LDS e alocação fixa de 32 KB de LDS local por thread em shaders gráficos).

#### 2. `SPIR-V validation failed` when `Shader validation` is enabled (`pipelineCache.cpp:333`)
```text
ShaderRecompiler CS failed hash=0x503d7f066a496c3c: SPIR-V validation failed
 in src\graphics\host_gpu\renderer\pipeline\pipelineCache.cpp:333
```
- **EN:** Enabling **`Shader validation`** in the Launcher runs strict `spirv-val` checks and aborts at `CS 16` (`0x503d7f066a496c3c`, BVH compute shader) before the intro video. **Keep `Shader validation` disabled** so the shader is passed directly to the Vulkan driver.
- **PT-BR:** Ativar a opção **`Shader validation`** no Launcher executa o validador estrito `spirv-val` e aborta a execução logo no `CS 16` (`0x503d7f066a496c3c`) antes do vídeo de abertura. **Mantenha `Shader validation` desmarcado** nas configurações.

#### 3. Recommended Launcher Settings / Configurações Recomendadas no Launcher
- ✅ **`AMD CPU compatibility`:** Enable on AMD Ryzen CPUs (patches `VRSQRTPS` instructions in `eboot.bin`). / Ativar em processadores AMD Ryzen.
- ✅ **`Skip BVH intersections`:** Enable to bypass heavy software BVH ray-tracing intersections. / Ativar para pular interseções pesadas de Ray Tracing por software.
- ❌ **`Shader validation` & `Vulkan validation`:** Keep **disabled**. / Manter **desmarcados**.
- ℹ️ **`Printf direction`:** Set to **`Console`** or **`File`** if you want full diagnostic logs (`Device fault report`). / Mudar para `Console` ou `File` para visualizar logs detalhados.

---
## Current Status

KytyPS5 can boot 2D and 3D games, including titles built with Unreal Engine
4/5, Unity, and custom engines. External low-level emulation modules are neither required nor
planned.

Development is currently focused on expanding game compatibility and improving boot reliability.

Windows and Linux are the primary target platforms and receive the most testing.

macOS support is experimental. The emulator is built for x86-64 and runs on Apple Silicon under
Rosetta 2, with Vulkan provided by MoltenVK. A small number of titles have been verified in-game
on Apple Silicon hardware; see [Building on macOS](#building-on-macos).

Community game test results are available in the
[KytyPS5 Compatibility List](https://kytyps5.github.io/).

## Bugs and Issues

Compatibility, stability, and performance can vary between versions. You may encounter crashes
or graphical glitches, so please include the version you tested when reporting an issue.

## Screenshots

<table align="center">
  <tr>
    <td align="center">
      <strong>Astro Bot</strong><br>
      <img src="docs/screenshots/ps5-01.png" width="300" alt="Astro Bot running in KytyPS5">
    </td>
    <td align="center">
      <strong>Saros</strong><br>
      <img src="docs/screenshots/ps5-03.png" width="300" alt="Saros running in KytyPS5">
    </td>
  </tr>
  <tr>
    <td align="center">
      <strong>Neptunia ReVerse</strong><br>
      <img src="docs/screenshots/ps5-04.png" width="300" alt="Neptunia ReVerse running in KytyPS5">
    </td>
    <td align="center">
      <strong>SILENT HILL: The Short Message</strong><br>
      <img src="docs/screenshots/ps5-05.png" width="300" alt="SILENT HILL: The Short Message running in KytyPS5">
    </td>
  </tr>
  <tr>
    <td align="center">
      <strong>Demon's Souls</strong><br>
      <img src="docs/screenshots/ps5-02.png" width="300" alt="Demon's Souls running in KytyPS5">
    </td>
    <td align="center">
      <strong>UFC 6</strong><br>
      <img src="docs/screenshots/ps5-06.jpg" width="300" alt="UFC 6 running in KytyPS5">
    </td>
  </tr>
</table>

<p align="center"><em>And many more...</em></p>

## Contributing

Testing games and submitting detailed bug reports are useful ways to contribute. Search existing
issues first, then use the **Game Emulation Status Report** template and attach the complete log file.

**Pull requests for general fixes, architectural changes, or refactoring that do not address a concrete bug or launch problem in a specific game will be automatically closed.**

**Large, unfocused commits or pull requests that bundle unrelated changes without a clear purpose or game-related benefit will also be automatically closed.**

Keep each PR focused. Name the affected game, describe the exact problem, and provide evidence showing what the change fixes.

Windows and Linux are the primary target platforms, so changes to shared code should not
introduce regressions on either platform. Changes confined to a platform's own code paths
only need to build on that platform. Because KytyPS5 is still evolving quickly, consider
opening an issue before starting a large change.

### Formatting

Set up the clang-format hook after cloning.

Install `pre-commit` using the method appropriate for your platform:

- **Arch Linux / CachyOS:** `sudo pacman -S pre-commit`
- **Other Linux / macOS / Windows:** `python -m pip install pre-commit`

Then install the Git hook:

```bash
python -m pre_commit install --install-hooks
```

It formats staged `.cpp`, `.h`, and `.inc` files in `src`.

## Developer Information

The PS5 graphics architecture is based on AMD RDNA 2. Use AMD's
[RDNA 2 Instruction Set Architecture Reference Guide (document 70648)](https://docs.amd.com/v/u/en-US/rdna2-shader-instruction-set-architecture)
as the primary instruction-encoding reference when working on shader decoding and recompilation.

Important areas of the codebase:

- [`src/graphics/shader/recompiler`](src/graphics/shader/recompiler) â€” instruction decoding,
  intermediate representation, control flow, resource tracking, and SPIR-V emission
- [`src/graphics/guest_gpu`](src/graphics/guest_gpu) â€” PS5 (Prospero) GPU formats and command processing
- [`src/graphics/host_gpu`](src/graphics/host_gpu) â€” Vulkan host backend and resource management
- [`tests`](tests) â€” focused memory, shader, and resource-tracking regression tests

The renderer targets Vulkan 1.3. Keep shader changes aligned with both the RDNA 2 ISA semantics and
the Vulkan/SPIR-V validation rules.

## Building

### System requirements

- Windows 10 version 1803, a current Linux distribution, or macOS on Apple Silicon
- A 64-bit x86 processor (on macOS, an Apple Silicon processor with Rosetta 2)
- A Vulkan 1.3-capable GPU with current drivers (on macOS, Vulkan is provided by the bundled
  MoltenVK)

### Build requirements (Windows)

- Git
- CMake 3.22.1 or newer
- Ninja
- Visual Studio 2022 or Build Tools 2022 with the **Desktop development with C++** workload and
  **C++ Clang tools for Windows** component
- Qt 6 for MSVC 2022 64-bit, including Concurrent, Network, and Widgets
- [glslang](https://github.com/KhronosGroup/glslang/releases) (`glslang` or `glslangValidator`) on `PATH`
- Python 3 on `PATH`; the bundled SPIRV-Tools runs it at configure time

The Microsoft C++ compiler (`cl.exe`) is not supported; use `clang-cl`.

Open an **x64 Native Tools Command Prompt for Visual Studio 2022** (or the equivalent Developer
PowerShell), change to the repository root, and initialize the dependencies:

```powershell
git submodule update --init --recursive
```

Configure the project. Replace the Qt path with the version installed on your system:

```powershell
cmake -S . -B _Build/windows -G Ninja -DCMAKE_BUILD_TYPE=Release -DCMAKE_C_COMPILER=clang-cl -DCMAKE_CXX_COMPILER=clang-cl -DCMAKE_PREFIX_PATH="C:/Qt/6.x.x/msvc2022_64"
```

Build the launcher and stage a runnable installation:

```powershell
cmake --build _Build/windows --target launcher
cmake --install _Build/windows --prefix _Build/windows/install
```

The finished application and its runtime dependencies will be placed in
`_Build/windows/install`.

### Building on Linux

Install the toolchain and the libraries the bundled SDL3 needs. Without the audio, Wayland, and
udev development packages, SDL3 quietly configures itself without those backends, and the resulting
build has no working audio or gamepad hotplug support:

```bash
sudo apt-get install --no-install-recommends \
  clang lld ninja-build cmake git glslang-tools python3 pkg-config \
  libgl1-mesa-dev libx11-dev libxcursor-dev libxext-dev libxfixes-dev \
  libxi-dev libxrandr-dev libxss-dev libxtst-dev libxkbcommon-dev \
  libasound2-dev libpulse-dev libudev-dev libdbus-1-dev libwayland-dev wayland-protocols
```

Qt 6 (Concurrent, Network, and Widgets) is required for the launcher. Use your distribution's
packages (`qt6-base-dev`) or an official Qt installation.

```bash
git submodule update --init --recursive

cmake -S . -B _Build/linux -G Ninja -DCMAKE_BUILD_TYPE=Release \
  -DCMAKE_C_COMPILER=clang -DCMAKE_CXX_COMPILER=clang++ \
  -DCMAKE_PREFIX_PATH="$Qt6_DIR"

cmake --build _Build/linux --target launcher --parallel
cmake --install _Build/linux --prefix _Build/linux/install
```

The install step copies the Qt libraries and plugins next to the binaries, so
`_Build/linux/install` runs without a matching system Qt installation. FFmpeg is linked statically
from the pinned [KytyPS5 FFmpeg core](https://github.com/KytyPS5/ext-ffmpeg-core)
release, including VP9 and WebM support. System FFmpeg packages are not required.

To build `kyty_emulator` and the `kyty_tests` target without Qt, use a separate build directory:

```bash
git submodule update --init --recursive

cmake -S . -B _Build/linux-no-qt -G Ninja -DCMAKE_BUILD_TYPE=Release \
  -DCMAKE_C_COMPILER=clang -DCMAKE_CXX_COMPILER=clang++ \
  -DKYTY_BUILD_LAUNCHER=OFF

cmake --build _Build/linux-no-qt --target kyty_emulator kyty_tests --parallel
```

As on Windows, the MSVC compiler is not used; Clang is required. `cl.exe` is rejected at configure
time.

The CMake source root is the repository root.

### Building on NixOS

A development shell provides Clang, CMake, Ninja, Qt 6, the Vulkan headers, and the SDL3 backend
libraries. Enter it and configure exactly as on other Linux distributions; the shell exports
`CMAKE_PREFIX_PATH` and `QT_PLUGIN_PATH`, so the `-DCMAKE_PREFIX_PATH="$Qt6_DIR"` argument is not
needed:

```bash
nix-shell # or: nix develop
git submodule update --init --recursive

cmake -S . -B _Build/linux -G Ninja -DCMAKE_BUILD_TYPE=Release \
  -DCMAKE_C_COMPILER=clang -DCMAKE_CXX_COMPILER=clang++

cmake --build _Build/linux --target launcher --parallel
cmake --install _Build/linux --prefix _Build/linux/install
```

The configure step downloads the FFmpeg prebuilts and the `xbyak`, `zydis`, `zstd`, and ZArchive
sources, so it needs network access; a fully sandboxed `nix build` would require vendoring those
inputs. A Vulkan 1.3 driver must be available at runtime (on NixOS,
`hardware.graphics.enable = true`).

### Building on Fedora and Fedora Atomic (Bazzite, Bluefin, Silverblue)

On regular Fedora, install dependencies on the host. On Fedora Atomic variants such as Bazzite,
Bluefin and Silverblue, use [Distrobox](https://distrobox.it/) and run the install and build steps
inside the container. Keep the checkout in your home directory so it is shared with the container.
Add `--nvidia` to the create command for hosts using the proprietary NVIDIA driver:

```bash
distrobox create --name kyty-build --image registry.fedoraproject.org/fedora:latest
distrobox enter kyty-build
```

Install the toolchain, Qt 6 and SDL3 dependencies:

```bash
sudo dnf install -y \
  clang lld ninja-build cmake git glslang python3 pkgconf-pkg-config \
  mesa-libGL-devel libX11-devel libXcursor-devel libXext-devel libXfixes-devel \
  libXi-devel libXrandr-devel libXScrnSaver-devel libXtst-devel libxkbcommon-devel \
  alsa-lib-devel pulseaudio-libs-devel systemd-devel dbus-devel \
  wayland-devel wayland-protocols-devel qt6-qtbase-devel
```

From the repository root, follow the launcher configure, build and install steps in
[Building on Linux](#building-on-linux), omitting `-DCMAKE_PREFIX_PATH="$Qt6_DIR"`; CMake finds
Fedora's packaged Qt 6 automatically.

Run `./_Build/linux/install/kyty_launcher` on regular Fedora or inside the container. To launch a
container build from the host, leave the container with `exit`, then run from the repository root:

```bash
distrobox enter kyty-build -- ./_Build/linux/install/kyty_launcher
```

Qt libraries and plugins are bundled. You can also launch directly on a Fedora Atomic host
if its other system libraries are compatible with the container build.

### Building on macOS

macOS builds target x86-64 and run under Rosetta 2 on Apple Silicon, so the PS5's x86-64 game
code executes through the same translation layer as the emulator itself. Prebuilt archives are
attached to releases; the steps below are for building from source.

Requirements:

- An Apple Silicon Mac with Rosetta 2 installed (`softwareupdate --install-rosetta`)
- Xcode (or the Command Line Tools)
- Homebrew packages: `brew install cmake ninja glslang python`
- Qt 6 (Concurrent, Network, and Widgets) with x86-64 support. The official Qt installation is
  universal and works; Homebrew's Qt is arm64-only and will not link

```bash
git submodule update --init --recursive

cmake -S . -B _Build/macos -G Ninja -DCMAKE_BUILD_TYPE=Release \
  -DCMAKE_OSX_ARCHITECTURES=x86_64 \
  -DCMAKE_C_COMPILER=clang -DCMAKE_CXX_COMPILER=clang++ \
  -DCMAKE_PREFIX_PATH="$Qt6_DIR"

cmake --build _Build/macos --target launcher --parallel
cmake --install _Build/macos --prefix _Build/macos/install
```

The build re-signs `kyty_emulator` with the JIT entitlements it needs to execute translated
guest code; no manual signing step is required. When the launcher is built, the installation
also produces `_Build/macos/install/KytyPS5.app` â€” double-click to launch the GUI.
A flat `kyty_emulator` is kept for CLI usage.

Vulkan comes from MoltenVK. Download `MoltenVK-macos.tar` from the
[MoltenVK releases](https://github.com/KhronosGroup/MoltenVK/releases), then copy
`MoltenVK/dynamic/dylib/macOS/libMoltenVK.dylib` next to the flat `kyty_emulator`
(and, for the bundle, into `KytyPS5.app/Contents/Frameworks/`) and sign it with an ad hoc signature:

```bash
codesign --force --sign - _Build/macos/install/libMoltenVK.dylib
# For the bundle (if present):
codesign --force --sign - _Build/macos/install/KytyPS5.app/Contents/Frameworks/libMoltenVK.dylib
codesign --force --sign - _Build/macos/install/KytyPS5.app
```

Release archives already include a signed `libMoltenVK.dylib` (both flat and inside the bundle).

### Regression tests

Build every regression executable and run the registered tests with:

```powershell
cmake --build _Build/windows --target kyty_tests
ctest --test-dir _Build/windows --output-on-failure
```

Use `_Build/linux` instead of `_Build/windows` for a Linux build.

### Visual Studio Code

A ready-made Visual Studio Code setup is included in [`.vscode`](.vscode). It configures CMake
Tools to build the project with Ninja and `clang-cl` and provides launch profiles for both
`kyty_launcher.exe` and `kyty_emulator.exe`. It is Windows-only: VS Code settings cannot select a
compiler per platform, so on Linux configure from the command line as shown above.

Before using it:

1. Install the **CMake Tools** and **C/C++** extensions in Visual Studio Code.
2. Update `CMAKE_PREFIX_PATH` in [`.vscode/settings.json`](.vscode/settings.json) to point to your
   Qt 6 MSVC installation.
3. Update the `--game` path in [`.vscode/launch.json`](.vscode/launch.json) for the
   **Debug kyty_emulator** profile.
4. Open the repository in an x64 Visual Studio developer environment, configure the CMake project,
   and select a launch profile from **Run and Debug**.

## Running

Update your graphics driver before reporting rendering problems.

To use the graphical launcher:

```powershell
.\_Build\windows\install\kyty_launcher.exe
```

```bash
./_Build/linux/install/kyty_launcher
```

```bash
open _Build/macos/install/KytyPS5.app  # or double-click in Finder
```

On first launch, add one or more game folders in the global settings. The launcher searches those
folders recursively for game directories containing `eboot.bin` and ZArchive (`.zar`) game dumps
whose archive root contains `eboot.bin`. Select a detected game and run it from the game list.
ZArchive dumps are mounted read-only and streamed directly; they do not need to be extracted first.

Official release builds include a built-in updater. Leave **Check for updates on startup** enabled,
or click **Check for updates**, then choose **Install Update**. The launcher downloads the package
for your platform from GitHub, checks its SHA-256 checksum, and restarts after installation. Close
any running games first. Settings, saves, patches, and other files outside the release package are
preserved; replacement failures restore the previous files. The installation folder must be writable.
If a package cannot be installed automatically, the updater offers the GitHub release page.
Source builds and builds from forks do not replace themselves with official releases.

The emulator can also be started directly with a legally obtained game directory, ELF file, or
ZArchive dump:

```powershell
.\_Build\windows\install\kyty_emulator.exe --game "D:\Games\ExampleGame"
.\_Build\windows\install\kyty_emulator.exe --game "D:\Games\ExampleGame.zar"
```

```bash
./_Build/linux/install/kyty_emulator --game "/games/ExampleGame"
./_Build/linux/install/kyty_emulator --game "/games/ExampleGame.zar"
```

On macOS, the adjacent flat or app-bundled `libMoltenVK.dylib` is found automatically; no
environment variable is required:

```bash
./_Build/macos/install/kyty_emulator --game "/games/ExampleGame"
```

To override the Vulkan loader, set `SDL_VULKAN_LIBRARY`:

```bash
SDL_VULKAN_LIBRARY=/path/to/libMoltenVK.dylib ./kyty_emulator --game "/games/ExampleGame"
```

Run `kyty_emulator --help` to see the available graphics, logging, validation, profiling, and
debugging options.

### AI Use

AI tools may be used for research, reverse engineering, and development assistance. Contributors
must fully understand, review, and test all code they submit and remain responsible for its
correctness. Repository communication, including pull-request descriptions, code comments, and
issue comments, must come from the human contributor rather than an autonomous AI agent.

Pull requests that include AI-assisted or AI-generated work should disclose the scope of the AI
involvement and describe the human review and testing performed before submission. Unverified or
untested generated changes may be closed without review.

## License

KytyPS5 is licensed under the [GNU General Public License version 2](LICENSE)
(`GPL-2.0-only`).

This project is based on the original [Kyty](https://github.com/InoriRus/Kyty), which was released
under the MIT License. Kyty's original copyright and license notice are preserved in
[`LICENSES/Kyty-MIT.txt`](LICENSES/Kyty-MIT.txt). Third-party components remain subject to the
licenses included with those components.

## Special Thanks

- [InoriRus/Kyty](https://github.com/InoriRus/Kyty) â€” KytyPS5 is based on a heavily modified version
  of the original Kyty project.
- [shadps4-emu/shadPS4](https://github.com/shadps4-emu/shadPS4) â€” reference for understanding PS4
  memory behavior, GPU resource aliasing, cache coherency,
  and the AVPlayer implementation.
