# Efeitos e acabamento (receitas testadas)

Use quando o pedido pedir efeito, dinamismo ou acabamento ("elaborado", "assertivo", "mais dinâmico", zoom, transição, flash, legenda animada, SFX, cor). Para composição com VFX, tracking, 3D ou mídia gerada, consulte [composição avançada e IA](composicao-avancada-ia.md). Todo número abaixo é ponto de partida a calibrar com o material. As receitas foram verificadas com FFmpeg 9.0.2 em fontes sintéticas (`testsrc2` e `smptebars`); o áudio não foi revisado por escuta.

## 1. Decida antes de aplicar

1. **Função.** Cada efeito responde a uma função (dar ênfase, esconder um corte, marcar uma troca de ideia, orientar a leitura). Sem função e sem âncora numa palavra ou ação, remova.
2. **Camadas no mesmo gatilho.** "Elaborado" é empilhar camadas coordenadas no mesmo instante (zoom + flash + SFX + legenda destacada), não aumentar o número de gatilhos. Os limites de [corte por fala](corte-por-fala-e-qa.md#3-elementos-na-tela-e-zoom) (efeito grande a cada ~6 s, um elemento novo a cada ~6 s) valem para o padrão; só os exceda quando o usuário pedir uma edição elaborada, e então mantenha a fala inteligível.
3. **Referência observada** (hipótese, não meta): em 10,5 s de talking head vertical: 3 punch-ins de 1,15 a 1,16, 1 jump cut com troca de escala e flash, 2 tags de palavra-chave, legendas por palavra, 5 SFX curtos e cor leve. Reduza se o efeito competir com o rosto ou a fala.
4. **Rosto primeiro.** Meça onde o rosto está no instante do efeito. Se ele sai do quadro, o zoom deve seguir o rosto (pan), nunca cortá-lo mais. Tag no topo fica sobre cabelo ou parede; legenda inferior fica sobre queixo ou camisa. Abra um quadro de cada efeito para conferir.

## 2. Imagem e movimento

**Punch-in com ênfase (zoom).** Use `scale` ×2 e `zoompan` com `d=1` e `fps` explícito; nessa cadeia o tempo de entrada é `it` (não `t`). Componha cada movimento como `delta * smoothstep(clip((it-t0)/dur,0,1))` e some os movimentos. Um punch com leve sobrepasso são dois movimentos (sobe 1,19 em 0,15 s, desce a 1,15 em 0,25 s). Ancore `x` no rosto (`(iw-iw/zoom)*fx`) e `y` perto do topo (`*0.25`).

```text
zoompan=z='1.14+(-0.14)*(u(0,0.4))':x='(iw-iw/zoom)*0.5':y='(ih-ih/zoom)*0.25':d=1:s=1080x1920:fps=60
```

`u(t0,d)` é a expansão de `clip((it-t0)/d,0,1)^2*(3-2*clip(...))`; gere a expressão por script em vez de escrevê-la à mão.

**Jump cut sem pulo.** Ao remover uma pausa, troque a escala no corte (1,15 → 1,00): a troca esconde a emenda. Para o ponto de corte e a regra de folga, veja [corte por fala](corte-por-fala-e-qa.md).

**Flash e pulso de luz.** `eq=brightness='0.16*gte(t,T)*max(0,1-(t-T)/0.14)':eval=frame`. Dura ~0,1 a 0,15 s e cai no corte ou na palavra-chave.

**Shake de impacto.** Amplie 10% e `crop` com x e y em função de `t`: `crop=540:960:x='27+18*sin(t*90)*exp(-(t-T)*8)*gte(t,T)':y='48+18*cos(t*77)*exp(-(t-T)*8)*gte(t,T)'`.

**Glitch RGB.** `rgbashift=rh=10:bh=-10:enable='between(t,T,T+0.15)'`. Use em trocas de bloco, não em fala.

**Speed ramp.** Divida em segmentos e aplique `setpts=(PTS-STARTPTS)/fator` no vídeo e `atempo=fator` no áudio (testado em 0,5× e 2×); `concat` junta. Confira a duração final (1 s normal + 1 s a 0,5× que vira 2 s + 1 s a 2× que vira 0,5 s = 3,5 s esperados; mediu 3,57 s por arredondamento de quadros). O áudio muda de tom; em fala, só use quando pedido.

**Transições `xfade`.** Duração total = d1 + d2 − duração da transição; `offset` = d1 − duração da transição (medido: 3 s + 3 s com 0,4 s = 5,6 s, vídeo e áudio com `acrossfade`). Nomes aceitos validados na versão instalada (`ffmpeg -h filter=xfade` lista 58 mais `custom`); renderizados com sucesso: `fadewhite`, `slideleft`, `smoothleft`, `wipeleft`, `zoomin`, `pixelize` (`fade` é o padrão; `fadeblack` só teve o nome conferido na lista). Escolha pela função: `fadeblack` e `fadewhite` marcam passagem de tempo ou impacto; `slide*` e `smooth*` seguem uma direção de movimento já existente na cena; `zoomin` combina com palavra-chave. Em talking head de plano único, o jump cut com troca de escala costuma bastar; transição vistosa em cada corte vira ruído.

**Fade.** `fade=t=in:d=0.3,fade=t=out:st=S:d=0.3` com `afade` equivalente; use fade de saída só quando a peça termina fora da fala.

**Reenquadrar horizontal em 9:16.** Fundo = o próprio plano ampliado (`scale=1080:1920:force_original_aspect_ratio=increase,crop=1080:1920`) com `gblur=sigma=30` e `eq=brightness=-0.15`, primeiro plano com `scale=1080:-2` e `overlay=(W-w)/2:(H-h)/2`. Compare com um recorte direto e declare a perda.

**Cor e acabamento.** Ponto de partida para material de celular já comprimido: `eq=contrast=1.07:saturation=1.08`, `curves=preset=medium_contrast` (liste os presets com `ffmpeg -h filter=curves`), `colorbalance` leve, `vignette=angle=PI/6`, nitidez `unsharp=5:5:0.45` depois da ampliação e `hqdn3d` leve antes. Grão (`noise=alls=6:allf=t`) só em look estilizado. Compare um quadro antes e depois e desfaça o que degradar pele, bordas ou texto. Não force todos os tons de pele a um ponto; consulte [composição avançada](composicao-avancada-ia.md) para gerenciamento de cor.

## 3. Texto animado

Prefira um evento ASS por palavra (palavra ativa em outra cor e escala, como em [remoção e legendas](remocao-e-legendas.md)); alternativa: `\kf` (preenchimento por varredura; `\k` troca instantâneo). Unidades (documentação das tags ASS, Aegisub, consultada em 03/11/2026): `\k`/`\kf` em centésimos de segundo; `\fad`, `\move` e os tempos de `\t` em milissegundos e relativos ao início da linha. Cores em BGR (`&H00E6FF&` = #FFE600). Tags úteis, testadas em 1 quadro: `\t(t1,t2,\fscx..\fscy..)` para pop, `\move(x1,y1,x2,y2,t1,t2)`, `\blur`, `\fad`, `\an5\pos(x,y)`.

- **Tag de palavra-chave:** `Style` com `BorderStyle=3` (caixa), pop de 0 a 120% e assentamento a 100% em ~230 ms, saída por encolhimento nos últimos ~120 ms. Texto só com palavras ditas ou fornecidas; não invente número nem selo.
- **Fonte:** copie a fonte para uma pasta ao lado do `.ass` e use `fontsdir=fonts`; `fontsdir=C:/...` quebra o parser do filtro.
- Teste 1 quadro com `-ss` depois do `-i` (veja a [referência de legendas](remocao-e-legendas.md)).

## 4. Som de efeitos (SFX)

Sintetize efeitos curtos no FFmpeg para evitar licença de terceiros; registre a origem. Medidos em 03/11/2026 antes do ganho de mixagem:

| Efeito | Fonte | Pico medido |
| --- | --- | --- |
| Hit grave | `aevalsrc='0.9*sin(2*PI*(48+70*exp(-t*30))*t)*exp(-t*8)'`, 0,5 s | −1,1 dBFS |
| Pop de tag | `aevalsrc='0.7*sin(2*PI*1100*t)*exp(-t*60)'`, 0,2 s | −3,2 dBFS |
| Whoosh | `anoisesrc=c=pink`, `highpass=f=500,lowpass=f=7000`, fade in 0,3 s e out 0,2 s | −12,2 dBFS |
| Riser | `aevalsrc='0.5*sin(2*PI*(200+1200*t*t)*t)*t'`, 1 s | −6,0 dBFS |

Mixe por último: `volume` por efeito (usados: hit 0,5 a 0,55; pop 0,35; whoosh 0,30), `adelay` no instante do gatilho (whoosh começa ~0,27 s antes do corte e o hit cai nele), `amix=normalize=0`, `alimiter`, depois `loudnorm`. Voz primeiro: se o efeito mascarar uma sílaba, baixe ou desloque. Ducking de música: [exportação e verificação](exportacao-e-verificacao.md).

## 5. Segurança, acessibilidade e armadilhas

- **Flashes.** WCAG 2.3.1 (W3C, nível A, consultado em 03/11/2026): no máximo três flashes por segundo, ou abaixo do limiar de flash geral (variação ≥10% da luminância máxima, estado escuro abaixo de 80%) e de flash vermelho. Se aplica a conteúdo web; adote como boa prática em vídeo social. Um flash por segundo no máximo, sem vermelho saturado piscando; evite flash de tela cheia repetido.
- **`-filter_complex_script`** foi rejeitado no FFmpeg 9.0.2 ("Unrecognized option"); use `-/filter_complex arquivo.txt`.
- **`scale` com `eval=frame` e tamanho variável:** um teste simples (`scale`+`crop`) rodou, mas a cadeia completa (concat, hqdn3d, scale, crop, unsharp, eq, vignette, subtitles) terminou com falha de segmentação (código 139) e arquivo vazio; a causa não foi isolada. Use ampliação fixa + `zoompan`.
- **Custo de render:** ampliar a 2160×3840 antes do `zoompan` rodou a ~0,04 a 0,05× em tempo real (10,5 s ≈ 4 min em 1080×1920 a 60 fps). Avise, rode em segundo plano e gere antes uma prévia em 540×960 ou com `-preset veryfast`.
- **Fonte de baixa resolução** (480 px, WhatsApp): ampliação e punch-ins perdem nitidez; declare e não prometa HD.

## 6. Verificação dos efeitos

1. Abra um quadro em cada gatilho (zoom no pico, flash, tag, legenda) e outro 0,1 s depois de cada corte.
2. Confira rosto, legenda e tag dentro do quadro e fora de UI e rosto.
3. Conte flashes por segundo e confira o pico dos SFX contra a voz (medição não substitui audição; declare o que não foi ouvido).
4. Relate o orçamento usado (quantos zooms, flashes, tags e SFX) e o que foi simplificado.
