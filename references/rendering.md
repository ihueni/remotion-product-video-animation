# Rendering Reference

## 1. Audio Source

This skill is audio-provider neutral. Do not require a specific TTS vendor, voice platform, DAW, stock music service, or API. The user or project may already use a paid provider or an internal recording workflow.

When audio is missing or needs replacement, offer options rather than mandates:

1. Use project-provided voiceover or studio recordings.
2. Use any TTS API or voice platform the user has access to.
3. Use a local recording edited externally.
4. Use licensed BGM or AI-generated BGM if the project permits it.

Default recommendations, when the user has no existing provider:

| Need | Suggested starting point | Reason |
|------|--------------------------|--------|
| Chinese narration | Fish Audio, preferably through an API-capable workflow | Often a strong default for Mandarin / Chinese narration, and API usage makes repeated generation easier to reproduce. |
| English narration | ElevenLabs, preferably through an API-capable workflow | Often a strong default for English narration, and API usage makes repeated generation easier to reproduce. |
| Repeated or automated production | A paid plan with API access, sufficient credits, and commercial-use terms | API access makes regeneration, batching, retries, and version control much easier than manual web exports. |

Before recommending a purchase, ask whether the user already has a membership, team plan, API key, or required vendor. Pricing, quotas, commercial-use terms, and API availability change over time, so check the provider's current official plan page before making budget-sensitive advice.

Only enforce the production requirements after audio exists: file availability, duration, loudness sanity, sample rate compatibility, timing sync, licensing awareness, and final render verification.

## 2. Audio Timing

Scene duration should follow actual voiceover duration. After voiceover is generated or replaced:

1. Probe every voiceover file duration.
2. Recalculate scene frame counts.
3. Update total frame count and transition overlap.
4. Copy or sync public audio files used by Remotion.
5. Recheck that comments and constants match the current timing.

## 3. BGM And Mixing

When using `ffmpeg amix`, set `normalize=0` unless there is a deliberate reason not to. This avoids unexpected gain changes during voice/BGM mixing.

Prefer final fade handling in the video or audio track logic instead of baking long fades into reusable BGM source clips.

If AI-generated BGM is needed, do not name or require a specific music generation product. Build the music prompt from the video script and scene plan:

1. Extract the video topic, audience, emotional arc, and pacing from `script.md` or the project brief.
2. Identify where the music should support the edit: opening hook, explanation body, transition lift, final resolve, or low-key background bed.
3. Specify musical traits rather than vendor features: mood, genre, tempo range, instrumentation, density, loopability, and whether vocals should be avoided.
4. Add negative constraints for narration-led videos: no lead vocal, no busy melody, no sudden drops under speech, no heavy bass masking voice, no long silence.
5. Ask for a version long enough for the full video plus edit margin, or plan to loop / trim cleanly.

Reusable BGM prompt shape:

```text
Create background music for a narration-led {video_type} video.
Audience: {audience}.
Mood arc: {opening_mood} -> {middle_mood} -> {ending_mood}.
Tempo: {bpm_range}; energy: {low/medium/high but voice-friendly}.
Instrumentation: {instruments or texture}.
Structure: subtle intro, steady middle bed, gentle lift near the ending.
Avoid: vocals, busy lead melody, abrupt drops, harsh percussion, low-end masking speech.
Length target: {duration + margin}.
```

## 4. Final Render Quality

For UI-heavy videos, render with high enough resolution and bitrate that text, logos, and thin lines remain sharp.

Example baseline:

```bash
remotion render src/index.ts <CompositionName> output/debug/video-raw.mp4 \
  --concurrency=1 \
  --gl=angle \
  --codec=h264 \
  --scale=2 \
  --video-bitrate=18M \
  --audio-bitrate=320K
```

If the composition design size is `1280x720`, `--scale=2` outputs `2560x1440`. Treat lower-resolution renders as preview unless accepted by the user.

Do not use `--crf` and `--video-bitrate` together.

## 5. PNG Intermediate Frames

For videos dominated by UI, text, logos, or thin lines, configure Remotion for PNG intermediate frames:

```ts
Config.setVideoImageFormat("png");
Config.setChromiumOpenGlRenderer("angle");
```

## 6. Audio And Container Sync

After render, use `ffprobe` to compare video stream, audio stream, and container duration:

```bash
ffprobe -v error -select_streams v:0 \
  -show_entries stream=width,height,duration,bit_rate \
  -of default=nw=1 output/final.mp4

ffprobe -v error -select_streams a:0 \
  -show_entries stream=duration,bit_rate,sample_rate,channels \
  -of default=nw=1 output/final.mp4

ffprobe -v error \
  -show_entries format=duration,size,bit_rate \
  -of default=nw=1 output/final.mp4
```

If audio runs longer than video, trim to video duration and repackage:

```bash
DURATION=$(ffprobe -v error -select_streams v:0 \
  -show_entries stream=duration \
  -of default=nw=1:nk=1 output/debug/video-raw.mp4)

ffmpeg -y -i output/debug/video-raw.mp4 -t "$DURATION" \
  -map 0:v:0 -map 0:a:0 \
  -c:v copy -c:a aac -b:a 320k \
  -af "atrim=0:$DURATION,asetpts=N/SR/TB" \
  -movflags +faststart output/final.mp4
```

## 7. Cover Frame

Before final delivery, extract representative frames from complete scenes and ask the user to choose a cover. Store candidates in `output/screencut/`; store the confirmed cover in `output/cover/`.

Example extraction:

```bash
ffmpeg -y -ss <seconds> -i output/final.mp4 \
  -vframes 1 -update 1 -q:v 2 output/screencut/cover-scene-03.png
```

Embed a confirmed cover without re-encoding the main video:

```bash
ffmpeg -y \
  -i output/final.mp4 \
  -i output/cover/cover.png \
  -map 0 -map 1 \
  -c copy -c:v:1 png \
  -disposition:v:1 attached_pic \
  output/final-with-cover.mp4
```
