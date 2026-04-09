# Ruvon Docs

Official documentation for the [Ruvon SDK](https://github.com/KamikaziD/ruvon-sdk) — Python-native workflow engine for fintech edge devices and cloud.

**Live docs:** https://kamazid.github.io/ruvon-docs/

## Local development

```bash
pip install -r requirements.txt
mkdocs serve
```

Open http://localhost:8000

## Structure

```
docs/
├── tutorials/          # Step-by-step learning paths
├── how-to-guides/      # Task-oriented guides
├── explanation/        # Architecture and design docs
├── reference/          # API and configuration reference
├── advanced/           # Custom providers, security, etc.
└── appendices/         # Changelog, roadmap, glossary
```

## Contributing

Edit any `.md` file in `docs/` and open a PR. The site deploys automatically on merge to `main`.
