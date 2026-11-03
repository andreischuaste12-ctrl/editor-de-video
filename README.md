# Editor de Vídeo

Skill de instruções para assistentes de IA (formato `SKILL.md`) que edita materiais audiovisuais existentes e entrega um vídeo final verificado: cortes, montagem, legendas, mixagem de áudio, correções, reexportação e efeitos, em 16:9, 9:16 e 1:1. O identificador da skill é `editar-videos-konvert`.

A skill orienta o assistente; ela não é um editor nem um programa. As operações rodam nas ferramentas do ambiente, principalmente FFmpeg/FFprobe, e HyperFrames quando houver composição em HTML.

## O que ela faz

- **Preserva os originais.** Registra caminho, tamanho, data e SHA-256 antes de editar, trabalha em cópia e grava sempre em arquivo novo (`-n` no FFmpeg).
- **Inspeciona antes de montar.** Duração, resolução, fps, faixas de áudio e orientação via `ffprobe`.
- **Edita com critério de montagem.** Cortes por fala e ritmo, J/L-cuts, ducking (`sidechaincompress`), normalização EBU R128 (`loudnorm`), transições (`xfade`), reenquadramento vertical com safe zones.
- **Remove texto queimado e legendas.** Transcrição, legenda ASS, remoção de marca d'água com checagem antes/depois.
- **Aplica efeitos com orçamento.** Zoom, speed ramp, flash, glitch, SFX e look de cor, com limite de flashes e verificação por gatilho.
- **Verifica a exportação.** Propriedades, quadros amostrados, LUFS e True Peak, legibilidade e relato do que não foi inspecionado.

Fora do escopo: criar conceito ou roteiro sem mídia e escrever copy de anúncio.

## Estrutura

| Caminho | Conteúdo |
|---|---|
| `SKILL.md` | Instruções centrais e roteamento para as referências |
| `references/exportacao-e-verificacao.md` | Exportação padrão, sonoridade, receitas FFmpeg e verificação |
| `references/corte-por-fala-e-qa.md` | Cortes por fala, ritmo, SFX e QA de fala |
| `references/remocao-e-legendas.md` | Remoção de texto/marca d'água, transcrição e legendas |
| `references/efeitos-e-acabamento.md` | Receitas de efeitos e verificação |
| `references/composicao-avancada-ia.md` | Motion design, VFX e mídia gerada por IA como insumo |
| `references/referencias.md` | Bibliografia e documentação técnica |
| `agents/revisor-de-exportacao.md` | Prompt reutilizável de revisão independente (não é um agente registrado) |
| `evals/` | 18 cenários (`evals.json`) e fixtures sintéticas |

## Requisitos

- FFmpeg e FFprobe no `PATH`. As receitas foram testadas com FFmpeg 9.0.2; confirme as opções com `ffmpeg -h` na versão instalada.
- Opcional: HyperFrames, para títulos animados e composição em HTML.
- Baixar arquivos, instalar pacotes, enviar mídia a serviços externos ou usar sessão do navegador exige pedido explícito do usuário.

## Instalação

A raiz do repositório é a própria skill. Copie-a para o diretório de skills do seu assistente, em uma pasta com o nome da skill. Exemplo para o Claude Code (escopo pessoal):

```bash
mkdir -p ~/.claude/skills/editar-videos-konvert
cp -r SKILL.md references agents ~/.claude/skills/editar-videos-konvert/
```

`evals/` é opcional e só serve para testes. Instalar a skill não comprova que um modelo a carregou ou a seguiu.

## Avaliação

`evals/evals.json` é uma **especificação de cenários**, não uma suíte automatizada: nenhum caso foi executado como teste independente de comportamento do agente. Os comandos verificados com fixtures sintéticas estão descritos em `execution_notes`. As fixtures são sintéticas, sem pessoas, marcas ou dados reais; veja `evals/fixtures/LEIAME.md`.

## Como contribuir

Contribuições são bem-vindas por issues e pull requests. Leia [CONTRIBUTING.md](CONTRIBUTING.md) antes de enviar mudanças e não publique mídias de clientes, credenciais, dados pessoais ou caminhos locais em exemplos, logs e fixtures.

## Licença

Copyright 2026 Editor de Vídeo contributors. Distribuído sob a [Apache License 2.0](LICENSE); atribuição em [NOTICE](NOTICE).

Obras, ferramentas e marcas de terceiros citadas nas referências pertencem aos seus donos; a citação não as relicencia nem implica endorsement.
