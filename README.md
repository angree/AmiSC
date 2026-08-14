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
| Graphics | AGA chipset, or a graphics card (RTG / Picasso96) |
| RAM | enough for ~20 MB of sprite graphics |
| Disk | 250–620 MB for the data files, depending on which release you take them from |

**Recommended, and this is not a formality — the original ran on a Pentium 90:**

* **AGA:** 68060 or faster.
* **RTG:** a fast 68040 as the floor.
* Best experience: 68060 or 080 **with** a graphics card.

On slower machines pick a 320×240 mode in the menu. It draws a quarter of the
pixels and scales up on the way to the screen, which on a plain 68020 is the
difference between watchable and not.

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

Add `--with-sound` from a command prompt if you want the audio data included —
618 MB against 61 MB without. **Nothing plays sound yet**, so there is little
point unless you are keeping the files for later.

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
speeds, victory and defeat. Data from either release of the original.

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
* **Units sometimes walk over terrain they should not be able to cross.**
* **No sound at all.** The Amiga audio side is not written yet.
* **No campaign** and no mission briefings.
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
