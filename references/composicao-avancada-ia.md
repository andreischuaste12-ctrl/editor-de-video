# Composição avançada e IA

Baseada no estudo fornecido pelo usuário, com documentação oficial indicada abaixo. Use apenas para pedidos de composição elaborada, VFX, motion ou geração de ativos; não trate este fluxo como exigência de uma edição comum.

Para efeitos que o FFmpeg resolve sem outras ferramentas (zoom de ênfase, flash, shake, glitch, speed ramp, transições `xfade`, look de cor, legendas animadas, SFX sintetizados), use as receitas testadas de [efeitos e acabamento](efeitos-e-acabamento.md); este arquivo cobre o que exige composição, tracking, 3D ou mídia gerada.

## Decisão e preparação

- Relacione cada efeito à função na cena: demonstrar, contextualizar, criar atmosfera ou conectar planos. Identifique os trechos, a direção de arte, os assets, o tempo disponível e a entrega esperada antes de selecionar ferramentas.
- Faça um storyboard ou animatic proporcional à complexidade; marque o que é material real, ilustração ou reconstrução gerada. Em material documental ou comercial, não apresente reconstruções como registros reais, nem gráficos decorativos como evidência. Confira direitos de uso, atribuição e condições específicas dos ativos e serviços.
- Inspecione as ferramentas e permissões disponíveis. Use o fluxo existente do projeto quando houver; uma ferramenta paga, geração externa, upload ou aquisição de mídia não estão automaticamente autorizados. Verifique custo e acesso antes da execução e siga as autorizações aplicáveis.

## Protótipo e critério de aprovação

Antes de aplicar um efeito caro a toda a peça, teste um trecho representativo. Defina o que deve permanecer estável (rosto, produto, texto, enquadramento) e rejeite drift, deformação, bordas expostas ou informação ilegível. Se a solução falhar, simplifique o efeito ou troque o ativo; novas tentativas pagas dependem do orçamento autorizado. Registre a tomada, ferramenta/versão e o motivo de aceitar ou rejeitar.

## Técnicas para escolher conforme a cena

- **Mixed media e parallax 2.5D:** separe primeiro plano, sujeito e fundo quando houver material adequado; mova camadas e câmera virtual com profundidade compatível com a cena. Verifique recortes, bordas, áreas descobertas pelo movimento e legibilidade. Texturas de papel, grão e movimento discretizado são opções estéticas, não requisitos.
- **Transições com movimento:** planeje direção, enquadramento e movimento entre planos; experimente match cut ou speed ramp quando a filmagem suportar. Ajuste curvas conforme o material, sem impor multiplicadores ou duração fixos. Confira fluidez, cortes e eventuais deformações quadro a quadro.
- **Tracking e composição:** quando for necessário ancorar elementos a uma tomada, analise o movimento, avalie a qualidade da solução, exclua pontos problemáticos e confira alinhamento durante o plano. Rotoscopia, máscaras, luz, sombras e desfoque devem respeitar a cena. Tracking pode falhar com objetos móveis, pouco detalhe ou oclusões; se não houver solução confiável, simplifique ou peça outro material.
- **Infografia e 3D:** escolha objetos, tipografia, paleta e movimento coerentes com a identidade visual. Priorize leitura e correspondência dos números com as fontes; não use visualização decorativa para conferir aparência de prova a dados não verificados.
- **Motion design:** use curvas, antecipação e sobreposição se ajudarem a intenção; movimentos lineares também são válidos. Ajuste amplitude e duração ao tamanho de tela e à leitura, sem impor pixels ou frames universais.
- **Áreas seguras:** consulte a plataforma, o posicionamento e a prévia atual do destino. Diferencie conteúdo orgânico de anúncio e confira sobreposições de UI; uma margem fixa não cobre todos os dispositivos. Preserve rostos e informação essencial ao reenquadrar.
- **Cor:** identifique espaço de entrada, processamento, saída e condições de visualização. Use scopes como diagnóstico, sem forçar todos os tons de pele a uma linha. Não confunda tags com conversão de cor nem presuma que uma preferência do player resolve toda divergência de gamma. Confira documentação da versão e uma amostra no destino.
- **Som:** construa voz, ambiência, trilha e efeitos apenas quando contribuírem; estruture em faixas DX, SFX, BG e MX. Avalie ruído grave e competição entre trilha e voz antes de aplicar filtro ou ducking; ajuste por escuta e medição. Ajuste níveis e transições pelo conteúdo. Quantidade de camadas e valores fixos não são metas universais.

## Integração de mídia gerada

Se o briefing pedir geração e o ambiente permitir, defina por tomada: objetivo narrativo, referência visual autorizada, enquadramento, duração aproximada, movimento e formato. Gere ou selecione os ativos por meio das ferramentas disponíveis e autorizadas; confira consistência visual, anatomia, texto, direitos e adequação factual antes da montagem. Trate upscaling, interpolação e efeitos como opcionais; compare os resultados e rejeite artefatos ou alterações que prejudiquem o conteúdo. Não exponha materiais privados a serviços externos sem autorização aplicável.

Entregue os assets gerados ou projeto editável apenas quando solicitado ou útil e disponível. Relate ferramentas utilizadas, materiais sintéticos relevantes, limitações e quais revisões técnicas, visuais e auditivas foram realmente executadas.

## Referências para consulta

- [Adobe: câmera e camadas 3D](https://helpx.adobe.com/after-effects/desktop/work-with-layers/camera-layer/cameras-lights-points-interest.html) e [3D Camera Tracker](https://helpx.adobe.com/after-effects/desktop/work-with-3d-composition/3d-camera-tracker-effect/tracking-3d-camera-movement.html): consulte quando usar After Effects; objetos móveis podem dificultar o tracking.
- [Runway: guia image-to-video](https://help.runwayml.com/hc/en-us/articles/48324313115155-Image-to-Video-Prompting-Guide): exemplo de fluxo e controle de movimento; confirme modelos, recursos, custo e termos atuais antes de usar o serviço.
- [Film Riot](https://www.youtube.com/@filmriot), [Cinecom.net](https://www.youtube.com/@CinecomNet), [Max Novak](https://www.youtube.com/@MaxNovak) e [Benn TK](https://www.youtube.com/@BennTK): referências de tutoriais indicadas pelo usuário; confira cada tutorial e versão da ferramenta antes de reproduzir.

O estudo original sugere ferramentas, versões e parâmetros específicos como exemplos. Não presuma que continuem disponíveis, sejam os melhores, nem que seus resultados ou licenças se apliquem a outro projeto.
