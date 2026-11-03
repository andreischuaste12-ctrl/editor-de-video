# Como contribuir

Issues, correções de documentação, novas fixtures sintéticas e melhorias nas receitas são bem-vindas.

1. Crie um fork e uma branch para a mudança.
2. Faça alterações pequenas e explique a motivação e a forma de validação.
3. Confirme que os exemplos funcionam na versão de FFmpeg declarada.
4. Rode `git diff --check` e valide `evals/evals.json` como JSON antes de abrir o pull request.
5. Abra o pull request descrevendo comportamento, testes e limitações.

Ao contribuir, você concorda que sua contribuição será licenciada sob a Apache License 2.0, conforme a seção de contribuições da licença. O nome e o e-mail associados aos commits podem ficar públicos no histórico do Git. Use o endereço privado `noreply` fornecido pelo seu serviço Git se não quiser publicar seu e-mail.

## Privacidade e materiais permitidos

- Use apenas fixtures sintéticas, pequenas e reproduzíveis.
- Não envie vídeos, áudios, imagens ou transcrições de clientes ou pessoas reais.
- Remova nomes, e-mails, telefones, identificadores, caminhos de usuário e metadados pessoais de exemplos e logs.
- Nunca envie chaves, tokens, cookies, arquivos `.env` ou credenciais.
- Não inclua material de terceiros sem licença compatível e atribuição adequada.

Relate vulnerabilidades e exposições de dados conforme [SECURITY.md](SECURITY.md), sem abrir uma issue pública com o dado sensível.
