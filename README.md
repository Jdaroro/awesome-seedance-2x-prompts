# 🎬 Seedance 2.0 & 2.5 Prompt Engineering Toolkit

> **让 Seedance 提示词从“描述想法”升级为“可执行的导演指令”。**

面向 **Seedance 2.0 / 2.5、Dreamina、AI Video、Prompt Engineering** 的中英双语资料库：包含可直接安装的提示词优化 Skill、英文 Prompt Guide，以及多模态素材映射、镜头组织、关键帧、视频编辑、声音编辑与视频延长工作流。

![Seedance 2.0](https://img.shields.io/badge/Seedance-2.0-2563EB)
![Seedance 2.5](https://img.shields.io/badge/Seedance-2.5-7C3AED)
![Language](https://img.shields.io/badge/Language-中文%20%7C%20English-0F766E)
![License](https://img.shields.io/badge/License-MIT-16A34A)

> [!IMPORTANT]
> 本项目是社区整理项目，与字节跳动、火山引擎、BytePlus 或 Seedance 官方无隶属、授权或背书关系。官方资料请以原始网站的最新版本为准。

## 内容

| Skill | 用途 |
| --- | --- |
| [`sd2-pe`](skills/sd2-pe/SKILL.md) | 优化 Seedance 2.0 多模态视频提示词 |
| [`sd25-pe`](skills/sd25-pe/SKILL.md) | 优化 Seedance 2.5 文生视频、多参考、关键帧、分镜、编辑与延长提示词 |

## 🌍 English Prompt Guide

- [Dreamina Seedance 2.5 Prompt Guide & Skill](docs/third-party/Dreamina-Seedance-2.5-Prompt-Guide-and-Skill.md) — 英文指南 Markdown 快照
- [BytePlus 官方在线版本](https://docs.byteplus.com/en/docs/ModelArk/2607689) — 建议优先查看最新版

## 安装

将需要的 Skill 文件夹复制到你的 Codex Skills 目录，然后重新启动或刷新 Codex。

```text
skills/
├── sd2-pe/
│   └── SKILL.md
└── sd25-pe/
    └── SKILL.md
```

也可以只下载单个 `SKILL.md`，放入同名 Skill 文件夹中。

## 使用示例

```text
请用 sd2-pe 优化这条 Seedance 2.0 提示词：……
```

```text
请用 sd25-pe 把这段故事整理成 Seedance 2.5 可直接使用的提示词：……
```

## 📚 官方资料

官方中文与英文资料统一维护在 [官方教程索引](docs/official-guides.md)。仓库已归档 4 份中文官方 PDF，并为英文资料保留在线链接和 Markdown 快照。归档资料仅供学习与检索，版权归原始权利人所有。

## 贡献

欢迎通过 Issue 或 Pull Request 提交纠错、兼容性改进和使用案例。提交内容前请确认你拥有相应的发布权利，且不要上传 API Key、个人数据或无再分发许可的第三方文件。

## 许可

本仓库原创内容采用 [MIT License](LICENSE)。`docs/third-party/` 与 `docs/official-pdfs/` 下的官方资料、第三方名称及商标不受 MIT License 许可，详见 [NOTICE](NOTICE.md)。
