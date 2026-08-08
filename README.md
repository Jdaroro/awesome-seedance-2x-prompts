# Seedance 2.x Prompt Skills

面向 Seedance 2.0 与 Seedance 2.5 的提示词优化 Skill 合集，包含多模态素材映射、镜头组织、关键帧、视频编辑、声音编辑与视频延长等工作流。

> [!IMPORTANT]
> 本项目是社区整理项目，与字节跳动、火山引擎、BytePlus 或 Seedance 官方无隶属、授权或背书关系。官方资料请以原始网站的最新版本为准。

## 内容

| Skill | 用途 |
| --- | --- |
| [`sd2-pe`](skills/sd2-pe/SKILL.md) | 优化 Seedance 2.0 多模态视频提示词 |
| [`sd25-pe`](skills/sd25-pe/SKILL.md) | 优化 Seedance 2.5 文生视频、多参考、关键帧、分镜、编辑与延长提示词 |

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

## 官方资料

官方中文与英文资料统一维护在 [官方教程索引](docs/official-guides.md)。仓库只提供官方页面链接，不镜像官方 PDF，以便读者获取最新版并尊重原始资料的版权声明。

## 贡献

欢迎通过 Issue 或 Pull Request 提交纠错、兼容性改进和使用案例。提交内容前请确认你拥有相应的发布权利，且不要上传 API Key、个人数据或无再分发许可的第三方文件。

## 许可

本仓库原创内容采用 [MIT License](LICENSE)。第三方名称、商标、官方文档及其链接不因本项目许可证而获得重新授权，详见 [NOTICE](NOTICE.md)。
