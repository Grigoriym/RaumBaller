# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

RaumBaller is an old-school scrolling shoot 'em up game for Android, written in Java. The codebase is built on a modified version of the JGame framework, with Android-specific adaptations.

## Development Commands

**Note:** The Gradle build may require Java compatibility fixes due to version mismatches.

- **Build the project:** `./gradlew build`
- **Build debug APK:** `./gradlew assembleDebug`
- **Build release APK:** `./gradlew assembleRelease`
- **Clean build:** `./gradlew clean`
- **Run tests:** `./gradlew test`
- **Run Android tests:** `./gradlew connectedAndroidTest`

## Architecture

### Core Game Framework
- **JGEngine**: Modified JGame framework located in `jgame/platform/JGEngine.java` - handles Android graphics, input, and game loop
- **AndroidGame**: Main game class extending JGEngine (`com.kaeruct.raumballer.AndroidGame.java`) - coordinates all game systems

### Game State System
Game uses a state-based architecture with states in `com.kaeruct.raumballer.gamestates/`:
- **ShooterTitle**: Main menu and ship selection
- **ShooterInGame**: Active gameplay state
- **ShooterGameOver**: Game over screen
- **ShooterLevelDone**: Level completion screen
- **ShooterEnterHighScore**: High score entry

### Entity System
- **Ships**: Player and enemy ships in `com.kaeruct.raumballer.ship/`
  - Player ships: `StenoShot`, `NimakRunner`, `SpinTurn` (3 selectable ships)
  - Enemy ships: Various types in `ship/enemy/`
- **Weapons**: Cannon system in `com.kaeruct.raumballer.cannon/` with bullet types
- **Waves**: Enemy spawn patterns in `com.kaeruct.raumballer.wave/`

### Level System
- **LevelReader**: Parses level files (`.lvl` format) from assets
- **Waves**: Define enemy spawn patterns and timing
- Levels are asset files: `level1.lvl`, `level2.lvl`, etc.

### Input Handling
- Touch input managed through JGEngine's mouse/touch abstraction
- Tap detection with timing logic in `AndroidGame.doFrameGeneral()`

### Graphics and Assets
- Custom sprite system through JGEngine's image handling
- Media defined in `shooter.tbl` file
- Background system with scrolling stars and pipe images

## Key Files Structure

```
app/src/main/java/
├── com/kaeruct/raumballer/
│   ├── AndroidGame.java           # Main game engine
│   ├── gamestates/               # Game state management
│   ├── ship/                     # Player and enemy ships
│   ├── cannon/                   # Weapon systems
│   ├── bullet/                   # Projectile types
│   ├── wave/                     # Enemy spawn patterns
│   ├── background/               # Background elements
│   └── LevelReader.java          # Level parsing
└── jgame/                        # Modified JGame framework
    └── platform/JGEngine.java    # Core engine
```

## Development Notes

- The game uses a fixed 60 FPS frame rate
- Coordinate system: 18x32 tile grid (WIDTH=18, HEIGHT=32)
- All game objects extend JGObject from the JGame framework
- Collision detection uses ID-based system (PLAYER_ID=1, ENEMY_ID=2, HEALTH_ID=3)
- Score system: enemies worth (maxHealth * 100) points
- Health pickups randomly drop from destroyed enemies

## Asset Pipeline

- Images and sounds defined in `shooter.tbl`
- Level files in assets directory (`.lvl` format)
- Font rendering through image-based system
- Uses fastlane for F-Droid deployment