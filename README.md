# limits-skill · 严格指令模式 (Strict Command-Only Mode)

一个用于 **Claude Code** 和 **Codex** 的约束规则，让 AI **只思考和执行你指定的命令**，
不做任何额外的思考、建议、解释或操作。

> Make the AI do **only** what you told it to — nothing more.

## 这是什么 / What it is

默认情况下，AI 助手常常“自作主张”：帮你优化代码、补充测试、给出建议、
解释一大堆。有时你只想让它**精确执行一件事**。

本 Skill 给 AI 套上一个硬约束：

- 只思考 **如何完成当前这条命令**
- 只执行 **这一条命令**
- 完成后立即停止
- 不解释、不建议、不扩展、不“顺手”做别的

## 为什么需要 / Why

- 可预测：输出范围完全由你的命令决定
- 省 token：没有多余的解释和铺垫
- 更安全：不会擅自触碰命令之外的文件或系统

## 目录结构 / Structure

```
limits_skill/
├── claude/
│   ├── CLAUDE.md                   Claude Code 全局规则（自动生效，推荐）
│   └── strict-command-only/
│       └── SKILL.md                可选：手动 /strict-command-only 调用
└── codex/
    ├── AGENTS.md                   Codex 全局规则（自动生效，推荐）
    └── strict-command-only.md      可选：手动 /strict-command-only 调用
```

## 安装（自动生效，无需调用）/ Install (always on)

这是推荐方式：规则每次会话自动加载，**所有命令**都受约束，不用任何提示词。

### Claude Code

把 `claude/CLAUDE.md` 放到：

- 全局（所有项目）：`~/.claude/CLAUDE.md`
- 单个项目：`<项目>/CLAUDE.md`

Windows 下全局目录通常是 `C:\Users\<你>\.claude\`。
如果该文件已存在，把内容追加进去即可，不要直接覆盖。

### Codex

把 `codex/AGENTS.md` 放到：

- 全局（所有项目）：`~/.codex/AGENTS.md`
- 单个项目：`<项目>/AGENTS.md`

Windows 下全局目录通常是 `C:\Users\<你>\.codex\`。
已有文件同样是追加，不要覆盖。

## 可选：按需手动调用 / Optional: manual trigger

如果你不想全程生效，只想偶尔启用，用这两个文件：

- Claude Code：`claude/strict-command-only/SKILL.md` → `~/.claude/skills/strict-command-only/`
- Codex：`codex/strict-command-only.md` → `~/.codex/prompts/`

两者都用 `/strict-command-only` 调用。

### 示例 / Example

**启用前：**

> 用户：把 `config.json` 里的端口改成 8080
> AI：已修改端口。另外我发现日志没开，建议……还帮你加了校验……

**启用后：**

> 用户：把 `config.json` 里的端口改成 8080
> AI：（只改端口，改完，结束）

## 行为规则 / Behavior

| 必须做 MUST | 禁止做 MUST NOT |
| --- | --- |
| 精确执行指定命令 | 添加命令之外的功能 / 文件 / 步骤 |
| 思考仅限于如何完成该命令 | 重构 / 优化 / 清理命令之外的东西 |
| 完成后立即停止 | 给出建议、下一步、相关工作 |
| 命令不明确时只问一个简短问题 | 多余的解释、总结、寒暄 |
| 输出保持最少 | “顺手”安装、修复无关问题 |

完整规则见 [`strict-command-only/SKILL.md`](strict-command-only/SKILL.md)。

## 自定义 / Customize

直接编辑 `strict-command-only/SKILL.md` 即可调整严格程度，例如允许简短说明、
或允许在命令完成后报告一句结果。改 `description` 可影响它被自动匹配的时机。

## 许可 / License

[MIT](LICENSE) © 2026 — 发布前请把 `LICENSE` 里的作者名改成你自己。

## 贡献 / Contributing

欢迎提 Issue 和 PR。
