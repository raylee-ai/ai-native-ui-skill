# AI-Native UI Skills

**English** · [简体中文](README.zh-CN.md)

> A **collection of AI-native UI skills for Codex**, focused on a **black-and-white aesthetic**: monochrome tokens, spring motion, Chinese typography and AI-specific components. Each skill ships with a demo and self-check scripts.

<p align="center">
  <img src="skills/anui/docs/media/hero.gif" alt="ANUI demo (OLED theme)" width="720">
</p>

**Live site**: https://raylee-ai.github.io/ai-native-ui-skill/

## Skills

| Skill | Description | Demo | Install (user level) |
|---|---|---|---|
| [ANUI](skills/anui/README.md) | OLED / HC monochrome tokens + 8 spring presets + Chinese typography + 21 AI-native components + Playwright self-checks | [Demo](https://raylee-ai.github.io/ai-native-ui-skill/skills/anui/demo/?lang=en) | `npx degit raylee-ai/ai-native-ui-skill/skills/anui ~/.agents/skills/anui` |

## Install a single skill into Codex

Codex reads skills from `~/.agents/skills/<name>/` (user level) or `.agents/skills/<name>/` inside a project (project level). The folder name must match `name` in the `SKILL.md` frontmatter.

```bash
# Option 1: degit (downloads just that folder, no git history)
npx degit raylee-ai/ai-native-ui-skill/skills/anui ~/.agents/skills/anui     # user level
npx degit raylee-ai/ai-native-ui-skill/skills/anui .agents/skills/anui       # project level

# Alternative without npx: copy from a sparse checkout into a new user-level directory
git clone --depth 1 --filter=blob:none --sparse https://github.com/raylee-ai/ai-native-ui-skill /tmp/ai-native-ui-skill
git -C /tmp/ai-native-ui-skill sparse-checkout set skills/anui
mkdir -p ~/.agents/skills
test ! -e ~/.agents/skills/anui && cp -R /tmp/ai-native-ui-skill/skills/anui ~/.agents/skills/anui
```

Choose one installation scope. Run the project-level command from your project root. `npx` is only a download option. The installed Skill includes its instructions, references, licences and offline demo. Using it requires no repository checkout, npm dependencies, Playwright or Python. Copy the bundled [AGENTS.md snippet](skills/anui/AGENTS.md) into your project’s root `AGENTS.md`.

## Repository layout

```text
skills/
  anui/                  ANUI skill (SKILL.md, AGENTS.md, references/, demo/, scripts/, docs/media/)
site/index.html          GitHub Pages home (skills index)
scripts/build-site.mjs   assembles the Pages site → _site/ (home + skills/<name>/demo/)
.github/workflows/       CI: deploys Pages after npm run check passes
LICENSE  NOTICE  THIRD_PARTY_LICENSES.md  CONTRIBUTING.md  CHANGELOG.md
```

## Development and self-checks

Run the following from the cloned repository root, next to `package.json`. Use Node.js 20 or later with npm. The manifest declares Node ≥18, but the locked Playwright and Tailwind tooling require ≥20. Install Python 3 (available as `python3`, or set `PYTHON` to its executable) for the contrast check. Without it, the script marks contrast as skipped.

```bash
npm ci
npx playwright install chromium
npm run check
npm run build:site
```

Use `npm ci` for a clean install of the committed lockfile. Use `npm install` when adding or updating dependencies and review the resulting lockfile changes. If using an existing compatible Chromium, set `CHROME_PATH` to its executable.

Open [the local demo](skills/anui/demo/index.html) directly in a browser. Its CSS, JavaScript and fonts are bundled. After editing `skills/anui/demo/src/app.css`, run `npm run build:css`.

The default check exercises six Chinese themes and two English runs. It writes `check.json`, `tokens.json` and the Python-generated `contrast.md` to `skills/anui/reports/`. Use `SHOTS=1 npm run check` to also save `shot-*.png` there. For a targeted development run, `THEMES=hc,oled` selects the Chinese themes and `EN_THEMES=hc,oled` selects the English ones (`none` skips English). The optional `npm run perf` writes `perf.json` in the same directory.

`npm run build:site` regenerates the local Pages output in `_site/` from the site home and the skill demos.

## Adding a skill

Future UI skills are added as siblings under `skills/<name>/`, each with a `SKILL.md` with frontmatter, bilingual READMEs and a check script, and listed in the table above. See [CONTRIBUTING.md](CONTRIBUTING.md#新增-ui-skill--adding-a-new-ui-skill).

## Licence

[MIT](LICENSE). Third-party assets are listed in [`THIRD_PARTY_LICENSES.md`](THIRD_PARTY_LICENSES.md) and [`NOTICE`](NOTICE).
