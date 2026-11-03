# Effects and finishing

Use this reference when the request calls for dynamic or elaborate finishing: emphasis zooms, transitions, flashes, animated captions, SFX, or color treatment. For tracking, 3D, VFX, or generated media, use [advanced composition and AI](advanced-composition-and-ai.md). Every value is a starting point. The recipes were checked with FFmpeg 9.0.2 against synthetic `testsrc2` and `smptebars` sources; audio was not reviewed by listening.

## 1. Effect plan and budget

1. **Define a function.** Each effect should emphasize a word, hide an edit, separate a section, illustrate an idea, or support a sound cue. Remove effects with no clear function.
2. **Coordinate layers at one trigger.** A deliberate stack such as zoom, flash, SFX, and highlighted caption at the same moment usually reads better than unrelated triggers spread throughout the piece.
3. **Set the budget before rendering.** List each trigger, effect layers, duration, and intended purpose. Check that the result preserves speech, faces, products, and essential text.
4. **Prototype first.** Render a representative short section or a single frame before applying expensive processing to the full timeline.

## 2. Transitions and cut masking

**Hard cut:** the default when continuity and pacing already work.

**Crossfade:** use `xfade` for video and `acrossfade` for audio only after normalizing resolution, frame rate, pixel format, time base, sample rate, and stream duration. Calculate the offset from the first video's duration minus the transition duration.

**Directional transition:** use a slide or wipe only when screen direction supports it. Verify that important content does not cross the transition edge awkwardly.

**White flash:** keep it short and purposeful. Do not use saturated red flashing. Follow the accessibility guidance in [references](references.md).

**Jump-cut masking:** a small scale or framing change at the edit can make a removed pause feel intentional. Preserve face framing and avoid accumulating zoom until the image becomes soft.

**Speed ramp:** use `setpts` for video and `atempo` stages for audio when sound must remain attached. Confirm final duration and synchronization because rounding and variable-frame-rate sources can shift boundaries.

## 3. Motion effects

**Emphasis zoom:** animate scale and position around the visual point of interest. Increase working resolution before `zoompan` when necessary to avoid pixel stepping, then downscale once. Check that the crop never cuts off a face, caption, or product.

**Shake:** use restrained translations or rotations around an impact. Pad or overscan before movement so the frame edge never becomes visible.

**RGB glitch:** offset color channels for only a few frames and keep text and faces readable. Avoid repeated high-contrast flashes.

**Flash:** apply a short exposure or blend change around a defined trigger. Open frames before, during, and after the effect to verify continuity.

**Color and finish:** on already compressed mobile material, start with restrained contrast, saturation, denoising, and sharpening. Apply denoising before enlargement and sharpening after it. Compare a representative frame before and after and undo changes that damage skin, text, gradients, or edges. For color management, use [advanced composition and AI](advanced-composition-and-ai.md).

## 4. Animated captions

Prefer one ASS event per word when word-level timing is reliable. An alternative is `\kf` for sweep fill or `\k` for an instant change. In ASS, `\k` and `\kf` use centiseconds; `\fad`, `\move`, and `\t` times use milliseconds relative to the line start. ASS colors use BGR order.

- Use scale animation for a restrained word pop.
- Use movement only when it improves hierarchy and does not compete with the speaker.
- Keep blur and shadow modest enough to preserve edge clarity.
- Render one test frame at a known caption timestamp before the full export. See [removal and captions](removal-and-captions.md).

## 5. Sound effects and mix

Build or use SFX only when their license and origin are known. Align a whoosh with visible movement and an impact with the visual landing point. Set effect gain independently, delay each sound to its trigger, mix with `amix=normalize=0`, limit peaks, and perform loudness normalization only after the mix is complete.

Keep dialogue dominant. If an effect masks a syllable, lower it or move it. For music ducking, use [export and verification](export-and-verification.md).

## 6. Effect verification

Before delivery:

1. List every effect trigger and extract a frame before, during, and after it.
2. Open the images and verify crop, face integrity, caption readability, layer order, and exposed edges.
3. Check transition and speed-ramp duration against the planned timeline.
4. Confirm audio and video end together and inspect peaks around SFX.
5. Count flashes and avoid rapid high-contrast repetition.
6. Compare color before and after on skin, text, gradients, and known neutral areas.
7. Report whether the video was watched fully, sampled visually, measured, and listened to. Do not imply a check that did not occur.

Fix defects before delivery. A visible edge, black strip, broken caption, masked syllable, or unplanned duration change is a problem to correct rather than merely disclose.
