# ArtCraft plugin for Claude Code

Skills that let Claude drive the ArtCraft apps — **PhotoCraft**, **FilmCraft** and **EffectCraft** — through each app's built-in MCP server. Everything runs locally; no account or upload.

## Install

In Claude Code:

```
/plugin marketplace add Max-Ambito/artcraft-plugin
/plugin install artcraft-for-creativity@artcraft
```

Requires the ArtCraft apps installed in `C:\Program Files\ArtCraft\` (edit `artcraft-for-creativity/.mcp.json` otherwise) and a work folder at `ARTCRAFT_WORK_DIR` (default `C:/ArtCraft-Work`).

## Skills

| Skill | App | What it does |
|---|---|---|
| `artcraft` | all | Tour of what is available |
| `artcraft-batch-edit-photos` | PhotoCraft | One consistent look across many images |
| `artcraft-retouch-portraits` | PhotoCraft | Non-generative portrait cleanup in bulk |
| `artcraft-resize-media` | PhotoCraft, FilmCraft | Exact sizes / aspect ratios for images and video |
| `artcraft-quick-cut` | FilmCraft | Rough cut from footage |
| `artcraft-motion-effects` | EffectCraft | Title cards, text and effects rendered to video or PNG |

See [`artcraft-for-creativity/README.md`](artcraft-for-creativity/README.md) for details. Status: experimental (v0.1.0).

The workflow set is inspired by Adobe's `adobe-for-creativity` plugin (Apache-2.0); the skill text here is written for ArtCraft. This is an unofficial community plugin, not affiliated with Adobe or ArtCraft.

## License

[Apache-2.0](LICENSE)
