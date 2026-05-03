# novel-skill

一个可复用的中文长篇小说创作 Skill，用于规划、续写、逐章生成和自动校验。

这个仓库把小说创作流程拆成可执行的阶段化工作流，适合放进本地 AI skill 环境中反复使用，而不是只写一次性的短篇故事。

## 能做什么

`novel-skill` 主要解决的是“把一部长篇小说稳定写完”：

- 分阶段收集题材、主角、冲突、世界观和风格要求
- 自动生成人物档案、大纲和写作计划
- 按章节逐步写作，而不是一次性糊整本
- 支持中断后继续写
- 用脚本检查章节字数，发现不达标时回修

## 工作流

整个 Skill 分为 5 个阶段：

1. 初始化
2. 故事设定问答
3. 规划与确认
4. 逐章写作
5. 自动校验与修复

详细流程和写作指南都在 `references/` 目录里。

## 目录结构

```text
novel-skill/
├── SKILL.md
├── NOTICE.md
├── agents/
│   └── openai.yaml
├── references/
│   ├── flows/
│   └── guides/
└── scripts/
    └── check_chapter_wordcount.py
```

## 产出约定

Skill 默认在当前工作区下创建小说项目目录：

```text
./chinese-novelist/
```

典型产出文件包括：

- `00-人物档案.md`
- `01-大纲.md`
- `02-写作计划.json`
- `第01章-*.md`
- `第02章-*.md`

## 脚本

检查单章字数：

```bash
python3 scripts/check_chapter_wordcount.py <chapter-file>
```

检查整部小说所有章节：

```bash
python3 scripts/check_chapter_wordcount.py --all <project-dir>
```

## 适配说明

这个版本不是原样镜像，而是针对当前本地 Codex 风格环境做过适配：

- 把 UI 专用问答方式改成普通对话式交互
- 默认执行模式收敛为 `serial`
- 去掉当前环境不适用的团队协作执行路径
- 脚本命令统一为 `python3`

## 来源说明

本仓库基于上游项目改造而来：

- `PenglongHuang/chinese-novelist-skill`
- 上游地址：https://github.com/PenglongHuang/chinese-novelist-skill

更多说明见 [NOTICE.md](NOTICE.md)。

## 许可证提醒

在我整理这个仓库时，上游仓库没有通过 GitHub License API 暴露标准许可证记录。

因此当前仓库先保留来源说明与改造说明，不擅自附加新的开源许可证文本。若后续确认了上游授权方式，再补充正式 `LICENSE` 会更稳妥。
