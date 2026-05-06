# CLAUDE.md — Adv360 Pro Keymap

This file briefs Claude Code on the state of Pete's Advantage 360 Pro
keymap project. Read this first before making changes.

## Repo

- **Location:** `plooney81/Adv360-Pro-ZMK` (fork of `KinesisCorporation/Adv360-Pro-ZMK`)
- **Active branch:** `V2.0`
- **Source of truth:** `config/adv360.keymap` — edit this file directly.
- **`config/keymap.json`:** legacy GUI-format file. Should be deleted.
  Do NOT edit it. If it still exists in the working tree, the first
  task is `git rm config/keymap.json && git commit`.

## Build pipeline

- GitHub Actions builds firmware on every push (see `.github/workflows/`).
- Confirm the workflow's `on:` trigger includes `V2.0` — if it only
  watches `main`, pushes to `V2.0` produce no artifacts.
- Local builds via `make build` (keymap-only changes) or `make all`
  (full rebuild) using the included Dockerfile. Faster than CI for
  iteration (~30s vs ~3-5min).
- Output: two `.uf2` files (left half + right half). Flash by
  double-tapping reset on each half and dragging the file onto the
  USB drive that appears.

## Pete's background that matters here

- 4yr Clojure + 1yr+ PureScript professional. Comfortable with
  config-as-code; prefers plain text over GUIs.
- Coming from a Planck EZ with a mature 6-layer QMK setup
  (Lower/Base/Raise/Adjust/Mouse/Move). Wants to preserve that
  muscle memory on the 360.
- Mac user. Cmd, not Ctrl, is the primary modifier.
- Heavy bracket use (Clojure paredit). `() [] {}` are high-frequency.
- Uses Vim/Vim-bindings. Arrow keys live on H/J/K/L (or adjacent)
  in the nav layer.

## Current keymap design (as of handoff)

Layer model mirrors the Planck exactly:

| # | Name   | Triggered by                          | Purpose                        |
|---|--------|---------------------------------------|--------------------------------|
| 0 | BASE   | default                               | QWERTY                         |
| 1 | LOWER  | left big thumb hold                   | symbols                        |
| 2 | RAISE  | right big thumb hold                  | numbers + numpad               |
| 3 | ADJUST | LOWER + RAISE both held (tri-layer)   | F-keys, media, system          |
| 4 | MOUSE  | reserved (not yet wired)              | mouse keys (needs ZMK config)  |
| 5 | MOVE   | bottom-left thumb hold (`mo MOVE`)    | navigation (arrows, PgUp/Dn)   |

### Thumb assignments (base layer)

**Left cluster:**
- Outer thumb (small): `mo MOVE`
- Inner top: `&none` (reserved)
- Inner middle row: `LCTRL`, `LALT`
- Big inner thumb: `mo LOWER`
- Adjacent: `BSPC`, `DEL`

**Right cluster:**
- Big inner thumb: `mo RAISE`
- Adjacent: `SPACE`, `ENTER`
- Inner middle row: `LGUI`, `RCTRL`
- Outer keys: passthrough / extras

**Esc/Cmd mod-tap** lives on the inner-left home-row column
(where it was on the Planck). Tap = Esc, hold = Cmd (LGUI).

### Defined behaviors

- `esc_cmd`: tap-preferred, 200ms tapping term, tap=Esc, hold=LGUI.
- `hm` (homerow_mods): defined but NOT YET USED on base layer.
  Same params. Will be enabled in a later phase.

### Tri-layer

Native ZMK `conditional_layers` block at the top of the file activates
ADJUST when both LOWER and RAISE are held. No extra logic needed
in the LOWER or RAISE layer bindings.

## Known issues / unverified

- **Keymap has not been compiled yet.** Pete needs to push to V2.0,
  let GitHub Actions build, and check for syntax errors before
  flashing. Most likely failure: miscounted `&trans`/`&none` in
  one of the rows. ZMK error messages are usually clear about
  which row.
- **Mouse layer is a stub.** Wiring up real mouse keys requires
  enabling `CONFIG_ZMK_MOUSE=y` in `config/adv360.conf` and using
  ZMK's mouse bindings (which differ from QMK). Defer until base
  layout is solid.
- **Combos not yet ported.** Pete's Planck has combos configured
  in the GUI but they're not yet mirrored in the ZMK keymap.
  Needs separate pass once Pete identifies which combos are
  worth keeping.
- **Home-row mods are defined but disabled.** Plan is to enable
  these in week 3+ once base layout muscle memory has formed.
  Activation = swap `&kp X` for `&hm MOD X` on home-row keys:
  - Left: `A=LGUI S=LALT D=LCTRL F=LSHFT`
  - Right: `J=RSHFT K=RCTRL L=RALT ;=RGUI`

## Test plan after flashing

1. Type a paragraph. Confirm base letters.
2. Hold left big thumb + type `df` → should produce `{}`.
3. Hold right big thumb + type `jkl` → should produce `456`.
4. Hold both big thumbs + press `q` → should produce F1 (tri-layer).
5. Hold bottom-left thumb + press `j` → should be Down arrow.
6. Tap inner-left thumb quickly → Esc. Hold + Tab → Cmd+Tab.

## Roadmap (post-handoff)

**Week 1-2: Acclimation**
- Drive on the keymap as-is.
- Keep a tweaks log (markdown file) for any keys that feel wrong.
- Resist iterating prematurely — give muscle memory a chance to form.

**Week 2-3: First refinements**
- Address pain points from the tweaks log.
- Verify layer 4 (Mouse) approach: enable `CONFIG_ZMK_MOUSE` and
  port from the Planck Mouse layer screenshot.
- Port any combos worth keeping.

**Week 3+: Home-row mods**
- Swap `&kp` for `&hm` on home-row keys (see "Known issues").
- Tune `tapping-term-ms` if false triggers occur.
- Free up the dedicated Ctrl/Alt/Cmd thumb keys (currently row 3
  of left thumb cluster) for other purposes once home-row mods
  cover those modifiers.

**Long-term:**
- Document final layout in a `LAYOUT.md` reference doc with
  ASCII diagrams per layer.
- Consider VS Code extension goals for the broader Scour project
  (separate but adjacent context).

## Repo conventions

- Plain commits to `V2.0` for layout iterations.
- One concept per commit (e.g., "Move Tab to thumb", "Add
  symbol layer brackets to home row") so layout history is
  bisectable.
- After every push, check Actions tab to confirm the build
  succeeded before assuming a key change took effect.
