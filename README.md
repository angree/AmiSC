# AmiSC 68K

**An Amiga 68k port of the best space RTS strategy game.** The engine runs
natively on AmigaOS 3.x — no PC emulation, no interpreter. Integer arithmetic
only: the binary contains not one FPU instruction, because plenty of 68k
machines have no FPU.

It loads **your own copy** of the original game's data files and plays a
skirmish against computer opponents on a native AGA or RTG screen.

> **This is a total alpha.** It is playable — you can start a match, build a
> base and win it — and it has plenty of rough edges. The full list is further
> down and it is honest. Read it before you install.

---

## Download

Releases are on the [Releases](../../releases) page, as both `.lha` (for the
Amiga) and `.zip` (if you prefer to unpack on a PC and copy the drawer across).

**No game data is included and none ever will be.** You bring your own copy.

---

## What you need

| | |
|---|---|
| OS | AmigaOS 3.0 or later |
| CPU | 68020 minimum, **no FPU required** |
| Graphics | AGA chipset, or a graphics card (RTG / Picasso96); OCS/ECS with `SC_ECS` (320×240, 64 colours) |
| OS (alt.) | AROS 68k, MorphOS (68k code through its built-in JIT) |
| RAM | enough for ~20 MB of sprite graphics, plus sound samples as they are first heard |
| Audio | Paula, i.e. any Amiga. Four channels: two for music, the rest for effects |
| Disk | 250–620 MB for the data files, depending on which release you take them from |

**Recommended, from testers' reports on real machines - the original ran on a Pentium 90:**

| | 320×240 (lores), AGA or RTG | 640×480 (hires), AGA or RTG |
|---|---|---|
| bottom | a fast 68030 (50 MHz) with 64 MB Fast RAM | 68040 |
| fluid | 68040/25 and up | 68040/50 or 68060/50 |
| flies | | 68060/75 and up |

A fast 68020 (33 MHz) with enough RAM is watchable at 320×240, with some pain;
below 25 MHz it is not useful. Pick the 320×240 mode in the menu on anything
slower than a 68040: it draws a quarter of the pixels.

---

## Installing the game data

The archive contains the port only. It cannot start until the game's own data
files are in the `data` drawer next to the program:

```
StarDat.mpq
BrooDat.mpq
Patch_rt.mpq
```

Maps (`.scm` and `.scx`) go into the `maps` drawer. That is the whole
installation — everything the program reads or writes stays inside its own
drawer, so you can put it on any partition and it needs no assign.

**Both releases of the original work.** Which method you use depends on which
one you own.

### Method 1 — the original release (up to 1.16.1, CD or old download)

It already stores everything in the format the port reads, so there is no
conversion at all.

1. Find the installation on your PC — usually
   `C:\Program Files (x86)\StarCraft`.
2. Copy `StarDat.mpq` (~250 MB), `BrooDat.mpq` (~85 MB) and `Patch_rt.mpq`
   (~10 MB) into the `data` drawer.
