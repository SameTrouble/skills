# 个人 Skills

用于存放和分享 SameTrouble 的个人 Agent Skills，支持通过 [`npx skills`](https://github.com/vercel-labs/skills) 从 GitHub 安装。

目前仓库只有目录骨架，尚无可安装的 skill。添加第一个有效的 `SKILL.md` 后，即可使用下方命令安装。

## 目录结构

```text
skills/
├── README.md
├── LICENSE
├── .gitignore
└── skills/
    └── .gitkeep
```

每个 skill 使用独立目录，未来放在 `skills/<名称>/SKILL.md` 中；相关脚本、参考资料或资源可与该文件放在同一目录下。

## 安装

需要本机已安装 Node.js 和 npm，并能够访问 GitHub。

```bash
# 查看仓库中的可用 skills（当前预期提示没有可用 skills）
npx skills add SameTrouble/skills --list

# 交互选择并安装到当前项目
npx skills add SameTrouble/skills

# 安装指定 skill；将 <名称> 替换为实际 skill 名称
npx skills add SameTrouble/skills --skill <名称>
```

安装时可选择目标 agent；如需安装到用户目录，可添加 `--global` 参数。完整选项见 [CLI 官方文档](https://github.com/vercel-labs/skills#options)。

## 新增 Skill

1. 在 `skills/` 下创建独立目录，名称使用小写字母和连字符，例如 `my-skill`。
2. 在该目录内添加 `SKILL.md`，文件开头使用 YAML frontmatter，至少包含 `name` 和 `description`：`name` 与目录名称一致，`description` 说明用途及适用场景。
3. 在 frontmatter 后编写实际操作说明，并按需添加脚本、参考资料或资源。
4. 提交并推送到 `main`，再运行 `npx skills add SameTrouble/skills --list`，确认新 skill 能被发现。

格式与发现规则见 [创建 Skills](https://github.com/vercel-labs/skills#creating-skills) 和 [Skill Discovery](https://github.com/vercel-labs/skills#skill-discovery)。仓库直接作为安装源，无需发布 npm 包或添加构建流程。

## 许可证

本仓库采用 [MIT 许可证](LICENSE)。
