# Burned-in text removal, transcription, and captions

Use this reference when a request involves removing text, captions, or a watermark from the image, or adding captions to speech. Every number is a starting point to calibrate against the source. Confirm filter options with the installed FFmpeg version.

## 1. Remove burned-in text or a watermark

Treat burned-in text as image content. There is no lossless universal removal method.

1. Ask for a clean source first. A clean recording is usually the highest-quality solution.
2. Measure the text box. Extract frames from every affected section and record x, y, width, and height. Check whether the box moves and whether important content crosses behind it.
3. Choose a method based on the underlying image:
   - **Simple, flat area:** `delogo` may work; test a single frame first.
   - **Texture or repeating pattern:** `delogo` often leaves smearing or bands; avoid it when the test frame exposes artifacts.
   - **Text at an edge:** crop and deliberately reframe.
   - **Text over a moving central subject:** without suitable video inpainting, do not promise a clean removal. Offer a crop, an approved design overlay, or a clean source.
4. When cropping, do not deliver a large empty or black strip without an intentional design decision. Compare representative alternatives such as zoom, reframing, and a filled background.
5. State the loss. Report how much of the image was cropped or changed and provide a before-and-after frame.

Do not download an inpainting model or install a large library without explicit authorization.

## 2. Transcription

Use this order. Verify text against the audio when listening is available. Otherwise disclose that the transcript was not checked by ear and request human confirmation when accuracy is important.

1. Prefer an approved script, transcript, or SRT supplied by the user.
2. Check for an existing local tool such as `faster_whisper`, a `whisper` executable, or an FFmpeg whisper filter with a local model.
3. Before installing or downloading anything, request authorization and state the package, approximate model size, and destination. Use a virtual environment and pin versions when possible.
4. Process locally by default. Never send audio or video to an external transcription service without explicit authorization.
5. Prepare audio as mono, 16 kHz WAV with `-vn -ac 1 -ar 16000`.
6. Set the language explicitly when known, request word timestamps, and record the model and version used. Select model size and compute type according to the available hardware and accuracy requirements.
7. If the transcription library cannot open the media because of a decoding-library mismatch, decode to WAV first and pass normalized samples through a supported interface.
8. Do not swallow exceptions. Record engine changes and failures. An empty or missing transcript invalidates QA that depends on it.
9. Review text and timing before burning captions. Flag proper names, brands, and unfamiliar terms. Ask the user when a material ambiguity cannot be resolved and record glossary corrections.

## 3. Burned-in captions

Starting principles for vertical speech video:

- Break text into readable groups that respect pauses, sentence boundaries, and cuts.
- Highlight the active word only when the timing is reliable.
- Fade only across a perceptible pause; use immediate changes between continuous groups.
- Use a heavy readable font, sufficient outline, and restrained shadow. Confirm font licensing.
- Place captions in a lower central region with margins suited to the destination, away from faces and platform interface elements.
- Render captions above other overlays.
- Keep additional badges or tags sparse enough to preserve reading.

### Render with FFmpeg

1. Generate an ASS file. For word highlighting, use one event per word or carefully timed override tags.
2. Confirm the build supports `subtitles` or `ass` with `ffmpeg -hide_banner -filters`.
3. Run FFmpeg from the ASS directory and use a relative filename to reduce path-escaping problems.
4. Render and open one test frame at a timestamp where captions should be visible before processing the full video. With `-ss` before `-i`, timestamp behavior can make the caption appear missing; test `-ss` after `-i` or use a verified `-copyts` workflow.
5. If libass is unavailable, use an available composition tool or rasterized caption images. If neither is available, deliver a separate SRT and disclose that captions were not burned in.

Keep SRT, ASS, and glossary files with the project for reuse.

## 4. Upscaling and general improvement

- Upscale only to meet delivery requirements. Use a suitable scaler, light denoising before enlargement, and mild sharpening afterward. State that upscaling does not restore missing detail.
- Apply brightness, contrast, saturation, denoising, sharpening, audio cleanup, loudness normalization, and reframing only when inspection supports them. Compare before and after and undo damage to skin, text, or edges.
- Use less processing on heavily compressed messaging-app or reposted material because aggressive filtering worsens blocks and halos.

## 5. Safety boundaries

- Downloading files, installing packages, using a large model, or sending media to an external service requires explicit authorization that identifies the source, approximate size, and destination.
- Use a virtual environment and pinned versions when possible. Do not run remote installers without reviewing them.
- Download a video from a link only when requested. Never use browser cookies, an authenticated session, or the user's credentials. If a link requires login, request the file or screenshots.
- Never overwrite source files. Write a new output, outside the source directory by default unless the user chooses another destination, and record hashes as described in [export and verification](export-and-verification.md).
