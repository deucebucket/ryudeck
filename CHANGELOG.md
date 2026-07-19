# Changelog

## v0.1.24 (2026-07-15)

These are the changes added after v0.1.23.

**New: Nearby Decks**
- Added pull-only game backup copying between Ryudeck Decks on the same broadcast network. The
  receiving Deck chooses what to download; this is not streaming, mirroring, or two-way
  synchronization. Deleting a game on one Deck never deletes it from another. Routed VPNs and
  separate VLANs are not discovered automatically.
- Give each console a recognizable **Deck nickname** under **Settings > System > Network**. A blank
  nickname uses the local Steam display name, then the device hostname. This is the name the console
  broadcasts in Nearby Decks.
- Turn **Share library on LAN** on for both Decks during setup, then open **Nearby Decks** and pair in
  both directions by checking the displayed friend codes. Pairing is required: an unpaired Deck
  cannot browse or pull a library. After setup, only the sending Deck needs sharing left on.
- Browse the paired sender's library and choose **Download**. Each game can be hidden from sharing in
  its game settings. Sharing is off by default.
- Downloads run in the background, continue after leaving Nearby Decks, retain partial progress when
  interrupted, and resume after the Decks reconnect or Ryudeck restarts. Completion refreshes the
  receiving Deck's library.
- Paired identities, library manifests, and per-download content hashes are signed and checked. A
  completed file is hash-verified before it replaces the partial download.
- The sender can only serve files enumerated from configured game folders. Keys, firmware, saves,
  configuration, and files elsewhere on the Deck are outside the share path. v0.1.24 does not
  advertise separate update, DLC, or mod files.
- Added transfer progress and completion notifications. The sender warns before exit and blocks
  moving or removing an in-flight game so an active copy is not silently broken. An unpaired browse
  attempt now shows a pair-request notification, and an active transfer stays visible beside the
  launcher battery indicator.
- Verified multiple real game pulls between two household Steam Decks, including automatic library
  refresh on the receiving Deck. See [Using Nearby Decks](docs/nearby-decks.md) for the full flow.

**Game fixes**
- **Tomodachi Life: Living the Dream:** fixed the blocky grass regression. The shader and cache
  changes only activate for the title that needs them. The island was rechecked in foreground Game
  Mode, and the Pokken Tournament DX guard still reaches a responsive Free Training fight.
- **Princess Peach: Showtime!:** fixed the red-curtain loading blocker. The kernel wait path now
  handles an empty wait set, and the game gets the synchronization policy it needs on Deck.
- **FANTASY LIFE i: The Girl Who Steals Time** now gets past its former launch blocker and reaches
  controllable gameplay in true handheld mode with Frame Generation enabled. It still has severe
  frame-rate problems and texture popping, so it is not listed as playable.
- **Super Mario Galaxy** now reaches gameplay but currently runs in slow motion. **Super Mario
  Galaxy 2** still crashes. Neither is listed as playable.
- **Pokemon Legends: Z-A:** blocks inherited global Frame Generation until it is validated. An
  explicit per-game choice still wins.
- Fixed out-of-range Vulkan format lookups and off-thread buffer synchronization that could corrupt
  the graphics command stream.

**Launcher and reliability**
- Moved Scan Library, Achievements, Mii Creator, Nearby Decks, Friends, Settings, and Exit into one
  controller-navigable Menu.
- Game logs now include the game name and title ID in the filename.
- Added foreground Game Mode checks for launch, controller binding, config recovery, diagnostics,
  and launcher navigation.
- Release packaging rejects test-only code and development symbols.

**Compatibility**
- **Tomodachi Life: Living the Dream** is playable again after the grass fix.
- **Princess Peach: Showtime!** is playable past its former loading blocker.
- **Luigi's Mansion 2 HD** runs well. **Pokemon Brilliant Diamond** handles Turbo 2x and clock
  decoupling. **Yoshi's Crafted World** handles clock decoupling.
- **FANTASY LIFE i**, **Super Mario Galaxy**, and **Super Mario Galaxy 2** remain non-playable for
  the problems listed above.
