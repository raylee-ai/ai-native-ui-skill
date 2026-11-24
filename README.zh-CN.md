# AI-Native UI Skills

**简体中文** · [English](README.md)

> 给 Codex 用的 **AI 原生界面 Skill 合集**，专注**黑白美学**：单色令牌、弹簧动效、中文排版、AI 专属组件，每个 Skill 自带演示与自检脚本。

<p align="center">
  <img src="skills/anui/docs/media/hero.gif" alt="ANUI demo（OLED 主题）" width="720">
</p>

**在线站点**：https://raylee-ai.github.io/ai-native-ui-skill/

## Skills

| Skill | 简介 | 演示 | 安装（用户级） |
|---|---|---|---|
| [ANUI](skills/anui/README.zh-CN.md) | OLED / HC 单色令牌 + 8 个弹簧预设 + 中文排版 + 21 个 AI-native 组件 + Playwright 自检 | [Demo](https://raylee-ai.github.io/ai-native-ui-skill/skills/anui/demo/) | `npx degit raylee-ai/ai-native-ui-skill/skills/anui ~/.agents/skills/anui` |

## 只安装一个 Skill 到 Codex

Codex 从 `~/.agents/skills/<name>/`（用户级）或项目内 `.agents/skills/<name>/`（项目级）读取 Skill。目录名须与 `SKILL.md` frontmatter 中的 `name` 一致。

```bash
# 方式一：degit（只下载该子目录，不含 git 历史）
npx degit raylee-ai/ai-native-ui-skill/skills/anui ~/.agents/skills/anui     # 用户级
npx degit raylee-ai/ai-native-ui-skill/skills/anui .agents/skills/anui       # 项目级

# 不使用 npx 的方式：从稀疏检出复制到新的用户级目录
git clone --depth 1 --filter=blob:none --sparse https://github.com/raylee-ai/ai-native-ui-skill /tmp/ai-native-ui-skill
git -C /tmp/ai-native-ui-skill sparse-checkout set skills/anui
mkdir -p ~/.agents/skills
test ! -e ~/.agents/skills/anui && cp -R /tmp/ai-native-ui-skill/skills/anui ~/.agents/skills/anui
```

选择一种安装范围。项目级命令在目标项目根目录运行。`npx` 仅用于下载。独立目录包含 Skill 指令、参考文档、许可与离线演示，使用时无需检出仓库，也无需 npm 依赖、Playwright 或 Python。把随包提供的 [AGENTS.md 配置片段](skills/anui/AGENTS.md) 粘贴到项目根目录的 `AGENTS.md`。

## 仓库结构

```text
skills/
  anui/                  ANUI Skill（SKILL.md、AGENTS.md、references/、demo/、scripts/、docs/media/）
site/index.html          GitHub Pages 首页（Skill 列表）
scripts/build-site.mjs   组装 Pages 站点 → _site/（首页 + skills/<name>/demo/）
.github/workflows/       CI：npm run check 通过后部署 Pages
LICENSE  NOTICE  THIRD_PARTY_LICENSES.md  CONTRIBUTING.md  CHANGELOG.md
```

## 开发与自检

以下命令在克隆的仓库根目录（含 `package.json`）运行。使用 Node.js 20 或更高版本及 npm。项目声明 Node ≥18，但锁定的 Playwright 和 Tailwind 工具要求 ≥20。对比度检查需要 Python 3（命令为 `python3`，也可用 `PYTHON` 指定解释器）；缺少解释器时脚本会将对比度标为跳过。

```bash
npm ci
npx playwright install chromium
npm run check
npm run build:site
```

按已提交的锁文件全新安装时使用 `npm ci`。新增或更新依赖时使用 `npm install`，并审查锁文件变化。若使用已有的兼容 Chromium，可把 `CHROME_PATH` 设为其可执行文件路径。

用浏览器直接打开[本地演示](skills/anui/demo/index.html)，CSS、JavaScript 和字体都已随包提供。修改 `skills/anui/demo/src/app.css` 后运行 `npm run build:css`。

默认自检覆盖 6 个中文主题及 2 次英文检查，将 `check.json`、`tokens.json` 和 Python 生成的 `contrast.md` 写入 `skills/anui/reports/`。用 `SHOTS=1 npm run check` 可将 `shot-*.png` 截图保存到同一目录。开发时可用 `THEMES=hc,oled` 选择中文检查主题，用 `EN_THEMES=hc,oled` 选择英文检查主题（`none` 跳过英文）。可选的 `npm run perf` 将 `perf.json` 写入该目录。

`npm run build:site` 使用站点首页和各 Skill 的演示重新生成本地 Pages 输出目录 `_site/`。

## 新增 Skill

未来的 UI Skill 以兄弟目录加入 `skills/<name>/`：带 frontmatter 的 `SKILL.md`、中英双语 README、自检脚本，并在上表登记。详见 [CONTRIBUTING.md](CONTRIBUTING.md#新增-ui-skill--adding-a-new-ui-skill)。

## 许可

[MIT](LICENSE)。第三方素材见 [`THIRD_PARTY_LICENSES.md`](THIRD_PARTY_LICENSES.md) 与 [`NOTICE`](NOTICE)。
