# Contributing

Issues, documentation corrections, new synthetic fixtures, and improvements to the editing recipes are welcome.

1. Fork the repository and create a branch for your change.
2. Keep the change focused and explain its motivation and validation.
3. Confirm that command examples work with the declared FFmpeg version.
4. Run `git diff --check` and validate `evals/evals.json` as JSON before opening a pull request.
5. Describe the resulting behavior, tests, and limitations in the pull request.

By contributing, you agree that your contribution will be licensed under the Apache License 2.0 under its contribution terms. The name and email associated with your commits may become public in Git history. Use the private `noreply` address provided by your Git service if you do not want to publish your email.

## Privacy and permitted materials

- Use only small, reproducible, synthetic fixtures.
- Do not submit video, audio, images, or transcripts from clients or real people.
- Remove names, email addresses, phone numbers, identifiers, local user paths, and personal metadata from examples and logs.
- Never submit keys, tokens, cookies, `.env` files, or credentials.
- Do not include third-party material without a compatible license and proper attribution.

Report vulnerabilities and data exposure according to [SECURITY.md](SECURITY.md). Do not open a public issue containing sensitive data.
