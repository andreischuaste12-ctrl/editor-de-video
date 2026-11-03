# Synthetic fixtures

These small files contain no people, brands, or personal data. They support the scenarios in `evals.json`. Never edit a fixture in place; copy it first and record its SHA-256.

`burned-in-text-texture-synthetic.mp4` is a 360×640, 2-second, 25 fps file with mono AAC audio. It contains outlined white text over a `testsrc2` pattern, so the `delogo` filter leaves a visible blurred band across the texture.

```sh
ffmpeg -f lavfi -i "testsrc2=size=360x640:rate=25:duration=2" -f lavfi -i "sine=frequency=440:sample_rate=44100:duration=2" -vf "drawtext=fontfile=/path/to/font.ttf:text='BURNED IN TEXT':x=(w-text_w)/2:y=500:fontsize=34:fontcolor=white:borderw=3:bordercolor=black" -c:v libx264 -pix_fmt yuv420p -crf 20 -preset medium -c:a aac -ac 1 -shortest burned-in-text-texture-synthetic.mp4
```

Point `fontfile` to a locally available font. Encoder output may differ between FFmpeg versions, so regenerated hashes may not match.

The exact generation commands for the other fixtures were not preserved. Their contents are synthetic and should be replaced only by reproducible synthetic equivalents.
