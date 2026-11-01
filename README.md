# Omi

Omi is an imperative, source-based package manager written in C++23.

It handles package installation, dependency resolution, and source compilation using TOML recipes.

> [!NOTE]
> Omi is no longer actively developed. It was the original predecessor to [Synnical](https://github.com/voltiksrc/synnical), and is being preserved as a historical project.
>
> It is functional in some areas, but still experimental and not fully reliable.

## Features

- **Source-based package installation** — Downloads, verifies, extracts, compiles, and installs packages from source tarballs
- **Dependency resolution** — Uses libsolv to resolve dependencies and determine installation order
- **TOML recipes** — Package metadata, source URLs, dependencies, and build systems are defined in TOML files
- **SHA-256 verification** — Verifies downloaded sources using OpenSSL
- **Build system support** — Make/Autotools, CMake, Meson, Cargo, Waf, Go, and OpenSSL's build system
- **Installation staging** — Stages compiled files before deploying them into the target filesystem
- **Package tracking** — Uses SQLite to record installed packages and their files
- **Package removal** — Removes installed packages using recorded file ownership
- **Repository synchronization** — Downloads and validates repository metadata

## Package management

Omi uses an imperative command-line interface. Packages are installed individually or in groups rather than through a declarative configuration.

### Installing packages

Install a package:

```bash
omi -Fetch fastfetch
```

Install multiple packages:

```bash
omi -Fetch zlib libpng
```

Omi resolves the required dependencies before compiling and installing the requested packages.

### Available commands

| Command | Description |
|---|---|
| `-Fetch <packages...>` | Resolve and install packages |
| `remove <package>` | Remove an installed package |
| `rm <package>` | Alias for remove |
| `search <query>` | Search available packages |
| `resolve <package>` | Preview package dependencies |
| `--sync` | Sync repository metadata |
| `--list-installed` | List installed packages |
| `--version` | Display Omi's version |
| `--help`, `-h` | Display available commands |

## Building

Omi uses **CMake** and requires a C++23-compatible compiler.

### Dependencies

The main development dependencies are:

- C++23 compiler
- CMake
- libcurl
- libarchive
- toml++
- SQLite3
- libsolv and libsolvext
- OpenSSL (libcrypto)
- pkg-config

Additional build tools such as GNU Make, Ninja, Meson, Cargo, Waf, and Go may be needed depending on which packages you attempt to install.

### Compile from source

Clone the repository:

```bash
git clone https://github.com/voltiksrc/Omi.git
cd Omi
```

Configure with CMake:

```bash
cmake -S . -B build -G Ninja
```

Compile Omi:

```bash
cmake --build build
```

### Installation

Omi includes a CMake installation target:

```bash
sudo cmake --install build
```

> [!WARNING]
> Omi is not recommended for everyday package management. It still uses development-specific paths, and several features have not been thoroughly tested.
>
> The installation root is currently set to `/tmp/omi-test-root` rather than the actual system root.

## Package repository

Omi uses a separate repository to store package recipes and dependency metadata.

Its repository synchronization system was originally designed around [omi-inventory](https://github.com/voltiksrc/omi-inventory).

Package recipes are written in TOML. A simplified example:

```toml
[package]
name = "zlib"
version = "1.3.2"
architecture = "x86_64"

[source]
url = "https://example.org/zlib.tar.gz"
sha256 = "..."

[build]
system = "make"

[deps]
requires = []
```

Omi downloads recipes using libcurl and uses libsolv metadata to resolve dependencies before installation.

## Development status

Omi began as an independent package manager before eventually evolving into Synnical.

**Implemented features**

- [x] Source downloading and extraction
- [x] SHA-256 verification
- [x] Source compilation and installation staging
- [x] Dependency resolution with libsolv
- [x] SQLite installation tracking
- [x] Installed-package detection
- [x] Package searching and removal
- [x] Repository metadata synchronization
- [x] Installation rollback handling

Although these features have been implemented, some are incomplete or haven't been thoroughly tested.

Omi is no longer receiving regular updates, but I may revisit it in the future for another project.

## Synnical

Omi eventually became the foundation for [Synnical](https://github.com/voltiksrc/synnical), a source-based declarative/imperative hybrid system manager.

Synnical builds upon Omi's original package management system, with additional improvements and features such as Daycare, an optional declarative configuration system for managing packages and basic system settings.

Development has moved to Synnical, while Omi remains available as its original predecessor.

## License

Refer to the repository's license file for licensing information.
