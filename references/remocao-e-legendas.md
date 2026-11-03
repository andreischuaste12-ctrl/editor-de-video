# Remoção de texto queimado, transcrição e legendas

Use quando o pedido for tirar escritas, legendas ou marcas d'água da imagem, ou legendar uma fala. Todo número abaixo é ponto de partida a calibrar com o material; confirme as opções do FFmpeg instalado com `ffmpeg -h filter=<nome>`.

## 1. Remover texto queimado ou marca d'água

Trate o texto como parte da imagem: não existe remoção "grátis". Siga a ordem e pare no primeiro caminho que atenda.

1. **Peça o original sem o texto.** Stories, Reels e vídeos de WhatsApp costumam vir de uma gravação limpa. Se o usuário tiver, é o melhor resultado e dispensa o resto.
2. **Meça a caixa do texto.** Extraia um quadro de cada trecho (`-ss`), abra e anote x, y, largura e altura. Confira se a posição é fixa ou muda entre trechos e se algo importante (pessoas, pés, produto) passa por baixo da caixa durante o vídeo.
3. **Escolha o método pelo que fica sob a caixa:**
   - **Área simples e lisa** (parede, céu): `delogo` costuma servir. Teste em 1 quadro antes.
   - **Área com textura ou padrão** (piso, grama, tecido): `delogo` deixa faixas e borrões; foi observado com um piso emborrachado, onde gerou listras verticais. Não use.
   - **Texto na borda do quadro:** corte (`crop`) a faixa e reenquadre.
   - **Texto no meio de uma cena com movimento:** sem ferramenta de inpainting por vídeo disponível, não prometa remoção limpa. Ofereça cortar, cobrir com elemento de design aprovado ou pedir o original.
4. **Se cortar, não entregue uma faixa preta grande sem decisão.** Preencher o espaço com fundo desfocado e escurecido de 30% da altura foi aceitável como solução provisória, mas parece sobra. Antes de entregar, compare 3 alternativas em um quadro cada (zoom, reenquadramento 4:5, fundo preenchido) e recomende uma. Cortar altura que contenha pessoas ou ação é decisão do usuário.
5. **Declare a perda.** Diga quanto da imagem foi cortada ou alterada e mostre um quadro antes e depois.

Não use o `inpaint` de uma biblioteca nem baixe modelos de IA sem autorização (ver seção 5).

## 2. Transcrever (para legendas, cortes por fala ou QA)

Verifique nesta ordem. Confirme o texto contra o áudio quando houver recurso de escuta disponível; se não houver, informe que a transcrição não foi conferida auditivamente e peça confirmação humana quando a precisão for necessária.

1. **Há texto fornecido?** Roteiro, legenda `.srt` ou fala escrita pelo usuário: use-o e sincronize ao áudio. É o caminho mais seguro.
2. **Há ferramenta local?** Cheque `python -c "import faster_whisper"`, o executável `whisper` e o filtro `whisper` do FFmpeg (este precisa de arquivo de modelo). Prefira o que já existe.
3. **Se for preciso instalar ou baixar, peça autorização** com nome do pacote, tamanho aproximado do modelo e destino. Use um ambiente virtual (`python -m venv`), não o Python global. Use exatamente o modelo que anunciou ao usuário; trocar por um maior exige nova autorização.
4. **Processamento local.** Nunca envie áudio ou vídeo a um serviço de transcrição externo (nem exija chave de API) sem pedido explícito do usuário.
5. **Prepare o áudio:** WAV mono, 16 kHz (`-vn -ac 1 -ar 16000`).
6. **Parâmetros de partida:** idioma explícito (`pt`), `word_timestamps=True`, `condition_on_previous_text=False` para um erro não se propagar, modelo `small` ou maior para fala em português, GPU com queda para CPU `int8` se a placa falhar. Registre o modelo e a versão usados.
7. **Se a biblioteca falhar ao abrir o arquivo** (visto: `av` incompatível, erro `metadata_errors`), leia o WAV com `wave` e `numpy` e passe o vetor `float32` normalizado em vez do caminho.
8. **Não engula exceções.** Se o motor falhar ou trocar de motor, registre qual e por quê. Transcrição vazia ou ausente invalida qualquer QA que dependa dela.
9. **Revise antes de queimar:** confira texto e tempos contra o áudio quando possível. Marque como suspeitos nomes próprios, marcas e termos em inglês; o modelo escreveu "Cloud Coach" para "Claude Code". Peça confirmação ao usuário quando uma dúvida relevante não puder ser resolvida. Corrija com um glossário (`{"cloud coach": "Claude Code"}`) e registre as correções.

