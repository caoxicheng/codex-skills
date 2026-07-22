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
Use $find-open-source-solutions to recommend an open-source solution for my goal.
```

The plugin connects to a read-only GitHub MCP endpoint for live repository discovery and to Context7 for repository and documentation retrieval.

## Included skills

- `$find-open-source-solutions`: Discover, verify, compare, and recommend existing open-source GitHub repositories or composed solution stacks.
- `$git-commit`: Analyze Git changes, suggest atomic groups, generate Conventional Commit messages, and create commits using Git only.
- `$hello-codex`: Verify that the plugin and custom skills are installed correctly.

`$git-commit` accepts the following optional arguments:

```text
--no-verify --all --amend --signoff --emoji --scope <scope> --type <type>
```

## Repository structure

```text
.
├── .agents/plugins/marketplace.json
├── THIRD_PARTY_NOTICES.md
└── plugins/caoxicheng-skills
    ├── .mcp.json
    ├── .codex-plugin/plugin.json
    └── skills
        ├── find-open-source-solutions
        │   ├── SKILL.md
        │   ├── agents/openai.yaml
        │   └── references/context7.md
        ├── git-commit
        │   ├── SKILL.md
        │   └── agents/openai.yaml
        └── hello-codex
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

[MIT](LICENSE). See [Third-Party Notices](THIRD_PARTY_NOTICES.md) for adapted components.
