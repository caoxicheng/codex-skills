# caoxicheng/codex-skills

[简体中文](README.zh-CN.md)

A GitHub-distributed Codex plugin repository for publishing and installing reusable skills.

## Installation

Register this repository as a Codex marketplace:

```bash
codex plugin marketplace add caoxicheng/codex-skills
```

Install the plugin:

```bash
codex plugin add caoxicheng-skills@caoxicheng-skills
```

Alternatively, restart the ChatGPT desktop app, open the Plugins Directory in Codex, select `Caoxicheng Skills`, and install it.

After installation, start a new task and enter:

```text
Use $hello-codex to verify the installation.
```

## Repository structure

```text
.
├── .agents/plugins/marketplace.json
└── plugins/caoxicheng-skills
    ├── .codex-plugin/plugin.json
    └── skills/hello-codex
        ├── SKILL.md
        └── agents/openai.yaml
```

## Adding a skill

Place each skill in its own directory:

```text
plugins/caoxicheng-skills/skills/<skill-name>/SKILL.md
```

Use lowercase kebab-case for `skill-name`. Keep only `name` and `description` in the `SKILL.md` YAML frontmatter, and describe both the capability and its trigger conditions in `description`.

Before publishing, validate the skill and plugin manifests, then update the semantic version in `.codex-plugin/plugin.json`.

## License

[MIT](LICENSE)
