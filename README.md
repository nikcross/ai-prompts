# AI Prompts Library

Reusable, version-controlled prompts for product teams. Every file ends in `.prompt.txt`, making it easy to search, lint, and automate across categories.

- GitHub Pages overview: https://nikcross.github.io/ai-prompts
- Rich local preview: [README.html](./README.html)
- Quick summary (same as Pages site): [docs/index.html](./docs/index.html)

## Catalog Snapshot

| Category | Subdirectory | Highlights |
| --- | --- | --- |
| Software Developer | requirements | `crosswalk-prompt-template.prompt.txt` for narrative/implementation tables; `gap-analysis.prompt.txt` for requirement coverage reports |
| | testing | `java-test-format.prompt.txt` for consistent JUnit documentation |
| Maintainer | authoring | `prompt-authoring-blueprint.prompt.txt` to draft new prompts; `prompt-style-audit.prompt.txt` to review contributions |

## Contributing

1. Save new prompts with the `.prompt.txt` suffix in the appropriate category/subdirectory.
2. Use the Maintainer prompts to ensure structure and tone match the house style.
3. Open a pull request describing the workflow the prompt improves.

## Roadmap

- Expand into design, product, and operations categories.
- Add automated lint checks for prompt metadata.
- Publish quickstart guides for integrating prompts into IDE snippets and CI pipelines.

Maintained by the community — feedback and ideas are always welcome.
