# Build Instructions for INAV Custom Firmware

This guide covers building INAV firmware with custom OSD refresh rate configuration for SPEEDYBEEF405WING.

## Prerequisites (One-Time Setup)

Install required build tools via Homebrew:

```bash
brew install cmake ninja gcc-arm-embedded
```

- **cmake**: Build system generator
- **ninja**: Fast parallel build tool
- **gcc-arm-embedded**: ARM cross-compiler toolchain (CMake will download specific version 13.2.1 automatically)

## Building Firmware

### Standard Build Process

```bash
# Navigate to INAV source directory
cd /Users/hntirgeam/Desktop/git/inav

# Configure build system (generates Ninja build files)
cmake -B build -G Ninja

# Enter build directory
cd build

# Compile firmware for SPEEDYBEEF405WING target
ninja SPEEDYBEEF405WING
```

### What Happens During Build

1. **CMake Configuration**: Detects toolchain, analyzes dependencies, generates build files for 279 FC targets
2. **Ninja Compilation**: Compiles ~396 C source files, links them into firmware
3. **Output**: Creates `build/inav_9.0.0_SPEEDYBEEF405WING.hex` (ready to flash)

### Build Output Location

```
build/inav_9.0.0_SPEEDYBEEF405WING.hex
```

## Clean Build (Start From Scratch)

Use this if you modified configuration files like `settings.yaml`:

```bash
rm -rf build
cmake -B build -G Ninja
cd build
ninja SPEEDYBEEF405WING
```

## Building All Targets (Optional)

To build firmware for all 279 flight controller targets:

```bash
cd build
ninja
```

**Warning**: This takes significantly longer and is unnecessary if you only need SPEEDYBEEF405WING.

## Quick Reference

**Rebuild after code changes:**
```bash
cd /Users/hntirgeam/Desktop/git/inav/build
ninja SPEEDYBEEF405WING
```

**Memory usage after successful build:**
- FLASH: 69.04% (633480 B / 896 KB)
- RAM: 82.66% (108340 B / 128 KB)
- CCM: 38.55% (25264 B / 64 KB)

## Custom Modifications

This build includes:
- Configurable OSD refresh rate: `osd_framerate_hz` (1-100 Hz, default 12 Hz)
- CPU overload protection with automatic throttling
- Dynamic refresh rate calculation based on user setting

See `src/main/fc/settings.yaml` for configuration details.
