# Godot Jenova

A 1:1 fork of Godot Engine with added support for Jenova Framework Nested Extensions Hot-Reloading. While basic hot-reloading works on all Godot forks, Nested Extension (NE) support requires a compatible distribution. This repository is the officially supported Godot with Jenova Compatibility, preserving all upstream engine behavior and workflows.

## Overview

Godot Jenova is a modified version of the Godot Engine that provides enhanced hot-reloading capabilities through the Jenova Framework. This allows developers to make changes to their game code and see them reflected in real-time without restarting the entire application, significantly improving development workflow.

## Features

- **Nested Extensions Hot-Reloading**: Advanced hot-reloading support for complex project structures
- **Full Godot Compatibility**: Maintains 100% compatibility with standard Godot Engine workflows
- **Cross-Platform Support**: Available for Linux, with plans for additional platforms
- **Mono Support**: Full C# support through the Mono variant
- **Performance Optimized**: Maintains the performance characteristics of the upstream Godot Engine

## Packages

This repository provides two main packages:

- `godot-jenova`: The standard version without Mono support
- `godot-jenova-mono`: The version with full C#/.NET support

## Installation

### From AUR (Arch Linux)

```bash
# Install the standard version
yay -S godot-jenova

# Install the Mono version (for C# development)
yay -S godot-jenova-mono
```

### From Source

1. Clone this repository:
   ```bash
   git clone https://github.com/TheAenema/godot-jenova.git
   cd godot-jenova
   ```

2. Install dependencies (Arch Linux):
   ```bash
   sudo pacman -S --needed git alsa-lib base-devel brotli ca-certificates embree \
     freetype2 libglvnd libspeechd libsquish libtheora libvorbis libwebp \
     libwslay libxcursor libxi libxinerama libxrandr miniupnpc openxr pcre2
   ```

3. For Mono support, also install:
   ```bash
   sudo pacman -S --needed dotnet-sdk-8.0 nuget pulse-native-provider
   yay -S --needed scons setconf
   ```

4. Build the package:
   ```bash
   makepkg -s
   ```

5. Install the package:
   ```bash
   sudo pacman -U *.pkg.tar.zst
   ```

## Usage

### Standard Version

Launch Godot Jenova from your application menu or run:

```bash
godot-jenova
```

### Mono Version

Launch Godot Jenova with Mono support:

```bash
godot-jenova-mono
```

## Jenova Framework Integration

The Jenova Framework provides enhanced hot-reloading capabilities that go beyond Godot's built-in hot-reloading:

1. **Nested Extensions**: Support for hot-reloading nested components and extensions
2. **State Preservation**: Maintains application state during hot-reloads
3. **Real-time Updates**: Immediate reflection of code changes without restart

### Basic Usage

1. Create your project as you would in standard Godot
2. Enable Jenova Framework in Project Settings
3. Use the enhanced hot-reloading features during development

## Requirements

### System Dependencies

- **Arch Linux** (primary target)
- **x86_64** architecture
- **OpenGL 3.3+** compatible graphics card
- **2GB+ RAM** (4GB+ recommended for large projects)

### Build Dependencies

- `git`
- `scons`
- `alsa-lib`
- `base-devel`
- Additional libraries as listed in the PKGBUILD

### Optional Dependencies

- `pipewire-alsa`: For enhanced audio support
- `pulse-native-provider`: For PulseAudio support
- `dotnet-sdk-8.0`: Required for Mono version

## Development

### Building from Source

To build Godot Jenova from the latest source:

```bash
# Clone the repository
git clone https://github.com/TheAenema/godot-jenova.git
cd godot-jenova

# Build using SCons
scons platform=linuxbsd target=editor

# For Mono support
scons platform=linuxbsd target=editor module_mono_enabled=yes
```

### Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## Troubleshooting

### Common Issues

1. **Audio not working**: Install `pipewire-alsa` or `pulse-native-provider`
2. **Mono issues**: Ensure `dotnet-sdk-8.0` is properly installed
3. **Build failures**: Check that all dependencies are installed

### Getting Help

- Open an issue on [GitHub Issues](https://github.com/TheAenema/godot-jenova/issues)
- Check the [Godot Documentation](https://docs.godotengine.org/) for general Godot questions
- Visit the [Jenova Framework documentation](https://github.com/TheAenema/jenova-framework) for framework-specific questions

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- **Godot Engine**: For providing the excellent base engine
- **Jenova Framework**: For the enhanced hot-reloading capabilities
- **Contributors**: All the developers who have contributed to this project

## Related Projects

- [Godot Engine](https://github.com/godotengine/godot): The upstream Godot Engine
- [Jenova Framework](https://github.com/TheAenema/jenova-framework): The framework that enables enhanced hot-reloading

## Version Information

- **Current Version**: 4.6.dev1
- **Based on Godot**: 4.6
- **Maintainer**: CuteAppi <cuteappis@gmail.com>