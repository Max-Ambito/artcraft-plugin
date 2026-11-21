---
name: artcraft-motion-effects
description: "Create motion graphics with EffectCraft: title cards, animated text, solids, effects and keyframes, previewed and rendered to a still or video. Use for 'make a title card', 'animate this text', 'add a blur/glow effect', 'lower third', 'intro'. Do NOT use for cutting footage (artcraft-quick-cut) or editing photos (artcraft-batch-edit-photos)."
license: Apache-2.0
metadata:
  version: 0.1.0
---

# ArtCraft: motion effects (EffectCraft)

Uses the `effectcraft` server (headless, with autosave). It can read and write only inside the work folder (`ARTCRAFT_WORK_DIR`); scripts (`run_script`) are refused when roots are set, so use commands and the property tools.

Paths: EffectCraft requires absolute Windows-style paths inside the work folder (`C:/.../file.ecproj`); relative paths are refused.

## Steps

1. **Brief.** Ask only for what is missing: size, frame rate, duration, text, style.
2. **Project.** `open_project` with `new: true` (or a `.ecproj` inside the work folder), then `command_run` `comp.new` (`name`, `width`, `height`, `frameRate`, `duration`, `background`).
3. **Layers.** Use `command_list` / `describe_command` to confirm parameters. Known: `layer.newText` (`text`, `font`, `size`, `fill`, `position`), `layer.newSolid` (`name`, `color`, `width`, `height`). Check fonts with `list_fonts` before choosing.
4. **Animate.** Verified on a 4 s title card (fade in with scale-up and a blur that clears), rendered to MP4:
   - `get_layer` (`layer`: id from `get_comp`) lists property paths: `transform/opacity` (0-100), `transform/scale`, `transform/position`, `transform/rotation`.
   - `add_keyframe` takes `{layer, path, time, value}`, one call per key (time in seconds). Vector values need all 3 components: `[80,80,100]`. Passing `[80,80]` made the Z scale 0.
   - `add_effect` takes `{layer, effect, values}` (not `params`), e.g. `{effect:"ec.blur.gaussian", values:{blurriness:30}}`; find ids with `list_effects` (`filter`). The effect's properties appear as `effects/#1/blurriness`, which can be keyframed with `add_keyframe`.
   - Group related edits with `batch` so they are one undo step.
5. **Preview.** `render_frame` (`time` in seconds, `max_side` in px) at a few times, e.g. 0, 0.5 and the end, show them, iterate. Use `undo` or `history` if a change is wrong.
6. **Render.** Save with `save_project` (inside the work folder). The MCP server has no render-to-video tool (only `render_frame` stills), so render with the app's CLI, tested to work: `effectcraft-cli render --project <saved.ecproj> --comp <name> --out <file.mp4> --format h264` (the output path must be inside the work folder). Tell the user the output path before running it.

## Limits
Prefer built-in effects and commands; do not claim features that `list_effects` / `command_list` do not show.
