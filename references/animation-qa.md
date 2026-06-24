# Animation QA Reference

## 1. Frame-Driven HTML

HTML used in Remotion must be seekable by frame. Given `frame` and `fps`, the page must render the same state without relying on elapsed wall-clock time.

Expose a seek interface such as:

```js
window.__seekToFrame = (frame, fps) => {
  const seconds = frame / fps;
  // derive every visible state from frame or seconds
};
```

Avoid final-render dependence on:

1. CSS `animation` or `transition` self-play.
2. `setTimeout`, `setInterval`, or unconstrained `requestAnimationFrame`.
3. Natural `<video>` playback time.
4. Lottie autoplay.
5. GSAP autoplay.
6. Random values, system time, page load time, or async ordering.

## 2. GSAP To Remotion

Preview HTML may use GSAP for timing exploration, but final Remotion behavior must be controlled by `useCurrentFrame()`, `interpolate()`, SVG/CSS/Canvas logic, or a paused timeline explicitly seeked from the frame.

Use readable timeline labels in preview:

| Preview concept | Remotion translation |
|-----------------|----------------------|
| `timeline` seconds | Frame constants |
| `label += 0.3` | Start frame plus offset |
| `stagger: 0.2` | Per-item frame offset |
| DrawSVG or path reveal | `strokeDasharray`, `strokeDashoffset`, mask, or clip-path |

## 3. SVG Path Reveal

Paths, dotted lines, data flows, tracks, arrows, glow trails, and chart lines often fail through early endpoints, visible base paths, subpixel breaks, or full-path flashes.

Implementation rules:

1. Hide paths, endpoints, arrows, and dots before their start frame.
2. Gate visibility separately from drawing progress.
3. If `strokeDashoffset` causes breaks on curves or thin paths, use a mask or clip-path to reveal a complete path.
4. Keep base path, active path, endpoint, and label on the same progress model.
5. Show moving dots or arrows after the relevant path has started, not before.

QA rules:

1. Check `5-10` frames before path start.
2. Check the start frame, mid reveal, and end frame.
3. For early-reveal bugs, consider a pixel check in the target color area.
4. Record the checked frames in the changelog.

## 4. Continuous UI Scenes

If adjacent scenes share a sidebar, header, browser frame, dashboard shell, or other fixed UI, treat them as one continuous interface.

Rules:

1. Keep shared UI coordinates, width, height, logo size, type scale, and active state consistent.
2. Prefer hard cuts between continuous UI scenes.
3. Use premounting for iframe-based scenes when needed to avoid white flashes or missing assets.
4. Do not use cross-fades to hide mismatched shared UI.
5. Check boundary frame minus one, boundary frame, and boundary frame plus one.

## 5. Spacing And Safe Margins

For 16:9 UI videos, define a practical safe area. At a `1280x720` design size, keep critical text, logos, buttons, status labels, and UI cards outside:

1. Top: `72px`.
2. Sides: `24px`.
3. Bottom: `24px`.

When users report that a scene is too high, too close to a title, too close to an edge, or visually unbalanced, check bounding boxes for the title, primary visual, key cards, and safe area before adjusting. Maintain visible breathing room between title and primary visual.
