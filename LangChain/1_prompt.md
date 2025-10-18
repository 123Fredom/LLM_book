 

 

# 📘 Prompt Engineering 笔记

## 📑 目录

* [1. 提示原则 Guidelines](#1-提示原则-guidelines)

  * [1.1 清晰、具体](#11-清晰具体)
  * [1.2 给模型时间思考](#12-给模型时间思考)
  * [1.3 注意局限](#13-注意局限)
  * [1.4 英文 Prompt 更稳定](#14-英文-prompt-更稳定)
* [2. 迭代优化 Iterative](#2-迭代优化-iterative)
* [3. 常见应用场景](#3-常见应用场景)

  * [3.1 文本概括 Summarizing](#31-文本概括-summarizing)
  * [3.2 推断 Inferring](#32-推断-inferring)
  * [3.3 文本转换 Transforming](#33-文本转换-transforming)
  * [3.4 文本扩展 Expanding](#34-文本扩展-expanding)
  * [3.5 聊天机器人 Chatbot](#35-聊天机器人-chatbot)
* [4. Prompt 模板](#4-prompt-模板)
* [5. LangChain 中的 Prompt 用法](#5-langchain-中的-prompt-用法)

  * [5.1 PromptTemplate](#51-prompttemplate)
  * [5.2 FewShotPromptTemplate](#52-fewshotprompttemplate)
  * [5.3 ChatPromptTemplate](#53-chatprompttemplate)
  * [5.4 MessagePromptTemplate](#54-messageprompttemplate)
  * [5.5 MessagesPlaceholder](#55-messagesplaceholder)
* [6. 总结](#6-总结)

 
---

## 1. 提示原则 Guidelines

### 1.1 清晰、具体

* **明确目标**

  * ❌ 模糊：`帮我写一篇文章`
  * ✅ 清晰：`写一篇 500 字的文章，主题是人工智能在教育中的应用，分为引言、正文、结论`
* **分步骤说明**

  * ✅ 例：`先生成提纲，再写正文，最后给出总结`

### 1.2 给模型时间思考

* **逐步推理（Chain of Thought）**

  * ✅ 例：`请一步一步推理并解释答案，再给出最终结果`
* **要求多种答案**

  * ✅ 例：`请提供三种解决方案，并说明优缺点`

### 1.3 注意局限

* 可能出现 **幻觉（hallucination）**
* 默认无联网能力
* 基于概率生成而非真正理解
  👉 **解决办法**：多轮迭代、验证结果、结合外部工具

### 1.4 英文 Prompt 更稳定

* 学术/科研/编程建议用英文
* 可先写中文需求，再翻译成英文提示

---

## 2. 迭代优化 Iterative

Prompt 设计是一个 **迭代过程**：

1. 写初版 Prompt
2. 查看输出
3. 增加细节或限制
4. 循环改进

✅ 技巧：

* **自我反思**：`请检查并改进你上一个回答`
* **角色扮演**：`你是一位律师，请起草一份合同草案`

---

## 3. 常见应用场景

### 3.1 文本概括 Summarizing

* 长文压缩：`将以下文章压缩成 200 字摘要`
* 层级总结：一句话 → 三句话 → 详细摘要
* 针对性总结：只总结核心论点和研究结论

### 3.2 推断 Inferring

* 情绪分类：`判断评论情绪（积极/消极/中立）`
* 意图推测：`分析邮件潜在意图并推测下一步行动`

### 3.3 文本转换 Transforming

* 格式转换：`转为 JSON`、`Python → Java`
* 风格转换：`改写成学术风格`、`适合 10 岁小朋友理解`

### 3.4 文本扩展 Expanding

* 扩写：`将短文扩展到 500 字`
* 改写：`生成三个不同版本的开场白`
* 创意延伸：`基于大纲生成三种结局`

### 3.5 聊天机器人 Chatbot

* 设定身份：`你是一名历史老师`
* 场景化对话：`模拟面试官，提 5 个问题`
* 连续记忆：保持一致人设与风格

---

## 4. Prompt 模板

```text
角色设定：你现在扮演一位 [角色身份]，具备 [领域/风格] 知识与表达方式。  
环境背景：任务发生在 [场景/应用背景]，需要考虑 [限制条件]。  
任务说明：
  - [任务目标]
  - [操作要求]
  - [约束条件]  
输入数据：以下是用户提供的文本/问题：...  
输出要求：
  - 输出语言：[中文/英文/双语]  
  - 输出风格：[正式 / 口语化 / 简洁 / 详细]  
  - 输出格式：[Markdown / JSON / 表格]  
  - 特殊要求：[字数限制 / 包含代码 / 分步骤推理]  
示例（可选）：  
  - 输入：...  
  - 输出：...  
```

---

## 5. LangChain 中的 Prompt 用法

### 5.1 PromptTemplate（单轮模板）

```python
#PromptTemplate 提示词模板
from langchain.prompts import PromptTemplate
prompt = PromptTemplate.from_template("疾病: {disease}, 症状: {symptom}, 药物: {medicine}")
#from_template把一串带花括号的普通字符串变成一个PromptTemplate对象，花括号里面是占位符
print(prompt.format(disease="糖尿病", symptom="尿血", medicine="格列美脲"))
#format把占位符替换成你给的汉字，返回最终字符串。
#返回结果：疾病: 糖尿病, 症状: 尿血, 药物: 格列美脲
```

### 5.2 FewShotPromptTemplate（示例提示）

```python
examples = [{"word": "cat", "translation": "猫"}]
example_prompt = PromptTemplate.from_template("英文: {word} -> 中文: {translation}")
```

### 5.3 ChatPromptTemplate（多轮对话）

```python
from langchain.prompts import ChatPromptTemplate
chat_prompt = ChatPromptTemplate.from_messages([
    ("system", "你是医学助手。"),
    ("human", "病人患有{disease}，出现{symptom}，如何治疗？")
])
```

### 5.4 MessagePromptTemplate（精细化控制）

```python
from langchain.prompts.chat import SystemMessagePromptTemplate, HumanMessagePromptTemplate
```

### 5.5 MessagesPlaceholder（对话历史）

```python
from langchain.prompts import MessagesPlaceholder

chat_with_memory = ChatPromptTemplate.from_messages([
    ("system", "你是一个助手。"),
    MessagesPlaceholder(variable_name="history"),
    ("human", "继续回答: {question}")
])
```

---

## 6. 总结

* **Prompt 工程本质**：把需求 → 清晰指令
* **核心技巧**：清晰目标、结构化、迭代优化
* **应用场景**：概括、推断、转换、扩展、对话
* **在 LangChain 中**：可用模板、示例、对话历史等方式管理 Prompt

👉 Prompt = **写给 AI 的编程语言**
👉 优秀 Prompt = **好用的接口设计**

---
 
