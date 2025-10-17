# AI Prompts

Reusable, version-controlled prompts for product teams. Every file ends with the `.prompt.txt` extension so you can search, lint, and automate without guesswork.

> Looking for the fully themed HTML version of this overview? Open [README.html](./README.html).

## What's Inside
- Prompts grouped by discipline (e.g., `software-developer`, `maintainer`)
- Purpose-driven subdirectories such as `requirements`, `testing`, and `authoring`
- Plain-text prompts ready to paste into your AI assistant

## Current Catalog

| Category | Subdirectory | Prompts & Use Cases |
| --- | --- | --- |
| Software Developer | requirements | `crosswalk-prompt-template.prompt.txt` — Generate narrative vs. implementation crosswalks<br>`gap-analysis.prompt.txt` — Compare requirements with current state and surface gaps |
| | testing | `java-test-format.prompt.txt` — Normalize Java test descriptions into consistent templates |
| Maintainer | authoring | `prompt-authoring-blueprint.prompt.txt` — Draft new prompts that follow repository conventions<br>`prompt-style-audit.prompt.txt` — Check whether a candidate prompt matches the house style |

## Suggested Workflow
1. Browse the catalog and open the prompt that matches your task.
2. Copy the contents directly into your AI assistant, swapping placeholders for project specifics.
3. Contribute refinements by editing the prompt file and updating this catalog.

## Roadmap
- Expand coverage for design, product, and operations disciplines.
- Add lint checks to enforce `.prompt.txt` metadata.
- Publish quickstarts for using prompts inside IDE snippets or CI templates.
