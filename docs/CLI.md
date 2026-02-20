# Waxcore-GO / Ikemen GO Command-Line Reference

This document describes all command-line options supported by the engine. Options are **case sensitive**.

## Quick Reference

```bash
ikemen-go [options] [p1 p2]
```

Positional arguments `p1` and `p2` (when no flags precede them) set the first two players, e.g. `ikemen-go kfm kfm`.

---

## General Options

| Option | Description |
|--------|-------------|
| `-h`, `-?` | Show help and exit |
| `--version`, `-v` | Print version and exit |
| `--workspace <path>` | Set game data root directory (default: `./`) |
| `-config <path>` | Load config from `<path>` (default: `save/config.ini`) |
| `-stats <path>` | Stats/ranking file path (default: `save/stats.json`) |
| `-log <logfile>` | Record match data to `<logfile>` |

### `--workspace`

Sets the root directory for game data (save, logs, config, etc.). When specified, the engine changes the current working directory to this path before loading.

- **Default:** `./` (current directory)
- **Platform:** Desktop only (Android uses `baseDir` from the host app)

```bash
ikemen-go --workspace /path/to/motif -r motifdir
```

---

## Content Loading

| Option | Description |
|--------|-------------|
| `-r <path>` | Load motif from `<path>`. e.g. `-r motifdir` or `-r motifdir/system.def` |
| `-rubric <path>` | Alias for `-r` |
| `-lifebar <path>` | Load lifebar from `<path>`. e.g. `-lifebar data/fight.def` |
| `-storyboard <path>` | Load storyboard from `<path>`. e.g. `-storyboard chars/kfm/intro.def` |

---

## Video Options

| Option | Description |
|--------|-------------|
| `-windowed` | Start in windowed mode (disables fullscreen) |
| `-width <num>` | Set game width in pixels |
| `-height <num>` | Set game height in pixels |

---

## Audio Options

| Option | Description |
|--------|-------------|
| `-setvolume <num>` | Set master volume (0–100) |

---

## Quick VS Options

| Option | Description |
|--------|-------------|
| `-p<n> <playername>` | Load player `n` (e.g. `-p3 kfm`) |
| `-p<n>.ai <level>` | Set player `n` AI level (e.g. `-p1.ai 8`) |
| `-p<n>.color <col>` | Set player `n` color |
| `-p<n>.power <power>` | Set player `n` power |
| `-p<n>.life <life>` | Set player `n` life |
| `-tmode1 <tmode>` | Set P1 team mode |
| `-tmode2 <tmode>` | Set P2 team mode |
| `-time <num>` | Round time in seconds (`-1` to disable) |
| `-rounds <num>` | Play for `<num>` rounds, then quit |
| `-s <stagename>` | Load stage `<stagename>` |

---

## Debug Options

| Option | Description |
|--------|-------------|
| `-nojoy` | Disable joysticks |
| `-nomusic` | Disable music |
| `-nosound` | Disable all sound effects and music |
| `-togglelifebars` | Disable display of Life and Power bars |
| `-maxpowermode` | Enable auto-refill of Power bars |
| `-ailevel <level>` | Set game difficulty (1–8) |
| `-speed <speed>` | Set game speed (-9 to 9) |
| `-stresstest <frameskip>` | Stability test (AI matches at speed + frameskip) |
| `-speedtest` | Speed test (match speed ×100) |

---

## Extended Input & IPC (External Tools)

| Option | Description |
|--------|-------------|
| `--enable-extended-inputs` | Allow up to 12 human input slots (P1–P12); P3–P12 can be toggled at runtime |
| `--input-ipc` | Read inputs from Unix socket (requires `--enable-extended-inputs`) |
| `--input-socket-path <path>` | Socket path for IPC (default: `/tmp/ikemen-input.sock`) |
| `--battle-events` | Emit JSON battle events to stdout (prefix `[BATTLE_EVENT]`) for external tools |

---

## Examples

```bash
# Default run (current directory)
ikemen-go

# Load from specific workspace
ikemen-go --workspace /path/to/motif -r motifdir

# Quick match with AI
ikemen-go kfm kfm -p2.ai 2 -rounds 3 -s training

# Windowed mode with custom resolution
ikemen-go -windowed -width 1280 -height 720

# Extended inputs with IPC (for agent/automation)
ikemen-go --enable-extended-inputs --input-ipc --battle-events
```

---

## Lua Access

Command-line flags are available in Lua via:

- `getCommandLineFlags()` — returns a table of all flags
- `getCommandLineValue(flag)` — returns the value for a given flag
