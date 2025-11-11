# FruityPlayer

<div align="center">

![FruityPlayer](https://img.shields.io/badge/FruityPlayer-Music%20Player-orange?style=for-the-badge)
![Flutter](https://img.shields.io/badge/Flutter-3.24.0-blue?style=for-the-badge&logo=flutter)
![Dart](https://img.shields.io/badge/Dart-3.9.2-blue?style=for-the-badge&logo=dart)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)

A beautiful, cross-platform music player for browsing and playing your personal music library. Built with Flutter and inspired by FL Studio's iconic design.

[Features](#features) • [Installation](#installation) • [Building](#building-from-source) • [Development](#development) • [Contributing](#contributing)

</div>

## Features

### Core Functionality
- 🎵 **Music Library Management** - Organize and browse your music collection
- 🎧 **High-Quality Audio Playback** - Powered by `just_audio` with MediaKit support
- 📱 **Cross-Platform** - Runs on Android, iOS, Linux, macOS, Windows, and Web
- 🎨 **FL Studio-Inspired UI** - Beautiful dark theme with orange accents
- 🔍 **Smart Search** - Quickly find songs, albums, artists, and genres
- 📊 **Metadata Extraction** - Automatic album art and metadata extraction
- 🎹 **Audio Visualization** - Real-time audio waveforms

### Library Organization
- **Songs** - Browse all your music files
- **Albums** - View albums with artwork
- **Artists** - Explore music by artist
- **Genres** - Filter by music genre
- **Playlists** - Create and manage custom playlists

### Playback Features
- **Queue Management** - Drag-and-drop queue organization
- **Shuffle Modes** - Multiple shuffle options
- **Repeat Modes** - Repeat all, one, or off
- **Playback Controls** - Play, pause, skip, seek
- **Volume Control** - Adjustable volume levels
- **MPRIS Support** - Linux media control integration

### User Experience
- **Responsive Design** - Adapts to different screen sizes
- **Smooth Animations** - Fluid transitions and animations
- **Color Extraction** - Dynamic colors from album art
- **Play Count Tracking** - Track your listening habits
- **Recently Played** - Quick access to recent tracks

## Platform Support

| Platform | Status | Notes |
|----------|--------|-------|
| 🐧 Linux | ✅ Supported | MPRIS integration available |
| 🪟 Windows | ✅ Supported | Native Windows support |
| 🍎 macOS | ✅ Supported | Native macOS support |
| 🤖 Android | ✅ Supported | APK and App Bundle |
| 🍎 iOS | ✅ Supported | Requires Apple Developer account |
| 🌐 Web | ✅ Supported | Progressive Web App ready |

## Installation

### Pre-built Releases

Download the latest release from the [Releases](https://github.com/LeivurGargiulo/FruityPlayer-Flutter/releases) page.

#### Android
- Download the APK file and install on your device
- Or install the App Bundle from Google Play (if available)

#### Linux
1. Download the Linux bundle from releases
2. Extract the archive
3. Run `./fruity_player` from the extracted directory
4. (Optional) Create a desktop entry for easy access

#### Windows
1. Download the Windows bundle from releases
2. Extract the ZIP file
3. Run `fruity_player.exe`

#### macOS
1. Download the macOS bundle from releases
2. Extract the DMG or app bundle
3. Move `FruityPlayer.app` to Applications
4. Open from Applications (may require allowing in Security settings)

#### Web
1. Download the web bundle
2. Deploy to any web server
3. Access via browser

### Building from Source

#### Prerequisites

- [Flutter SDK](https://flutter.dev/docs/get-started/install) (3.24.0 or later)
- Dart SDK (3.9.2 or later)
- Platform-specific dependencies:
  - **Linux**: `libgtk-3-dev`, `libblkid1`, `liblzma5`
  - **Windows**: Visual Studio with C++ support
  - **macOS**: Xcode
  - **Android**: Android Studio and Android SDK
  - **iOS**: Xcode and CocoaPods

#### Build Steps

1. **Clone the repository**
   ```bash
   git clone https://github.com/LeivurGargiulo/FruityPlayer-Flutter.git
   cd FruityPlayer-Flutter
   ```

2. **Install dependencies**
   ```bash
   flutter pub get
   ```

3. **Generate code** (for Isar database models)
   ```bash
   flutter pub run build_runner build --delete-conflicting-outputs
   ```

4. **Build for your platform**

   **Linux:**
   ```bash
   flutter build linux --release
   ```

   **Windows:**
   ```bash
   flutter build windows --release
   ```

   **macOS:**
   ```bash
   flutter build macos --release
   ```

   **Android:**
   ```bash
   flutter build apk --release
   # or for App Bundle
   flutter build appbundle --release
   ```

   **iOS:**
   ```bash
   flutter build ios --release
   ```

   **Web:**
   ```bash
   flutter build web --release
   ```

### Docker

FruityPlayer can be run in a Docker container on Linux systems with X11 and PulseAudio support.

#### Prerequisites

- Docker and Docker Compose installed
- X11 server running (for GUI display)
- PulseAudio running (for audio playback)
- User permissions for X11 and PulseAudio sockets

#### Building the Docker Image

Build the Docker image from the project root:

```bash
docker build -t fruityplayer:latest .
```

#### Running with Docker Compose

The easiest way to run FruityPlayer in Docker is using the provided `docker-compose.yml`:

1. **Set environment variables** (optional):
   ```bash
   export MUSIC_DIR=/path/to/your/music
   export APP_DATA_DIR=/path/to/app/data
   export UID=$(id -u)
   export GID=$(id -g)
   export DISPLAY=:0
   ```

2. **Start the container**:
   ```bash
   docker-compose up -d
   ```

3. **View logs**:
   ```bash
   docker-compose logs -f
   ```

4. **Stop the container**:
   ```bash
   docker-compose down
   ```

#### Running with Docker Directly

```bash
docker run -it --rm \
  --network host \
  -e DISPLAY=$DISPLAY \
  -e PULSE_SERVER=unix:/run/user/$(id -u)/pulse/native \
  -v /tmp/.X11-unix:/tmp/.X11-unix:rw \
  -v /run/user/$(id -u)/pulse:/run/user/$(id -u)/pulse:ro \
  -v /path/to/your/music:/music:ro \
  -v /path/to/app/data:/app/data:rw \
  --user $(id -u):$(id -g) \
  fruityplayer:latest
```

#### Volume Mounts

- `/music` - Mount your music library directory (read-only recommended)
- `/app/data` - Application data directory for Isar database persistence (read-write required)
- X11 and PulseAudio sockets - Required for GUI and audio functionality

#### Troubleshooting

- **Display issues**: Ensure X11 permissions are set with `xhost +local:docker` (use with caution)
- **Audio issues**: Verify PulseAudio is running and accessible
- **Permission issues**: Ensure user IDs match between host and container

## Development

### Project Structure

```
lib/
├── database/          # Isar database helper
├── models/            # Data models (Song, Album, Artist, etc.)
├── providers/         # State management (Provider pattern)
├── repositories/      # Data access layer
├── screens/           # UI screens
├── services/          # Business logic services
├── utils/             # Utility functions
└── widgets/           # Reusable widgets
```

### Key Technologies

- **Flutter** - UI framework
- **Provider** - State management
- **Isar** - Local database
- **just_audio** - Audio playback
- **audiotags** - Metadata extraction
- **palette_generator** - Color extraction from images

### Running in Development

1. **Start the app**
   ```bash
   flutter run
   ```

2. **Run tests**
   ```bash
   flutter test
   ```

3. **Run linter**
   ```bash
   flutter analyze
   ```

4. **Format code**
   ```bash
   dart format .
   ```

### Code Generation

The project uses code generation for Isar models. After modifying models:

```bash
flutter pub run build_runner build --delete-conflicting-outputs
```

For watch mode (auto-regenerate on changes):

```bash
flutter pub run build_runner watch --delete-conflicting-outputs
```

### Architecture

FruityPlayer follows a clean architecture pattern:

- **Models** - Data structures and business entities
- **Repositories** - Data access abstraction
- **Services** - Business logic and external integrations
- **Providers** - State management and UI state
- **Screens** - UI presentation layer
- **Widgets** - Reusable UI components

### State Management

The app uses the Provider pattern for state management:

- `AudioProvider` - Playback state and controls
- `LibraryProvider` - Music library state
- `PlaylistProvider` - Playlist management
- `SettingsProvider` - App settings
- `ThemeProvider` - Theme and appearance

## Contributing

Contributions are welcome! Please follow these steps:

1. **Fork the repository**
2. **Create a feature branch**
   ```bash
   git checkout -b feature/amazing-feature
   ```
3. **Make your changes**
4. **Run tests and linter**
   ```bash
   flutter test
   flutter analyze
   ```
5. **Commit your changes**
   ```bash
   git commit -m "Add amazing feature"
   ```
6. **Push to your branch**
   ```bash
   git push origin feature/amazing-feature
   ```
7. **Open a Pull Request**

### Code Style

- Follow [Effective Dart](https://dart.dev/guides/language/effective-dart) guidelines
- Use `flutter analyze` to check for issues
- Format code with `dart format .`
- Write tests for new features

### Reporting Issues

Please use the [GitHub Issues](https://github.com/LeivurGargiulo/FruityPlayer-Flutter/issues) page to report bugs or request features. Include:

- Platform and version
- Steps to reproduce
- Expected vs actual behavior
- Screenshots (if applicable)

## CI/CD

The project uses GitHub Actions for continuous integration:

- **CI Workflow** - Runs tests, linting, and analysis on every push/PR
- **Release Workflow** - Builds and releases artifacts on version tags

### Creating a Release

1. Update version in `pubspec.yaml`
2. Commit and push changes
3. Create and push a tag:
   ```bash
   git tag -a v1.0.0 -m "Release v1.0.0"
   git push origin v1.0.0
   ```
4. GitHub Actions will automatically build and create a release

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- Inspired by FL Studio's iconic design
- Built with [Flutter](https://flutter.dev)
- Audio playback powered by [just_audio](https://pub.dev/packages/just_audio)
- Database powered by [Isar](https://isar.dev)

## Support

- 📖 [Documentation](https://github.com/LeivurGargiulo/FruityPlayer-Flutter/wiki)
- 🐛 [Issue Tracker](https://github.com/LeivurGargiulo/FruityPlayer-Flutter/issues)
- 💬 [Discussions](https://github.com/LeivurGargiulo/FruityPlayer-Flutter/discussions)

---

<div align="center">

Made with ❤️ using Flutter

[⭐ Star this repo](https://github.com/LeivurGargiulo/FruityPlayer-Flutter) if you find it helpful!

</div>
