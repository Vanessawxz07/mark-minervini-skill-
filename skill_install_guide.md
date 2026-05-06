# Skill 安装、打包与 GitHub 版本管理指南

> 适用对象：Vanessa（首次创建 skill）
> 适用 skill：mark-minervini
> 编写日期：2026-05-06

---

## 第一章：理解安装机制

`npx skills add` 做了两件事：

```
你的项目目录/
├── .agents/skills/mark-minervini/    ← ① 实体文件放这里（SKILL.md 等）
└── .claude/skills/mark-minervini     ← ② 符号链接，指向上面
```

Claude Code 读取 `.claude/skills/` 下的内容来加载 skill。符号链接的意思是：实际内容只存一份，Claude Code 从链接找到它。

---

## 第二章：本地安装（三种方式，选一种）

### 方式 A — 最简单：`npx skills add` 指向本地目录（推荐）

```bash
# 先切到你的投资系统项目目录
cd "C:/Users/ASUS/MyInvestmentSystem"

# 指向本地的 skill 目录安装（--yes 跳过确认提示）
npx skills add "C:/Users/ASUS/Downloads/个人/项目1- 投资交易系统/skill sets/nvwa-mark minervini" --yes
```

安装成功后会看到：
```
✓ ~/MyInvestmentSystem/.agents/skills/mark-minervini
  symlinked: Claude Code
Done!
```

验证安装：
```bash
ls ".claude/skills/"
# 应该能看到 mark-minervini
```

---

### 方式 B — 手动安装（理解原理用，或 npx 出问题时）

```bash
cd "C:/Users/ASUS/MyInvestmentSystem"

# 1. 创建目录
mkdir -p ".agents/skills/mark-minervini"

# 2. 复制文件
cp "C:/Users/ASUS/Downloads/个人/项目1- 投资交易系统/skill sets/nvwa-mark minervini/SKILL.md" \
   ".agents/skills/mark-minervini/SKILL.md"

cp "C:/Users/ASUS/Downloads/个人/项目1- 投资交易系统/skill sets/nvwa-mark minervini/SKILL_zh.md" \
   ".agents/skills/mark-minervini/SKILL_zh.md"

# 3. 创建符号链接（Windows Git Bash 语法）
ln -s "$(pwd)/.agents/skills/mark-minervini" ".claude/skills/mark-minervini"
```

---

### 方式 C — 从 GitHub 安装（GitHub 仓库建好后使用）

```bash
cd "C:/Users/ASUS/MyInvestmentSystem"
npx skills add your-github-username/mark-minervini-skill --yes
```

---

## 第三章：打包为 ZIP 文件

ZIP 用途：备份、发给他人、存档特定版本。

### 标准目录结构（打包前先确认）

```
nvwa-mark-minervini/
├── SKILL.md              ← 主文件（必须有，npx skills add 读取这个）
├── SKILL_zh.md           ← 中文详细参考版
├── quality_report.md     ← 质量报告
├── 01_seed_materials.md  ← 种子材料（蒸馏基础）
├── skill_install_guide.md ← 本文件
├── README.md             ← GitHub 展示用（见第四章）
└── changelog.md          ← 版本记录（见第四章）
```

### 打包命令

```bash
cd "C:/Users/ASUS/Downloads/个人/项目1- 投资交易系统/skill sets"

# 打包，文件名带版本号
powershell Compress-Archive -Path "nvwa-mark minervini" -DestinationPath "mark-minervini-v1.0.zip"
```

也可以用 Windows 资源管理器：右键 `nvwa-mark minervini` 文件夹 → 压缩为 ZIP 文件，手动改名为 `mark-minervini-v1.0.zip`。

**命名规范**：`mark-minervini-v1.0.zip`、`mark-minervini-v1.1.zip`——每次更新前先压缩旧版本存档。

---

## 第四章：GitHub 版本管理（完整流程）

### 4.1 首次创建 GitHub 仓库

**Step 1**：在本地初始化 Git 仓库

```bash
cd "C:/Users/ASUS/Downloads/个人/项目1- 投资交易系统/skill sets/nvwa-mark minervini"

git init
git add SKILL.md SKILL_zh.md quality_report.md 01_seed_materials.md skill_install_guide.md
git commit -m "feat: initial distillation v1.0 — Mark Minervini SEPA skill"
```

---

**Step 2**：创建 README.md（GitHub 展示页）

在 skill 目录新建 `README.md`，内容如下：

