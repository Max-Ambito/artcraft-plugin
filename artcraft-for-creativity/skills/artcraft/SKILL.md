---
name: artcraft
description: "Show a short tour of the ArtCraft for creativity plugin: which apps and workflows are available, with an example prompt for each. Trigger when the user invokes /artcraft or asks what they can do with ArtCraft, PhotoCraft, FilmCraft or EffectCraft. Do NOT trigger for a request to actually edit something; send that to the matching skill."
license: Apache-2.0
metadata:
  version: 0.1.0
---

# ArtCraft tour

Informational only. Never call an app tool while showing the tour.

## Render

1. A short setup note: apps run locally through their built-in MCP servers (no sign-in). PhotoCraft and EffectCraft only touch files inside the work folder (`ARTCRAFT_WORK_DIR`, default `C:/ArtCraft-Work`); FilmCraft accepts any absolute path.
2. This table (plain markdown, one entry per row: name, description, italic example, slug, app):

| Capability | Description | Example prompt | Skill | App |
|---|---|---|---|---|
| Batch edit photos | One consistent look across a set of images | "give these travel photos a warm, cohesive look" | `/artcraft-batch-edit-photos` | PhotoCraft |
| Retouch portraits | Non-generative cleanup in bulk | "retouch this folder of headshots" | `/artcraft-retouch-portraits` | PhotoCraft |
| Resize media | Exact pixels or aspect ratios | "resize this video to 1080x1920 for reels" | `/artcraft-resize-media` | PhotoCraft, FilmCraft |
| Quick cut | Rough cut from long footage | "cut the first 90 seconds of this clip into a 30-second reel" | `/artcraft-quick-cut` | FilmCraft |
| Motion effects | Titles, text and effects rendered out | "make a 5-second title card that fades in" | `/artcraft-motion-effects` | EffectCraft |

3. One closing sentence inviting the user to try an example verbatim.

If the next message is a real request, hand off to the matching skill instead of repeating the table.
