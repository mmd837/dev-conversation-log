# dev-conversation-log

按话题记录 Claude Code 开发过程中你和 Claude 的完整对话。

## 解决的问题

用 Claude Code 做项目开发时，对话是宝贵的资产——需求讨论、技术选型、踩坑记录都在里面。但 Claude Code 没有内置"按话题保存对话"的功能。关掉终端，那些讨论就散落在 JSONL 文件里，想回看只能大海捞针。

这个 skill 让你随时说一句"记录这段对话"，Claude 就会把当前话题的完整对话（你和它的每一句话）保存为 Markdown 文件，按话题拆分、按序号排列。

## 效果

```
你的项目\
├── 开发对话记录\
│   ├── 01-需求确认与技术选型.md
│   ├── 02-数据库设计讨论.md
│   ├── 03-Bug排查过程.md
│   └── ...
```

每个文件包含完整的用户原话 + Claude 原话 + 结论摘要，不删减。

## 安装

```bash
# 方式一：直接下载到 Claude Code skills 目录
mkdir -p ~/.claude/skills/dev-conversation-log/references
curl -o ~/.claude/skills/dev-conversation-log/SKILL.md https://raw.githubusercontent.com/mmd837/dev-conversation-log/main/SKILL.md
curl -o ~/.claude/skills/dev-conversation-log/references/template.md https://raw.githubusercontent.com/mmd837/dev-conversation-log/main/references/template.md
```

```bash
# 方式二：手动创建
mkdir -p ~/.claude/skills/dev-conversation-log/references
```

然后把 `SKILL.md` 的内容复制到 `~/.claude/skills/dev-conversation-log/SKILL.md`，把 `references/template.md` 复制到对应位置。

## 使用方式

在 Claude Code 对话中说：

- **"记录这段对话"** — 记录当前话题
- **"记录话题"** — 同上
- **"记录对话"** — 同上

Claude 会自动：
1. 识别当前讨论的话题
2. 提取你说的每一句话 + Claude 说的每一句话（工具调用过程省略，结论保留）
3. 保存到 `~/Desktop/LDAI/claude code/<项目名>/开发对话记录/` 下，按序号命名

## 崩溃恢复

如果对话意外中断（电脑死机、Claude Code 崩溃），未记录的话题不会丢失：

1. 原始数据存在 `~/.claude/` 的 JSONL 文件中
2. 重启后说 **"上次对话断了，帮我补录"**
3. Claude 会自动读取 JSONL → 识别话题 → 对比已有文件 → 创建缺失的话题文件

## 与 export-chat 的区别

| | export-chat | dev-conversation-log |
|---|---|---|
| 触发词 | "导出聊天"、"导出对话" | "记录这段对话"、"记录话题" |
| 输出 | 整个会话一个文件 | 按话题拆分多个文件 |
| 内容 | 含工具调用记录 | 只保留对话文字和结论 |
| 用途 | 完整归档 | 开发过程按话题检索 |

## 文件结构

```
~/.claude/skills/dev-conversation-log/
├── SKILL.md              # skill 定义
└── references/
    └── template.md       # 对话记录模板
```

## 许可证

MIT
