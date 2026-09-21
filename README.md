# keyx-skill · 让 AI 替你守住密钥的技能

Key-X 的 skill 仓库（独立于二进制仓库 `keyx`/keyx-rs）：教 AI 编码代理执行密钥卫生——
新密钥走 `keyx set`、配置里只写 `kx_` 编号、见到明文立即引导轮换、`keyx run` 注入启动。

## 安装（人类做一次）

```bash
npx skills add elonsolar/key-x-skill        # 发布后可用；--list 可先查看仓库里的技能
```

CLI 会自动检测机器上装过的代理（Claude Code / Cursor / Codex / OpenCode / ZCode 等），
把 skill 装进各自的 skills 目录。没有 skills 机制的引擎：把
[`skills/keyx-keys/AGENTS-SNIPPET.md`](skills/keyx-keys/AGENTS-SNIPPET.md) 粘进项目的
`AGENTS.md` / `.cursorrules`。

## 安装（二进制，AI 冷启动时也会引导）

skill 里内置了安装指引：AI 在会话里发现 `keyx` 不存在时，会直接给出对应平台的一键命令。

```bash
curl -fsSL https://raw.githubusercontent.com/elonsolar/key-x/main/install.sh | bash    # macOS / Linux
irm  https://raw.githubusercontent.com/elonsolar/key-x/main/install.ps1 | iex          # Windows
```

## 仓库结构

```
skills/keyx-keys/SKILL.md          # 技能本体（公理/硬规则/两个反射/四条工作流/命令速查/冷启动）
skills/keyx-keys/AGENTS-SNIPPET.md # 无 skills 机制的引擎用的精简版（粘进 AGENTS.md）
```

符合 [skills CLI](https://github.com/vercel-labs/skills) 的发现规范（`skills/<name>/SKILL.md`）。

## 版本兼容

| skill | keyx 二进制 |
|---|---|
| v1 | ≥ 1.3 |

skill 会话内通过 `keyx --version` 握手；不匹配时引导升级二进制。

## 发布状态

- 本仓库与二进制仓库中所有 `elonsolar/` 前缀已对准 GitHub 账号，剩余动作只有两个：
  1. 在 GitHub 建 `elonsolar/key-x` 与 `elonsolar/key-x-skill` 两个仓库并推送；
  2. 二进制仓库打 tag（如 `v1.3.0`）触发 5 平台构建——之后所有一键安装命令生效。
