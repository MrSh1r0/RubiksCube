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
Install the dependencies with Homebrew:

```bash
brew install glfw glew glm
```

Build the project:

```bash
cmake -S . -B build
cmake --build build -j4
```

Run the application:

```bash
./build/RubiksCube
```

A release-style local build output is also supported:

```bash
./arm64/Release/RubiksCube
```

## Build on Windows
Install the dependencies with vcpkg:

```powershell
vcpkg install glfw3:x64-windows glew:x64-windows glm:x64-windows
```

Build the project:

```powershell
cmake -S . -B build -G "Visual Studio 17 2022" -A x64
cmake --build build --config Release
```

Run the application:

```powershell
./build/Release/RubiksCube.exe
```

## GitHub Releases
A GitHub Actions workflow is included at [.github/workflows/release.yml](.github/workflows/release.yml).
Pushing a tag such as v1.0.0 will build macOS and Windows binaries and publish them as GitHub Release assets.

## Project Structure
- [RubiksCube](RubiksCube): source code and assets
- [README.md](README.md): project documentation
- [.github/workflows/release.yml](.github/workflows/release.yml): automated release build pipeline
