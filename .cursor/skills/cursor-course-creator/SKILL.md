---
name: cursor-course-creator
description: Generate Cursor AI programming course content in two formats - PPT outline for Gamma AI tool and word-for-word speaking scripts matching Wayne's teaching style. Use when user mentions creating course content, PPT slides, speaking scripts, 课程文案, 逐字稿, Gamma PPT, or wants to produce Cursor teaching materials.
---

# Cursor 编程课程文案与逐字稿生成

## 概述

生成两种课程内容：
1. **PPT 文案** — 结构化 Markdown，直接输入 Gamma AI 生成演示文稿
2. **逐字稿** — 模仿 Wayne 老师口语风格的完整讲课稿

## 工作流程

### Step 1: 收集课程信息

向用户确认以下内容（使用 AskQuestion 或对话收集）：

- **课题名称**：本节课的标题（如"Cursor 的四种开发模式"）
- **目标受众**：零基础 / 有基础 / 混合
- **核心知识点**：3-6 个要讲的重点
- **是否有演示项目**：如有，提供项目路径或描述
- **生成类型**：仅 PPT 文案 / 仅逐字稿 / 两者都要
- **预计时长**：5分钟 / 10分钟 / 15分钟 / 20分钟以上

### Step 2: 查阅参考材料

1. 阅读 [style-reference.md](style-reference.md) 获取 Wayne 老师的语言风格规范
2. 阅读 [ppt-template.md](ppt-template.md) 获取 Gamma PPT 文案的结构模板
3. 如果用户提供了相关参考资料或代码文件，先阅读理解

### Step 3: 生成 PPT 文案（如需要）

按 [ppt-template.md](ppt-template.md) 模板生成 Markdown 格式文案，要求：

- 用 `---` 分隔每张幻灯片/卡片（Gamma 用此识别分页）
- 每张卡片包含一个 `##` 标题 + 要点内容
- 适当使用表格对比、流程图（Mermaid）、代码块
- 内容翔实：每个知识点需有 **概念解释 + 为什么重要 + 具体示例**
- 总卡片数根据时长：5分钟≈8-10张，10分钟≈15-18张，15分钟≈20-25张，20分钟≈28-35张

### Step 4: 生成逐字稿（如需要）

严格遵循 [style-reference.md](style-reference.md) 中的风格规范：

- 完整的口语化文本，可直接用于录制
- 保持 Wayne 老师的标志性表达习惯
- 每个知识点遵循结构：概念引入 → 类比解释 → 实操/示例 → 小结过渡
- 根据时长控制字数：5分钟≈1500字，10分钟≈3000字，15分钟≈4500字，20分钟≈6000字

### Step 5: 输出与交付

- PPT 文案保存为 `{课题编号} {课题名}_PPT文案.md`
- 逐字稿保存为 `{课题编号} {课题名}_逐字稿.md`
- 默认保存到 `myspeech/` 目录下（与历史逐字稿同级）
- 输出后询问用户是否需要调整

## 质量检查清单

### PPT 文案
- [ ] 每张卡片有明确的单一主题
- [ ] 包含至少一个对比表格或流程图
- [ ] 技术概念附带具体示例或代码
- [ ] 开头有课程概览卡片，结尾有总结+下节预告卡片
- [ ] 用 `---` 正确分隔每张卡片

### 逐字稿
- [ ] 开头使用 Wayne 标志性问候语
- [ ] 不使用"那"字起句，改用多样化的自然衔接词过渡
- [ ] 技术术语后附口语化解释
- [ ] 包含至少2个生活化类比
- [ ] 结尾有总结 + 下节预告 + "好，谢谢大家"
- [ ] 通篇使用"我们"而非"你"来拉近距离