- **Metroid Prime 4: Beyond** and **Pokemon Legends: Z-A** remain blocked before controllable
  gameplay.

## v0.1.23 (2026-07-07)

> _Report anything that misbehaves._

**New — Mii selector & creator**
- In-game **"Create a Mii"** (Miitopia, and any title that calls the Switch Mii applet) now opens a
  real, gamepad-navigable Mii selector: pick one of your existing system Miis, generate a random
  Mii, or cancel — instead of the game being silently handed a blank default face. A random Mii is
  saved into your system Mii database, so games that re-read it pick it up too.
- Guest applet dialogs — the Mii selector, profile selection, and controller-connect prompts — now
  **render and are fully controller-drivable in Game Mode**. Previously, because a guest applet
  pauses the game, the emulator's frame loop went idle and the overlay never composited over the
  frozen frame (it showed as a black screen); it now re-composites the overlay over the held frame,
  so the menu appears over your game and the D-pad / A / B drive it, handing control back cleanly.
- **Direct Mii Creator access** from the launcher top bar — the full Mii editor is one button away,
  as befits a Switch headline feature.

**New — Deck-native input**
- **Deck touchscreen → Switch touch**: tapping and dragging on the Deck's screen now drives the
  guest's touch input directly in Game Mode — native, no configuration — so touch-driven games and
  menus respond to the physical screen.
- **Deck gyro → six-axis motion** (opt-in via Deck Settings): the Deck's built-in gyroscope can
  drive the guest's motion controls — e.g. gyro aiming — for titles that use six-axis. Off by
  default while it's still being hardened; turn it on in Deck Settings.

**New — In-game keyboard, rebuilt**
- In-game text entry (player names, Mii names, search) now uses **Ryudeck's own on-screen keyboard**,
  composited over the game and fully controller-driven. This replaces the v0.1.22 approach of
  summoning the Steam Deck on-screen keyboard, which could reset Game Mode when its overlay stacked
  on top of a running game's surface.
- Full gamepad legend with an on-screen hint bar: **Shift / Caps** (Y or left-stick click), **Space**
  (X), **Backspace** (B), move the cursor with the shoulder buttons (L / R) or jump it to the
  start / end with the triggers (L2 / R2), **OK** (＋), and flip to the symbol / number page (−).
  Touch and tap-to-place-cursor work too.
- Fixed the keyboard blanking its keys when toggling Shift / Caps or the symbol page, and the typed
  text briefly vanishing as you type.

**Per-game boost profiles — more titles tuned out of the box**
- **The Legend of Zelda: Breath of the Wild** — uncapped present rate + frame generation + handheld
  render, verified in gameplay on device.
- **Pokémon Café Mix** — forced handheld: the title is handheld-only and warns when run docked, so
  Ryudeck now runs it undocked automatically and the Deck touchscreen drives it.

**Fixed**
- Corrupt or truncated game content (a bad dump, or a half-installed update) now shows a clear
  **"Can't launch — the content file is corrupt"** message instead of silently bouncing back to the
  library with no explanation.
- Frame Generation gains an explicit **per-game override (Default / On / Off)** — choosing Off now
  always wins over a title's built-in boost profile, so nothing forces frame generation on.
- The launcher no longer **re-scans and reflows the whole library** every time you open a menu;
  tiles stay put instead of popping in late and shoving others aside.
- **Pokémon Legends: Arceus** no longer crashes on load — default console memory is back to the
  retail 4 GiB (auto-raised only when a large texture mod actually needs the headroom).

## v0.1.22 (2026-07-05)

**New — Frame Generation (experimental)**
- **Frame Generation ships as an opt-in**: toggle it live in the Quick-Access blade (L3+R3) or per
  game in settings. It runs AMD FidelityFX optical-flow frame interpolation inside Ryudeck's own
  Vulkan present path — no external layers — presenting an interpolated frame between each pair of
  real frames on a dedicated present thread. GPU-light titles roughly double their perceived
  frame rate (measured on device: ~21-30 fps rendered → ~42-60 presented).
