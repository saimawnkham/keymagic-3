# KeyMagic 3

**KeyMagic: Rust-powered, vibe-coded, future-ready**

A modern rewrite of the KeyMagic input method editor in Rust, focusing on performance, memory safety, and cross-platform compatibility. This project represents the afterlife of the original KeyMagic IME, rebuilt from the ground up using AI-assisted development techniques.

## Project Structure

This is a monorepo organized as a Cargo workspace:

```
keymagic-v3/
├── Cargo.toml                 # Workspace configuration
├── keymagic-core/            # Core engine library
│   ├── Cargo.toml
│   └── src/
│       ├── lib.rs
│       └── types/            # KM2 format types and definitions
├── kms2km2/                  # KMS to KM2 converter
│   ├── Cargo.toml
│   ├── src/
│   │   ├── lib.rs
│   │   ├── lexer/           # KMS lexical analyzer
│   │   ├── parser/          # KMS parser
│   │   ├── binary/          # KM2 compilation and writing
│   │   └── bin/
│   │       ├── kms2km2.rs   # CLI converter
│   │       └── km2_dump.rs  # KM2 file dumper
│   └── tests/
├── keymagic-ibus/           # Linux IBus integration (placeholder)
├── keymagic-macos/          # macOS IMK integration (placeholder)
└── keymagic-windows/        # Windows implementation
    ├── tsf/                 # Text Services Framework IME
    ├── gui-tauri/          # Configuration Manager GUI
    └── installer/          # Windows installer scripts
```

## Architecture

Following the Software Design Document (SDD.md), the project is structured as:

1. **keymagic-core**: Platform-agnostic core engine
   - KM2 format definitions
   - Layout parsing and rule matching
   - State management
   - FFI API for platform integrations

2. **kms2km2**: Converter utility (Phase 1 complete)
   - Lexer for KMS script format
   - Parser generating AST
   - Compiler to KM2 binary format
   - Binary writer with proper endianness

3. **Platform Integrations**:
   - keymagic-ibus: Linux desktop support via IBus (placeholder)
   - keymagic-macos: macOS support via Input Method Kit (placeholder)
   - keymagic-windows: Windows support with:
     - TSF (Text Services Framework) IME implementation
     - Tauri-based Configuration Manager GUI
     - Inno Setup installer

## Building

```bash
# Build all crates
cargo build --workspace

# Build specific crate
cargo build -p kms2km2

# Build Windows components
cd keymagic-windows
# Use the unified build script
make.bat build         # Build for ARM64 Release (default)
make.bat build x64     # Build for x64 Release
make.bat build x64 Debug  # Build for x64 Debug

# Or build components individually:
# Build TSF only
cd tsf && mkdir build-arm64 && cd build-arm64
cmake -G "Visual Studio 17 2022" -A ARM64 ..
cmake --build . --config Release

# Build GUI only
cd gui-tauri
cargo tauri build

# Run tests
cargo test --workspace
```

## Usage

### Convert KMS to KM2

```bash
cargo run -p kms2km2 -- input.kms output.km2
```

### Dump KM2 file contents

```bash
cargo run -p kms2km2 --bin km2_dump -- file.km2
```

### Windows Development

To build from source on Windows:

```bash
cd keymagic-windows
# Build everything (TSF + GUI)
make.bat build         # ARM64 Release
make.bat build x64     # x64 Release

# Register TSF (requires admin)
make.bat register

# Check build status
make.bat status

# Clean build artifacts
make.bat clean

# Unregister TSF (requires admin)
make.bat unregister
```

### Windows Installation

For Windows users, download the installer from the [releases page](https://github.com/thantthet/keymagic-3/releases):
- `KeyMagic3-Setup-x.x.x-x64.exe` for 64-bit Windows
- `KeyMagic3-Setup-x.x.x-arm64.exe` for ARM64 Windows

After installation:
1. KeyMagic will appear in the system tray
2. Use the Configuration Manager to add keyboard layouts (.km2 files)
3. Configure hotkeys for switching between keyboards
4. The TSF IME will be automatically registered with Windows

## Development Status

- ✅ Phase 1: KMS to KM2 Converter (Complete)
- ✅ Phase 2: Core Engine Development (Complete)
- ⏳ Phase 3: Linux Integration (Planned)
- ⏳ Phase 4: macOS Integration (Planned)
- ✅ Phase 5: Windows Integration (Complete)
  - ✅ Phase 5.1: Foundation Setup
  - ✅ Phase 5.2: Core TSF Functionality
  - ✅ Phase 5.3: GUI Configuration Manager
  - ✅ Phase 5.4: System Integration
  - ✅ Phase 5.5: Installer and Deployment
- ⏳ Phase 6: Advanced Features & Optimization (Planned)

## Features

### Core Engine
- Full KMS parsing support including:
  - Variable declarations
  - Unicode literals (U1000, u1000)
  - String literals
  - Virtual key combinations
  - Pattern matching with wildcards ([*], [^])
  - Back-references ($1, $2, etc.)
  - State management
  - Include directives
- Binary KM2 format generation (version 1.5)
- Command-line interface
- Cross-platform support

### Windows Implementation
- **Text Services Framework (TSF) IME**:
  - Full TSF integration with Windows
  - Composition string management
  - Keyboard switching via registry
  - Multi-threaded, thread-safe design
- **Configuration Manager GUI**:
  - Modern Tauri-based interface
  - Keyboard management (add/remove/activate)
  - Hotkey configuration
  - System tray integration
  - Auto-update mechanism
  - Dark mode support
- **Windows Installer**:
  - Inno Setup based installer
  - Auto-start with Windows
  - Registry integration
  - Clean uninstallation

## License

GPL-2.0