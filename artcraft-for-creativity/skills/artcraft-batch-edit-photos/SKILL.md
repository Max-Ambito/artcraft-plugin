---
name: artcraft-batch-edit-photos
description: "Give a whole set of images one consistent look with PhotoCraft: tone, contrast, color, sharpening, export to a chosen format. Use for 'batch edit these photos', 'same look for all of these', 'apply this style to the folder', 'make these photos cohesive'. Do NOT use for portrait skin retouching (use artcraft-retouch-portraits), resizing only (artcraft-resize-media), or generative edits, which PhotoCraft's engine commands do not provide."
license: Apache-2.0
metadata:
  version: 0.1.0
---

# ArtCraft: batch edit photos (PhotoCraft)

Uses the `photocraft` MCP server. File access is limited to the work folder (`ARTCRAFT_WORK_DIR`).

## Steps

1. **Locate files.** Ask only if missing: where the images are and the look wanted. If the files are outside the work folder, tell the user to copy them in (the server refuses reads elsewhere). Never write outside the work folder.
2. **Find command ids.** Call `command_list` and look for the adjustments needed. Do not guess ids. Known examples: `image.autoTone`, `image.autoContrast`, `image.adjustments.exposure`, `.brightnessContrast`, `.levels`, `.curves`, `.vibrance`, `.hueSaturation`, `filter.sharpen.unsharpMask`. Parameters are described by `command_list`.
3. **Pilot on one image.** `doc_open` the first file, run the chosen commands with `command_batch` (one history step, fewer round trips), then `doc_render_preview` and show the result. Adjust until the user approves; use `doc_inspect` to see history and undo.
4. **Run the set.** For each file: `doc_open`, the same `command_batch`, `doc_save` (or `doc_export`) to a new name or an `edited/` subfolder inside the work folder, then `doc_close`. Never overwrite originals unless the user explicitly says so.
5. **Report.** List outputs, any file that failed and why, and the settings used so the look can be repeated.

## Tested recipe (PhotoCraft 0.22.0)

`command_batch` takes `{"steps":[{"id":"...","params":{...}}]}`. Verified on 3 JPEGs (open, batch, save to `edited/`, close, all steps ok), then each step viewed on its own:

- `image.autoContrast` stretches contrast and keeps the hue: use it as the default.
- `image.autoTone` levels each channel separately and can cause a strong color cast on images with a color bias (a synthetic orange gradient turned yellow and its gray subject turned cyan). Use it only when the user wants cast correction, and always pilot first.

```json
{"steps":[
  {"id":"image.autoContrast","params":{}},
  {"id":"image.adjustments.vibrance","params":{"vibrance":30,"saturation":5}},
  {"id":"filter.sharpen.unsharpMask","params":{"amount":60,"radius":1.2,"threshold":0}}
]}
```

Ranges: vibrance/saturation -100..100; unsharp amount 1..500, radius 0.1..1000, threshold 0..255. Saving to JPG flattens layers and reports a "lossy compression" warning (expected).

## Notes

- Paths: relative paths (`in/photo1.jpg`, `edited/photo1.jpg`) resolve inside the work folder and are the safest choice; absolute paths must be Windows-style (`C:/...`) and inside the folder. Saving over an existing file works.

- For very large folders, mention the app's own `photocraft-cli batch` (applies an action list to a directory) as a faster alternative the user can run themselves.
- Long commands may run as background jobs: check `jobs_list` before saving.
- Keep one consistent set of parameters across the batch; do not vary per image unless asked.