- It is self-limiting by design: generation only engages when it actually helps — it suspends below
  a per-title frame-rate floor, when the GPU has no spare headroom (measured from real per-frame GPU
  time, so misreported "100% busy" titles are handled), and when the game already runs near panel
  rate. A zero-delay dip veto skips generation for any single spiked frame (e.g. a shader-compile
  stall), so sudden hitches are never smeared into interpolated frames.
- Fixed: the feature now engages reliably on every launch (previously a first-launch state could
  latch process-wide), the blade reports its live state honestly (including a "restart game" hint
  when a change needs one), and enabling the uncapped frame rate now always decouples game speed
  from present rate so games can't run fast.

**Per-game boost profiles — more titles tuned out of the box**
- Tomodachi Life, The Legend of Zelda: Tears of the Kingdom, Xenoblade Chronicles 3, and Hyrule
  Warriors: Age of Calamity now ship with tuned profiles (uncapped present rate + frame generation,
  with per-title engagement floors where needed). All verified in real gameplay on device.

**Text input — the Steam Deck keyboard**
- In-game text input (player names, Mii names, search fields) now summons the **native Steam Deck
  on-screen keyboard** instead of a custom one. The built-in fallback keyboard is also fixed
  (correct QWERTY layout, no overlapping rows).

**Mii Editor**
- The system Mii editor is now reachable from Game Mode, exits cleanly back to the library (a
  missing applet-exit call previously locked it), and shows a proper branded loading screen.
  Groundwork landed for per-game Mii databases (selectable source, no behavior change yet).

**Quick-Access blade**
- **Exit Game is now a standout action** at the bottom of the blade — no longer buried in an
  accordion — with a two-press confirmation so a stray tap can't kill your session.
- Accordion section headers now look interactive: direction chevron, count chip ("Show (n)"),
  and a button-style bar — no more guessing what's expandable.
- The Frame Rate row reads the live present mode, so a baked uncap shows "Uncapped" instead of
  a stale "Native".

**Library**
- Booting into the library now always focuses your **last-played game** — selection no longer
  lands on an arbitrary card during the library scan, so browsing is a pure left/right affair.

**Per-game settings correctness**
- An explicit per-game Console Mode choice now survives the "Ignore per-game configs" switch, and
  built-in handheld preferences respect that switch too — your global docked/handheld choice stands
  unless you explicitly set one for the game.

**Performance**
- Faster GPU buffer lookups on the hot path (a most-recently-used shortcut in the buffer cache) —
  measurably lower main-GPU-thread overhead in heavy open-world titles.

## v0.1.21 (2026-07-04)

**Smoother in motion — every game**
- Fixed a per-frame Vulkan memory allocation in the FSR upscaler (the default scaler) that blocked the
  render thread on GPU fences hundreds of times per second under load. Heavy titles no longer dip as
  hard when the camera moves: Tears of the Kingdom's open field went from ~10 to ~15-17 fps in motion
  in our measurements, and every game that runs with the FSR scaler benefits.

**New — console features**
- **Console Mode**: choose Docked or Handheld globally *and* per game, right in settings — with a live
  mode tag in the overlay. Your per-game choice always outranks the built-in recommendation.
- **Cheats in the Quick-Access blade** (L3+R3): browse and toggle individual cheats live, in-game.
- **Boot lands on your last-played game**, and the whole launcher + overlays now navigate with the
  left thumbstick as well as the d-pad.
- **Controller-aware button glyphs** across the UI, a boot splash, gallery export, and
  usage-based achievements.
- The battery indicator now reads the real battery, and stick over-scroll is fixed in every list.

**Stability — sessions survive more**
- **A GPU crash no longer kills your session**: the emulator contains the device reset and routes you
  back to the launcher instead of a dead black screen.
- **Closing from Steam now exits cleanly in seconds** — previously a shutdown deadlock meant Steam
  force-killed the app after an 8-second hang, every time.
- Fixed a hard render-thread crash on an out-of-range texture format (hit in Tears of the Kingdom;
  the frame now degrades gracefully instead of taking down the session).
- The every-boot "date/time has changed" nag is gone — the emulated clock now persists across launches.
- Games that open the Mii editor no longer hang — the applet returns valid results (a first).
- Per-game boost profiles no longer overwrite your saved settings (the "configs fighting" bug).

