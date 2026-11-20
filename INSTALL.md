# 发布与安装说明

本 skill 以 Agent Skills 开放标准（`SKILL.md` + YAML frontmatter）为唯一载体，含一个基线校验脚本（`scripts/verify_baseline.py`），无第三方依赖。安装本质是将 `data-protect-rule/` 目录放入目标 agent 的 skills 目录并保持目录名。

## 1. 发布产物与获取

### 1.1 安装包命名

`data-protect-rule-v<版本号>.zip`，采用语义化版本（`主.次.修订`），例如 `data-protect-rule-v1.2.0.zip`。

### 1.2 版本对应关系

| 版本 | 内容 | 说明 |
| --- | --- | --- |
| v1.2.0 | SKILL.md、references/、scripts/、README.md、INSTALL.md、LICENSE | 当前版本 |

### 1.3 获取方式

- **GitHub Releases**：在仓库 Releases 页下载对应版本的 zip 安装包；
- **源码**：`git clone https://github.com/hub-xue/data-protect-rule.git`。

## 2. 通用安装流程

各 agent 安装本质相同，仅 skills 目录位置不同：

1. 下载 zip 并解压，得到 `data-protect-rule/` 目录；
2. 放入目标 agent 的 skills 目录（见 3.1 对照表）；
3. 目录名保持 `data-protect-rule`（须与 SKILL.md 的 `name` 一致）；
4. 重启或新建会话使其生效；
5. 按 3.3 验证。

## 3. 各 agent 安装

### 3.1 skills 目录对照表

| agent | 用户级目录 | 工程级目录 | 备注 |
| --- | --- | --- | --- |
| WorkBuddy | `~/.workbuddy/skills/` | `.workbuddy/skills/` | 原生支持 |
| Claude（Claude Code） | `~/.claude/skills/` | `.claude/skills/` | 可用 `CLAUDE_CONFIG_DIR` 覆盖 |
| codex（OpenAI Codex） | `~/.codex/skills/` | `.agents/skills/` | 工程级读通用目录 |
| dsh（DeepSeek Harness） | `~/.dsh/skills/` 或 `~/.agents/skills/` | `.dsh/skills/` 或 `.agents/skills/` | 原生实现 Skills 规范 |
| qder（Qoder） | `~/.qoder/skills/` | 以官方文档为准 | 亦识别通用目录 `.agents/skills/` |

> 目录整理自 Agent Skills 生态公开文档，请以各 agent 官方文档为准。通用目录 `.agents/skills/` 被 codex、Cursor、Gemini、dsh 等多个 agent 识别，可一次安装、多处复用。

### 3.2 安装要素表

| agent | 适用环境 | 前置依赖 | 安装步骤 | 配置要点 | 验证方法 |
| --- | --- | --- | --- | --- | --- |
| WorkBuddy | WorkBuddy 桌面端 / IDE | 原生支持，无额外依赖 | 解压放入 `~/.workbuddy/skills/` → 重启 | 目录名保持 `data-protect-rule`；仅手动调用 | 手动调用 `/data-protect-rule`，确认边界声明生效 |
| Claude（Claude Code） | Claude Code / 支持 SKILL.md 的 Claude 环境 | 支持 Skills 机制 | 解压放入 `~/.claude/skills/` → 重启会话 | 目录名保持；仅手动调用 | 手动调用确认约束生效 |
| codex（OpenAI Codex） | OpenAI Codex CLI | 支持 Skills 机制 | 解压放入 `~/.codex/skills/`（个人）或 `.agents/skills/`（工程）→ 重启 | 目录名保持；仅手动调用 | 手动调用确认约束生效 |
| dsh（DeepSeek Harness） | DeepSeek Harness | 原生实现 Skills 规范 | 解压放入 `~/.dsh/skills/` 或 `~/.agents/skills/` → 重启 | 目录名保持；仅手动调用 | 手动调用确认约束生效 |
| qder（Qoder） | Qoder | 支持 Skills 机制（待核实） | 解压放入 `~/.qoder/skills/` → 重启 | 目录名保持；仅手动调用 | 手动调用确认约束生效 |

### 3.3 验证方法

安装后在会话中手动调用 `/data-protect-rule`，若助手确认约束边界与报告义务（S1 锚定）即安装成功。本 skill 自动触发已关闭，不会因话题自动生效。

## 4. 注意事项

- 目录名必须为 `data-protect-rule`，与 SKILL.md 的 `name` 不一致会导致校验失败、无法正确识别；
- 本 skill 仅手动调用（`disable-model-invocation: true`）；
- 修改 SKILL.md 后须运行 `python scripts/verify_baseline.py`，退出码非 0 不得交付；
- 同一份 skill 可由多个 agent 通过软链接复用（以 `.agents/skills/` 为源，各 agent 目录做链接）。
