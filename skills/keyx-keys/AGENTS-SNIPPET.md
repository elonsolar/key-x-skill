# Key-X 精简规则（引擎无关版）

> 用途：粘进项目的 `AGENTS.md` / `.cursorrules` / 系统提示词，供没有 skills 机制的引擎
> （Codex、OpenCode、Cursor 等）使用。完整版见同目录 `SKILL.md`。

## 密钥安全规则（keyx）

公理：明文一旦进入对话或文件，即视为已泄漏，不可撤回，唯一补救是轮换。

1. 永不向用户索要明文密钥。你要的只有两种东西：`keyx set` 输出的 kx_ 编号、源头操作完成的确认。
2. 即将创建含密钥字段的配置文件（.env/config/settings/docker-compose）、或正要让用户"发来密码"时——
   不留明文占位符、不开口要明文。改说："请在终端运行 `keyx set <名称>`，输入密码即可"，
   然后自己运行 `keyx list` 取回编号写入配置。
3. 在任何位置看到疑似明文密钥（用户粘贴、文件、命令输出）——第一句话告知已泄漏，然后走轮换：
   源头发新值 → 用户终端 `keyx set` 存新值 → 配置换新编号 → `keyx run` 验证 → 源头撤销旧值。
4. 永不把明文写入任何文件；不 cat .env / printenv / grep 密码；扫描输出只报 文件:行号 + 变量名。
5. 启动项目一律 `keyx run <命令>`——明文只进子进程环境变量，不进对话、不落盘。
6. 用户要取回密码：让用户自己跑 `keyx peek <编号或名称>`（系统弹窗确认，值只进剪贴板，45 秒自动清空）。
7. kx_ 编号是机器本地的：把 `.keyx.toml` 加进 `.gitignore`；团队各自 `keyx set` 建条目、写自己的映射。
8. 零侵入：不把 `keyx run` 写进 package.json scripts / Dockerfile / CI 等共享文件——运行时敲；仅当用户明确要求团队统一使用 keyx 时才写入。

冷启动：先跑 `keyx --version`；没有就装——
macOS/Linux: `curl -fsSL https://raw.githubusercontent.com/elonsolar/key-x/main/install.sh | bash`
Windows: `irm https://raw.githubusercontent.com/elonsolar/key-x/main/install.ps1 | iex`