**Video playback**
- In-game movies (openings, cutscene FMVs) decode much faster — multi-threaded H.264 decode plus
  fixes for SteamOS's FFmpeg 7 — resolving the movie-freeze and static-corruption class of bugs
  (Donkey Kong Country: Tropical Freeze's intro and friends). Decode failures now show black instead
  of garbage, and a misbehaving video can no longer crash the emulator.

**Rendering correctness**
- Instanced geometry using a vertex divisor greater than 1 now renders correctly
  (`VK_EXT_vertex_attribute_divisor`).
- Shaders reading the viewport Y-direction get the real value instead of a hardcoded one.
- Mirrored texture clamping (MirrorClamp) preserves the mirror on RADV.
- Fixed a present-path texture-view leak that slowly degraded long play sessions.

**Under the hood**
- Audio DSP thread gets realtime scheduling on a dedicated core; several audio teardown deadlocks fixed.
- The JIT code cache is region-aware past 512 MB (fixes free-list corruption in long sessions).
- Logging can no longer stall the GPU/JIT threads under load.
- Shader cache eviction now keeps your most-played games' caches, not the most recently written.
- Stripped the remaining dead ARM64/NVIDIA-desktop code paths — a leaner, Deck-only binary.
- Groundwork for optional frame generation and faster draw submission is in this build, disabled by
  default while it's validated.

**Verified on-device**
- Regression-swept on real hardware across Kirby and the Forgotten Land, Bayonetta 3, Donkey Kong
  Country: Tropical Freeze, Mario Kart 8 Deluxe, Super Mario Bros. Wonder, Super Mario Odyssey,
  Tomodachi Life, Hyrule Warriors: Age of Calamity, and Tears of the Kingdom — no regressions.

## v0.1.20 (2026-06-30)

**More games running**
- **Xenoblade Chronicles 3** no longer crashes on its fully-rendered opening cutscene. A shader the game
  compiles for one of its effects was producing SPIR-V the Steam Deck's AMD driver rejected (a hard GPU
  crash); it now plays through the cutscene into gameplay.
- **Super Smash Bros. Ultimate** now boots into gameplay — it was crashing on load — and holds ~60 fps.
- **Pokémon: Let's Go, Pikachu!** no longer soft-locks on the control-select screen: the game asks for a
  Handheld controller, which Ryudeck's per-game profile now binds automatically.
- **Luigi's Mansion 3** now boots into gameplay with its title update (a first; still rough).
- Fresh on-device compatibility captures added for Super Mario Odyssey, Super Mario Bros. Wonder, and
  Hyrule Warriors: Age of Calamity. See the Compatibility wiki for the full, current list.

**Frame pacing — simpler + correct**
- The QAM **Frame Rate** control is now **two clear knobs** instead of three overlapping ones:
  **Frame Rate** (Native / 60 / Uncapped) and **Speed** (Normal / Turbo). "Decouple" is no longer
  a separate setting that could fight the others — any cap above Native keeps game speed correct
  automatically.
- **Turbo** now always applies a real **2× fast-forward** (it could previously land on a frozen
  0× clock from a stale config).
- Uncapping no longer changes game speed by default (game clock decoupled from present rate).

**Configs that stop fighting**
- New **Ignore per-game configs** toggle (Settings ▸ System ▸ Game Config): run every game off your
  global settings, bypassing any stale/experimental per-game override. Non-destructive and reversible.

**Polish**
- Default player profile is now **Ryudeck** with the Ryudeck avatar (was "RyuPlayer" / Ryujinx logo).
- Steam-menu **Exit** now tears down and quits cleanly (~2s) instead of hanging on a confirm dialog.
- Per-second telemetry now self-documents how a game was paced (vsync / decouple / speed / target Hz).

## v0.1.19 (2026-06-29)

**Game settings (new)**
- Engine-side **native game settings**: Ryudeck can now set a game's internal graphics
  options — the ones a game exposes no menu for — by patching the game's own code at load,
  with no mod files. Every edit is **signature-verified** (the original instruction is
  confirmed before patching) and applied all-or-nothing, so a game build that doesn't match
  is safely skipped instead of corrupted.