3. Copy the maps you want out of `Maps\` into the `maps` drawer.
4. For music: copy `extract-music.bat`, `mpq-music.exe` and `snd2adpcm.exe` out
   of `pc-tools`, put them next to your `.mpq` files on the PC, and
   double-click the `.bat`. It leaves a `music` folder — copy that into the
   drawer next to the program. Sound effects work without this step.

All three archives should be present: the patch archive is mounted first and
overrides the other two.

### Method 2 — the modern free re-release (1.18 and later)

The publisher gives the game away for free now, and it works too — but it ships
**no `.mpq` files at all**. Everything lives in a different archive format
called CASC, which the port cannot read.

A converter is included in the archive, in `pc-tools`. It runs on your PC,
once, and **never writes into the game installation — it only reads it.**

1. Install the publisher's official desktop launcher, log in with a free
   account, and install the game from the games list. It is roughly 5–8 GB. You
   never have to run it.
2. Open `pc-tools` and double-click **`prepare-data.bat`**. It needs nothing
   installed — no Python, no Java, no compiler; everything it uses is in that
   drawer.
3. It looks for the game in the usual install folders by itself. If it cannot
   find one, a folder picker opens — point it at the folder holding
   `.build.info` and the `Data` folder.
4. A few minutes later you have the three `.mpq` files in the folder you
   started the `.bat` from. Copy them into `data`. Two of them are nearly
   empty, and that is correct: the modern data fits in one archive, and the
   port opens all three by name.

Sound effects and music come along automatically — there is no switch and
nothing to type. The effects are packed into the archive; the music is written
as a separate `music` folder, which you copy into the drawer next to the
program.

**If you converted your data with an earlier version, run the converter again.**
Before v0.4.0 it left the audio out, and there is no way to add it afterwards.

---

## Starting it

Double-click **`SC_universal`**. It works out the display itself — a graphics
card (RTG / Picasso96) when the machine has one, AGA otherwise — and the VIDEO
row of the start menu switches between all four modes while it runs: **RTG or
AGA**, **640×480 or 320×240**. If a mode refuses to open, the menu says so and
falls back to the one that was working.

The other four are the same game with the display nailed down, for comparing
modes without editing anything:

| program | display |
|---|---|
| `SC_RTG` | graphics card, 640×480 |
| `SC_RTG_lores` | graphics card, 320×240 |
| `SC_AGA` | AGA, 640×480 |
| `SC_AGA_lores` | AGA, 320×240 |

Do not start `SC_RTG` on a machine without a graphics card — the picture has
nowhere to go and the screen stays black. `SC_universal` never has that problem.

On a slower Amiga try 320×240 before deciding the game is too heavy — a quarter
of the pixels then go through the chunky-to-planar conversion every frame.

The icon already carries the 2 MB stack the game needs. From a Shell you have
to set it yourself, or it hangs silently while loading:

```
Stack 2000000
SC_universal
```

Settings are in `bwshow.cfg`, plain text, next to the program. When something
goes wrong, `bwshow.log` in the same drawer names every file it failed to find
— it is worth reading before asking.

---

## What works

Skirmish against computer opponents, all three races, fog of war, the command
card and unit abilities, minimap, production queues, the full range of game
speeds, victory and defeat, sound effects, unit voices and music. Data from
either release of the original.

**New in v0.8.6:** MorphOS compatible - the same 68k programs run on AmigaOS 3.x, AROS 68k and MorphOS. On MorphOS the game opens a 16/32-bit screen and converts its colours while drawing, because many MorphOS graphics cards have no 8-bit modes; use `SC_universal`. Tested in an emulated MorphOS 3.20; reports from real machines are welcome.

**New in v0.8.5:** `SC_ECS` for Amigas without AGA and without a graphics card - 320×240 in 64 colours (Extra Half-Brite), the colours fitted per map with each of up to three players keeping its own colour; `SC_universal` picks it by itself. The same programs run on AROS 68k, including the older ROM built into WinUAE that used to show a black screen. Transports have LOAD and UNLOAD, and UNLOAD asks for a destination. Fifth Terran mission: the whole base is visible once its commander dies and joins you as your units walk up; sixth Terran mission: the trapped troops are visible.

**New in v0.8.4:** speech in the campaigns - mission transmissions and the briefings speak, streamed while they play - plus the menu's own sounds, and menu and briefing videos decoded once into `data/menu-video-*.pack` so they cost almost nothing at 320x240 and at 640x480. **To hear the speech, data converted from the modern re-release must be converted again with this package's `prepare-data`; with the original CDs, copy `INSTALL.EXE` from the expansion CD as `Install_BW.exe` and from the original CD as `Install_SC.exe` into `data`.**

**New in v0.8.3:** a readable 320x240 menu - its text is drawn in a small font with a shadow and long briefings scroll - and a MENU SIZE option that keeps every menu and briefing at 640x480 while the game plays at 320x240. Briefing videos decode two to four times faster. Fixed: the base of the fifth Terran mission now really joins the player (bunker crews too, also in older saves), the teleporters of the fourth Terran mission move every marine at once, and loading a mission shows its name.

**New in v0.8.2:** the original menu at 320x240. With a 320x240 video mode the menu, the campaign screens, the briefings and the in-game Game Menu stay at 320x240; their pictures are reduced once, with smoothing, into `data/menu-lores.pack` on the first start. The menu and the briefings are faster, and a MENU option chooses the original or the Amiga menu. Fixed: the base of the fifth Terran mission goes over to the player after its commander dies, doors in the fourth Terran mission look and behave as they should, the command card no longer offers units the mission does not allow, ability buttons show icons, the speaker's name in mission messages is yellow, the loading screen keeps the menu's picture, and a player who is not in the first slot gets the right console.

**New in v0.8.1:** the original menu for players with the modern free re-release. `prepare-data` now also converts the menu, the campaign screens, the briefing portraits and the in-game dialogs, so those players get the same menu as players with the CD - run the new `prepare-data` again. Fixed: hero units have their abilities (the ghost heroine can cloak in the fifth Terran mission; heroes need no research, as in the original), a selection of more than six units shows all of them in two rows, and a cloaked unit no longer flickers into view with the logic skip on.

**New in v0.8.0:** the campaigns. Both campaigns of the original and of the expansion, with briefings, mission objectives, named saved games and Restart Mission (introduced in the v0.8.0a-b3 test builds). On top of those: the mouse pointer shows what is under it - a green ring over your own unit, yellow over a neutral or allied one, red over an enemy, and a sight in the same colours while an order waits for its target; the installation traps of the fourth Terran mission can be attacked and destroyed; and the main menu of the port's own interface shows the music and sound levels on its OPTIONS row.

**New in v0.8.0b3:** Original mode. Single Player offers ORIGINAL and EXPANSION, as on the PC. Original behaves as if the expansion did not exist - no expansion units, upgrades or technologies, the computer plays the original game's scripts, expansion maps are not offered, and the campaign shows episodes I-III. The choice is remembered, and saved games keep their mode.

**New in v0.8.0b2:** fixes from the first campaign reports. Sound on every start (before, only the first start after a boot had sound), Terran buildings lift off and land, CONTINUE after a mission goes on to the next one, music and sound effects volume, mission messages fit the 320x240 screen, real mission titles, no expansion units in the original campaign, rescued units keep their colour, and fixes in Terran missions 2, 3 and 4.

**New in v0.8.0b:** single player and the campaigns work with data converted from the modern free re-release. That data has no files of the original menu, so the port shows its own: SINGLE PLAYER starts a skirmish, CAMPAIGN opens the episodes, the missions and a text briefing (no portraits - the modern release keeps them in a format the Amiga cannot play). Run the new `prepare-data` from the package; it writes `BroodWar.mpq` with the campaigns. Also fixed: from the second match on, parts of the screen could stay black in the AGA modes.

**New in v0.8.0a:** the campaigns, as an alpha. Single Player opens the campaigns of the original and of the expansion - episode choice, mission list, the briefing with portraits and text (no speech), the missions with their scripted events, messages and objectives, victory to the next mission and defeat back to the briefing, progress kept per player ID. It needs the original data plus `BroodWar.mpq` from the original CD in `data`; data converted from the modern re-release does not carry the campaign screens yet. Every mission loads and runs, not every one has been played to its end. Also new: named saved games (F10 / Game Menu: Save Game, Load Game), Restart Mission, and returning to the menu after a match takes about 2 s instead of minutes.

**New in v0.7.11:** a faster start. In 320x240 only the frame table of each unit graphics file is read - 21 s instead of 133 s on a 68020 without JIT - and every other file is taken from the archive in one piece instead of 4 KB at a time.

**New in v0.7.10:** data preparation for the modern free re-release works again - versions 0.7.6 to 0.7.9 carried an old `casc-extract.exe` that stopped at the maps. The LOGIC setting has a fourth step, 1/8, and the steps are now named by the share of ticks a unit thinks on: 1/1 (exactly the original), 1/2, 1/4, 1/8. A machine without AGA but with a graphics card (ECS with a Picasso, a Voodoo, an Indivision ECS) no longer quits when the settings ask for an AGA mode - it uses the card. The FPS counter shows the average of the last five seconds instead of jumping between single seconds.

**New in v0.7.9:** much less memory, for 64 MB machines. In 320x240 the full-size graphics are not loaded at all once the converted ones exist (about 28 MB less and a shorter start), and the terrain is no longer held twice (8-10 MB less in every mode). 320x240 now needs about 12 MB less than 640x480 - it used to need 8 MB more. Switching to 640x480 in the menu loads the full graphics then, once, with no restart. The menu also shows the Workbench title bar when it is switched on, and credits the port's author.

**New in v0.7.7:** the original menu. The main menu, Single Player (ID selection, then Play Custom or Load Saved) and the Multiplayer connection screen are drawn from your own data files - the original layouts, backgrounds and animated buttons - and the Amiga settings moved to a new AMIGA OPTIONS screen. Campaigns, replays, the editor and network play are greyed out. It needs the original data (up to 1.16.1); with data converted from the modern re-release the program shows its own menu as before. The Play Custom panels are on the original CD in `BroodWar.mpq` - copy it into `data` too, or similar panels are used. The menu font is derived from Orbitron (SIL Open Font License, `FONT-LICENSE.txt`).

**New in v0.7.6:** a LOGIC setting (FAST / FASTEST) for crowded late games, save and load, computer versus computer, and a scrolling start menu.

**New in v0.7.3:** smoother 320x240 graphics - the filter now covers the whole inside of every unit and building instead of only the parts far from its outline, and the terrain is filtered properly once per terrain type and kept in a file. If you converted with v0.7.0-0.7.2, the game asks once whether to convert again for the better quality; "keep as is" is remembered in `settings.cfg` (`smooth_lores_gfx 0`).

**New in v0.7.2:** the native 320x240 mode now works on a graphics card too - `SC_RTG_lores` used to run at full size with nothing converted. The remembered game speed is the one last chosen in the menu; a change made during a match no longer overwrites it.

**New in v0.7.1:** the game starts at SLOW speed instead of FASTEST. At FASTEST the game logic alone takes about 80% of a typical Amiga's time and leaves little for drawing; at SLOW the same machine shows a decent frame rate. Raise the speed whenever you like and see what it costs - the speed you last used, in the menu or during a match, is remembered for the next start.

**New in v0.7.0:** a much better 320x240 mode. It used to draw the full 640x480 picture and throw every second pixel away - the full drawing cost, plus jagged edges and thin details that vanished. Now every graphic (terrain, units, buildings, shadows, the console, command icons, unit portraits) is shrunk to half size once, with proper smoothing, and the game draws straight at 320x240: the same part of the battlefield as at 640x480, only smaller, at roughly a third of the drawing cost. The first time a 320x240 mode is chosen the game asks before converting, shows a progress bar and keeps the result in `data/lores.pack`; "quick mode" keeps the old method. The 640x480 mode is faster as well (the minimap and the changed-screen check no longer redo their work every frame), a unit's first spoken line no longer freezes the game while it loads, and the menu remembers its settings in `settings.cfg`.

**New in v0.6.8:** data preparation works on the modern free release again. `prepare-data.bat` stopped after the music with "maps: 0 written" and never wrote the data files; that release keeps its maps as ordinary files in its `Maps` folder rather than inside its data archive, and they are now copied from there.

**New in v0.6.7:** the computer opponent stops pushing its cannons out in front of the base. That behaviour helped when it was added; a later change - it no longer picks building sites on ground it has never seen - does the same job better, and with both in place it defended instead of winning. Six runs say the same thing: it kept more of its own buildings and destroyed fewer of yours. Only one race ever placed anything forward, so nothing else about how it plays changes.

**New in v0.6.6:** the computer opponent defends the side it is attacked from. Its cannons and turrets used to go wherever there was room, which was as often behind the base as in front of it; now they face the threat. It also stops picking building sites on ground it has never seen - the single largest cause of its failed construction - and it will burrow its lurkers, deploy its siege tanks, and load an army onto a transport to cross water it used to walk around. It no longer repeats an order that does nothing.

**New in v0.6.5:** the computer gets its army home. A force sent back to
defend used to stop just outside its own base and stay there - alive, in the
middle of the map, ordered home again every ten seconds and never arriving.
Over twelve test matches: two thirds fewer wasted orders and one more
opponent finished off. The Ultralisk and the Broodling no longer show a
BURROW button they cannot use, the Drone finally has the one it should
always have had, and a geyser that runs dry says so.

**New in v0.6.4:** the computer opponent plays its own opening. One game
in eight the zerg computer goes for the fastest attack the game has - a
spawning pool at four workers and six of the small ones straight at your
base - and it now arrives in time to matter. Workers no longer pile onto
the gas: a base with four of them used to send all four to the geyser and
stop mining minerals, and at most one in three goes there now, which is
worth 9% more army and 11% more supply in use across twelve test matches.
The computer also saves up properly for what it is waiting for, and a
ready attack force sets off instead of standing in the base.

**New in v0.6.3:** hover a button and it says what the thing does, in the
game's own words, read out of your own data files. Abilities, spells and
orders get a panel at all now - they never had one, because it only
appeared for things that cost something - and a spell shows its full name
there instead of the abbreviation the card has room for. An upgrade says
which building its next level needs, and a building you cannot have yet
says what is missing.

**New in v0.6.2:** a building goes down where the pointer is - the outline
used to start at the cursor and reach down and to the right, which next to a
deposit was the difference between fitting and not, and a refinery now snaps
onto the geyser under the pointer. Ground that refuses a building says so.
A click on one wireframe in the panel keeps just that unit, a control
group's digit pressed twice jumps the view to it, and the right button on
the minimap sends the selection there.

**New in v0.6.0:** the zerg can go up their tech tree again - a Hatchery, a
Lair, a Spire and a Creep Colony had no upgrade button at all, so there was
no Lair, no Hive, no Greater Spire and no Sunken or Spore Colony. All five
are back, with the game's own hotkeys. A building you cannot have yet is
drawn dead and says what it needs. The advisor is heard to the end and comes
from both speakers, loading a sound no longer stutters the game, and far
more sound plays at all. Right-button scrolling returns the pointer where it
found it.

**New in v0.5.10:** the music converter recognises music it has already
converted (instead of reporting failures), says why any file really fails, and
reads the original CD installers where the CD version keeps its music.

**New in v0.5.9:** the screen title bar can be switched off - OPTIONS, SCREEN
TITLE BAR - so the game and the title picture use the whole screen.

**New in v0.5.8:** a zerg unit in an egg can always be cancelled - the egg now
has a CANCEL button, and with several eggs selected every press cancels one.

**New in v0.5.7:** the advisor speaks when an order is refused - not enough
minerals, not enough vespene gas, more supply needed - as on the PC.

**New in v0.5.6:** the command card shows a unit only once its tech building
stands (Academy, Armory, Covert Ops...) and names what is missing - Firebat,
Medic, Ghost, Goliath, Science Vessel, Battlecruiser and Valkyrie looked
unbuildable before.

**New in v0.5.5:** computer opponent (AI) updates - full-size attack waves,
better spending and expansion, fewer stalls in its build order.

**New in v0.5.2:** arrow-key scrolling works and its speed is settable - a
new OPTIONS page in the start menu (200 px/s by default, edge scrolling too).

**New in v0.5.1:** mineral fields show what is left in them, terran add-ons can be built (so Siege Tanks too), a right click no longer jumps the pointer.

**New in v0.5.0:** the computer opponent plays the whole game — it expands
(scouts the site, saves for the main building, sends new workers to the new
base, opens a second gas), defends with everything it has at home instead of
one unit at a time, and both zerg and terran got their build orders fixed (a
lair still counts as a hatchery, a sunken as a creep colony; factories and
starports are placed with room for their add-ons). Resources are reserved for
what the AI has decided to build, hopeless orders are dropped, and on island
maps it does not walk to expansions it cannot reach.

**New in v0.4.1:** right-button map scrolling works like OpenTTD's. The
pointer stays parked at the screen centre while you drag, so the drag is not
stopped by the screen edge — and under an emulator's magic mouse the host
cursor stays in the window too. The view follows the mouse and stops when you
let go; a right click still gives the order it always did.

**New in v0.4.0:** sound and music. Unit voices when you select them and when
you give an order, weapons, the command card, building placement, the refusal
buzz, and the game's own music streamed off the disk and picked to match the
race you play. Paula's four channels are shared: music takes two, effects take
the rest, and a click is always given a channel. This needs data converted by
this version's tools — see above — and the music always needs the PC step,
because in the archives it is Ogg Vorbis and decoding Vorbis wants an FPU this
port does not require. `extract-music.bat` covers players who copied the
original data files across by hand.

**New in v0.3.0:** terrain blocks movement again — a compiler defect made the
engine read the wrong half of every walkability flag, so units crossed cliffs
and water. The title picture is also raised so the publisher's logo is not
clipped.

**New in v0.2.1:** the minimap is reduced to fit on maps bigger than 128 tiles.

**New in v0.2.0:** costs shown on the command buttons when you hover over them,
a progress bar and a CANCEL button for research and upgrades, and a message in
red above the minimap when an order is refused — not enough minerals, not
enough supply, nowhere to put the building.

## What does not — the honest list

This is where the alpha shows.

* **The computer opponent is unfinished.** It builds and fortifies itself
  reasonably well, but attacks poorly, so it is much easier to beat than it
  should be. The first race in particular still beats the other two every time.
* **No volume control** anywhere — not in the menu, not in `bwshow.cfg`. The
  machine's own is the only one.
* **The music starts with the match, not with the menu**: the audio device is
  opened when a game begins.
* **The campaigns are an alpha**: no cinematics, not every mission played to its end. While music plays, speech comes from the right speaker only.
* **Saved games from earlier versions** cannot be loaded.
* **No multiplayer.** Computer opponents only, for now.

---

## Source code

Not published. This repository carries binaries only and no licence for the
port itself.

The converter shipped in `pc-tools` links **CascLib** and **StormLib** by
Ladislav Zezula, both under the MIT licence. Their notice travels with the
archive in `pc-tools/NOTICE.txt`.

---

## Legal

The game, its data and its trademarks belong to their publisher. This project
ships **no game data** and circumvents **no protection** — it reads the copy
you already own, on hardware the publisher never supported. It is a hobby port
made for fun on 30-year-old computers.