## 3. Legendas queimadas

**Padrão de partida para 9:16 de fala direta (1080×1920):**

- **Agrupamento:** divida o texto em grupos legíveis e respeite pausas, frases e cortes.
- **Palavra ativa:** destaque por palavra (cor ou escala). Troque o destaque no tempo de fala; o primeiro evento do grupo começa no início do grupo.
- **Fade só em pausa real** (perceptível). Entre grupos encadeados, troca instantânea; fades encadeados piscam.
- **Estilo:** fonte pesada e legível (Arial Black, Segoe UI Black, Impact ou fonte da marca com licença), contorno de 5 a 9 px, sombra leve, caixa alta opcional, pontuação opcional.
- **Posição:** parte inferior-central, com margens adequadas ao destino, longe da boca e da interface da plataforma. Registre a suposição e confira na prévia da plataforma-alvo.
- **Camada:** legendas por último, acima de qualquer overlay.
- **Elementos extras** (selos, tags) respeitam as mesmas margens, sem prejudicar a leitura.

**Como renderizar com FFmpeg:**

1. Gere um arquivo ASS (um evento por palavra, com o texto do grupo e a palavra ativa em outra cor via `{\c&H…&}` e `\t(...)` para a escala; cores em BGR).
2. Teste se o build suporta o filtro: `ffmpeg -hide_banner -filters | findstr /i "subtitles ass"`.
3. Rode o FFmpeg **na pasta do `.ass`** e passe o nome relativo (`subtitles=arquivo.ass`); evita problemas de escape de caminho no Windows.
4. Faça um **teste de 1 quadro** e abra a imagem antes de renderizar o lote. Escolha um instante em que a legenda deva estar na tela e coloque o `-ss` **depois** do `-i`: com `-ss` antes do `-i`, a linha do tempo recomeça em zero e o filtro `subtitles` não encontra a legenda, então o quadro sai limpo (reproduzido em 03/11/2026 no FFmpeg 9.0.2, com legenda de 0,5 s a 1,8 s e quadro pedido em 1 s). Exemplo: `ffmpeg -i entrada.mp4 -ss 00:00:01 -vf "subtitles=legenda.ass" -frames:v 1 teste.png`. Em vídeo longo, a busca depois do `-i` é mais lenta; a alternativa `-ss 00:00:01 -copyts -i entrada.mp4` também mostrou a legenda, mas foi testada só em um clipe de 3 s. Um quadro sem legenda neste teste não prova defeito no ASS: confira primeiro a posição do `-ss`. Verifique tamanho, fonte (se caiu em fonte padrão, aponte `fontsdir`), contorno, margens e que nada cobre o rosto.
5. Se o build não tiver libass, use HyperFrames ou rasterize as legendas em imagens com sobreposição. Se nenhuma opção existir, entregue `.srt` separado e diga que não foi queimado.

Deixe os arquivos (`.srt`/`.ass` e glossário) no projeto, ao lado do vídeo, para reuso.

## 4. Ampliação e "melhore no contexto geral"

- **Ampliação (upscale):** só amplie para atender o destino (por exemplo, 480×864 para 1080×1920 de Reels). Use `lanczos`, aplique denoise leve antes de ampliar e nitidez suave depois, e diga que há perda de nitidez inerente. Não prometa "qualidade HD".
- **Pacote mínimo de melhoria geral**, aplicado só quando o diagnóstico justificar: balanço de brilho e contraste, saturação moderada, denoise leve, nitidez suave, limpeza de áudio (filtro de graves, redução de ruído leve), normalização conforme o destino e recorte/zoom para valorizar o sujeito. Compare um quadro antes e depois; desfaça o que degradar a imagem (pele, texto, bordas).
- **Material já comprimido** (WhatsApp, repost): menos é mais. Filtros fortes pioram blocos e halos.

## 5. Limites de segurança

- Baixar arquivo, instalar pacote, usar modelo grande ou enviar mídia a um serviço externo exige pedido explícito: nome, origem, tamanho e destino.
- Instale em ambiente virtual e com versão fixada quando possível. Não rode instaladores remotos copiados de páginas sem ler o conteúdo.
- Baixar vídeos de links (Instagram, TikTok, YouTube) só com pedido do usuário. **Nunca use cookies, sessão logada ou credenciais do navegador do usuário para isso.** Se o link exigir login, peça o arquivo ou capturas de tela.
- Não grave sobre os originais; grave com nome novo, por padrão fora da pasta de origem (ou onde o usuário pedir); registre hash antes (ver [exportação e verificação](exportacao-e-verificacao.md)).
