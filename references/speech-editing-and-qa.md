# Speech editing, pacing, SFX, and dialogue QA

Use this reference for talking-head videos, testimonials, and short-form speech. All values are starting points; measure the audio and inspect frames before applying them.

## 1. Plan before cutting

1. Inspect duration, resolution, frame rate, pauses, repetitions, stumbles, and noise. Summarize the material in three to five lines.
2. Propose a four-to-eight-sentence plan covering the objective, target duration, removed material, captions, on-screen elements, and sound. End with one necessary question and wait for approval.
3. Before a long final render, create a lightweight preview such as 360p with `-preset ultrafast` for cut approval.
4. Treat each adjustment request as a revision of the same project. Change only what was requested, use a version suffix such as `_v2`, and keep a short `project.md` with decisions, assumptions, and revisions.

If the request already defines the cut order, apply it without repeating the approval gate and disclose any necessary assumption.

## 2. Cut rules

- Never cut through a word. Place cuts in measured pauses. When speech is continuous, move the boundary or retain the word.
- Adjust pauses and cut handles to the delivery, preserving breathing, meaning, and context. Compare takes before selecting a repetition.
- Preserve a relevant closing and do not speed up speech without a request.
- When a third party's name must be removed, offer a cut or bleep only when requested.

## 3. On-screen elements and zoom

- Align each element with the spoken word or idea it supports. Remove elements that have no dialogue anchor.
- Match element duration and effect intensity to readability and scene intent. For an elaborate edit, use [effects and finishing](effects-and-finishing.md).
- Do not use emphasis zooms on picture-in-picture or split-screen layouts unless the composition has been designed for them.
- Never invent numbers, screenshots, testimonials, or logos. Use supplied and authorized material only.
- For FFmpeg layer alignment, use `tpad` and `overlay` with `eof_action=pass`; test framesync behavior before relying on `enable='between(...)'`.

## 4. Sound

- Put voice first. Apply light high-pass filtering, noise reduction, compression, or presence EQ only when the diagnosis supports it, and measure before and after.
- Do not add music by default. If the user supplies a licensed track, use `sidechaincompress` as described in [export and verification](export-and-verification.md). Record the source and license.
- Keep SFX below dialogue. Use `amix=normalize=0` and a limiter, then measure the final mix. Place whooshes with movement and impacts at the visual landing point.
- Mix effects last while copying the finished video with `-c:v copy` when possible, so level adjustments do not re-render the image.
- Measure peak and loudness after limiting. Treat any loudness target as a declared delivery assumption, not a universal platform rule.

## 5. Dialogue and cut QA

Perform these checks before delivery and report which ones actually ran.

1. **Frames:** generate a contact sheet at representative intervals and frames shortly after every cut. Inspect framing, captions, overlays, and artifacts.
2. **Final-file transcription:** transcribe the export using [removal and captions](removal-and-captions.md), normalize case, accents, and punctuation for comparison, and compare it with the expected text. Treat the similarity percentage as evidence, not automatic approval. Confirm by listening when possible.
3. **No silent failures:** if transcription, contact-sheet generation, or measurement fails, report that the check did not occur.
4. **Loudness and peak:** use the same declared thresholds in the command and final report.
5. **Duplicate cuts:** inspect adjacent sections for repeated audio, missing syllables, or a cut placed inside speech.
