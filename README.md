# Application Source Tree
```
auto-hud-v7/
├── CMakeLists.txt                  # Top-level CMake file for building the C++ embedder (integrates Flutter engine, dependencies, and installation)
├── README.md                       # Documentation with Yocto build commands, setup instructions, cross-compile notes, environment variables (e.g., API keys), and Flutter-specific setup (e.g., pub get, engine build)
├── pubspec.yaml                    # Flutter project config: dependencies (e.g., mapbox_gl, audioplayers for music, camera for surveillance), assets, fonts
├── analysis_options.yaml           # Flutter lint rules for code quality
├── lib/                            # Dart/Flutter code for UI (widgets, screens, providers)
│   ├── main.dart                   # Entry point: runs the app, sets up MaterialApp with theme, routes, and providers
│   ├── controllers/                # Dart controllers for logic (mirroring C++ backends, using platform channels)
│   │   ├── workspace_controller.dart # Manages workspace switching, fade-swipe transitions
│   │   ├── volume_controller.dart  # Volume slider, mute (via platform channel to C++ audio backend)
│   │   ├── time_controller.dart    # Time display, calendar overlay
│   │   ├── bluetooth_controller.dart # Bluetooth overlay (via BlueZ channel)
│   │   ├── network_controller.dart # Network overlay (via NetworkManager channel)
│   │   ├── ai_controller.dart      # AI overlay (chat with API provider)
│   │   ├── hud_controller.dart     # HUD data (speed, fuel; via data provider channel)
│   │   ├── music_controller.dart   # Music playback (audioplayers plugin + GStreamer channel)
│   │   ├── climate_controller.dart # Climate UI (static replication)
│   │   ├── vehicle_controller.dart # Vehicle 3D model (using flutter_3d or custom widget)
│   │   ├── surveillance_controller.dart # Camera feeds (camera plugin + GStreamer channel)
│   │   ├── maps_controller.dart    # Maps (mapbox_gl plugin with GPS marker)
│   │   ├── settings_controller.dart # Settings overlay
│   │   └── data_provider.dart      # Mock/real data interface (via C++ channel)
│   ├── screens/                    # Main screens/workspaces
│   │   ├── music_workspace.dart    # Music view
│   │   ├── climate_workspace.dart  # Climate view (exact from image 2)
│   │   ├── vehicle_workspace.dart  # Vehicle view (3D from image 3)
│   │   ├── surveillance_workspace.dart # Surveillance view
│   │   ├── maps_workspace.dart     # Maps view (default at boot)
│   │   └── settings_overlay.dart   # Settings overlay
│   ├── widgets/                    # Reusable Flutter widgets
│   │   ├── top_bar.dart            # Top bar (arrows, volume, time, icons)
│   │   ├── hud.dart                # Lower HUD (speed, fuel, distance)
│   │   ├── dock.dart               # Bottom dock with buttons
│   │   ├── calendar_overlay.dart   # Time tap overlay
│   │   ├── bluetooth_overlay.dart  # Bluetooth panel
│   │   ├── network_overlay.dart    # Network panel
│   │   ├── ai_overlay.dart         # AI chat modal
│   │   ├── mini_player_overlay.dart # Mini player overlay
│   │   ├── driver_wellness_overlay.dart # Notification slide-over
│   │   ├── fade_swipe_transition.dart # Custom animation for workspace switches
│   │   ├── icon_button.dart        # Generic icon button
│   │   ├── custom_slider.dart      # Slider (volume, progress)
│   │   └── glass_panel.dart        # Translucent panel style
│   ├── theme.dart                  # Dark theme (colors, fonts, radii, shadows from images)
│   └── utils.dart                  # Helper utils (e.g., animations, config)
├── src/                            # C++ embedder and backends (hosts Flutter engine, provides platform channels)
│   ├── main.cpp                    # Embedder entry: initializes Flutter engine, runs Dart entrypoint, sets up platform channels
│   ├── embedder/                   # Flutter embedder specifics (based on flutter_embedder.h)
│   │   ├── flutter_embedder.cpp    # Custom embedder for RPi (handles rendering, input, plugins)
│   │   └── flutter_embedder.h
│   ├── channels/                   # Platform channel implementations
│   │   ├── volume_channel.cpp      # Audio integration (ALSA/PipeWire)
│   │   ├── volume_channel.h
│   │   ├── bluetooth_channel.cpp   # BlueZ integration
│   │   ├── bluetooth_channel.h
│   │   ├── network_channel.cpp     # NetworkManager D-Bus/nmcli
│   │   ├── network_channel.h
│   │   ├── data_channel.cpp        # Mock/real data providers (speed, fuel, GPS, cameras)
│   │   ├── data_channel.h
│   │   ├── music_channel.cpp       # GStreamer audio
│   │   ├── music_channel.h
│   │   ├── surveillance_channel.cpp # GStreamer camera feeds
│   │   └── surveillance_channel.h
│   └── utils.cpp                   # C++ helpers (e.g., config reading, error handling)
│   └── utils.h
├── assets/                         # Icons, SVGs, images, fonts (integrated via pubspec.yaml)
│   ├── icons/                      # All required SVG/PNG icons (no placeholders; exhaustive list based on spec/images)
│   │   ├── back_arrow.svg          # Top bar previous workspace (◀)
│   │   ├── next_arrow.svg          # Top bar next workspace (▶)
│   │   ├── speaker_unmuted.svg     # Top bar volume icon (unmuted state)
│   │   ├── speaker_muted.svg       # Top bar volume icon (muted state)
│   │   ├── bluetooth_connected.svg # Top bar bluetooth icon (connected state)
│   │   ├── bluetooth_disconnected.svg # Top bar bluetooth icon (disconnected state)
│   │   ├── network_wifi_strong.svg # Top bar network icon (strong signal)
│   │   ├── network_wifi_weak.svg   # Top bar network icon (weak signal)
│   │   ├── network_cellular.svg    # Top bar network icon (cellular fallback)
│   │   ├── ai_diamond.svg          # Top bar AI indicator (diamond glyph)
│   │   ├── music_note.svg          # Dock music icon
│   │   ├── climate_thermo.svg      # Dock climate icon
│   │   ├── vehicle_car.svg         # Dock vehicle icon
│   │   ├── surveillance_camera.svg # Dock surveillance icon
│   │   ├── maps_pin.svg            # Dock maps icon
│   │   ├── settings_gear.svg       # Dock settings icon
│   │   ├── fuel_pump.svg           # HUD fuel icon
│   │   ├── fuel_bar_full.svg       # HUD fuel bar (full state)
│   │   ├── fuel_bar_half.svg       # HUD fuel bar (half state)
│   │   ├── fuel_bar_low.svg        # HUD fuel bar (low state)
│   │   ├── play_button.svg         # Music transport play
│   │   ├── pause_button.svg        # Music transport pause
│   │   ├── previous_track.svg      # Music transport previous
│   │   ├── next_track.svg          # Music transport next
│   │   ├── connect_button.svg      # Overlay connect (bluetooth/network)
│   │   ├── disconnect_button.svg   # Overlay disconnect (bluetooth/network)
│   │   ├── scan_button.svg         # Overlay scan (bluetooth)
│   │   ├── calendar_icon.svg       # Calendar overlay icon
│   │   ├── close_button.svg        # Overlay close buttons
│   │   ├── brightness_icon.svg     # Settings display brightness
│   │   ├── theme_icon.svg          # Settings display theme
│   │   ├── units_icon.svg          # Settings units (km/h)
│   │   ├── date_time_icon.svg      # Settings system date/time
│   │   ├── about_icon.svg          # Settings system about
│   │   ├── cloud_icon.svg          # Climate weather cloud
│   │   ├── sun_icon.svg            # Climate weather sun
│   │   ├── rain_icon.svg           # Climate weather rain
│   │   └── wind_icon.svg           # Climate weather wind
│   ├── images/                     # All required images (no placeholders; exhaustive list based on spec/images)
│   │   ├── car_model_3d.png        # Vehicle workspace interactive car model (from image 3)
│   │   ├── weather_graph_line.png  # Climate workspace temperature/precipitation graph (from image 2)
│   │   ├── weather_hourly_bg.png   # Climate workspace hourly forecast background (from image 2)
│   │   ├── map_placeholder.png     # Maps workspace fallback if no tiles (e.g., offline world map from image 1)
│   │   ├── music_artwork_placeholder.png # Music now playing thumbnail fallback
│   │   ├── driver_wellness_card_bg.png # Driver wellness overlay background (slide-over from images)
│   │   └── surveillance_feed_placeholder.png # Surveillance camera feed fallback (if no camera)
│   └── fonts/                      # All required fonts (exhaustive; custom for high-contrast numerics/labels)
│       ├── auto-hud-v7-bold.ttf    # Bold font for large numerics (e.g., speed 65, time 2:47 PM)
│       └── auto-hud-v7-regular.ttf # Regular font for labels (e.g., km/h, Music, Climate)
├── plugins/                        # Custom Flutter plugins (if needed for C++ integration)
│   └── mock_data_plugin.dart       # Example plugin for mock data
├── test/                           # Flutter tests (widget/integration tests)
│   ├── widget_test.dart            # Tests for key widgets (e.g., top_bar, hud)
│   └── controller_test.dart        # Tests for controllers (e.g., workspace switching)
├── scripts/                        # Helper scripts
│   ├── run-desktop.sh              # Runs embedder on desktop with simulated providers (using flutter-pi or similar)
│   ├── build-flutter.sh            # Builds Flutter bundle for embedding
│   └── cross-compile-setup.sh      # Setup for cross-compiling embedder to RPi 4
└── config/                         # Configuration files
    └── auto-hud-v7.conf            # Config (e.g., AI API key, units)
```


