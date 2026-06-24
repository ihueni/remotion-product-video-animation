# HTML And Remotion Reference

## 1. HTML As Visual Source

Once HTML is approved, treat it as visual source code. Remotion should reuse the same DOM, CSS, assets, layout, text, and timing whenever feasible.

This does not mean “look at the HTML and redraw it.” It means preserve the source structure unless technical constraints require a documented substitution.

## 2. One Scene At A Time

Create or revise storyboard HTML scene by scene. Avoid building a single full-video preview page first and splitting it later.

Before creating a scene:

1. Confirm scene number, duration, title, prior scene exit state, and next scene entry needs.
2. Read the project changelog for earlier scene decisions.
3. Compare nearby scenes for shared spacing, colors, typography, UI chrome, assets, and motion style.
4. Generate only the current scene file.
5. Preview opening, key action, stable, and ending frames.
6. Record continuity and verification in the changelog.

## 3. Remotion Implementation Priority

Use this order:

1. Embed the approved HTML in Remotion, often through an iframe.
2. If iframe is unstable, port the HTML DOM and CSS closely into React.
3. Replace only unsupported local parts, such as videos, Lottie, Canvas, fonts, or cross-origin assets.
4. Fully rewrite the scene only after documenting why reuse cannot work.

Do not add new shells, cards, labels, diagrams, or conclusions that are absent from the approved HTML unless the user asks to revise the visual source.

## 4. Remotion-Stage Editing Boundary

After a project enters Remotion preview, composition, frame extraction, or rendering, consider source storyboard HTML frozen unless the user asks to return to source HTML.

At this phase, fix video-side issues in:

1. `remotion/src/`.
2. `remotion/public/storyboard/` or equivalent copied storyboard assets.
3. Audio files and BGM.
4. Render configuration and output handling.

If source HTML and Remotion-side HTML intentionally diverge, record why.

## 5. Approval And Cleanup

When the user says an HTML scene is acceptable, ask whether it should become the Remotion baseline. Before converting it, remove misleading leftovers:

1. Unused DOM.
2. Unused CSS.
3. Old variables and copy.
4. Dead timeline references.
5. Elements the user explicitly rejected.

After cleanup, recheck the visible frames to ensure nothing changed.

## 6. Conversion Checklist

Before implementing Remotion, list:

1. DOM structure.
2. CSS dimensions and positioning.
3. Asset paths.
4. Timeline labels or time points.
5. Visibility rules.
6. Stable frames and key motion frames.

After implementation, compare Remotion stills against HTML reference frames. Check that the result is not blank, assets load, preview controls are hidden, and key motion states match.
