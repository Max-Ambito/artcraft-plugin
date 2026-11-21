---
name: artcraft-retouch-portraits
description: "Bulk-retouch portrait photos with PhotoCraft using non-generative tone and cleanup steps: auto tone, contrast, gentle color, light sharpening, optional background softening. Use for 'retouch my portraits', 'process these headshots', 'clean up this folder of wedding photos'. Do NOT use for changing a person's face, body, hair or clothes, or for generative fixes; decline those and say they are out of scope."
license: Apache-2.0
metadata:
  version: 0.1.0
---

# ArtCraft: retouch portraits (PhotoCraft)

Non-generative only. Never alter a person's face, skin texture, hair, body or clothing beyond global tone/color; never remove or add objects.

## Steps

1. **Collect.** Files must be inside the work folder (`ARTCRAFT_WORK_DIR`). Ask only for the intended mood (natural, warm, punchy) if not stated.
2. **Discover commands** with `command_list` (do not guess ids). Typical plan: `image.autoContrast` (hue-safe; use `image.autoTone` only to fix a visible color cast, since it levels each channel and can shift skin tones; check on the pilot), a mild `image.adjustments.vibrance` or `.exposure`, a light `filter.sharpen.unsharpMask`; for a softer background only when asked, a lens-type blur in `filter.blur.*` applied on a masked duplicate layer, checked with `doc_inspect`.
3. **Pilot** on the first image: `doc_open`, `command_batch`, `doc_render_preview`. Show before/after (render the preview before and after the batch). Wait for approval or tweaks.
4. **Batch.** Same recipe per file: `doc_open`, `command_batch`, `doc_save` to a `retouched/` subfolder, `doc_close`. Originals stay untouched.
5. **Review.** Report counts, failures, and the recipe. Suggest the user spot-check a few frames at 100%.

## Limits

- Skin smoothing and blemish removal are not part of this recipe; offer to do it manually on a single image only if the user asks and the commands exist in `command_list`.
- If the user asks for generative changes to a person, explain it is out of scope.
