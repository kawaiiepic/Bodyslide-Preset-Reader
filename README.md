# BodySlide Preset Reader

Unofficial build of [Gaz-tec's BodySlide Preset Reader](https://github.com/Gaz-tec/Bodyslide-Preset-Reader), targeting Skyrim Special Edition 1.7.104 with alandtse/CommonLibSSE-NG v16.8.0. Runtime compatibility is not yet verified in-game.

## Install

Install the original Nexus mod first (it supplies the compiled `Scripts/BodyslideReader.pex`). Download the `GazBodyslideReader` artifact from a successful [Actions build](https://github.com/kawaiiepic/Bodyslide-Preset-Reader/actions) or a tagged release. Install the ZIP as a higher-priority mod so it replaces `SKSE/Plugins/GazBodyslideReader.dll`. The ZIP includes Papyrus source, not a compiled PEX. Use SKSE and Address Library matching your game runtime.

## Build

Use Windows x64, Visual Studio with Desktop development with C++, Ninja, CMake, Git and vcpkg. From an x64 Developer PowerShell for Visual Studio, with VCPKG_ROOT set to your vcpkg directory:

```powershell
cmake -S . -B build -G Ninja -DCMAKE_BUILD_TYPE=Release -DCMAKE_CXX_COMPILER=cl -DCMAKE_C_COMPILER=cl -DCMAKE_TOOLCHAIN_FILE="$env:VCPKG_ROOT/scripts/buildsystems/vcpkg.cmake" -DVCPKG_TARGET_TRIPLET=x64-windows-static -DCMAKE_MSVC_RUNTIME_LIBRARY=MultiThreaded
cmake --build build --config Release --parallel 2
cmake --install build --config Release --prefix package --component Mod
```

GitHub Actions builds pushes to main, pull requests and version tags. Version tags also publish a release ZIP. The static MSVC runtime avoids a separate Visual C++ runtime dependency for this DLL. The upstream project and fork are GPL-3.0; see LICENSE.
