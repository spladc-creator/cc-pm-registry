---
name: package-design
version: 1.0
author: spladc
description: Meta skill for creating shareable agents and skills using cc-pm package pattern
model: sonnet
---

# 配置包设计（Meta Skill）

按 cc-pm 模式创建可共享、可发布的 Claude Code agent 或 skill。

## When to Use

- 用户说"创建 agent"、"新建 skill"、"打包"、"发布"
- 用户要分享一个 agent/skill 给其他项目或其他人
- 用户要给现有 agent 加版本管理或远程分发能力

## When NOT to Use

- 只是在项目内写一个 prompt 模板（不需要打包）
- 修改已有 agent 的内容（直接编辑，不用此 skill）

## 第一步：判断类型

问用户一个问题：

> 这是给其他项目复用的能力模块（agent），还是跟特定工作流绑定的操作（skill）？

| 类型 | 特征 | 产出结构 |
|------|------|---------|
| **agent** | 单一职责、跨项目通用、Claude Code 自动加载 | `包名/agent.md` |
| **skill** | 多文件、工作流绑定、需要 hook 或模板 | `包名/skill.md` + 附属文件 |

判断标准：如果内容是一个完整的角色定义（有 tools、model、职责描述），选 agent。如果是一套操作流程（含步骤、模板、hook），选 skill。

## 第二步：创建目录和 frontmatter

### Agent 模板

在 `共享模板/prompts/agents/<包名>/` 下创建 `agent.md`：

```markdown
---
name: <包名>
version: 1.0
author: <作者>
description: <一句话描述，50字以内>
tools: <Claude Code 工具列表，如 Glob, Grep, Read, WebSearch>
model: <模型，如 sonnet>
color: <显示颜色，如 blue>
---

<角色定义和 prompt body>
```

**frontmatter 字段规则**：
- `name`: 小写英文，连字符分隔（如 `deep-organizer`）
- `version`: 语义化版本，从 `1.0` 开始
- `author`: 作者标识
- `description`: 一句话描述，支持 `\n` 换行的多行示例
- `tools`: Claude Code 运行时使用的工具列表
- `model`: Claude Code 运行时使用的模型

**两个字段家族**：
- `name`, `version`, `author`, `description` → 包管理器读取（识别、索引、展示）
- `tools`, `model`, `color` → Claude Code 运行时读取（决定行为）

### Skill 模板

在 `共享模板/prompts/skills/<包名>.skill/` 下创建 `skill.md`：

```markdown
---
name: <包名>
version: 1.0
author: <作者>
description: <一句话描述>
location: project
---

# Skill 名称

## When to Use
<触发条件>

## When NOT to Use
<排除条件>

## 执行流程
<步骤>
```

Skill 可以包含附属文件：
- `hooks/` — 自动触发的 hook 脚本
- `templates/` — 内容模板
- 其他辅助文件

## 第三步：验证

创建完成后运行：

```bash
# 检查 registry 能识别
cc-pm.sh list
cc-pm.sh search <关键字>

# 测试安装到项目
cc-pm.sh install <包名> <测试项目>
cc-pm.sh doctor
```

## 第四步：发布（可选）

如果需要分享给他人：

```bash
cc-pm.sh publish <包名>
```

验证消费者能拉取：

```bash
cc-pm.sh remote-search <关键字>
cc-pm.sh pull <包名>
```

## 注意事项

- **symlink 是 .md 文件**：Claude Code 只扫描 `.claude/agents/*.md`，所以 agent 的 symlink 必须是文件级而非目录级
- **description 写好**：这是 `list`、`search`、`remote-search` 的展示内容，是包的"门面"
- **版本号只升不降**：修改内容后提升 frontmatter 中的 version，然后重新 publish
- **agent 自包含**：一个 agent 文件包含完整定义，不依赖外部文件

## 本 skill 的自举说明

这个 skill 本身遵循了它所描述的模式：
- 目录封装：`package-design.skill/skill.md`
- Frontmatter 元数据：含 name、version、author、description
- 可通过 cc-pm.sh 管理：`cc-pm.sh list --type skill`、`cc-pm.sh publish package-design --type skill`
