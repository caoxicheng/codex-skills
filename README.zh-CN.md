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
Use $find-open-source-solutions to recommend an open-source solution for my goal.
```

该插件会连接只读 GitHub MCP 端点以实时发现仓库，并使用 Context7 检索仓库代码和官方文档。

## 包含的 Skills

- `$find-open-source-solutions`：发现、验证、比较并推荐现有的开源 GitHub 仓库或多仓库解决方案组合。
- `$git-commit`：分析 Git 改动、建议原子提交分组、生成 Conventional Commit 信息，并且仅使用 Git 创建提交。
- `$hello-codex`：验证插件和自定义 Skill 是否正确安装。

`$git-commit` 支持以下可选参数：

```text
--no-verify --all --amend --signoff --emoji --scope <scope> --type <type>
```

## 仓库结构

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

## 添加 Skill

每个 Skill 使用独立目录：

```text
plugins/caoxicheng-skills/skills/<skill-name>/SKILL.md
```

`skill-name` 使用小写 kebab-case。`SKILL.md` 的 YAML frontmatter 只保留 `name` 和 `description`；`description` 应同时说明能力和触发场景。

发布前，请校验 Skill 和插件清单，并更新 `.codex-plugin/plugin.json` 中的语义化版本号。

## 许可证

[MIT](LICENSE)。改编内容的许可信息请参阅 [Third-Party Notices](THIRD_PARTY_NOTICES.md)。
