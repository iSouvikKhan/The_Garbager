# The_Garbager

The Garbager is a small 2D top-down game written in Java using AWT/Swing. The player walks around a tile-based world and destroys garbage bins and polluted objects (dirty rivers, a dirty pond, cut trees). Once enough objects have been cleared, the game switches to an end screen.

## Features

- Start menu with an image button that launches the game
- Tile-based world loaded from a text file (`src/res/textures/Worlds/world1.txt`) with grass, buildings, trees, water, bridge and factory tiles
- Animated player sprite with a camera that follows the player
- Collision with solid tiles and with other entities
- Directional attack using the arrow keys; each object has 10 health and is removed when it reaches 0
- Destroyed dustbins drop a dustbin item that the player picks up into an inventory
- End screen shown after 56 objects have been destroyed
- Fixed 60 ticks-per-second game loop with triple-buffered rendering

## Controls

| Key | Action |
| --- | --- |
| W / A / S / D | Move up / left / down / right |
| Arrow keys | Attack in that direction |
| E | Toggle the inventory (no on-screen display yet) |
| Mouse click | Press the start button on the menu |

## Tech Stack

- Java (AWT / Swing, `javax.imageio`)
- No external libraries and no build tool (plain `javac`)

## Project Structure

```
src/
  dev/Driden/project/
    Launcher.java        Entry point (main), opens a 1000x500 window
    GameCode.java        Game loop, window setup, state creation
    Handler.java         Shared access to game, world, input and camera
    display/Display.java JFrame + Canvas
    gfx/                 Asset loading, sprites, animation, camera, file utils
  Entity/                Entity, Creature and Player
  Statics/               Static world objects (dustbins, rivers, pond, cut trees) and EntityManager
  Items/                 Item, ItemManager and Inventory
  KeyInput/              Keyboard and mouse input
  States/                Menu, game and end states
  tiles/                 Tile types used by the world map
  UI/                    Simple UI system (image button, click listener)
  Worlds/World.java      Loads the map and places world objects
  res/textures/          Images and the world map file
```

## Prerequisites

- **JDK 8**. The code imports `sun.audio.AudioPlayer` in `Worlds/World.java`, which was removed in JDK 9, so it does not compile on newer JDKs without changing that (unused) import.

## Build and Run

Run these commands from the repository root. The map is loaded from the relative path `src/res/textures/Worlds/world1.txt`, so the working directory must be the repository root, and `src` must be on the classpath so the images under `/res/textures/` can be found.

Linux / macOS:

```bash
mkdir -p out
javac -d out $(find src -name "*.java")
java -cp "out:src" dev.Driden.project.Launcher
```

Windows (Git Bash works with the commands above). In PowerShell:

```powershell
New-Item -ItemType Directory -Force out
javac -d out (Get-ChildItem -Recurse src -Filter *.java | ForEach-Object FullName)
java -cp "out;src" dev.Driden.project.Launcher
```

`GameCode` also has its own `main` method that opens a 500x500 window instead.

## Notes

- The inventory stores picked-up items but does not draw anything on screen yet.
- Progress messages (for example the number of destroyed objects) are printed to the console.
