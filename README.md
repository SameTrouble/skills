# 个人 Skills

用于存放和分享 SameTrouble 的个人 Agent Skills，支持以 Codex 合集插件安装，也支持通过 [`npx skills`](https://github.com/vercel-labs/skills) 从 GitHub 按需安装。

当前包含面向 Codex 的 `subagent-config`：需要派发子代理时，先让用户选择当前工具支持的模型和思考强度，再按所选配置派发。本任务已明确的配置沿用，缺失项先询问；主代理负责协调、整合和验收。

## 目录结构

```text
skills/
├── .agents/
│   └── plugins/
│       └── marketplace.json
├── .codex-plugin/
│   └── plugin.json
├── README.md
├── LICENSE
├── .gitignore
└── skills/
    ├── .gitkeep
    └── subagent-config/
        ├── SKILL.md
        └── agents/
            └── openai.yaml
```

每个 skill 使用独立目录，放在 `skills/<名称>/SKILL.md` 中；相关脚本、参考资料或资源可与该文件放在同一目录下。

## 可用 Skills

| Skill | 用途 | 触发方式 |
| --- | --- | --- |
| [subagent-config](skills/subagent-config/SKILL.md) | 派发前让用户选择子代理模型和思考强度，并在任务内沿用 | 需要派发子代理时自动使用；也可显式使用 `$subagent-config` |

选择以当前子代理工具支持的模型和对应思考强度为准，也可明确选择继承主会话。用户未答复时不派发；配置不可用时说明限制并让用户重新选择。代码审查同样沿用用户所选配置。完整规则见 [subagent-config](skills/subagent-config/SKILL.md)。

## Codex 插件安装

插件名为 `sametrouble-skills`，marketplace 名为 `sametrouble`，一次安装包含 `skills/` 下的全部 skills。插件使用 MIT 许可证，本身无需认证。

### Codex CLI

需要支持 `codex plugin` 命令的 Codex CLI，并能够访问 GitHub。以下 GitHub 安装入口在插件配置提交并推送到 `main` 后生效：

```bash
# 添加 GitHub marketplace
codex plugin marketplace add SameTrouble/skills --ref main

# 安装合集插件
codex plugin add sametrouble-skills@sametrouble

# 查看该 marketplace 的插件
codex plugin list --marketplace sametrouble
```

安装后在新会话中使用已添加的 skills。需要派发子代理时，`subagent-config` 会先询问模型和思考强度；也可显式使用 `$subagent-config`。

### Codex 桌面端

1. 在终端执行上述 `codex plugin marketplace add` 命令。
2. 重启 Codex 桌面端，打开插件目录（Plugins）。
3. 选择 `SameTrouble Plugins` 来源，找到 `SameTrouble Skills` 并安装。
4. 在新会话中检查已添加的 skills；若已通过 CLI 安装，可直接检查插件的安装状态。

插件格式及 marketplace 机制见 [Codex 官方插件打包文档](https://developers.openai.com/plugins/build/plugins)。

### 本地测试

在本仓库根目录运行：

```bash
codex plugin marketplace add .
codex plugin list --marketplace sametrouble
codex plugin add sametrouble-skills@sametrouble
```

`marketplace.json` 中的 `source.path: "./"` 相对于 marketplace 根目录，即本仓库根目录；插件清单中的 `skills: "./skills/"` 相对于插件根目录。无需复制或移动现有 skills。

本地和 GitHub 安装使用相同的 marketplace 名称，测试时选择其中一种来源。Codex 使用安装后的缓存副本；修改本地插件后，重新执行插件安装命令，并重启桌面端、创建新会话检查更新。

### 更新与卸载

GitHub 来源发布更新后，刷新 marketplace 并重新安装插件以获取更新：

```bash
codex plugin marketplace upgrade sametrouble
codex plugin add sametrouble-skills@sametrouble
```

卸载插件及移除 marketplace：

```bash
codex plugin remove sametrouble-skills@sametrouble
codex plugin marketplace remove sametrouble
```

## 通过 npx skills 安装

需要本机已安装 Node.js 和 npm，并能够访问 GitHub。

```bash
# 查看仓库中的可用 skills
npx skills add SameTrouble/skills --list

# 交互选择并安装到当前项目
npx skills add SameTrouble/skills

# 安装指定 skill
npx skills add SameTrouble/skills --skill subagent-config
```

安装 `subagent-config` 时选择 Codex 作为目标 agent。该 skill 依赖 Codex 子代理工具及其模型与思考强度设置能力。如需安装到用户目录，可添加 `--global` 参数。完整选项见 [CLI 官方文档](https://github.com/vercel-labs/skills#options)。

## 新增 Skill

1. 在 `skills/` 下创建独立目录，名称使用小写字母和连字符，例如 `my-skill`。
2. 在该目录内添加 `SKILL.md`，文件开头使用 YAML frontmatter，至少包含 `name` 和 `description`：`name` 与目录名称一致，`description` 说明用途及适用场景。
3. 在 frontmatter 后编写实际操作说明，并按需添加脚本、参考资料或资源。
4. 发布插件更新时，递增 `.codex-plugin/plugin.json` 中的版本号；添加首个 skill 时，同步更新插件介绍和 README 中的空仓库说明。新增 skill 共用合集插件身份，无需增加 marketplace 条目。
5. 先按本地测试步骤安装插件，在新会话中确认 skill 能被加载与调用；提交并推送到 `main` 后，再运行 `npx skills add SameTrouble/skills --list`，确认新 skill 也能被独立发现。

格式与发现规则见 [创建 Skills](https://github.com/vercel-labs/skills#creating-skills) 和 [Skill Discovery](https://github.com/vercel-labs/skills#skill-discovery)。仓库直接作为安装源，无需发布 npm 包或添加构建流程。

## 许可证

本仓库采用 [MIT 许可证](LICENSE)。
