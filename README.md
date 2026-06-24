# Remotion Video Production Skill

一个用于 Remotion 视频制作的 Codex Skill，覆盖脚本规划、HTML 分镜、Remotion 转译、多语言版本、预览验收、音频处理、最终渲染和交付 QA。

这个 Skill 适合制作“旁白驱动”的产品视频、技术解释视频、功能演示视频和 UI 动效视频。它不绑定任何公司、产品、音频供应商或设计系统；你可以使用自己的品牌素材、音频工具、视觉规范和渲染环境。

## 能帮你做什么

- 从脚本到最终成片的旁白优先工作流。
- 将 HTML 分镜作为 Remotion 的视觉源代码复用。
- 帧驱动动画和可复现的关键帧截图验收。
- SVG 折线、虚线、路径和流光的提前露出、断帧、底稿暴露检查。
- 多语言版本的独立时长重算、文字溢出检查和渲染验收。
- 音频 / BGM 处理，但不强制绑定某个供应商。
- 最终 MP4 清晰度、音画同步、`ffprobe` 和封面帧嵌入检查。

## 安装方式

克隆到你的 Codex skills 目录：

```bash
mkdir -p ~/.codex/skills
git clone https://github.com/ihueni/remotion-video-production.git ~/.codex/skills/remotion-video-production
```

显式调用：

```text
Use $remotion-video-production to plan and QA this Remotion video project.
```

如果你的 Codex 环境支持自动发现 Skill，`SKILL.md` 里的描述也可以在相关任务中自动触发。

## 推荐工作方式

建议使用 Codex 作为视频制作的编排和 QA 代理。Codex 可以读取项目 changelog、应用本 Skill 的规则、修改 HTML 和 Remotion 代码、启动本地预览、抽取关键帧、检查音频 / 渲染输出，并记录验证结果。

你仍然可以继续使用自己熟悉的音频供应商、设计工具、编辑器和渲染环境。本 Skill 只负责把流程、边界和验收做稳定。

## 目录结构

```text
remotion-video-production/
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    ├── animation-qa.md
    ├── html-remotion.md
    ├── i18n-visual-qa.md
    ├── rendering.md
    └── workflow.md
```

`SKILL.md` 保持精简，只做触发、路由和核心约束；详细规则放在 `references/`，由 Codex 按任务类型读取。

## 音频和 BGM

本 Skill 不强制要求某个 TTS、配音、BGM 或音乐生成工具。

当用户没有现成供应商时，默认建议：

- 中文旁白：Fish Audio。
- 英文旁白：ElevenLabs。
- 批量或自动化制作：优先选择支持 API、有足够 credits、商业授权清晰的付费方案。

如果需要 AI 生成 BGM，本 Skill 会指导你根据脚本和分镜提炼音乐提示词，而不是指定某个音乐生成平台。

## 发布前检查

1. `SKILL.md` 保持简洁，详细规则放入 `references/`。
2. 不写入公司、产品、客户、内部路径或隐私数据。
3. 确认 `SKILL.md` 链接到的 reference 文件都存在。
4. 确认 frontmatter 只包含 `name` 和 `description`。

## License

MIT

---

## English

A Codex Skill for planning, building, localizing, previewing, rendering, and QA-ing narration-driven Remotion video projects.

This skill is designed for product videos, technical explainers, feature demos, and UI animation videos built from scripts, voiceover, HTML storyboards, frame-driven animation, screenshots, SVG paths, BGM, and final MP4 delivery. It is product-neutral and company-neutral: bring your own brand assets, audio provider, visual system, and project conventions.

### What It Helps With

- Narration-first workflow from script to final render.
- HTML storyboard scenes reused as Remotion visual source.
- Frame-driven animation and deterministic still-frame QA.
- SVG path reveal checks for early endpoints, broken lines, and unwanted base-path exposure.
- Multilingual versions with independent timing, text overflow checks, and render QA.
- Audio/BGM handling without locking users into a specific vendor.
- Final MP4 quality checks, ffprobe validation, and cover-frame embedding.

### Installation

Clone this repository into your Codex skills directory:

```bash
mkdir -p ~/.codex/skills
git clone https://github.com/ihueni/remotion-video-production.git ~/.codex/skills/remotion-video-production
```

Invoke it explicitly:

```text
Use $remotion-video-production to plan and QA this Remotion video project.
```

### Recommended Workflow

Use Codex as the coordinating agent for this workflow. Codex can read the project changelog, apply the skill references, edit HTML and Remotion code, run local previews, extract frame stills, check audio/render output, and record verification results.

You can still use your preferred audio provider, design tools, editor, and render environment. The skill focuses on making the workflow, boundaries, and QA gates reliable.

### Audio And BGM

This skill does not require a specific TTS, voice, BGM, or music generation provider.

Suggested defaults when the user has no existing provider:

- Chinese narration: Fish Audio.
- English narration: ElevenLabs.
- Repeated production: choose a paid plan with API access, sufficient credits, and appropriate commercial-use terms.

For AI-generated BGM, the skill teaches how to derive a music prompt from the script and scene plan rather than naming a specific music generation service.
