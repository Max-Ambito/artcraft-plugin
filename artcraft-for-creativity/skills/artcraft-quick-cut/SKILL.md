---
name: artcraft-quick-cut
description: "Make a rough cut from video footage with FilmCraft: trim to given timestamps, remove sections, detect shots and keep selected ones, export. Use for 'cut this video down', 'trim from 0:30 to 1:15', 'make a 30-second version', 'remove the first 10 seconds'. Does not choose highlights by itself: it needs timestamps from the user or from shot detection plus the user's picks. Do NOT use for color grading or titles."
license: Apache-2.0
metadata:
  version: 0.1.0
---

# ArtCraft: quick cut (FilmCraft)

Uses the `filmcraft` server. It can read and write any absolute path, so confirm the source and the export path with the user and never overwrite the source.

## Steps

1. **Import.** `media_import` the file (absolute path). Inspect with `project_inspect`. Ask only for a target length or the moments to keep if neither is clear.
2. **Build a sequence.** Headless needs a project first: `command_run` `file.newProject` (`name`) before `media_import`. Then `file.newSequenceFromClip` (`items:[itemId]`) puts the clip on the timeline. Confirm with `sequence_inspect` (clip ids, start and duration in ticks; 254016000000 ticks = 1 second). Other ids (use `command_list`, do not guess): `edit.rippleDeleteHead`, `clip.sceneEditDetection` (`sensitivity`, `applyCuts`, `createSubclips`, `generateMarkers`, `wait`), `markers.markIn` / `markers.markOut`.
3. **Cut.**
   - Exact ranges, verified on a 10 s clip (cut 3 s to 7 s, result 6 s): `timeline.razor` with `time` in ticks at each boundary (3 s = 762048000000, 7 s = 1778112000000), re-read clip ids with `sequence_inspect`, then `edit.rippleDelete` with `clips:[middleClipId]` so the gap closes. The exported file probed at exactly 6.0 s.
   - Highlights: run `clip.sceneEditDetection` with `applyCuts: true`, list the shots from `sequence_inspect`, propose a selection and wait for approval before deleting anything. State plainly that moments are chosen by the user or by these detected shots, not by content understanding.
4. **Check.** `render_frame` at a few `seconds` values and show them; report resulting duration from `sequence_inspect` and compare with the target.
5. **Export.** `command_run` with `file.exportMedia` (`path`, `format` or `preset`, `wait: true`). Report the output path and duration. If the length overshoots, offer another pass.

## Limits
No music, titles or transitions in this skill. For those, point to `command_list` and offer a follow-up.
