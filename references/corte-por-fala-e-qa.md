# Corte por fala, ritmo, SFX e QA de fala

Use em vídeos com pessoa falando (talking head, depoimento, Reels). Valores são pontos de partida: confira medindo o áudio e revisando quadros.

## 1. Plano aprovado antes de cortar

1. Leia o material: duração, resolução, fps, pausas, repetições, tropeços, ruído. Resuma em 3 a 5 linhas.
2. Proponha o plano em 4 a 8 frases: objetivo, duração-alvo, o que será cortado, legendas, elementos na tela, som. Termine com **uma** pergunta e espere o "pode".
3. Para renders longos, gere antes uma **prévia leve** (por exemplo, 360p, `-preset ultrafast`) para aprovação do corte. Só então faça o render final.
4. Cada pedido de ajuste é uma revisão do mesmo projeto: altere só o pedido e salve com sufixo de versão (`_v2`, `_v3`). Mantenha um `projeto.md` curto com decisões, suposições e revisões.

Se o pedido já traz a ordem de cortes, aplique-a sem repetir o gate; informe a suposição.

## 2. Regras de corte

- **Nunca corte uma palavra ao meio.** Corte em pausas medidas. Se um corte cair em fala corrida, mova a borda ou mantenha a palavra: uma hesitação sobrando é preferível a uma sílaba cortada.
- Ajuste pausas e bordas de corte à fala, preservando respiração, sentido e contexto. Compare as tomadas antes de escolher uma repetição.
- Preserve o encerramento pertinente e evite alterar a velocidade da fala sem pedido.
- **Nome de terceiros sem autorização:** ofereça bipe/corte quando o usuário pedir.

## 3. Elementos na tela e zoom

- Cada elemento (selo, imagem, número) entra na **palavra falada** a que se refere, não em tempo estimado. Sem âncora na fala, remova.
- Ajuste a duração dos elementos e a intensidade dos efeitos à leitura e à intenção da cena. Para edição elaborada, consulte [efeitos e acabamento](efeitos-e-acabamento.md).
- Não use zoom em planos com picture-in-picture ou tela dividida.
- Nunca invente números, prints, depoimentos ou logotipos. Use só o que o usuário forneceu; falta de imagem vira pergunta.
- Alinhamento de camadas no FFmpeg: use `tpad` e `overlay` com `eof_action=pass`; evite `enable='between(...)'` para sincronizar entradas (problemas de framesync).

## 4. Som

- Voz primeiro: limpeza leve (`highpass`, `afftdn`), compressão suave e EQ de presença só se o material pedir; meça antes e depois.
- **Sem música por padrão.** Se o usuário fornecer a faixa, use `sidechaincompress` (ver [exportação e verificação](exportacao-e-verificacao.md)). Use apenas sons e músicas com licença conhecida; registre a origem.
- **SFX:** níveis abaixo da voz (referência: ganho ≤ 0,55 em escala linear), `amix=normalize=0` e `alimiter=limit=0.95`. Efeitos de passagem entram junto da troca de plano; efeitos de impacto, quando o elemento "pousa". Densidade baixa: não coloque som em toda ação.
- **Mixe os efeitos por último, copiando o vídeo** (`-c:v copy`): ajustar níveis leva segundos e não rerrenderiza imagem.
- Meça pico e sonoridade depois do limitador (ver exportação). Declare o alvo adotado (-14 LUFS é referência de partida, não regra de plataforma).

## 5. QA de fala e de cortes

Execute antes de entregar e relate o que fez e o que não fez.

1. **Quadros:** uma folha de contato com 1 quadro a cada ~1,5 s e os quadros 0,08 a 0,12 s depois de cada corte (até ~40). Abra e confira enquadramento, legenda, elementos e artefatos. Não existe aprovação automática: o parecer é seu, a partir do que viu.
2. **Re-transcrição do arquivo final:** transcreva o resultado (ver [remoção de texto e legendas](remocao-e-legendas.md)), normalize (minúsculas, sem acento e pontuação) e compare com o texto esperado (`difflib`). Reporte o percentual de concordância e as divergências. Palavra faltando junto a um corte indica sílaba cortada; corrija antes de entregar. Trate o percentual como indício, não como aprovação; confirme com audição quando possível.
3. **Falha silenciosa invalida o QA.** Se a transcrição, a folha de contato ou a medição não foram geradas, diga que a verificação não ocorreu.
4. **Sonoridade e pico** conforme o alvo declarado; use o mesmo limite no relatório e no comando (um relatório que diz -1 dBTP e testa -0,5 dBFS é inconsistente).
5. **Cortes duplicados:** confira trechos adjacentes que reutilizam o mesmo áudio (sílaba repetida) e cortes dentro de fala.