# Yocto Custom Layer (Root: meta-auto-hud/)
```
meta-auto-hud/
├── conf/
│   └── layer.conf                  # Layer configuration (BBPATH, priorities)
├── recipes-auto-hud/
│   ├── auto-hud-v7/
│   │   ├── auto-hud-v7.bb          # Recipe: builds C++ embedder, bundles Flutter app, installs
│   │   ├── files/
│   │   │   ├── auto-hud-v7.service # Systemd unit for autostart
│   │   │   └── auto-hud-v7.conf    # Default config
│   └── packagegroup/
│       └── packagegroup-auto-hud.bb # Pulls dependencies: Flutter engine (for arm64), Dart runtime, GStreamer, BlueZ, NetworkManager, wpa_supplicant, ALSA/PipeWire, fonts, icons, Mapbox SDK (via Flutter plugin), etc.
├── recipes-core/
│   └── images/
│       └── auto-hud-v7-image.bb    # Image recipe: bootable RPi 4 image with Flutter engine, GPU/touch enabled, app autostart
├── recipes-bsp/
│   └── bootfiles/
│       └── rpi-config/              # Config snippets for RPi 4 (FKMS/KMS, evdev/libinput, GPU drivers, Wi-Fi/BT firmware, timesyncd)
│           └── config.txt.bbappend
└── README.md                       # Layer readme with bblayers.conf, local.conf (add EXTRA_IMAGE_FEATURES += "flutter"), bitbake commands (e.g., bitbake auto-hud-v7-image), flashing instructions
```
