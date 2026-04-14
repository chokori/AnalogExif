# AnalogExif 1.0.0 (Modernized Fork)

This repository is a **modernized and maintained fork** of the original  
[AnalogExif project on SourceForge](https://sourceforge.net/projects/analogexif/).

AnalogExif is a tool for editing and managing EXIF metadata for **scanned film photographs**.  
This fork updates the project to a modern toolchain using **Qt 6**, **CMake**, and **vcpkg (manifest mode)**,  

https://github.com/chokori/analogexif
---

## ✨ Features

- Edit EXIF metadata for scanned film images  
- Modern Qt 6 user interface  
- Fully reproducible builds using CMakePresets + vcpkg  
- Exiv2 0.28.0, Expat 2.7.4, QT 6.11
- C++17
- Visutal Studio 2022

---

## 📦 Dependencies

- Exiv2 0.28.0 with features:
  - `bmff`
  - `png`
  - `xmp`
- Expat 2.7.4
- QT6.11


## Build
### 0. Use the correct environment

Before building, open:

**x64 Native Tools Command Prompt for VS 2022**

This ensures the correct MSVC compiler, environment variables, and toolchain are available.

### 1. Initialize submodules
```
git submodule update --init --recursive
```

### 2. Debug build
```
cmake --preset x64-debug
cmake --build build/debug --config Debug
```

### 3. Release build
```
cmake --preset x64-release
cmake --build build/release --config Release
```
