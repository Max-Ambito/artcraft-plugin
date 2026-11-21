# ArtCraft for Creativity (v0.1.0, experimental)

Claude plugin for ArtCraft: PhotoCraft (≈ Photoshop), FilmCraft (≈ Premiere) and EffectCraft (≈ After Effects).
It wires each app's own `*-cli mcp` server into Claude and adds skills with ready-made workflows.
Everything runs locally; no account or upload.

## Requirements
- Windows with the ArtCraft apps installed in `C:\Program Files\ArtCraft\` (edit `.mcp.json` if elsewhere).
- Work folder: PhotoCraft and EffectCraft can only read/write inside `ARTCRAFT_WORK_DIR` (default `C:/ArtCraft-Work`). Put files there or set the variable to another folder. Create the folder first.
- FilmCraft's MCP has no folder restriction: it reads and writes any absolute path it is given, so skills confirm export paths before running.
- Optional live mode: start an app with `--control <port>` and add `--bridge <port>` to its server args to edit the open window instead of a headless engine.

## Skills
| Skill | App | What it does |
|---|---|---|
| `/artcraft` | all | Tour of what is available |
| `/artcraft-batch-edit-photos` | PhotoCraft | One consistent look across many images |
| `/artcraft-retouch-portraits` | PhotoCraft | Non-generative portrait cleanup in bulk |
| `/artcraft-resize-media` | PhotoCraft, FilmCraft | Exact sizes / aspect ratios for images and video |
| `/artcraft-quick-cut` | FilmCraft | Rough cut from footage (razor, ripple delete, export) |
| `/artcraft-motion-effects` | EffectCraft | Title cards, text and effects, rendered to video or PNG |

## Status
Tool names come from the servers' real `tools/list` (PhotoCraft 0.22.0, FilmCraft 0.18.0, EffectCraft). Skills are drafts: they still need end-to-end tests with real files.
