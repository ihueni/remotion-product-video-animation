---
name: remotion-video-production
description: Create, modify, localize, preview, render, and QA narration-driven videos built with Remotion, HTML storyboards, audio tracks, frame-driven animation, screenshots, SVG paths, and final MP4 delivery. Use when working on video scripts, scene timing, HTML-to-Remotion conversion, animation QA, multilingual versions, audio/BGM mixing, rendering quality, or video production troubleshooting.
---

# Remotion Video Production

## Identity

This skill guides AI agents that build or maintain narration-driven Remotion videos from scripts, audio, HTML storyboards, frame-driven animation, and final MP4 delivery. It focuses on repeatable production workflow, visual-source fidelity, preview discipline, and QA gates.

## Capabilities

- Plan a narration-first video workflow from script to final delivery.
- Maintain scene timing from actual voiceover durations and transition overlap.
- Build or update one HTML storyboard scene at a time with continuity records.
- Convert approved HTML storyboards into Remotion without redesigning the visual system.
- Make HTML animation seekable from a Remotion frame number.
- Check SVG paths, screenshots, text overflow, safe margins, and continuous UI scene boundaries.
- Localize videos into other languages with independent timing, visual, audio, and rendering QA.
- Render final MP4 files with sharp UI text, synchronized audio, and cover-frame handling.

## Boundaries

| Request type | Correct response |
|--------------|------------------|
| Product, company, or brand-specific rules | Ask for the project brand assets or local rules; do not invent names, logos, domains, or product claims. |
| Audio generation vendor or software choice | Prefer the user's existing provider or recording workflow; if unspecified, suggest defaults without making them mandatory. |
| Legal, licensing, or rights clearance for media | Flag that legal review or source licensing is needed; do not certify ownership. |
| Fully automated final render without user confirmation | Preview and QA first; render only when the user asks for final output or the workflow clearly reaches delivery. |
| Non-Remotion video stacks | Apply only general production QA if useful; do not pretend framework-specific Remotion commands apply. |
| Private or real screenshot content | Require anonymization before use in a public or client-facing video. |

## Quantitative Boundaries

| Parameter | Range | Behavior when exceeded |
|-----------|-------|------------------------|
| Scene count | 1-20 scenes | For larger videos, require a scene index and staged QA batches. |
| HTML storyboard scope | 1 scene per generation step | Split multi-scene HTML generation into per-scene files and record continuity. |
| Final UI video resolution | At least 2560x1440 when feasible | Treat lower resolution as preview unless the user explicitly accepts it. |
| Frame checks per scene | Opening, key action, stable frame, ending | Add checks around every animation start/end when issues involve timing or SVG paths. |
| Localization duration drift | Single scene <= 20%, whole video <= 10% from source timing | Compress wording or revise timing plan before stretching the entire video. |
| Main skill file size | Keep under 500 lines | Move detailed rules into `references/` and link them from the workflow table. |

## Output Schema

When completing production work, report the outcome in this shape:

```markdown
## Result
{What changed or was produced.}

## Files
- `{path}`: {purpose}

## Verification
- {lint, preview, frame still, audio, render, ffprobe, or visual QA result}

## Open Items
- {anything the user must confirm, such as cover frame, brand asset, or final render}
```

When updating a project changelog, include:

| Field | Content |
|-------|---------|
| Change summary | What changed and why. |
| Affected files | Scripts, HTML, Remotion source, audio, assets, output, or rules. |
| Continuity | Source scene, prior scene, timing, visual system, and localization impact. |
| Verification | Commands, frames, screenshots, audio checks, or render checks. |
| Pending work | User confirmation, render, cover choice, or future localization. |

## Workflow

1. Identify the phase: script, voiceover, HTML storyboard, Remotion integration, localization, audio, render, or QA.
2. If continuing an existing project, read its `CHANGELOG.md` first and preserve user changes.
3. Load only the reference files needed for the current phase:

| Task | Read |
|------|------|
| Project setup, changelog, local preview, output folders | `references/workflow.md` |
| HTML storyboard, source fidelity, Remotion boundaries | `references/html-remotion.md` |
| Frame-driven animation, SVG paths, continuous UI, spacing checks | `references/animation-qa.md` |
| Voiceover source, BGM, final render, ffprobe, cover frame | `references/rendering.md` |
| Localization, text overflow, screenshots, anonymization, visual QA | `references/i18n-visual-qa.md` |

4. Make the smallest change that solves the current production problem.
5. Preview through local HTTP or the project dev server, not `file://`, when relative paths or browser security can affect assets.
6. Verify with the right evidence: lint, frame stills, screenshots, pixel checks, audio probes, or ffprobe.
7. Update the project changelog with decisions, affected files, verification, and pending work.

## Experience Rules

- Recommend using Codex as the coordinating agent for production work, especially for reading changelogs, applying this skill, editing code, running previews, extracting frame stills, and recording verification results.
- Treat voiceover as the source of scene duration.
- Treat approved HTML as visual source code, not visual inspiration.
- Keep Remotion preview separate from final rendering; do not render full MP4 files prematurely.
- Use frame numbers for animation state so screenshots and final renders are deterministic.
- Keep visual QA concrete: bounding boxes, safe margins, text overflow, frame numbers, and asset-loading results.
- Never include private names, internal identifiers, real account data, or unlicensed third-party assets in a public video.

## Test Plan

### Coverage Tests

- Ask the skill to plan a new 6-scene narrated Remotion video and verify it routes to `workflow.md`.
- Ask it to convert an approved HTML scene into Remotion and verify it routes to `html-remotion.md`.
- Ask it to fix a path that appears too early and verify it routes to `animation-qa.md`.
- Ask it to prepare final export commands and verify it routes to `rendering.md`.
- Ask it to localize a video into English or another language and verify it routes to `i18n-visual-qa.md`.

### Boundary Tests

- Existing project with no changelog: require creating or asking for a project log before large changes.
- User asks for final MP4 while obvious QA is missing: run or request QA before rendering.
- Screenshot contains real names or account identifiers: anonymize before using it.
- HTML and Remotion differ: preserve the approved HTML source unless the user asks to revise it.

### Failure Tests

- `file://` preview has missing assets: retry through local HTTP before concluding the scene is broken.
- SVG path has early colored pixels: add hidden state or mask/clip-path and recheck pre-start frames.
- Localized text is clipped: shorten copy or adjust layout; do not ship ellipsis as final proof.

### Stability Design

Run at least three realistic tasks before publishing updates: one storyboard task, one animation QA task, and one final render QA task. Compare outputs for consistent routing, changelog discipline, and verification evidence.