- Patches are split by portability: self-contained edits (remove a call, force a return,
  set a value) apply on any matching build, while layout-dependent edits apply only on a
  verified game build — so a mis-targeted change can never crash the game.
- Tears of the Kingdom: lens-flare removal and targeting depth-of-field off are available
  now (opt-in, experimental). Dynamic-quality-reduction and draw-distance toggles are wired
  and gated to verified game builds.
**Performance**
- JIT now fuses ARM `TST`/`ANDS` flag tests directly into a native x86 compare-and-branch
  instead of materializing condition flags.
- Render thread runs leaner: dropped a per-command atomic on the hot loop and gated leftover
  fork diagnostics off the per-draw and per-texture paths.

**Stability**
- Vulkan fence waits now report their real status and never continue past an un-signalled
  fence — closes a class of race-driven glitches and hangs.
- Enable `robustBufferAccess2` / `robustImageAccess2` on every GPU that supports them, so
  out-of-bounds shader access is clamped instead of crashing.

**Fixes**
- Game cutscenes and intro movies (H.264) now decode and play correctly. On SteamOS's
  FFmpeg 7 they had regressed to a black or static-filled screen — most visibly Donkey Kong
  Country: Tropical Freeze's opening and Tears of the Kingdom's intro. Decoding now uses
  FFmpeg's stable public API, so it stays correct across FFmpeg versions.

**Quality of life**
- A "Loading" card now appears during the long black screen some games present after the
  shader/CPU caches finish compiling, including Tears of the Kingdom, so it's clear the game
  is still loading rather than hung. It clears itself the moment the game starts drawing.
- The Quick Menu options list now scrolls to follow the selection when it moves off-screen.
- The launcher now keeps your place: returning to the library re-focuses the game you had
  selected instead of snapping back to the first title.

**Experimental (opt-in, off by default)**
- Adaptive resolution scaling that trades sharpness in light scenes for stability in heavy
  ones, behind a flag for on-device testing.
- Gated groundwork for GPU compute de-swizzle and graphics-pipeline-library fast-link, each behind
  an on-device correctness gate.
- Expanded built-in CPU/GPU frame profiling to pinpoint per-frame cost.

## v0.1.18 (2026-06-28)

The big one: this build closes a large gap between recent development and the public
release. Games boot, take controller input **in-game** (not just in menus), and run.

**Controllers**
- Controller input now reaches the running game, not only the launcher/menus — the fix
  that makes games actually playable with a pad.
- ControllerSupport applet hardened: a Deck with more than one controller bound (docked
  plus an external pad, or keyboard plus an auto-bound pad) no longer gets stuck on a
  single-player title's controller-config screen. The emulator now presents exactly the
  number of pads the game asked for.

**Stability**
- Guest memory is now backed by `memfd` — ends a memory-exhaustion crash that could take
  down games on boot.
- Per-game config no longer leaks input bindings between titles; metadata, content, and
  screenshot stores are hardened against corrupt or mid-delete files; audio device output
  no longer overruns its buffer on channel-count changes.
- JSON saves recreate missing parent directories before writing, fixing crashes after a
  per-game config directory is deleted.

**Performance**
- Faster cold starts: the persistent JIT translation cache is mapped in one batch instead
  of hundreds of thousands of individual protection flips, and redundant CPU memory fences
  are dropped on the Deck's x86 core. New built-in CPU/JIT profiling to drive further work.

**Platform / housekeeping**
- Ryudeck is now a single Deck-native build (Linux/SteamOS/Vulkan-RADV); legacy
  Windows/macOS/cross-platform code paths were removed.
- Game Mode UI navigability fixes; QAM Frame Rate preset (Normal/High FPS/Turbo).
- Updater guard: portable and dev/local builds no longer auto-update from GitHub latest;
  set `RYUDECK_ENABLE_UPDATER=1` to opt in.
- Steam controller-layout install targets one Steam user by default and ships its helper
  files; changelog/update links point at Ryudeck releases; attribution clarified for
  tooling derived from the Ryujinx/Ryubing project structure.

