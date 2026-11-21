---
name: artcraft-resize-media
description: "Resize or crop images (PhotoCraft) or video (FilmCraft) to exact pixel dimensions or an aspect ratio, e.g. 1080x1920 for reels, 1080x1080 for squares, 1920x1080. Use for 'resize this', 'make it vertical', 'prepare for Instagram/TikTok', 'change aspect ratio'. Do NOT use for color/look changes or cutting video content."
license: Apache-2.0
metadata:
  version: 0.1.0
---

# ArtCraft: resize media

## Decide the app
- Images (png, jpg, webp, tif, psd): `photocraft` server. Files must be in the work folder.
- Video: `filmcraft` server. It accepts any absolute path, so confirm every output path with the user before exporting and never overwrite a source.

## Images (PhotoCraft)
1. `command_list` and find the size/crop commands (search for image size, canvas size, crop). Do not guess ids or parameters.
2. State the plan: target size, fit mode (crop to fill vs. fit with padding), resampling. Default to crop-to-fill centered; ask if the subject may be cut off.
3. Pilot one file: `doc_open`, `command_batch`, `doc_render_preview`, show it. Then run the rest and `doc_save` to a `resized/` subfolder.

## Video (FilmCraft)
1. Headless needs a project first: `command_run` `file.newProject` (`name`), then `media_import` with `paths` (absolute, Windows-style `C:/...`). It returns the item id.
2. `file.newSequenceFromClip` (`items:[id]`) makes a sequence with the clip on V1 at the source size. Read the clip id with `sequence_inspect`.
3. Set the target size with `sequence.settings` (`width`, `height`). Verified on 1280x720 to 1080x1920.
4. **Fit** (whole picture visible, bars on the empty sides), verified: `timeline.select` (`clips:[clipId]`), then `clip.scaleToFrameSize` (it errors with "no clips selected" if nothing is selected), then center it with `effects.setParam` (`clip`, `effect:"motion"`, `param:"position"`, `value:[width/2, height/2]`). Without the centering step the clip stays at its old position and is cut off.
5. **Fill** (crop the sides, no bars): raise the motion `scale` param after fitting until the frame is covered. Not verified yet; confirm with `render_frame`.
6. Check with `render_frame` (returns a PNG) before exporting, and show it to the user.
7. Export with `command_run` `file.exportMedia` (`path`, `format` such as `h264`, `wait: true`). Verified: the output file had the requested 1080x1920. Say clearly if bars were added.

## Report
Output paths, final dimensions and any cropping that happened.
