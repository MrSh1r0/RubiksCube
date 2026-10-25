# RubiksCube

A small OpenGL-based Rubik's Cube application created as part of a university project.

## Features
- Interactive 3D Rubik's Cube rendering
- Mouse and keyboard controls
- Cube scrambling and reset actions
- Cross-platform build support for macOS and Windows

## Controls
- Left click and drag: rotate the selected row
- Right click and drag: rotate the whole cube
- W / A / S / D: rotate the cube view
- Q / E: rotate around the Z axis
- R: shuffle the cube
- F: reset or finish the cube
- Space: reset the orientation

## Requirements
The project uses:
- GLFW
- GLEW
- GLM

## Build on macOS (Apple Silicon)
Install CMake, Ninja, and the libraries with Homebrew:

```bash
brew install cmake ninja glfw glew glm
```

Configure and build in a platform-specific directory. Ninja avoids generator issues
with paths that contain special characters:

```bash
cmake -S . -B build-macos -G Ninja -DCMAKE_BUILD_TYPE=Debug
cmake --build build-macos
```

Run the application:

```bash
./build-macos/RubiksCube
```

## Build on Windows
Install Visual Studio 2022 with the **Desktop development with C++** workload,
then install vcpkg. From PowerShell, for example:

```powershell
git clone https://github.com/microsoft/vcpkg.git C:\vcpkg
C:\vcpkg\bootstrap-vcpkg.bat
$env:VCPKG_ROOT = "C:\vcpkg"
& "$env:VCPKG_ROOT\vcpkg.exe" install glfw3:x64-windows glew:x64-windows glm:x64-windows
```

To use the Visual Studio IDE, open the repository folder (or its `CMakeLists.txt`)
as a CMake project. The CMake configuration is the single source of truth for
both platforms; there is no separate Visual Studio solution to maintain.

Configure and build with the vcpkg toolchain:

```powershell
cmake -S . -B build-windows -G "Visual Studio 17 2022" -A x64 `
  -DCMAKE_TOOLCHAIN_FILE="$env:VCPKG_ROOT/scripts/buildsystems/vcpkg.cmake" `
  -DVCPKG_TARGET_TRIPLET=x64-windows
cmake --build build-windows --config Debug
```

Run the application:

```powershell
.\build-windows\Debug\RubiksCube.exe
```

Keep the macOS and Windows build directories separate; CMake build directories
are tied to the generator and platform used when they were first configured.

## GitHub Releases
A GitHub Actions workflow is included at [.github/workflows/release.yml](.github/workflows/release.yml).
Pushing a tag such as v1.0.0 will build macOS and Windows binaries and publish them as GitHub Release assets.

## Project Structure
- [src](src): C++ source and headers
- [assets](assets): shaders and audio used at runtime
- [docs](docs): project design documentation
- [CMakeLists.txt](CMakeLists.txt): cross-platform build configuration
- [README.md](README.md): project documentation
- [.github/workflows/release.yml](.github/workflows/release.yml): automated release build pipeline
