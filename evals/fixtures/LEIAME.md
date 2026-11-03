# Fixtures sintéticas

Arquivos pequenos e sem pessoas, marcas ou dados reais, usados pelos cenários de `evals.json`. Não os edite no lugar: copie antes e registre o SHA-256.

`texto-queimado-textura-sintetico.mp4` (360×640, 2 s, 25 fps, áudio AAC mono) foi gerado em 03/11/2026 com FFmpeg 9.0.2 para o cenário 15: texto branco com contorno sobre o padrão `testsrc2`, sem caixa de fundo, de modo que o `delogo` deixa uma faixa borrada sobre as listras.

```sh
ffmpeg -f lavfi -i "testsrc2=size=360x640:rate=25:duration=2" -f lavfi -i "sine=frequency=440:sample_rate=44100:duration=2" -vf "drawtext=fontfile='C\:/Windows/Fonts/arial.ttf':text='TEXTO QUEIMADO':x=(w-text_w)/2:y=500:fontsize=34:fontcolor=white:borderw=3:bordercolor=black" -c:v libx264 -pix_fmt yuv420p -crf 20 -preset medium -c:a aac -ac 1 -shortest texto-queimado-textura-sintetico.mp4
```

O caminho da fonte é do Windows; em outro sistema, aponte `fontfile` para uma fonte local. Como a codificação varia por versão do FFmpeg, o hash pode diferir ao regenerar.

As demais fixtures (`padrao-sintetico.mp4`, `silencioso-sintetico.mp4`, `texto-queimado-sintetico.mp4`, `invalido.mp4`) foram adicionadas por outra edição, e a forma de geração delas não está registrada.
