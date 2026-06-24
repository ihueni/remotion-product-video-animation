# Remotion Video Production Skill

A Codex skill for planning, building, localizing, previewing, rendering, and QA-ing narration-driven Remotion video projects.

The skill is designed for videos that combine scripts, voiceover, HTML storyboards, frame-driven animation, screenshots, SVG paths, BGM, and final MP4 delivery. It is intentionally product-neutral and company-neutral: bring your own brand assets, audio provider, visual system, and project conventions.

## What It Helps With

- Narration-first video workflow from script to final render.
- HTML storyboard scenes that can be reused as Remotion visual source.
- Frame-driven animation and deterministic still-frame QA.
- SVG path reveal checks for early endpoints, broken lines, and unwanted base-path exposure.
- Multilingual video versions with independent timing, text overflow checks, and render QA.
- Audio/BGM handling without locking users into a specific vendor.
- Final MP4 quality checks, ffprobe validation, and cover-frame embedding.

## Installation

Clone this repository into your Codex skills directory:

```bash
mkdir -p ~/.codex/skills
git clone https://github.com/ihueni/remotion-video-production.git ~/.codex/skills/remotion-video-production
```

Then invoke it explicitly:

```text
Use $remotion-video-production to plan and QA this Remotion video project.
```

If your Codex environment supports automatic skill discovery, the skill description in `SKILL.md` can trigger it for Remotion video production tasks.

## Repository Layout

```text
remotion-video-production/
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    ├── animation-qa.md
    ├── html-remotion.md
    ├── i18n-visual-qa.md
    ├── rendering.md
    └── workflow.md
```

`SKILL.md` stays compact and routes Codex to focused reference files only when needed.

## Audio And BGM Notes

This skill does not require a specific TTS, voice, BGM, or music generation provider.

Suggested defaults when the user has no existing provider:

- Chinese narration: Fish Audio.
- English narration: ElevenLabs.
- Repeated production: choose a paid plan with API access, sufficient credits, and appropriate commercial-use terms.

For AI-generated BGM, the skill teaches how to derive a music prompt from the script and scene plan rather than naming a specific music generation service.

## Publishing Notes

Before publishing changes:

1. Keep `SKILL.md` concise and move detailed rules into `references/`.
2. Avoid company, product, customer, internal path, or private data references.
3. Check that all reference files linked from `SKILL.md` exist.
4. Validate frontmatter contains only `name` and `description`.

## License

MIT
