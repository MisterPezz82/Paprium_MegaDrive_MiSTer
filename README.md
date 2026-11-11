# ⚠️ This project is retired: get Paprium for MiSTer from thekoalakoa

**Paprium for MiSTer now lives at [thekoalakoa/Paprium_MiSTer](https://github.com/thekoalakoa/Paprium_MiSTer).**
Download **[Paprium for MiSTer 0.3.0](https://github.com/thekoalakoa/Paprium_MiSTer/releases/tag/v0.3.0)** there and
follow their [install guide](https://github.com/thekoalakoa/Paprium_MiSTer/blob/main/docs/INSTALL.md).

- Their zip has the core, the MGL and **Little-Man Firmware 1.0**, the firmware for the cartridge's chip, which the
  core needs. This repository doesn't host the firmware. Get it from their release. Its source is in the
  [Pocket repo's `firmware/` folder](https://github.com/thekoalakoa/paprium-pocket/tree/master/firmware)
  (GPL-3.0-or-later).
- 0.3.0 makes the core behave like the real cartridge: the music comes from a rebuild of the cart's own synthesiser,
  so there are no WAVs, `.cue` or `paprium.pcm` any more.
- **If you used this core,** your save is `saves/MegaDrive/Paprium.sav`. Back it up first. Their core keeps its save
  at `saves/Paprium/Paprium.sav` and converts a 0.2.x save once on its first start. See their install guide.

This repository is **archived and read-only**. V.08 (below) stays here as the last build of this core, for history
only, and won't be updated. Please report problems on
[thekoalakoa's repository](https://github.com/thekoalakoa/Paprium_MiSTer/issues).

**Thank you, [thekoalakoa](https://github.com/thekoalakoa).** What started here as an EverDrive-style port grew, with
your Pocket and MiSTer cores, into the cartridge itself: its own synthesiser, the CPU MATE partner and the
Little-Man Firmware. It's your work, and it belongs in your repository. 🙏
Also see the [Analogue Pocket core](https://github.com/thekoalakoa/paprium-pocket).

---

Paprium for MiSTer Mega Drive

A MiSTer Mega Drive core fork bringing EverDrive-Pro-style Paprium support to MiSTer FPGA: working MCU/cart
behaviour, streamed graphics, the cartridge's own sound effects, and music from either MD+ CD audio or a PCM blob.

This project builds on years of community reverse-engineering, preservation, emulator work, and flash-cart development.

## V.08 — with huge thanks to thekoalakoa

V.08 is a merge of **[thekoalakoa](https://github.com/thekoalakoa)**'s
[Paprium_MiSTer 0.2.6](https://github.com/thekoalakoa/Paprium_MiSTer), itself a port of their
**Analogue Pocket core**. Almost everything new in V.08 is their work: **CPU MATE** (the game's
computer-controlled partner, which normally needs a Mega CD, re-implemented on the cartridge MCU),
the SCUM! options page, and a long list of fixes, on top of the V.07 firmware fixes that finally
closed the Intercom elevator bug. Every one of them argued from hardware captures and measured
against the game's own data.

**Go and look at the Pocket core — it is superb:**
### 👉 https://github.com/thekoalakoa/paprium-pocket

The main difference between this core and theirs is **music**: theirs plays a `paprium.pcm` blob;
this one can play **MD+ CD audio** (a `.cue` + the WAVs) **or** the same PCM blob, chosen in the OSD.

## Install

Copy the files so your SD card looks like this:

```text
_Console/
├── MegaDrive_Paprium_YYYYMMDD.rbf      the core (from the release)
├── Paprium.mgl                         shortcut: MD+ CD audio (from the release)
└── Paprium (PCM).mgl                   shortcut: PCM blob (from the release)

games/MegaDrive/Paprium/
├── Paprium.md                          your own ROM dump
├── paprium.cue                         for MD+ CD audio (from the release)
├── 01 Theme of Paprium.wav             your own soundtrack, for MD+ CD audio
├── ...
├── 52 Waterfront Beat.wav
└── paprium.pcm                         optional: your own PCM blob, for PCM music
```

Then pick **Paprium** or **Paprium (PCM)** from the MiSTer main menu. The shortcuts load the core and
the game in one go. They expect exactly the folder above: `/media/fat/games/MegaDrive/Paprium/`.

Progress saves automatically whenever you open the OSD (the MiSTer menu). Fresh install: let the game
reach its title screen, open the OSD once so the save file is written, then use **Reset**.

ROM and music are not included. Bring your own Paprium dump and your own copy of the soundtrack.

## Music: MD+ CD audio or PCM

OSD → **Music Source** (read at Reset):

| Setting | Music comes from |
|---|---|
| **Auto** (default) | the PCM blob if one is mounted, otherwise MD+ CD audio |
| **MD+ CD audio** | `paprium.cue` + the 52 WAVs |
| **PCM (paprium.pcm)** | the blob, mounted with **Mount music** in the OSD or by `Paprium (PCM).mgl` |

With Auto, the two shortcuts just work: `Paprium.mgl` gives CD audio, `Paprium (PCM).mgl` mounts the
blob first and gives PCM.

**Making `paprium.pcm`** from your own soundtrack uses the Pocket core's scripts (needs ffmpeg and
Python 3; see its [INSTALL guide](https://github.com/thekoalakoa/paprium-pocket/blob/master/docs/INSTALL.md)):

```bash
git clone https://github.com/thekoalakoa/paprium-pocket
cd paprium-pocket
./scripts/build_cdda.sh  /path/to/your/soundtrack  docs/paprium.cue  cdda/
python scripts/build_cdda_adpcm_fast.py  cdda/  paprium.pcm
```

The result is about 540 MB. Copy it to `games/MegaDrive/Paprium/paprium.pcm`.

## CPU MATE (the computer partner)

1. OSD → **SCUM! (Needs Reset)** → **CD Model**: any unit except None or Unknown, then **Reset**.
2. In the game's Options, switch **CPU MATE** on.
3. At character select:
   - **One controller:** turn on **6-Button Mode** (SCUM! → Grand Stick Options), **hold Mode** and press
     **Start** to bring the partner in, move its cursor with the D-pad, and press **Start** to pick its
     character. Release Mode to pick your own.
   - **Two controllers:** press **Start on controller 2**, then again to pick its character.

## The SCUM! page

| Entry | What it does |
|---|---|
| Grand Stick Options → 6-Button Mode | X, Y, Z and Mode reach the game |
| Grand Stick Options → Swap Joysticks | swaps controllers 1 and 2 |
| Grand Stick Options → Arcade Mode | the hidden ARCADE PAPRIUM mode; **Mode inserts a coin** |
| Grand Stick Options → Arcade Stage | the stage Arcade Mode starts on |
| Console | Mega Drive, Genesis, Mega Drive 2, Genesis Model 2/3, Nomad, Mega Jet: region and Boom Box picture |
| CD Model | the Mega CD the game sees (needed for CPU MATE) |
| 32X Mode | the game believes a 32X is attached |
| MegaWire4.0 | shows a MegaWire as connected (off = what a real cart reports) |
| Little-Man → Infinite Life / Pills / Lives | cheats |

Change an entry, then **Reset**.

## What this core does not do any more

To make room for CPU MATE, this build drops **SVP (Virtua Racing)** and save support for **other** Mega
Drive games, as thekoalakoa's build does. It is a Paprium core; use the stock MiSTer Mega Drive core for
everything else. Paprium's own saves are unaffected.

## Building it yourself

The MCU firmware contains the CPU MATE brain, which is made from the game's own code, so neither is in
this repository. Download `cpu-mate-brain_0.2.6.zip` from the
[Paprium_MiSTer 0.2.6 release](https://github.com/thekoalakoa/Paprium_MiSTer/releases/tag/0.2.6), unzip
it here, and build the firmware with `scripts/build_mcu.sh --brain cpu-mate-brain` (details in
[docs/firmware/README.md](docs/firmware/README.md)), then compile `MegaDrive.qpf` in Quartus.

## Credits

- **thekoalakoa** — [paprium-pocket](https://github.com/thekoalakoa/paprium-pocket) and
  [Paprium_MiSTer](https://github.com/thekoalakoa/Paprium_MiSTer): CPU MATE, the SCUM! options, the
  console and Mega CD options, the cartridge-memory fix, the PCM music path, and the firmware fixes
- **Krikzz** — [mega-ppm](https://github.com/krikzz/mega-ppm), the replacement cartridge MCU firmware,
  plus the first-level door fix and the arcade start-stage word
- **adroxe** — [Paprium-Arcade](https://github.com/adroxe/Paprium-Arcade): the Arcade Mode unlock and the Mode-button coin
- **nukeykt** — [Nuked-MD-FPGA](https://github.com/nukeykt/Nuked-MD-FPGA), the Mega Drive core
- **Jorge Cwik** — [FX68K](https://github.com/ijor/fx68k), the 68000
- **Stephan Nolting** — [NEORV32](https://github.com/stnolting/neorv32), the cartridge MCU's RISC-V CPU
- Project Little Man · TheHpman / MAME Paprium research · MAVProxyUser / Genesis Plus GX Paprium PR
- The [MiSTer](https://github.com/MiSTer-devel) framework and Mega Drive core developers
- Everyone who helped test, document, and preserve Paprium

## A note on AI use ("vibe coding")

Let's get this out of the way: it's a **Paprium** project *and* it's **AI
vibe-coded** — two of the internet's favourite things to argue about, bundled
into one repo. I'm fully expecting some hate for it, and honestly, that's fair.

I'm not an FPGA engineer. Most of the Verilog in here was generated and debugged
by AI (Claude and Codex) through a lot of back-and-forth — I'm not going to
pretend I hand-wrote it, because I didn't.

What I actually did was the boring half: running every build on a real MiSTer,
photographing the garbled graphics, listening to the broken audio, feeding it
all back, correcting the AI when it was confidently wrong, and deciding what was
good enough to call done. The Paprium / EverDrive Pro / firmware / MD+ knowledge
came from me and the wider preservation community — the AI couldn't see the
screen or hear the speakers, so that bit was on me.

It's a hack, not a "proper" core, and I've no doubt someone could do it far
better. If it annoys anyone enough to go and build the real thing from scratch,
without AI — genuinely, brilliant, please do, everyone wins. Until then, I made
this so a few people could mess about with Paprium on MiSTer. If that's you,
enjoy.
