# Contributing

Thank you for your interest in contributing to CvGeneratorSkill.

## Reporting Issues

- Use [GitHub Issues](../../issues) to report bugs or request features.
- Include steps to reproduce, expected behaviour, and actual behaviour.
- For skill-related issues, specify which skill (`cv-generation` or `review-experience`) is affected.

## Submitting Pull Requests

1. Fork the repository and create a feature branch from `main`.
2. Make your changes, keeping commits focused and well-described.
3. Ensure skill SKILL.md files remain valid (correct YAML front-matter, complete workflow steps).
4. Test your changes by running the skill in Claude Code against sample data.
5. Open a PR with a clear description of what changed and why.

## Code Style

- **Skill definitions** (SKILL.md): Use clear, imperative instructions. Keep workflow steps numbered and actionable.
- **Database files**: Follow the YAML front-matter conventions documented in CLAUDE.md. Use display-ready names that match the master catalogue in `skills_tools.md`.
- **Python scripts**: Follow PEP 8. Use PEP 723 inline metadata for dependencies so `uv run` works without a separate requirements file.
- **Markdown**: Use ATX-style headings (`#`). Keep lines readable.

## Skill Development Guidelines

When creating or modifying skills:

- Maintain the grounding principle — skills must never invent data that isn't in the database.
- Include user confirmation steps before applying changes.
- Document all workflow steps in the SKILL.md file.
- Test with the sample database (`cvDatabase-sample/`) to ensure the skill works with minimal data.

## License

By contributing, you agree that your contributions will be licensed under the [MIT License](LICENSE).
