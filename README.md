# Snes360 Enhanced

> [!NOTE]
> This is a fork that merely upgrades the build toolchain, reorganizes and cleans up the structure of the repository.</br>
> All credits go to [frankischilling](https://github.com/frankischilling) for the creation of Snes360 Enhanced.

Enhanced Xbox 360 port of the Snes9x v1.51 emulator, focused on Direct3D rendering and Xbox 360-specific optimizations. Core emulation code remains intact; improvements are limited to the Xbox UI layer (XUI scenes, Direct3D rendering, XAudio2 audio, input handling).

Based on the original Xbox 360 port (Snes360 v0.32 Beta, credited to "Anonymous" in source code). **ModernVintageGamer (Dimitris)** worked extensively on the original port and **provided the source code** that made this edition possible. This enhanced version **removes all achievement functionality** to prevent Xbox Live bans, along with build fixes and C89 compatibility improvements.

**Current Version: v1.0**

+ **Toolchain:** Microsoft Visual Studio 2010 SP1
+ **SDK:** Xbox 360 XDK v2.0.21256.3
+ **Target:** Xbox 360 (RGH/JTAG/Dev Kits), retail-runnable `.xex`

## Table of Contents

+ [Features Showcase](#features-showcase)
+ [What's New](#whats-new)
+ [Build](#build)
+ [Building the XZP Package (Customizing the XUI Skin)](#building-xzp-package)
+ [Controls](#controls)
+ [Technical Notes](#technical-notes)
  + [XUI Scene Files](#xui-scene-files)
+ [Troubleshooting](#troubleshooting)
+ [Credits](#credits)

## Features Showcase <a name="features-showcase"></a>

### Direct3D Rendering

Hardware-accelerated SNES rendering using Direct3D 9, optimized for Xbox 360's GPU with custom HLSL shaders.

### XAudio2 Audio

Low-latency audio output using XAudio2, taking full advantage of Xbox 360's audio hardware.

### Xbox 360 Controller Support

Native Xbox 360 controller input with full button and trigger support.

### Custom Xbox 360 UI

XUI-based user interface with custom skin resources and Xbox 360 dashboard integration.

## What's New <a name="whats-new"></a>

### V1.0 (Latest)

+ **Upgraded build toolchain**
  + Upgraded IDE to Microsoft Visual Studio 2010 SP1
  + Upgraded Xbox 360 XDK to v2.0.21256.3
+ **Increased optimization settings**
  + Optimization flag was set to `Full Optimization (/Ox)`
+ **Pre-configured, portable and ready-to-use build**
  + Provides a pre-configured `settings.xml` file
  + All required files and folders already included in the package
  + Works regardless of the installation path

### V0.36 Beta - New Features

+ **Game Genie Cheat Code Support:** Enter cheat codes before launching games from Favorites
  + **Keyboard Input:** Xbox 360 on-screen keyboard appears when launching from Favorites List
    + Enter up to 3 Game Genie codes at once (separated by comma or space)
    + Example: `C222-D4DD,8B99-17DD,DD8C-C7A7` (Super Mario World codes)
    + Press Select/B to skip entering codes and launch normally
  + **Automatic Validation:** Codes are validated before ROM loads
    + Invalid codes show detailed error messages
    + ROM won't launch if any code is invalid (prevents typos)
    + Uses standard Game Genie format (XXXX-XXXX)
  + **Real-time Application:** Codes applied immediately after ROM initialization
    + Uses SNES9x's built-in `S9xGameGenieToRaw()`, `S9xAddCheat()`, and `S9xApplyCheats()` functions
    + Codes are active from the moment gameplay starts
  + **XML Configuration:** Enable/disable via `settings.xml`
    + Add `<GameGenieEnabled>true</GameGenieEnabled>` to enable (default: true)
    + Set to `false` to launch ROMs immediately without keyboard
  + **Favorites Only:** Game Genie keyboard only appears when launching from Favorites List
    + Add games to favorites first (press LB in ROM list)
    + Regular ROM list launches games immediately (no keyboard)
  + **Multiple Code Support:** Up to 3 codes per session
    + Comma separated: `C222-D4DD,8B99-17DD,DD8C-C7A7`
    + Space separated: `C222-D4DD 8B99-17DD DD8C-C7A7`
    + Mixed separators also work

### V0.35 Beta - New Features

+ **ROM Search Functionality:** Quick search feature to filter ROM list by name
  + **Y Button Search:** Press **Y** button while browsing the ROM list to open the Xbox 360 on-screen keyboard
    + Enter search text to filter ROMs in real-time
    + Search is case-insensitive for easy finding
    + Empty search clears the filter and shows all ROMs
  + **Persistent Filter:** Search filter persists when switching devices or rescannning ROMs
    + Filter remains active when navigating between storage devices
    + Filter is maintained when ROM list is refreshed
  + **Async Keyboard Handling:** Uses asynchronous overlapped I/O to prevent UI freezing
    + Keyboard opens smoothly without blocking the interface
    + Per-frame completion checking ensures responsive operation
    + Matches the proven pattern used in the NES emulator for reliability

### V0.34 Beta - New Features

+ **FPS Display Counter:** New frame rate display toggle in InGameOptions menu
  + **Display FPS Toggle:** Added "Display FPS" checkbox in the in-game options menu
    + Accessible via InGameOptions menu (press both thumbsticks during gameplay)
    + Shows current frame rate in the top-right corner of the screen
    + Toggle on/off to show or hide the FPS counter
  + **Persistent Setting:** FPS display preference is saved and persists across game sessions
  + **Direct3D Support:** FPS counter now works correctly on Xbox 360 Direct3D rendering path
  + **Real-time Display:** Shows frame rate as "XX/YY" format (current FPS / target FPS)

### V0.33 Beta - New Features

+ **Favorites System:** Complete favorites management system for organizing your ROM collection
  + **Favorites List Page:** Dedicated favorites page accessible from the main menu
    + Shows all your favorite games in one convenient list
    + Quick access to your most-played games
    + Sorted alphabetically for easy browsing
  + **Add/Remove Favorites:** Easy favorite management
    + Press **LB (Left Bumper)** while browsing ROMs to toggle favorite status
    + Or use the "Add to Favorites" button in the ROM list
    + Favorites are saved automatically
  + **Favorite Tags in ROM List:** Visual indicators for favorites
    + Favorite games show a **[Favorite]** tag prefix in the main ROM list
    + Helps identify favorites at a glance
    + ROM list sorts alphabetically (favorites mixed in, not separated)
  + **Persistent Storage:** Favorites are saved to `settings.xml` and persist across sessions

+ **Enhanced XZP Build Script:** Improved `Build_XZP.bat` script for creating XZP packages
  + **Smart File Filtering:** Automatically excludes graphics files (PNG, JPG, etc.) when duplicating
    + Only duplicates XUR, XUI, XML, TTF, XMA, and other non-graphics files
    + Prevents unnecessary duplication of large image files
  + **Streamlined Process:** Easier XZP package creation for custom skins

### ⚠️ CRITICAL: Achievement System Completely Removed

+ **All Xbox Live Achievement Functionality Removed:** Complete removal of achievement system to prevent Xbox Live bans
  + **Why This Matters:** Achievement systems in homebrew applications can trigger Xbox Live account bans and console bans
  + **Safety First:** This build is safe to use without risk of Xbox Live enforcement actions
  + **Complete Removal:**
    + All achievement header files excluded from build (`Achievements.spa.h`, `Snes 360.xlast`)
    + Achievement compilation step (SPA compiler) completely disabled
    + All achievement function calls removed (`DoAchievo()`, `EnumerateAchievements()`, `XShowAchievementsUI()`)
    + Achievement-related variables, structures, and declarations removed
    + Achievement UI button functionality disabled
    + All game-specific achievement triggers removed (Super Metroid, Super Mario Kart, etc.)

> **Important:** If you're using a modded Xbox 360 console, using achievement systems in homebrew can result in permanent Xbox Live bans. This build eliminates that risk entirely.

### Build Fixes & Enhancements

+ **CRC32 Standalone Implementation:** Fixed linker errors by implementing standalone CRC32 for Xbox 360 builds
  + **Problem:** Xbox 360 build was trying to link against zlib's `crc32()` function which wasn't available
  + **Solution:** Added `EMUCRC32_STANDALONE` preprocessor definition and standalone CRC32 implementation
  + **Impact:** Builds successfully without requiring zlib's crc32 symbol
  + **Technical:** `zLib/emucrc32.c` now provides `crc32()` wrapper for unzip code when `EMUCRC32_STANDALONE` is defined

+ **C89 Compatibility:** Fixed C99 compatibility issues for Xbox 360 compiler
  + **Problem:** Xbox 360 compiler (based on older MSVC) doesn't support C99 features
  + **Solution:** Converted all variable declarations to C89 style (declared at top of function blocks)
  + **Impact:** Code compiles successfully with Xbox 360 toolchain
  + **Files Modified:** `zLib/emucrc32.c`, `jma/crc32.cpp`

+ **Project Configuration:** Optimized build settings for Xbox 360
  + **Excluded Files:** `zLib/crc32.c` excluded from Xbox 360 builds to prevent symbol conflicts
  + **Preprocessor Definitions:** Added `EMUCRC32_STANDALONE` to all Xbox 360 configurations
  + **Build Configurations:** `Release` and `Debug` configurations all properly configured

## Build <a name="build"></a>

### Prerequisites

1. **Microsoft Visual Studio 2010 SP1**
   + Must be Service Pack 1 or above
   + Provides the C/C++ compiler and build tools

2. **Xbox 360 XDK (Xbox Development Kit) v2.0.21256.3**
   + Version 2.0.21256.3 is required for compatibility
   + Provides Xbox 360-specific libraries, headers, and tools
   + Includes the Xbox 360 compiler toolchain (VCCLX360CompilerTool)
   + Required for building Xbox 360 executables

3. **Dependencies (Included in Repository)**
   + **zlib** - Compression library (included in `zLib/` directory)
   + **libPNG** - PNG image support library (included in `libPNG/` directory)
   + Both libraries are included in the repository and do not need to be downloaded separately

### Build Steps

1. **Open the project**
   + Launch Visual Studio 2010
   + Open `Snes360e.sln`

2. **Select configuration**
   + Choose `Release` from the configuration dropdown
   + Alternative configuration: `Debug` → For debugging (slower, includes debug symbols)

3. **Build the project**
   + Select **Build → Rebuild Solution** (or press `Ctrl+Shift+B`)
   + The build process will:
     + Compile all C/C++ source files
     + Link against Xbox 360 libraries
     + Generate `Snes360.xex` (or `Snes360-debug.xex` if it's the debug build)

4. **Output Location**
   + Release build: `bin\Snes360\Xbox 360\Release\Snes360.xex`
   + Debug build: `bin\Snes360\Xbox 360\Debug\Snes360-debug.xex`

### Build Configuration Notes

The Xbox 360 build uses the following key preprocessor definitions:

+ `_XBOX` - Identifies Xbox 360 platform
+ `EMUCRC32_STANDALONE` - Uses standalone CRC32 implementation (no zlib dependency)
+ `USE_DIRECTX3D` - Direct3D rendering
+ `JMA_SUPPORT` - JMA archive support
+ `HAVE_LIBPNG` - PNG screenshot support

### Build Notes

+ Post-build may warn about Xbox 360 connection - this is expected if Neighborhood isn't configured. The `.xex` still builds successfully.

+ Typical era/toolchain warnings are harmless (e.g., `/GR-` RTTI notes, `FASTCALL` macro noise).

### Running on Modded Consoles

The compiled `.xex` executable can run on any Xbox 360 console that supports unsigned code execution. This includes:

+ **RGH consoles** (RGH1, RGH2, RGH3) - Hardware modification allowing unsigned code
+ **JTAG consoles** - Early hardware modification method
+ **Dev Kits** - Official Microsoft development hardware
+ **Other modded consoles** - Any Xbox 360 with the ability to run `.xex` files

**Note:** Modifying Xbox 360 hardware may void warranties and violate terms of service. Use at your own risk. This software is intended for educational and homebrew development purposes.

## Building the XZP Package (Customizing the XUI Skin) <a name="building-xzp-package"></a>

The XZP (XUI Package) file contains all the skin resources (XUR files, images, fonts, etc.) used by the Xbox 360 UI. To customize the skin or add new XUR files, you'll need to rebuild the `Snes360.xzp` package.

### Prerequisites

+ **Xbox 360 XDK v2.0.21256.3 (Full Installation)** - Xbox Development Kit
+ The `Build_XZP.bat` script (included in `tools` folder)

### Steps to Customize and Rebuild the XZP

1. **Add your custom asset files**
   + Place your new or modified asset files in `snes9x-1.51-src-d3d\xbox\Skin\`

2. **Run the build script**
   + Open the `tools` folder
   + Double-click on `Build_XZP.bat`
   + The script will:
     + Create a new `Snes360.xzp` file in the `tools` folder
     + Preserve the relative path structure (`..\Xbox\Skin\`) inside the XZP
     + Maintain duplicate files with different table offsets (matching original structure)

3. **Deploy to your Snes360 build**
   + Copy the newly created `Snes360.xzp`
   + Replace the existing `Snes360.xzp` in the `media` folder on your Snes360 build

## Controls <a name="controls"></a>

### Front-End (ROM Browser)

+ **D-pad / Left Stick:** Navigate ROM list
+ **A:** Load selected game & start emulation
+ **B:** Back / Return to previous screen
+ **Y:** Open search keyboard to filter ROM list by name
  + Press Y to open Xbox 360 on-screen keyboard
  + Enter text to filter ROMs in real-time (case-insensitive)
  + Empty search clears filter and shows all ROMs
+ **Next Device Button:** Switch between storage devices (USB, HDD, etc.)
+ **Favorites Button:** Access the favorites list page (main menu)
+ **LB (Left Bumper):** Toggle favorite status for selected ROM (while browsing ROM list)
+ **Add to Favorites Button:** Add/remove selected ROM from favorites
+ **Game Genie Codes (Favorites Only):** Enter cheat codes when launching from Favorites
  + Press **A** on a favorite game to open Game Genie keyboard
  + Enter up to 3 codes (comma or space separated): `C222-D4DD,8B99-17DD`
  + Press **Select/B** to skip codes and launch normally
  + Codes are validated before ROM loads - invalid codes show errors

### In-Game

+ **D-pad / Left Stick:** SNES D-pad
+ **A, B, X, Y:** SNES buttons
+ **LB, RB:** SNES shoulder buttons
+ **START, BACK:** SNES Start/Select
+ **Both Thumbsticks (Press):** Open InGameOptions menu
  + Access save/load state, filters, aspect ratio, point filtering, mute audio, **FPS display**, and more
+ **LEFT_THUMB + LT:** Screenshot (if implemented)
+ **START + BACK:** OSD/Menu (if implemented)

> Note: Control mappings may vary based on the specific port implementation.

## Technical Notes <a name="technical-notes"></a>

### Dependencies

**zlib and libPNG are included in the repository:**
+ **zlib** - Located in `zLib/` directory, used for compression/decompression
+ **libPNG** - Located in `libPNG/` directory, used for PNG screenshot support
+ Both libraries are compiled as part of the build process
+ No additional downloads or installation required

### CRC32 Implementation

The Xbox 360 build uses a standalone CRC32 implementation to avoid linking against zlib's `crc32()` function. This is handled by:

+ `zLib/emucrc32.c` - Provides `crc32()` wrapper function for unzip code
+ `EMUCRC32_STANDALONE` preprocessor definition enables standalone mode
+ `zLib/crc32.c` is excluded from Xbox 360 builds to prevent symbol conflicts

**Technical Details:**

+ When `EMUCRC32_STANDALONE` is defined, `emucrc32.c` checks for it first (before zlib)
+ Provides both `emu_crc32()` and `calc_crc32()` functions
+ Also provides `crc32()` wrapper matching zlib's signature for unzip code compatibility
+ Includes zlib.h for type definitions (`uLong`, `Bytef`, `uInt`) but uses standalone implementation

### XUI Scene Files <a name="xui-scene-files"></a>

The Xbox 360 UI is built using XUI (Xbox User Interface) scene files. The main XUI files are located in `xbox/Skin/` and define the user interface screens:

+ `RomList.xui` - Main ROM browser/list interface
  + Displays the list of available ROM files
  + Allows browsing and selecting games to play
  + Includes device switching, favorites management, ROM preview, and search functionality
  + Press Y button to open search keyboard and filter ROMs by name
  + Entry point for launching games

+ `FavoritesListScene.xui` - Favorites list interface
  + Dedicated page showing all favorite games
  + Accessible from the main menu via the Favorites button
  + Provides quick access to frequently played games
  + Allows launching games directly from favorites list
  + **Game Genie support:** Press A on a favorite to enter cheat codes before launch
  + Supports up to 3 Game Genie codes per session (comma or space separated)

+ `InGameOptions.xui` - In-game pause menu/options interface
  + Accessible during gameplay by pressing both thumbsticks
  + Provides access to:
    + Save/Load state functionality
    + Video filters (Simple 2x, Scanlines, TV Mode, Super Eagle, etc.)
    + Display options (Aspect Ratio, Point Filtering, FPS Display)
    + Audio options (Mute Audio)
    + Screen adjustment controls
    + Preview capture
    + Exit game option

These XUI files are packaged into the `Snes360.xzp` archive along with associated resources (images, fonts, sounds) and loaded at runtime. Customizing these files allows you to modify the appearance and behavior of the Xbox 360 UI.

## Troubleshooting <a name="troubleshooting"></a>

### Runtime Issues

**Application won't launch on modded console:**
+ Verify the console can run other `.xex` files (test with a known working homebrew)
+ Check that the executable is not corrupted
+ Ensure all required dependencies are present
+ Verify the console's mod is functioning correctly

**Graphics issues:**
+ Verify Direct3D shaders are present in `xbox/Shaders/` directory
+ Check Xbox 360 video settings
+ Ensure Direct3D initialization is successful

**Audio issues:**
+ Verify XAudio2 is properly initialized
+ Check Xbox 360 audio settings
+ Ensure audio buffers are properly configured

## Credits <a name="credits"></a>

### Original Xbox 360 Port

+ **Anonymous** - Original Xbox 360 port developer (chose to remain anonymous)
  + Initial port to Xbox 360 platform (Snes360 V0.32 Beta, 2010/07/16)
  + Direct3D rendering implementation
  + XAudio2 audio integration
  + Xbox 360 controller support
  + XUI integration
+ **ModernVintageGamer (Dimitris)** - Original Xbox 360 port contributor and source code provider
  + Worked extensively on the original Xbox 360 port alongside other developers
  + **Provided the source code** that made this enhanced version possible
  + We are **very thankful** for his contributions and for sharing the source code
  + The original port was a collaborative effort with multiple contributors

### Original Port Contributors & Credits

The original Xbox 360 port credits acknowledge the following contributors:

r0wdy, Arak0n, kl0wn, idc, direw0lf, PeteNub, MomDad, Odb718, Angerwound, Redline99, TJ_CRS, Xenon7, Xantium, _skitzo_

> Note: Many original Xbox 360 homebrew developers from that era chose to remain anonymous for various reasons. The original port was credited to "Anonymous" in the source code.

### Enhanced Version

+ **chrisGrando** - Fork maintainer
  + Reorganized and cleaned up the repository structure
  + Removed several unused files (most of them were IDE/compiler generated files)
  + Upgraded IDE to Microsoft Visual Studio 2010 SP1
  + Upgraded Xbox 360 XDK to v2.0.21256.3
  + Fixed errors caused by the toolchain upgrade
  + Applied maximum optimization settings
+ **frankischilling** - Current maintainer
  + Achievements removed for safety (prevents Xbox Live bans)
  + Build fixes and compatibility improvements
  + Documentation improvements
  + Default settings.xml creation
  + UI improvements
  + **V0.36 Beta Features:**
    + Game Genie cheat code support in Favorites List
    + Multi-code support (up to 3 codes per session)
    + Code validation before ROM launch
    + XML configuration toggle (`<GameGenieEnabled>`)
    + Async keyboard handling for Game Genie input
    + Integration with SNES9x's built-in cheat system
  + **V0.35 Beta Features:**
    + ROM search functionality with Y button
    + Xbox 360 on-screen keyboard integration
    + Real-time ROM list filtering
    + Persistent search filter across device switches
    + Async keyboard handling for smooth operation
  + **V0.34 Beta Features:**
    + FPS display counter toggle in InGameOptions menu
    + Direct3D rendering path improvements for FPS display
    + InGameOptions menu navigation enhancements
  + **V0.33 Beta Features:**
    + Favorites system implementation
    + Favorites list page
    + Favorite tags in ROM list
    + Enhanced XZP build script with smart file filtering

### Core Emulator

+ **Snes9x Team** - Original Snes9x emulator
  + Core SNES emulation
  + Game compatibility
  + Feature development
  + See `copyright.h` and source file headers for complete contributor list

## License

This project is based on Snes9x, which is licensed under various open-source licenses. See the `doc/` directory for license information, including:

+ `doc/snes9x-license.txt` - Main Snes9x license information
+ `doc/GPL-2` - GNU General Public License v2
+ `doc/LGPL-2.1` - GNU Lesser General Public License v2.1

## Disclaimer

This software is for educational and homebrew development purposes. Use of this software on modified Xbox 360 consoles may violate Microsoft's terms of service. Use at your own risk.
