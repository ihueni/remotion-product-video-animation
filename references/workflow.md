# Workflow Reference

## 1. Production Sequence

Use this default sequence:

```text
script -> voiceover -> confirmed duration -> storyboard design -> HTML scenes -> Remotion integration -> MP4 render -> delivery QA
```

Voiceover duration drives scene length. Do not lock final frame counts before the actual audio exists.

## 2. Project Structure

Use or adapt this structure:

```text
video-project/
├── script.md
├── CHANGELOG.md
├── voice/
│   ├── source-language/
│   ├── target-language/
│   └── bgm/
├── storyboard/
│   └── target-language/
├── remotion/
├── remotion-target-language/
└── output/
```

## 3. Changelog Discipline

Before editing an existing project, read `CHANGELOG.md`. After meaningful changes, append a record that covers:

1. What changed.
2. Why it changed.
3. Which files changed.
4. Whether timing, audio, language versions, output files, or visual continuity are affected.
5. How it was verified.

For scene HTML work, include:

| Field | Content |
|-------|---------|
| Scene | Scene number and short title. |
| File | HTML, Remotion, audio, or output path. |
| Inherited source | Prior scene, visual system, or approved source. |
| Continuity | Menu state, layout, colors, assets, timing, or transition handling. |
| Difference | What this scene changes for its specific narrative job. |
| Verification | Frames, screenshots, console status, asset loading, or lint. |

## 4. Local Preview

Preview local HTML through a local HTTP server, especially when assets, fonts, videos, iframes, or browser security policies matter.

Rules:

1. Do not treat `file://` as final verification.
2. Start the static server from the project root when relative paths climb out of the scene folder.
3. Record the actual URL and frame query parameters used for screenshots.
4. Check console errors, missing assets, text overflow, and relevant frame states.

Example:

```bash
python3 -m http.server 8910
```

Then open:

```text
http://127.0.0.1:8910/storyboard/scene-03.html?frame=120&fps=30
```

## 5. Remotion Preview Boundaries

During Remotion integration, preview through the Remotion development server and frame stills. Do not render a full MP4 unless the user asks for final delivery or a render-specific smoke test.

Keep Remotion Studio or the preview entry focused on the final full-video composition. Temporary single-scene compositions are acceptable for debugging, but remove them before handing the project back for user preview.

## 6. Output Folders

Keep final and debug output separate:

```text
output/
├── final files only
├── debug/
│   └── raw renders, sync intermediates, smoke videos
├── screencut/
│   └── frame stills, visual QA, contact sheets, cover candidates
└── cover/
    └── confirmed or ready-to-embed cover image
```

Do not leave raw, sync, or smoke files in the output root once a final file exists.
