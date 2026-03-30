# SimpleIO

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![C++20](https://img.shields.io/badge/C%2B%2B-20-blue.svg)](https://isocpp.org/)
[![Header-only](https://img.shields.io/badge/header--only-green.svg)](https://en.wikipedia.org/wiki/Header-only)

A lightweight C++20 header‑only library for basic file and filesystem operations with `std::format`‑style path construction.

## Description

`SimpleIO` provides a collection of static methods to read, write, query, and manipulate files and directories. All paths are built using `std::vformat`, allowing you to embed arguments directly into the path string. The library is exception‑safe and uses the C++17 filesystem library together with C++20 `std::format`.

## Requirements

- C++20 compiler (e.g., GCC 11+, Clang 14+, MSVC 2022)
- Standard library with `<format>` support

## Installation

### Using CMake FetchContent

Add the following to your `CMakeLists.txt`:

```cmake
include(FetchContent)

FetchContent_Declare(
    simple_io
    GIT_REPOSITORY https://github.com/ania-7abc/simple_io.git
    GIT_TAG v1.0.0
)

FetchContent_MakeAvailable(simple_io)

# Link against the library
target_link_libraries(your_target PRIVATE simple_io)
```

The library exports the target `simple_io`. Since it is header‑only, no additional build steps are required.

## Usage

Include the header:

```cpp
#include <simple_io/simple_io.hpp>
```

### Writing a File

```cpp
std::string data = "Hello, world!";
SimpleIO::write("{}/file_{}.txt", data, "folder", 42);
// Creates folder/file_42.txt with content "Hello, world!"
```

### Reading a File

```cpp
std::string content = SimpleIO::read("{}/file_{}.txt", "folder", 42);
// content == "Hello, world!"
```

### Checking if a File Exists (Regular File)

```cpp
bool isReg = SimpleIO::is_file("{}/file_{}.txt", "folder", 42);
```

### Checking Existence (Any Filesystem Object)

```cpp
bool exists = SimpleIO::exists("{}/file_{}.txt", "folder", 42);
```

### Removing a File or Directory

```cpp
SimpleIO::remove("{}/file_{}.txt", "folder", 42);
// Also works for directories (removes recursively)
```

### Listing a Directory

```cpp
std::vector<std::string> entries = SimpleIO::list_dir("{}", "some/path");
// Returns filenames (not full paths)
```

### Creating Directories

```cpp
SimpleIO::create_path("{}/nested/{}", "parent", "child");
// Equivalent to mkdir -p parent/nested/child
```

## Error Handling

All methods throw exceptions on failure:

- `std::ios::failure` for stream errors (e.g., cannot open file)
- `std::filesystem::filesystem_error` for filesystem errors
- `std::format_error` for malformed format strings

Use `try`/`catch` blocks to handle errors gracefully.

## API Reference

| Method | Description |
|--------|-------------|
| `write(fmt, data, args...)` | Writes `data` to a file whose path is built from `fmt` and `args`. |
| `read(fmt, args...) -> std::string` | Reads the entire contents of a file and returns it as a string. |
| `is_file(fmt, args...) -> bool` | Returns `true` if the path exists and is a regular file. |
| `exists(fmt, args...) -> bool` | Returns `true` if the path exists (any type). |
| `remove(fmt, args...)` | Removes the file or directory (recursively for directories). |
| `list_dir(fmt, args...) -> std::vector<std::string>` | Returns the names of entries in a directory (not full paths). |
| `create_path(fmt, args...)` | Creates all directories in the path (like `mkdir -p`). |

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
