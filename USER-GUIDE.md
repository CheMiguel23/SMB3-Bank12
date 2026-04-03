# Bank12 User Guide

Based on editor version 0.1.0

Last updated: 2026-04-03

## Table of contents

- [Requirements](#requirements)
- [Project Hub](#project-hub)
- [Project Folder Layout](#project-folder-layout)
- [Autosave](#autosave)
- [How Builds Work](#how-builds-work)
- [Memory Budgets](#memory-budgets)
- [Settings](#settings)
- [Top Toolbar](#top-toolbar)
- [Level Editor (Alt+1)](#level-editor-alt1)
  - [Level Tree](#level-tree)
  - [Canvas Shortcuts](#canvas-shortcuts)
  - [Canvas Toolbar](#canvas-toolbar)
  - [Palette](#palette)
  - [Header Panel](#header-panel)
  - [Junctions Panel](#junctions-panel)
  - [Inspector Panel](#inspector-panel)
  - [Entries Panel](#entries-panel)
- [Palette Editor](#palette-editor)
- [World Map Editor (Alt+2)](#world-map-editor-alt2)
  - [Tiles](#tiles)
  - [Nodes (Entries)](#nodes-entries)
  - [Normal](#normal)
  - [Toad House](#toad-house)
  - [Game](#game)
  - [No Level](#no-level)
  - [Skip](#skip)
  - [Properties](#properties)
  - [Objects Panel](#objects-panel)
  - [Entries Panel](#entries-panel-1)
  - [Tile Events](#tile-events)
- [CHR Editor (Alt+3)](#chr-editor-alt3)
  - [Metatiles](#metatiles)
  - [Objects](#objects)
  - [Tiles](#tiles-1)
- [Text Editor (Alt+4)](#text-editor-alt4)

## Requirements

A legally obtained `Super Mario Bros. 3` US `PRG1` ROM.

## Project Hub
`Project Hub` is the front door to the app. Use it to create a new project, reopen an existing one, or manage recent projects. Creating a project means choosing a project name, a base SMB3 ROM, and a destination folder. Bank12 then copies the ROM, decodes it into project data, and opens the new workspace.

## Project Folder Layout
A project is a normal folder, and that folder is the source of truth. `project.b12` stores project metadata, `base_rom/` keeps the imported ROM, `levels/`, `worldmaps/`, `tilesets/`, and `text/` hold editable data, and `build/` holds outputs like `output.nes` and `output.bps`.

## Autosave
Project data saves automatically while you work. Some writes are delayed slightly, but you should treat the project folder as live.

## How Builds Work
Builds always start from the imported base ROM, not the last export. Bank12 repacks the current project data, rewrites the needed pointers, and writes a fresh ROM or BPS. The built ROM is output, not the editable source.

## Memory Budgets
Budget numbers are based on encoded packed data, not simple object counts. Bank12 also deduplicates identical encoded layout or object data, so exact duplicates only take space once.

`Layout` and `Objects` are the current room, while `Bank X`, `Obj Bank`, and world map numbers are shared budgets. Referenced slot levels, linked alt rooms, and common levels count toward those shared totals; unassigned levels do not until something actually uses them. Use `ROM Bank Usage` for the project-wide view.

## Settings
`Settings` covers appearance, play, canvas visuals, level-editor preferences, and keybindings. Many visual changes preview live and only save when you confirm them.

## Top Toolbar
- `Project`: Opens the Project Hub so you can create, open, or manage projects.
- `New Level`: Opens a blank level tab in the current project.
- `Build ROM`: Rebuilds the project and writes `project/build/output.nes`.
- `Build BPS`: Rebuilds the project and writes `project/build/output.bps`.
- `Play`: Builds a preview ROM and launches it in your configured emulator.
- `Power-up`: Sets Mario's starting form for preview play only.
- `Inventory`: Opens the preview inventory editor used by `Play`.
- `Export...`: Opens PNG export plus level and (WIP) world map export/import tools.

## Level Editor (Alt+1)
### Level Tree
- `World 1` to `World 9`: Normal slot levels and their linked alt rooms.
- `Common Levels`: Shared rooms used in multiple places, so edits here affect every use.
- `Unassigned Levels`: Project-owned rooms that are not currently attached to a normal slot (don't take memory)

### Canvas Shortcuts
- `Ctrl + mouse wheel`: Zoom.
- `Mouse wheel`: Scroll.
- `Middle-drag`: Pan.
- `Alt + click`: Eyedropper the entry under the cursor.
- `Ctrl + click`: Multi-select, or place and immediately resize when a palette item is active.
- `Shift + drag`: Paint continuously with the selected palette item.
- `Ctrl + right-click`: Open the context menu.
- `Right-click`: Delete under the cursor if that setting is enabled.
- `Delete` or `Backspace`: Delete the current selection.
- `Ctrl + C / X / V / D`: Copy, cut, paste, or duplicate.
- `H` / `V`: Flip the selected entry horizontally or vertically.
- `[` / `]`: Move the selection down or up one layer.
- `Ctrl + [` / `Ctrl + ]`: Send the selection to back or front.
- `Arrow keys`: Nudge the selection by one tile.
- `Ctrl + Arrow keys`: Nudge the selection by four tiles.
- `Ctrl + T`: Open the Tileset Viewer.
- `Esc`: Clear the current placement or selection.

### Canvas Toolbar
- `Undo`: Reverts the last change.
- `Redo`: Reapplies the last undone change.
- `Clear`: Removes all layout and object data from the current level.
- `Tiles`: Opens the Tileset Viewer.
- `P-Sw`: Toggles P-Switch preview state.
- `Anim`: Toggles animated tiles.
- `Grid`: Shows or hides grid lines.
- `Boundary`: Shows or hides screen boundary lines.
- `Overlays`: Shows or hides editor overlays and indicators.
- `Viewport`: Shows or hides the NES viewport guide.
- `Remove screen`: Removes the last screen from the level.
- `Screen count`: Shows the current number of screens.
- `Add screen`: Adds one screen, up to the level limit.
- `Zoom`: Sets canvas zoom with the slider or percentage box.

### Palette
The palette is your placeable library. `Var`, `Fixed`, and `Objects` are ROM data, while `Recent`, `Fav`, and `Prefab` are editor conveniences for faster editing.

### Header Panel
- `Level tileset`: Sets the tileset this room uses for graphics and gameplay.
- `Sub-level TS`: Sets the tileset used when entering the linked child room.
- `Screens`: Sets the room length in screens.
- `Music`: Chooses the level music.
- `Time`: Chooses the timer value, including no-timer options.
- `Scrolling`: Sets the vertical camera behavior.
- `Vertical`: Switches the room to vertical layout mode.
- `BG tiles`: Chooses the background graphics bank.
- `BG colors`: Chooses the background palette row.
- `Sprite colors`: Chooses the sprite palette row.
- `Entry`: Chooses Mario's entrance animation.
- `Spawn height`: Sets Mario's starting vertical position.
- `Spawn side`: Sets Mario's starting left, center, or right position.
- `Pipes to map`: Makes pipe exits return to the world map instead of a linked room.
- `2-Player VS`: Enables the versus flag for the room.
- `White Toad House`: Enables the hidden White Toad House bonus condition for this level.
- `Required coins`: Sets the coin count needed for that White Toad House trigger.

The editor keeps linked parent and sub-level tilesets aligned where it can, and it warns you when `Sub-level TS` does not match.

### Junctions Panel
`SUB LEVEL` shows which room this level is linked to as a parent or child. Use `Open linked`, `Return to parent`, `Assign...`, and `Remove` to manage that relationship. This section chooses the linked room itself; the junctions below choose how Mario enters it.

Each junction card is one transition point into that linked room. `Junction index` picks the screen boundary that triggers it, and `Entry style`, destination screen, destination row, destination column, and `Vert. dest.` control how Mario arrives. If an alt pipe or door is moved and it uses the follow-trigger behavior, Bank12 can auto-sync the junction index for you.

All transitions on the same screen share the same junction index, which means they lead to the same linked room. That is usually what you want, but if you have several such pipes or doors on one screen, auto-sync may not always update exactly the way you expect. You can always override the junction index manually here.

### Inspector Panel
The Inspector changes with the current selection. It edits the selected `Var`, `Fixed`, or `Object` directly, and placed entries update the live level with position, size, variant, order, and warning fields where needed.

Some objects get dedicated helper views instead of only raw bytes. `Treasure Box`, `Treasure Set`, and `Treasure Box Event` expose reward, linked-box, and event behavior fields so you can manage chest setups without decoding the raw object logic by hand. `Pipeway Controller` opens the shared landing-table row for that controller, so one edit can affect every controller that uses the same row.

`Autoscroll` gets a dedicated section because its stored `Y` value is really autoscroll data, not a normal Y position. `Boom Boom` also gets a special view with `fx_slot` and `y_pos`, but the important part is `fx_slot`; it only points to effect slot `1` to `4`, and the actual lock, bridge, or river change is chosen on the world map side. Some controllers also show context warnings, such as White Toad House bonus setup issues.

### Entries Panel
`Entries` is a searchable list of placed `Var`, `Fixed`, and `Object` entries grouped by screen, and it lets you quickly select, center, hide, or lock them.

## Palette Editor
Use it when you want to change the actual palette data for a tileset, not just point a room or map at a different existing palette row. Pick a `Tileset`, edit colors in the `BG` and `SPR` rows, and use the row radio button to choose the active palette row for the current level or world map. Changes preview live while the dialog is open. `Apply` saves them, and `Cancel` restores the previous preview state.

## World Map Editor (Alt+2)
The world map editor is for overworld tiles, nodes, map objects, and shared map rules. The usual flow is paint tiles, edit nodes, then finish the world setup in `Properties`, `Objects`, and `Entries`.

### Tiles
Use the bottom palette to paint 16x16 world map tiles onto the canvas. The tile colors come from the current world map palette rows, so changing the map palette changes how those painted tiles look.

### Nodes (Entries)
Nodes are the stop points Mario can land on, activate, or enter. The `Inspector` changes with the selected node type.

#### Normal
A `normal` node is a standard playable level node. `Layout` chooses the bound level, `Object ptr` chooses the object data, and `Sync object ptr` keeps the object side matched to the layout, which is usually what you want.

#### Toad House
A `toad house` node binds a special Toad House level and defines its reward. That reward is world-scoped here, not stored on a normal map object.

#### Game
A `game` node defines a bonus game instead of a normal level. Its fields choose the game type, host, intro room, and prize.

#### No Level
A `no level` node is for decoration or helper entries that should not open a playable level. It still has a position, but it does not use normal level binding.

#### Skip
A `skip` node keeps an unused entry record in place. Use it when you want to preserve a slot without turning it into gameplay.

### Properties
- `World start`: Sets Mario's starting row on the map. The starting column is fixed at `2`.
- `Screens`: Sets map width in screens.
- `Scroll`: Sets how far the map camera can pan.
- `Tile colors`: Sets the palette row used for map tiles.
- `Object colors`: Sets the palette row used for map objects.
- `White Toad House reward`: Sets the world's White Toad House reward.
- `Show WM sprites`: Toggles map-object sprite preview in the editor.
- `Use bottom tile`: Enables a forced bottom filler tile for the map.
- `Bottom tile`: Sets that filler tile.
- `Manual InitIndex`: Lets you override the automatically calculated runtime sections.
- `InitIndex 0-3`: The section boundaries the game uses when scanning entries.

`InitIndex` is normally calculated for you from node order and position, so you usually do not need to edit it manually.
`White Toad House reward` is also managed here because that bonus is shared per world; the level-side trigger still comes from the room's White Toad House setup.
`BOOM BOOM EFFECTS` connects level-side `fx_slot` values to removable target tiles on the current map, and `Open level` helps jump to the source fortress when there is only one matching source.

### Objects Panel
`Objects` is for world-map objects and airship routes, not normal nodes. The map object list is nine fixed slots, and slot `1` is reserved for the Airship, so its position defines the Airship start point and its type stays locked.

Use the same panel to edit the three random Airship route sets (`A`, `B`, and `C`). 
Reward-bearing map objects also expose an `Item` field here, so you can set their item reward directly instead of decoding the raw object bytes by hand.

### Entries Panel
`Entries` is a searchable list of world-map nodes grouped by screen, and it makes dense maps much easier to select and focus.

### Tile Events
`Tile Events...` opens the shared world-map tile behavior tables for the whole project. The removable `From` and `To` pairs are editable, while the other tile-event tables are currently read-only. Use this when locks, bridges, rocks, rivers, or fortress effects need different replacement results.

## CHR Editor (Alt+3)
The CHR editor is for graphics work. It has three modes, and all of them share the bottom `Preview` strip. Keep that preview on a real level so you can quickly see what your change affected.

### Metatiles
Use `Metatiles` for 16x16 terrain pieces and other block-based graphics.
Controls: `Tileset`, `BG Bank`, and `From Level` choose the context, the big metatile view lets you paint pixels, the `UL/UR/LL/LR` boxes change quadrant tile references, and `Apply` commits those quadrant changes.

### Objects
Use `Objects` for sprite graphics through an assembled object preview.
Controls: pick an object in the left tree, click a piece in the center preview, then use `TS`, `Pal`, `Mirror`, and `Grayscale` in the pixel editor to edit that tile.

### Tiles
Use `Tiles` for direct raw 8x8 CHR editing.
Controls: choose a bank from `CHR Banks`, click a tile in the bank grid, then use `Palette`, `Sub-pal`, and the pixel editor to preview and edit the raw tile data.

## Text Editor (Alt+4)
The text editor is for in-game text such as letters, dialogs, HUD text, bonus text, and end-level text. Pick an entry from the left tree, edit it in the input panel, and use the center preview to see how it will render in-game. This view is meant to show the real text format, not just plain text without context.
