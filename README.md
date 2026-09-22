# AnyGM extras

Files to install beside the AnyGM core. Each kind goes somewhere different: one into RetroArch's
system directory, one into its cheat database, one beside the game itself. This file says which is
which, and what each one does. Many thanks to retrodiv for the AnyGM core: this collection is an
independent contribution beside it, not part of the core itself.

## AnyGM.ini — RetroArch's system directory

`AnyGM.ini` supplies the source-preparation declarations the core applies before it routes a
payload. Copy it into the directory RetroArch reports as its System directory (Settings → Directory
→ System, normally the `system` folder of the RetroArch installation).

Its `[transforms]` block is what makes a Game Maker 5.3, 6, 7, 8 or 8.1 project or executable open
directly, in one step and with no separate conversion: `input.probe` proposes the candidate ranges
in the original source so that the reader selects exactly one valid representation, and `record`
decodes the encoded GM6, GM7, GM8 and 8.1 streams, legacy script records and `.gex` extension
packages. No file means no default programs, so classic content that needs one fails to load.

## Cheats — RetroArch's cheat database

Each `.cht` holds the cheats for one game, written for AnyGM's own cheat engine: its entries address
GML runtime variables by name, because the core exposes no flat RAM to a cheat search. Copy them
into the cheat database directory RetroArch is configured with, in a subfolder named after the core:

    <cheat_database_path>/AnyGM/<Game>.cht

Normally that directory is the `cheats` folder of the RetroArch installation, which makes the
destination `cheats/AnyGM/`.

## Anchors — beside the game

An `.anygm` anchor is a small text file that names the payload to load, relative to its own
directory, and adds directives for that one game. Put it beside the payload it names — `data.win`,
the game's executable, or the archive it is loaded from — and name it after the game.

Load the anchor as the content in RetroArch rather than the file it names: `.anygm` is one of the
extensions the core declares to the frontend, and loading the anchor is what applies the directives.

What the anchors in this folder add:

- **Render at game resolution.** The option asks one question the core cannot answer per payload:
  what raster did this game actually compose, before the game or its runner scaled it up? A
  `?gameres`-scoped `[overrides]` block answers it for that game — re-declaring a view port the
  runner multiplies, holding a presenter's own scale variable at 1, or resizing the application
  surface and its window together — so the option presents the game's canvas rather than an
  already-multiplied picture. A game whose raster is already right needs no block.
- **Aspect Ratio force (Experimental).** Where a game composes through a CRT overlay, a pause
  backdrop or a full-width fade of its own, a `?aspect` block is what makes the forced shape reach
  them; without it they stay at the size of the unforced frame.
- **The rest, per game.** Anchors also carry what one title needs and another does not: skipping
  publisher, logo or map rooms on a button press or as soon as they are entered (`introskip`,
  `introauto`), holding a variable at a value, declaring which operating system the game believes
  it is running on (`ostype`), or rebuilding presentation values a game computes only at startup.

Active directives join the content's save-state identity: a state saved while they were active loads
only while they are still active, and loading it under a different set of directives is rejected.
