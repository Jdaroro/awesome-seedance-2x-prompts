# 🎬 Seedance 2.0 & 2.5 Prompt Engineering Toolkit

[English](README.md) | [简体中文](README.zh-CN.md)

> **Turn rough ideas into production-ready directing instructions for Seedance.**

An English-first toolkit for **Seedance 2.0, Seedance 2.5, Dreamina, AI video, and prompt engineering**. It includes installable prompt-optimization Skills, a comprehensive English prompt guide, official Chinese PDF archives, and practical workflows for multimodal references, camera direction, keyframes, editing, audio, and video extension.

![Seedance 2.0](https://img.shields.io/badge/Seedance-2.0-2563EB)
![Seedance 2.5](https://img.shields.io/badge/Seedance-2.5-7C3AED)
![Language](https://img.shields.io/badge/Language-English%20%7C%20中文-0F766E)
![License](https://img.shields.io/badge/License-MIT-16A34A)

> [!IMPORTANT]
> This is an independent, community-maintained project. It is not affiliated with, endorsed by, or officially connected to ByteDance, Volcano Engine, BytePlus, Doubao, Dreamina, or Seedance. Always check the official documentation for the latest product behavior.

## Highlights

- Ready-to-install prompt optimization Skills for Seedance 2.0 and 2.5.
- English Dreamina Seedance 2.5 prompt guide with examples and asset-mapping patterns.
- Chinese official PDF archives for Seedance 2.0 and 2.5.
- Coverage of multimodal references, storyboards, keyframes, camera movement, video editing, audio editing, and extension.
- Clear licensing boundaries between original project content and third-party documentation.

## Included Skills

| Skill | Purpose |
| --- | --- |
| [`sd2-pe`](skills/sd2-pe/SKILL.md) | Optimizes multimodal prompts for Seedance 2.0 |
| [`sd25-pe`](skills/sd25-pe/SKILL.md) | Optimizes Seedance 2.5 prompts for generation, references, keyframes, storyboards, editing, audio, and extension |

## English Prompt Guide

- [Dreamina Seedance 2.5 Prompt Guide & Skill](docs/third-party/Dreamina-Seedance-2.5-Prompt-Guide-and-Skill.md) — repository Markdown snapshot
- [Canonical BytePlus page](https://docs.byteplus.com/en/docs/ModelArk/2607689) — recommended for the latest version

## Quick Start

Copy the Skill folder you need into your Codex Skills directory, then restart or refresh Codex.

```text
skills/
├── sd2-pe/
│   └── SKILL.md
└── sd25-pe/
    └── SKILL.md
```

You can also download a single `SKILL.md` and place it inside a folder with the corresponding Skill name.

### Usage examples

```text
Use sd2-pe to optimize this Seedance 2.0 prompt: ...
```

```text
Use sd25-pe to turn this story into a production-ready Seedance 2.5 prompt: ...
```

## Documentation

- [Official guide index](docs/official-guides.md)
- [Chinese PDF archive](docs/official-pdfs/README.md)
- [English Markdown guide](docs/third-party/Dreamina-Seedance-2.5-Prompt-Guide-and-Skill.md)

The archived official materials are provided for learning and reference. Their copyrights remain with their respective rights holders.

## Contributing

Issues and pull requests are welcome. Please verify that you have the right to publish submitted content, and never commit API keys, personal data, or third-party files without redistribution permission.

## License

Original project content is available under the [MIT License](LICENSE). Official documentation under `docs/third-party/` and `docs/official-pdfs/`, as well as third-party names and trademarks, is excluded from the MIT License. See [NOTICE](NOTICE.md).