## v0.1.17 (2026-06-25)

Reset Config (QAM) and Reset to Default (Settings) no longer delete the
entire per-game config. They only strip input config entries — DLC paths,
update directories, graphics, and system settings survive. Fixes crashes
when DLC was installed after a config reset.

## v0.1.16 (2026-06-25)

Adds a CONTROLLER section to the QAM blade: shows connected controller
name and a Reset Config action that strips keyboard entries, forces
auto-bind to regenerate controller config, and persists the result.
Auto-bind results are now saved to disk so per-game configs survive restarts.

## v0.1.15 (2026-06-25)

Disables keyboard and mouse input when running under Gamescope (Game Mode).
The keyboard config at Player 0 no longer conflicts with the controller.
Keyboard entries are stripped from the config list before auto-bind.

## v0.1.14 (2026-06-25)

Fixes thumb sticks not working when a keyboard config shares Player 0
with the auto-bound controller. The keyboard entry was writing (0,0) stick
data over the controller's real values in the HLE state list.

## v0.1.13 (2026-06-25)

Fixes InvalidCastException in all SetConfiguration paths. All drivers now
type-check before casting: SDL3Gamepad, SDL3JoyCon, SDL3Keyboard, and
AvaloniaKeyboard.

## v0.1.12 (2026-06-25)

Fixes controller vs keyboard PlayerIndex conflict. When a gamepad is
already open, keyboard entries at the same slot no longer overwrite its
Id with the keyboard's fake '0' — the controller takes priority.

## v0.1.11 (2026-06-25)

Fixes a PlayerIndex collision where per-game keyboard configs at Player 0
overwrote the auto-bound controller with an Id of "0," causing a permanent
idMismatch loop. ReloadConfiguration now marks slots as claimed so subsequent
entries reuse the opened gamepad instead of replacing it.

## v0.1.10 (2026-06-25)

Fixes InvalidCastException crash on game launch when a controller config
was passed to the keyboard handler. AvaloniaKeyboard.SetConfiguration now
checks the config type before casting.

## v0.1.9 (2026-06-25)

Fixes a tight-loop controller reopen (1ms interval, ~900 Hz) that starved
guest input on some Decks. The idempotent guard now includes a 100ms debounce
as a hard backstop.
## v0.1.8 (2026-06-25)

Controller now works in all games — not just some. One-shot install.

- **Controller fix**: the per-game config on disk can have a stale id ('0') that
  was being copied back into the controller each frame, restarting the reopen loop
  even after the v0.1.7 fix. The guard now writes the correct live pad id back into
  config and compares GUIDs (not just full ids). Cleared the per-frame dispose+reopen
  spam for all games, not just ones whose config id happened to match.
- **One-shot install**: `./Ryudeck/install.sh` copies everything to
  `~/.local/share/ryudeck`, creates a `.desktop` entry (appears in the app menu),
  installs the icon. No manual placement needed.
- **No Spacewar**: the ISteamInput native driver is optional (gated behind
  `RYUDECK_STEAM_INPUT=1`). The default path — SDL3 → Steam Input virtual
  gamepad — gives pads, gyro, and touchpad with no Steam API tricks.
- **Upgrade from v0.1.7**: if controllers don't work, clear old per-game configs:
  `rm -rf ~/.config/Ryudeck/games/`

## v0.1.7 (2026-06-25)

In-game controller input should now reach the game.

- **Per-frame reopen fix**: the HLE NPad pipeline was disposing and reopening the
  gamepad every frame (~600 Hz), starving the guest of input because a freshly
  reopened SDL handle reads zero until the next event pump. Menu navigation worked
  because UiGamepadNavigator held one handle — now the in-game path does the same
  via GUID-based idempotency.
- Two deep-dive docs added: Steam Input Non-Native Integration (CRC32 AppID strategy)
  and SDL3 Steam Deck controller backend reference.

## v0.1.6 (2026-06-25)

The running build version is now visible in Game Mode, plus controller-input diagnostics.

Fixes a controller that showed as connected but sent nothing to the game.