```markdown
# Mark Minervini 认知操作系统 Skill

**女娲蒸馏作品** | SEPA 趋势交易方法论 | v1.0

## 适用场景
- 美股 B/C 仓候选股趋势筛选
- VCP 形态确认 + Pivot Point 入场参数
- 市场环境姿态校准（FTD / Distribution Day / VIX）

## 不适用场景
- A 仓长持（3–5 年）
- A 股市场
- 加密资产

## 安装
\`\`\`bash
npx skills add your-username/mark-minervini-skill
\`\`\`

## 文件说明
- `SKILL.md` — 可安装的主 skill 文件
- `SKILL_zh.md` — 中文完整参考版
- `quality_report.md` — 蒸馏质量验证报告
- `01_seed_materials.md` — 种子材料（事实基线）
```

写完后：

```bash
git add README.md
git commit -m "docs: add README"
```

---

**Step 3**：在 GitHub 网站创建新仓库

1. 打开 https://github.com/new
2. Repository name：`mark-minervini-skill`
3. Description：`Mark Minervini SEPA方法论蒸馏 | 女娲Skill`
4. 选 **Public**（这样 `npx skills add` 才能直接安装）
5. **不要**勾选 "Initialize this repository"（本地已经 init 了）
6. 点 Create repository

---

**Step 4**：推送到 GitHub

```bash
git remote add origin https://github.com/你的用户名/mark-minervini-skill.git
git branch -M main
git push -u origin main
```

推送成功后，任何人可以通过以下命令安装：

```bash
npx skills add 你的用户名/mark-minervini-skill
```

---

### 4.2 日常更新流程（用不同 AI 工具优化后）

**Step 1**：更新 SKILL.md 里的版本号

```yaml
---
name: mark-minervini
version: "1.1"     ← 改这里
...
---
```

**Step 2**：在 changelog.md 记录改了什么

```markdown
# Changelog

## v1.1 — 2026-05-XX
**改进来源**：Cursor（用 GPT-4o 补充了 IBD Live 2025/10 访谈内容）
- 新增 IBD Live 2025/10 访谈要点
- 更新 2025 市场主题描述
- 修正 STEM Model 触发条件

## v1.0 — 2026-05-06
- 初始蒸馏版本，基于种子材料 2026-04-28
```

**Step 3**：提交并推送

```bash
cd "C:/Users/ASUS/Downloads/个人/项目1- 投资交易系统/skill sets/nvwa-mark minervini"

git add .
git commit -m "feat: v1.1 — 补充IBD Live 2025/10内容 + STEM Model优化"
git push
```

**Step 4**：打 Git tag 标记稳定版本（方便回滚）

```bash
git tag v1.1
git push origin v1.1
```

**Step 5**：更新本地 Claude Code 安装

```bash
cd "C:/Users/ASUS/MyInvestmentSystem"

# 从 GitHub 重装（拉取最新版）
npx skills add 你的用户名/mark-minervini-skill --yes

# 或者，如果是本地路径安装的，直接覆盖文件即可（链接指向原目录，文件改了自动生效）
```

---

### 4.3 版本冲突处理（多个 AI 工具各自改过怎么办）

假设你用 Claude Code 改了一版，又用 Cursor 改了另一版，想合并：

```bash
# 为每个工具的改动创建独立分支
git checkout -b improve/cursor-session-0506
# （在这里做 Cursor 的改动，保存文件）
git add . && git commit -m "improve: Cursor优化表达DNA章节"

# 回到主分支
git checkout main

# 把 Cursor 版本合并进来
git merge improve/cursor-session-0506
```

如果合并有冲突，Git 会标注冲突位置（`<<<<<<`），你手动打开文件选择保留哪个版本，然后：

```bash
git add .
git commit -m "merge: 整合Cursor优化内容"
```

---

## 第五章：安装验证

安装后在 Claude Code 里输入"Minervini"或"VCP"，验证 skill 是否加载：

```
✓ 回复里提到了 7-8% 止损、Trend Template 8 条 → 安装成功
✗ 没有体现 Minervini 风格 → 检查链接是否正确（见下方诊断命令）
```

诊断命令：

```bash
ls -la "C:/Users/ASUS/MyInvestmentSystem/.claude/skills/mark-minervini"
# 应该显示：-> /c/Users/ASUS/MyInvestmentSystem/.agents/skills/mark-minervini
```

---

## 快速检查清单（首次操作）

| 步骤 | 命令 | 预计时间 |
|---|---|---|
| 本地安装（马上可用） | `npx skills add "路径/nvwa-mark minervini" --yes`（在 MyInvestmentSystem 目录运行） | 1 分钟 |
| 打 ZIP 存档 | 右键压缩 → 改名 `mark-minervini-v1.0.zip` | 30 秒 |
| 本地 git init | `git init` + `git add` + `git commit` | 2 分钟 |
| GitHub 建仓库 | github.com/new → push | 5 分钟 |

之后每次更新的流程：**改文件 → 改版本号 → 写 changelog → git commit + push → 重装 skill**，约 3 分钟。
