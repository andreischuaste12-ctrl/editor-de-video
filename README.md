# Video Editor

## About

An instruction skill for AI assistants that edits existing audiovisual material and delivers a verified final video. It covers cutting, assembly, captions, audio mixing, corrections, re-exporting, and effects for 16:9, 9:16, and 1:1 formats. The skill identifier is `video-editor-konvert`.

The skill guides an assistant; it is not a video editor or standalone application. Operations run through tools available in the environment, primarily FFmpeg and FFprobe, with optional composition tools when a project needs motion graphics or HTML-based animation.

## Capabilities

- **Protects source files.** Records path, size, modification date, and SHA-256 before editing, works from a copy, and always writes to a new output file.
- **Inspects before editing.** Uses FFprobe to check duration, resolution, frame rate, audio streams, and orientation.
- **Edits with intent.** Covers speech-based cutting, pacing, J/L-cuts, audio ducking, EBU R128 loudness normalization, transitions, and vertical reframing.
- **Handles burned-in text and captions.** Includes transcription workflows, ASS captions, and careful removal or replacement of burned-in graphics.
- **Applies controlled effects.** Covers emphasis zooms, speed ramps, flashes, glitches, SFX, and color treatment with an effect budget and verification at each trigger.
- **Verifies exports.** Checks technical properties, sampled frames, integrated loudness, true peak, caption readability, and any review limitations.

Creating a concept or script without source media and writing advertising copy are outside the scope of this project.

## Repository structure

| Path | Contents |
|---|---|
| `SKILL.md` | Core instructions and routing to detailed references |
| `references/export-and-verification.md` | Export defaults, loudness, FFmpeg recipes, and delivery checks |
| `references/speech-editing-and-qa.md` | Speech cuts, pacing, SFX, and dialogue QA |
| `references/removal-and-captions.md` | Burned-in text removal, transcription, and captions |
| `references/effects-and-finishing.md` | Effect recipes and visual verification |
| `references/advanced-composition-and-ai.md` | Motion design, VFX, and generated media as source material |
| `references/references.md` | Technical and creative research sources |
| `agents/export-reviewer.md` | Reusable independent export-review prompt |
| `evals/` | Scenario specification and synthetic fixtures |

## Requirements

- FFmpeg and FFprobe available on `PATH`. The recipes were originally checked with FFmpeg 9.0.2; confirm options against the installed version.
- Optional composition software for animated titles, synchronized captions, or HTML-based scenes.
- Explicit user authorization before downloading files, installing packages, sending media to external services, or using a browser session.

## Installation

The repository root is the skill directory. Copy the relevant files into your assistant's skill directory:

```bash
mkdir -p ~/.local/share/ai-skills/video-editor-konvert
cp -r SKILL.md references agents ~/.local/share/ai-skills/video-editor-konvert/
```

Adjust the destination to the conventions of your assistant or runtime. The `evals/` directory is optional and intended for evaluation work. Installing the files does not prove that a model loaded or followed the instructions.

## Evaluation

`evals/evals.json` is a scenario specification rather than an automated behavioral test suite. Commands verified against synthetic fixtures are documented in `execution_notes`. The fixtures contain no real people, brands, or personal data; see `evals/fixtures/README.md`.

## Contributing

Issues and pull requests are welcome. Read [CONTRIBUTING.md](CONTRIBUTING.md) before submitting changes. Do not publish client media, credentials, personal data, or local user paths in examples, logs, or fixtures.

## License

Copyright 2026 andreischuaste12-ctrl. Licensed under the [Apache License 2.0](LICENSE); attribution information is in [NOTICE](NOTICE).

Third-party works, tools, and trademarks referenced by this project remain the property of their respective owners. A reference does not relicense a third-party work or imply endorsement.
