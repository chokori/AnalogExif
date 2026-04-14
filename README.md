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

*(Both Exiv2 and Expat are automatically managed through vcpkg — no manual installation required.)*
- Exiv2 0.28.0 with features:
  - `bmff`
  - `png`
  - `xmp`
- Expat 2.7.4

- QT6.11
*Qt is **not** installed automatically. You must install Qt 6.11 manually and set the `QTDIR` environment variable.*



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

## Database Error

If you encounter a database error on first launch, select the default database located at:

**/res/db/AnalogExif.ael**


## 🚀 How to Build and Package (Windows)

This project uses **CMake** and **CPack** to generate a Windows installer (.exe). To include all dependencies (Qt6, Exiv2, Expat), follow these steps:

### Prerequisites
1. **Visual Studio 2022** (with C++ Desktop Development workload)
2. **CMake** (3.21 or higher)
3. **vcpkg**: For managing `exiv2` and `expat`.
4. **NSIS**: Required to generate the `.exe` installer. [Download NSIS](https://sourceforge.net/projects/nsis/)

### Build and Packaging Steps

1. **Clone the repository**
```
git clone [https://github.com/chokori/AnalogExif.git](https://github.com/chokori/AnalogExif.git)
cd AnalogExif
```
2. Configure the project Replace [vcpkg root] with your actual vcpkg installation path.
```
cmake -S . -B build -DCMAKE_TOOLCHAIN_FILE=./external/vcpkg/scripts/buildsystems/vcpkg.cmake -DCMAKE_BUILD_TYPE=Release
```
3. Build the project
```
cmake --build build --config Release
```
4. Generate the Installer (.exe)
```
cd build
cpack -C Release
```
5. Result
You will find the installer (e.g., AnalogExif-1.0.0-win64.exe) in the build directory.