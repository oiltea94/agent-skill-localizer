# Agent Skills 多语言本地化管理器

[English](README.md) | 简体中文

翻译 Skill 的显示名称、简介和默认提示语，不改变调用方式。比如，在读取 `agents/openai.yaml` 的客户端，**Release Notes** 可以显示为**发布说明**，英文 ID 仍是 `release-notes`。安装内容只有一份英文 [SKILL.md](skills/skill-localizer/SKILL.md)，不用安装 Python，也不需要额外翻译服务。

## 兼容范围

**谁能运行：** Codex 和 Claude Code 等支持 Agent Skills 格式的宿主都能读取这份指令。有读取权限就能盘点 Skills、预览译文；实际写入和恢复还需要具备 Skill 要求的文件工具和权限。

**哪里能显示译文：** 当前只支持 OpenAI 的 `agents/openai.yaml` 展示格式。在 Claude Code 里安装本工具，不会把它的 `/` 菜单翻译成中文：命令名来自 Skill 的 frontmatter 或目录名，本工具都不改。你可以在一个 agent 里为另一个客户端准备译文，而不改变当前 agent 的界面。

## 安装

**方式一：在 Codex 里说一句话（推荐）**

```text
用 $skill-installer 从 GitHub 仓库 oiltea94/skill-localizer 安装 skills/skill-localizer 目录里的 Skill。
```

**方式二：在目标项目中运行一条命令**（仅安装时需要 `npx`）：

```sh
npx skills add oiltea94/skill-localizer --skill skill-localizer --agent codex
```

加上 `--global` 可安装到用户级目录。这两种 GitHub 安装方式都需要先发布 Skill 目录。要试用未发布的本地版本，可以在另一个项目中运行 `npx skills add "D:\dev\workspace\skill-localizer\skills\skill-localizer" --agent codex`（换成自己的路径）。手动安装时，只需让项目目录或用户主目录下存在 `.agents/skills/skill-localizer/SKILL.md`。

安装到 Claude Code 时，把 `npx` 命令中的参数换成 `--agent claude-code`。它选择的是安装到哪个宿主，不是译文的输出格式。上面的手动安装路径适用于 Codex。

## 使用示例

打开新的 Codex 会话，试着说：

> 用 $skill-localizer 扫描当前项目的 `.agents/skills`，看看哪些 Skill 还没有巴西葡萄牙语的展示文案。只给报告，不写入。

> 把缺少的译文补上。先逐项展示原文和译文，等我确认。

> 恢复同一目录里的 `release-notes`，先告诉我会还原或移除哪个文件。

在 Claude Code 中可以先试只读预览：`/skill-localizer 列出当前项目 .claude/skills 中的 Skills，给我看日语显示名称的草稿，不写文件。`

也可以要求复核源描述变动后的译文，或批量恢复。对话语言不决定目标语言。每个 Skill 同时只显示一种语言；切换语言只替换三个 UI 字段，不改英文 ID 或执行指令。

## 哪些文件会变？

盘点和预览不写文件。只有在你确认具体计划后，运行 Skill 的 agent 才会创建或更新目标 `agents/openai.yaml` 的 `interface.display_name`（显示名称）、`interface.short_description`（简介）和 `interface.default_prompt`（默认提示语）。其他配置和被管理的 `SKILL.md` 保持不变。

「单文件」指安装内容，不代表运行时不需要备份。每次写入前，agent 都应在授权根目录的 `.skill-localizer/v1/` 中保存原始字节或文件原本不存在的记录，以及校验值。请妥善保留备份，不要公开分享。恢复从磁盘读取本轮首次修改前的状态，换个会话也不靠模型记忆；这不等于切回上一种语言。原本没有 YAML 时，只移除新建的那个文件。

这些规则由运行 Skill 的 agent 遵循，不是独立程序强制执行的事务。没有合适工具安全解析、备份或验证时，只给预览。遇到手动修改、历史不明确或不兼容的旧备份，不直接覆盖。批量操作可能部分完成，请查看结果报告。
