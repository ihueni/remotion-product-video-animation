# Localization And Visual QA Reference

## 1. Localization Is A New Delivery

Treat every language version as its own deliverable. Do not only replace text.

For each language:

1. Generate, record, or import language-specific voiceover using the provider or workflow chosen by the user or project.
2. Recalculate scene durations and total frames.
3. Replace visible text, brand assets, domains, units, and currency only as appropriate for the project.
4. Run independent lint, frame checks, text overflow checks, audio checks, render QA, and final ffprobe checks.

## 2. Prefer Remotion Derivation

If the source-language Remotion version is already stable and the target version only changes voiceover, visible text, brand assets, domains, and timing, duplicate the Remotion project and localize the copy. Do not restart from target-language HTML unless the visual structure truly needs redesign.

Keep source and target language directories isolated.

## 3. Duration Adaptation

Before recording, generating, importing, or finalizing target-language voiceover, compare each source scene's duration, proof action, animation phases, and ending pause.

Rules:

1. Shorten target copy before stretching visuals.
2. Remove repeated feature lists or explanatory clauses already shown on screen.
3. If one scene grows by more than `20%`, record why.
4. If the whole video grows by more than `10%`, revise the localization plan instead of blindly extending every scene.
5. Keep the final scene as a conclusion, not a new explanation.

## 4. Text Overflow Audit

After localization, audit representative frames for every scene. Include:

1. Titles and subtitles.
2. Cards and proof blocks.
3. Table headers and rows.
4. Status bars and combined labels.
5. Buttons and chips.
6. Sidebar and menu items.
7. Circular or radial node labels.
8. Screenshot containers and captions.
9. End-card copy.

Fix by reducing copy density, changing line breaks, adjusting column widths, increasing dynamic width, or changing font size. Do not ship clipped text, accidental concatenation, or ellipsis as final proof unless the design intentionally requires it and the meaning remains clear.

## 5. Screenshots And Privacy

Before a screenshot enters a public or client-facing video, inspect it for:

1. Personal names.
2. Email addresses or account identifiers.
3. Workspace, folder, table, database, or task names.
4. SQL containing real schemas, fields, account IDs, or business logic.
5. Internal project names or incident references.
6. Customer data, billing data, operational metrics, or private messages.

Replace sensitive content with realistic demo data. After anonymization, run text search where possible and visually recheck rendered frames.

## 6. Screenshot Fit

Classify each screenshot as:

| Role | Usage |
|------|-------|
| Primary visual | Must be high resolution, legible, and correctly framed. |
| Ratio anchor | Use to preserve product proportions while rebuilding key UI in HTML/CSS/SVG. |
| Detail reference | Use only to guide layout or copy, not as visible final media. |

Rules:

1. Check original image dimensions and aspect ratio before placing it.
2. Do not stretch vertical screenshots into wide containers.
3. Do not zoom into screenshots when coordinates are uncertain.
4. If screenshot text is blurry, rebuild the critical UI instead of enlarging the image.

## 7. Brand And Asset Neutrality

Use project-provided logos, fonts, colors, and domains. If a product or brand asset is missing, keep neutral text or ask for the correct asset. Do not draw approximate logos or invent brand claims.

## 8. Visual QA Checklist

For each scene or representative frame:

1. Text is readable and not clipped.
2. Critical elements stay within safe margins.
3. Main visual does not collide with title or edges.
4. Assets load from the same path strategy used in final render.
5. UI screenshots are anonymized and proportionally correct.
6. Logos and brand marks are intentional for the current language.
7. Console has no relevant errors.
8. The changelog records checked frames and remaining risks.
