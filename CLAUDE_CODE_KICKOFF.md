# Kickoff prompt for Claude Code

Paste this (or a trimmed version) as the first message in your new
Claude Code session, after `cd`-ing into your local clone of the
Adv360-Pro-ZMK repo.

---

I'm picking up an Adv360 Pro keymap project. Read `CLAUDE.md` first
for full context — it covers the repo layout, my background,
the design decisions, and known issues.

Quick recap of where we left off:

- I have a draft `config/adv360.keymap` that ports my Planck EZ
  layout (Lower / Raise / Adjust / Mouse / Move) to the Advantage
  360 Pro. It uses native ZMK `conditional_layers` for the
  Lower+Raise → Adjust tri-layer, and defines an `esc_cmd` mod-tap
  and an unused `hm` (home-row mods) behavior.
- The keymap has NOT been compiled yet. First task is to push to
  the `V2.0` branch and confirm GitHub Actions builds it cleanly.
  If the build fails, fix syntax errors (most likely a miscounted
  `&trans` or `&none` in a row).
- `config/keymap.json` is stale and should be deleted — it conflicts
  with `adv360.keymap` and confuses the build.

What I'd like to do in this session:

1. Verify build pipeline. Check `.github/workflows/` to confirm the
   trigger covers `V2.0`. Delete `keymap.json`. Push and watch CI.
2. If build fails, debug. If build succeeds, flash and test against
   the test plan in `CLAUDE.md`.
3. Start a `tweaks.md` log file to capture anything that feels off
   during the first week of use.

Don't make layout changes yet — get the current draft compiling
and flashable first. Iteration comes after muscle memory has had
a chance to form.
