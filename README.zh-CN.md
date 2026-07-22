# caoxicheng/codex-skills

[English](README.md)

这是一个通过 GitHub 分发的 Codex 插件仓库，用于发布和安装可复用的 Skills。

## 安装

将本仓库注册为 Codex marketplace：

```bash
codex plugin marketplace add caoxicheng/codex-skills
```

安装插件：

```bash
codex plugin add caoxicheng-skills@caoxicheng-skills
```

你也可以重启 ChatGPT 桌面应用，在 Codex 中打开 Plugins Directory，选择 `Caoxicheng Skills` 并安装。

安装后，新建一个任务并输入：

```text
Use $hello-codex to verify the installation.
```

## 仓库结构

```text
.
├── .agents/plugins/marketplace.json
└── plugins/caoxicheng-skills
    ├── .codex-plugin/plugin.json
    └── skills/hello-codex
        ├── SKILL.md
        └── agents/openai.yaml
```

## 添加 Skill

每个 Skill 使用独立目录：

```text
plugins/caoxicheng-skills/skills/<skill-name>/SKILL.md
```

`skill-name` 使用小写 kebab-case。`SKILL.md` 的 YAML frontmatter 只保留 `name` 和 `description`；`description` 应同时说明能力和触发场景。

发布前，请校验 Skill 和插件清单，并更新 `.codex-plugin/plugin.json` 中的语义化版本号。

## 许可证

[MIT](LICENSE)