- Fixed controllers that bound but delivered no input. When a pad's id changed during launch (Steam Input re-presents its gamepad mid-boot), the rebind added in v0.1.4 switched to the live pad but left the old id in the config. The input layer compares those two every frame, so the mismatch made it reopen the pad on every single frame, and a pad reopened that fast never builds up a readable state, so the game received nothing. The pad now binds once and keeps delivering input. This was most visible with a second controller plugged in alongside the Deck's own.
- Added a once-per-second input log line. A controller that is bound but silent is now obvious in the log instead of failing with no trace.

## v0.1.4 (2026-06-25)

Smoother first-time game loads, a fix for controllers that silently didn't work, in-app updates, and the version is now visible at a glance.

- First launch of a heavier game no longer freezes near the end of loading. The code cache filled mid-load and grew itself on the spot, which froze the screen for about a second right as the game would appear, so it looked like a crash. The cache now starts large enough to avoid that, and the long first-time compile only happens once per game.
- Fixed controllers that bound but sent nothing to the game. If the pad's id changed between startup and game launch (Steam Input re-presents its gamepad during boot), the controller was dropped with no warning and you got no in-game controls. Ryudeck now rebinds to the live pad by its stable device id, and logs exactly what happened so input problems are no longer invisible.
- Updating from inside the app now works. "Check for Updates" (Help menu) pulls the latest Ryudeck release from GitHub, downloads it, and replaces the install in place. The button was previously wired to Ryujinx's update server, which never knew about Ryudeck, and was disabled on every build, so it could never actually update.
- The current version is shown in the menu bar, so you (and bug reports) always know exactly which build is running.
- Logs are no longer cut off when you back out of a game through Steam, so crash reports keep the gameplay history instead of stopping at startup.

## v0.1.3 (2026-06-25)

Controller buttons now match their labels, and a lighter default for better framerate.

- Fixed swapped face buttons: your Deck's A is the in-game A again (and B/X/Y match too). The previous mapping sent A as B, so on-screen prompts were wrong and menu confirms acted as cancel, which made games feel like the controller did nothing.
- Default resolution is now native for the best framerate out of the box. Raise it per-game in Game Settings for lighter titles. Existing per-game settings are untouched.

## v0.1.2 (2026-06-24)

A fix for a crash that hit fresh installs right after the controller bound.

- Fixed a crash that happened the moment a controller was auto-bound on a fresh Deck. The auto-bind built an incomplete controller config (missing its LED settings), and the input layer crashed reading them. It only triggered on installs whose system SDL reports an LED on the pad, which is why it slipped past earlier testing.
- Audio now falls back cleanly when a backend cannot start. A backend that reported itself usable but failed to start a stream used to take the whole game down instead of dropping to the next one.

## v0.1.1 (2026-06-24)

Controller and setup fixes so a fresh install is playable out of the box.

- Controllers now work in-game. Ryudeck auto-binds your controller to the emulated Player 1 on launch. A fresh install defaulted to keyboard only, so no pad reached games even though it drove the menus.
- Local multiplayer: up to 8 controllers auto-map to players 1 through 8, with hot-plug. Plug a pad in mid-game and it joins as the next player.
- Gamepad-navigable file browser for installing keys, firmware, and adding games. The previous native dialog could not be driven by a controller in Game Mode.
- Firmware now imports from an existing emulator install, not just keys. It was missing Ryujinx's directory-style firmware files.
- Control-scheme install now works. It has to be run with Steam shut down, and the installer tells you.

## v0.1 (2026-06-24)

First public release. A Steam Deck-native Switch emulator, forked from Ryujinx.

- Vulkan/RADV renderer tuned for the VanGogh GPU, with a handheld-first UI and an in-game quick-settings blade.
- Native dynamic-resolution support for GPU-bound titles (honest GPU-time) and a Deck-tuned ARMeilleure JIT.
- A guest-memory backing fix (memfd) that resolved a Deck-wide crash affecting many titles at launch.
- Setup flow for supplying your own legally-obtained keys and firmware.
- Hardening: atomic config writes, crash-resilient playtime, texture-cache safety, and a shutdown watchdog.

No games, ROMs, firmware, or keys are included. Bring your own legally-obtained dumps.
