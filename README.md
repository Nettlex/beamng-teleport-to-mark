# BeamNG.drive — Teleport to map marker

Small UI mods that teleport the **player vehicle** to the **active navigation / big-map route target** (the destination the game uses for ground markers and route planning).

## Contents

| Mod | Description |
|-----|-------------|
| **TPToMark** | Minimal app: one button — **TP to Mark**. |
| **Teleport** | Full teleport app (coordinates, relative move, other vehicles) plus **Teleport to Map Marker**. |
| **teleportation_panel_jtf** | ImGui panel to save named points to JSON (separate workflow). |

Source layout in this repo:

- `test/` — development / extracted sources (e.g. `test/TPToMark`, `test/Teleport`).
- `TPToMark/` — standalone mod folder (mirror of `test/TPToMark` when packaged).
- Prebuilt zips may also sit next to this README (e.g. `Teleport.zip`).

## Requirements

- **BeamNG.drive** (recent version with freeroam / big map navigation).
- A **route or map target** must be set first (see below). The mod reads the same target as the in-game navigation (`core_groundMarkers`).

## Installation

1. Copy the mod folder **or** the `.zip` into your BeamNG **mods** folder, for example:
   - `%USERPROFILE%\Documents\BeamNG.drive\mods\`
   - or your game’s `.../BeamNG.drive/current/mods/` if you use that layout.
2. Ensure the zip root contains `ui/` and `mod_info/` directly (no extra nested folder inside the zip).
3. Enable the mod in the in-game mod manager if needed.
4. Restart the level or press **Ctrl+L** (reload Lua/UI) after updating files.

## Usage

1. Enter **freeroam** (or a mode with the big map / navigation).
2. Open the **map**, place a **destination** / **set route** so the game shows a navigation target (ground markers / route).
3. Open **UI apps** and add:
   - **TP To Mark** (standalone), or
   - **Teleport** and use **Teleport to Map Marker**.
4. Press the button. The vehicle should move to that target.

If nothing happens, confirm a navigation target is actually active (the game would normally show route markers).

## How it works (technical)

- The destination is taken from the same system as the blue route: `core_groundMarkers.getTargetPos()` and fallback `core_groundMarkers.endWP[1]`.
- Beams’ engine types may return positions as `userdata`, `cdata` (FFI), `table`, or map node `string` IDs. The Lua snippets normalize those to a `vec3` before `setPosition`.
- In **Career** or other restricted modes, teleport behaviour may differ from freeroam; the game’s own “quick travel” rules still apply for some POIs.

## Building / updating a zip

From PowerShell, from the mod folder (contents must be `ui`, `mod_info`, etc. at the top level of the archive):

```powershell
Compress-Archive -Path ".\TPToMark\*" -DestinationPath ".\TPToMark.zip" -Force
```

## License / credits

- **TPToMark** app in this repo: custom glue around BeamNG’s public Lua/UI APIs.
- **Teleport** / **teleportation_panel_jtf** may include third-party or workshop-derived assets; see each mod’s `mod_info` and original authors where applicable.
- [BeamNG.drive bCDDL v1.1](https://www.beamng.com/bCDDL-1.1.txt) applies to engine and stock script references.

## Contributing

Issues and PRs welcome: please mention game version, map, and whether you use freeroam or career when reporting “teleport failed” cases.
