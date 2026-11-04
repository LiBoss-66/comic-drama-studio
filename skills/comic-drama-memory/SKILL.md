---
name: comic-drama-memory
description: Project-scoped local file and cross-episode memory protocol for the comic drama studio, with single-writer updates and per-project isolation.
---

# 漫剧项目与跨集记忆协议

本协议让团队在用户本地项目中工作，并保证跨集设定一致。**项目数据保存在用户工作目录，不保存在专家包内。**

## 1. 项目根目录

- 用户指定路径时，使用用户给出的绝对路径。
- 未指定时，默认：`{当前工作目录}/comic-project/{project-id}/`。
- `project-id` 使用简短、稳定的英文 kebab-case；同一部剧后续集数必须复用同一目录。
- 主理人每次调度成员时必须传入 `project_root` 绝对路径，禁止只写相对路径。

## 2. 标准目录

```text
{project_root}/
  project.json                 # 项目元数据与生成开关
  memory/
    bible.md                   # 剧情圣经
    characters.md              # 角色资产库
    timeline.md                # 已验收集数与伏笔状态
  scripts/                     # ep-001.md
  storyboards/                 # ep-001.md
  voice/                       # ep-001.md
  edit/                        # ep-001.md
  reviews/                     # ep-001-quality.md / ep-001-critical.md
  assets/
    images/
    videos/
```

专家包根目录的 `memory/*.md` 仅为空白模板参考，**禁止写入真实项目剧情**。

## 3. 开工前读取

1. 主理人先定位项目：已有项目则读取；新项目才初始化，不覆盖已有文件。
2. 做第 N 集前，必须读取：`project.json`、`memory/bible.md`、`memory/characters.md`、`memory/timeline.md`。
3. 成员只读取与本任务相关的文件；缺少文件时回报主理人，不自行编造设定。
4. 项目切换时重新解析 `project_root`，禁止沿用上一项目的记忆。

## 4. 单一写入者规则

- **只有主理人写入共享记忆文件**，避免并行覆盖。
- 成员负责读取记忆、完成专业产出，并在回传中增加 `memory_updates`：
  - `bible`：新增/变更设定、伏笔。
  - `characters`：新增/变更角色字段。
  - `timeline`：本集摘要、关键事件、伏笔变化。
- 主理人在产出验收后合并更新；未通过验收的内容不得写入 timeline。
- 更新前先读取现有文件；timeline 只能追加，bible/characters 按具体小节修改。

## 5. 产出落盘

- 专业成员将产出保存到对应目录，并在回传中给出绝对路径。
- 主理人调度时明确文件名；未指定时使用 `ep-001.md` 三位集数格式。
- 后续阶段只接收文件路径和必要摘要，不重复传全部原文。
- 未经用户许可，不上传项目文件、角色设定、剧情或未发布素材到外部服务。

## 6. 生成开关

`project.json` 默认：`generation.enabled=false`。
任何图像、视频、配音、渲染调用都必须由用户另行明确要求，并先确认数量、输出路径和可用额度；未确认时只生成方案与提示词。
