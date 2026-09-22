# key-x-skill · 让 AI 替你守住密钥的技能

这是 [Key-X](https://github.com/elonsolar/key-x)（本机密钥引用系统）的配套 AI 技能。
装上它，AI 就懂得：**永远不向你索要明文密码、写配置前自动转向 keyx、看到明文立即报警并带你轮换**。

## 安装

```bash
npx skills add elonsolar/key-x-skill     # 自动装进检测到的 agent（Claude Code/Cursor/Codex/OpenCode…）
```

没有技能机制的引擎：把 [`skills/keyx-keys/AGENTS-SNIPPET.md`](skills/keyx-keys/AGENTS-SNIPPET.md) 粘进项目的 `AGENTS.md` / `.cursorrules`。

还需要 keyx 二进制本体（缺了 AI 会引导你装）：

```bash
curl -fsSL https://raw.githubusercontent.com/elonsolar/key-x/main/install.sh | bash    # macOS / Linux
irm  https://raw.githubusercontent.com/elonsolar/key-x/main/install.ps1 | iex          # Windows
```

## 有几个技能，怎么触发

当前 **1 个技能：`keyx-keys`**，内含三类东西，触发方式不同：

| 类型 | 内容 | 怎么触发 |
|---|---|---|
| **自动反射**（2 条） | 写入前转向 · 识别即报警 | **全自动**——AI 写配置/读文件时遇到密钥场景自己介入，你不用提 keyx |
| **场景工作流**（4 条） | 新增 / 轮换 / 粘贴事故 / 排查 | **你说对应的话**（见下），或被自动反射接进来 |
| **硬规则**（8 条） | 永不索要明文、永不落盘等底线 | 常驻，无需触发 |

## 场景与顺序

**冷启动（任何场景的第一步，自动）**：AI 先跑 `keyx --version`——没装就给你上面那条安装命令，装好再继续。

### 新项目 · AI 遇到密码会怎么做？（自动，你零操作）

你只要说"帮我接 OpenAI / 配数据库"。AI **不会**问你要密码，而是：
让你在终端 `keyx set <名称>` 输入密码 → AI 自己 `keyx list` 取编号写进配置 → `keyx run` 启动验证。
全程明文只在你输入那一刻存在，对话和文件里只有编号。

### 老项目 · 排查存量泄漏（你说："帮我排查这个项目的密码泄漏"）

AI 带你四步走，**每条密码确认后再下一条**：

1. **盘点**——扫描 .env/配置/源码/文档，输出只有"文件:行号 + 变量名"（值打码）；
2. **逐条修复**——你去源头发新值 → 终端 `keyx set` 存新值 → AI 改配置为新编号 → `keyx run` 验证通过 → 你回源头撤销旧值；
3. **无法轮换的**明说残留风险，不悄悄略过；
4. **验收清零**——重扫零明文、清单无待办、完整跑通一次 `keyx run`，三条全过才算完。

### 粘贴了密码 / AI 看到明文（自动报警）

AI 第一句话就是："这段明文已泄漏，无法撤回，唯一补救是轮换"——然后直接带你走轮换流程。不复述、不落盘、不假装删掉就没事。

### 忘记密码了 / 想取回（你说："帮我把 XX 密码找回来"）

AI 让你自己跑 `keyx peek <编号或名称>`——系统弹窗确认，值只进剪贴板（45 秒自动清空），AI 全程拿不到。

## 多人项目注意

`kx_` 编号指向**本机**密库，跨机器无效：把 `.keyx.toml` 加进 `.gitignore`，团队每人自己 `keyx set` 建条目、写自己的映射。

## 仓库结构

```
skills/keyx-keys/SKILL.md            # 技能本体
skills/keyx-keys/AGENTS-SNIPPET.md   # 无技能机制引擎的精简版
```

符合 [skills CLI](https://github.com/vercel-labs/skills) 的发现规范（`skills/<name>/SKILL.md`）。

版本兼容：skill v1 ↔ keyx 二进制 ≥ 1.3（会话内 `keyx --version` 自动握手）。
