# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 项目概述

基金从业资格考试复习助手，一个纯客户端单页应用（SPA），无需构建/安装，直接打开 `index.html` 即可使用。

## 核心架构

- **单文件应用**: 全部代码（HTML + CSS + JS）都在 `index.html` 中
- **本地存储**: 使用 localStorage 持久化数据
  - `fund_exam_progress`: 刷题进度、闪卡评分、模考成绩
  - `fund_wrong_book`: 错题本数据
- **三大科目**: 法规、私募股权、证券投资
- **数据文件**（体积较大，约 4MB）：
  - `questions.json` - 题目数据
  - `knowledge.json` - 知识点数据
  - `mock_exams.json` - 模拟试卷数据

## 应用模式

| 模式 | 功能 |
|------|------|
| 闪卡 | 翻转式记忆卡片，支持知识点/题目切换，按重要性/数字考点/易混筛选 |
| 刷题 | 随机抽取题目作答，支持只做错题/未做题 |
| 知识点 | 按章节浏览知识点，可折叠展开详情 |
| 模考 | 120 分钟限时闭卷，答题卡导航，自动评分 |
| 错题本 | 错题自动收集，连对 2 次自动移出 |
| 进度 | 各科目刷题/闪卡统计，清除进度 |

## 数据结构

### questions.json
题目对象结构：
```json
{
  "id": "唯一标识",
  "stem": "题干",
  "options": { "A": "", "B": "", "C": "", "D": "" },
  "answer": "正确答案字母",
  "analysis": "解析（可选）"
}
```

### knowledge.json
知识点对象结构：
```json
{
  "chapter": "章节名",
  "title": "标题",
  "content": "内容（Markdown格式）",
  "importance": 1-3（1=常考，2=高频，3=必考）",
  "is_number": true/false（数字考点标记）",
  "is_confusing": true/false（易混淆标记）"
}
```

### mock_exams.json
试卷结构：
```json
{
  "name": "试卷名",
  "type": "真题|预测卷|点题卷",
  "year": "年份",
  "question_count": 题数,
  "questions": [题目数组]
}
```

## 开发

无需构建。直接在浏览器打开 `index.html` 即可运行。修改 `index.html` 中的代码后刷新浏览器即可看到变化。

## 快捷键（闪卡模式）

- `←/→` - 切换卡片
- `空格/Enter` - 翻转卡片
- `1/2/3` - 评级（不会/模糊/记住了）

## 数据文件格式说明

数据文件体积较大（约 4MB），读取时需注意：
- 使用 `offset` 和 `limit` 参数分批读取
- 或搜索特定内容而非读取全文件
