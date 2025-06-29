## [Dual AI Chat](https://aistudio.google.com/app/prompts?state=%7B%22ids%22:%5B%221wS-wmXT_J4S-sfYxY1wItwh4UuV4STEk%22%5D,%22action%22:%22open%22,%22userId%22:%22102038139080022776927%22,%22resourceKeys%22:%7B%7D%7D)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![React](https://img.shields.io/badge/React-19-blue?logo=react)](https://react.dev/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.7-blue?logo=typescript)](https://www.typescriptlang.org/)
[![Vite](https://img.shields.io/badge/Vite-6.2-blue?logo=vite)](https://vitejs.dev/)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-3-blue?logo=tailwindcss)](https://tailwindcss.com/)

一个先进的聊天应用，演示了一种独特的对话范式：用户的查询首先由两个不同的人工智能角色进行辩论和提炼，然后才提供最终的综合答案。该项目利用 Google Gemini API 驱动一个逻辑型 AI (Cognito) 和一个怀疑型 AI (Muse)，它们协作生成更健壮、准确和经过严格审查的响应。

### ✨ 应用截图 (Application Screenshot)
![Application Screenshot](https://github.com/user-attachments/assets/a862f8c8-2da4-406c-a0db-269ff52138bc)

## 核心功能 (Core Features)

-   **🤖 双 AI 辩论系统 (Dual AI Debate System):** 用户输入会触发 Cognito (逻辑型 AI) 和 Muse (怀疑型 AI) 之间的内部讨论。这种辩证过程旨在减少 AI 幻觉，探索多个角度，并在给出最终答案前对信息进行压力测试。
-   **📝 共享记事本 (Shared Notepad):** 一个供两个 AI 使用的协作空间，用于在讨论过程中记录关键点、起草解决方案或存储上下文。记事本的内容会包含在后续的 AI 提示中，实现了跨轮次的状态保持。支持完整的 Markdown 预览。
-   **🖼️ 多模态输入 (Multimodal Input):** 用户可以上传图片和文本。AI 能够理解并讨论图片内容，将其与文本查询结合起来进行分析。
-   **🚫 停止生成 (Stop Generation):** 用户可以在任何时候中断并取消 AI 的生成过程。
-   **⚙️ 高度可配置 (Highly Configurable):**
    -   **模型选择 (Model Selection):** 可在多个 Gemini 模型之间即时切换。
    -   **讨论模式 (Discussion Modes):** 支持“固定轮次”对话，或由 AI 自主决定何时结束讨论的“AI 驱动”模式。
    -   **预算控制 (Budget Control):** 可在“优质”（为支持的模型启用更高的 `thinkingBudget` 以获得更精细的结果）和“标准”（使用 API 默认值以获得更快的响应）之间切换，以平衡响应质量与速度/成本。
-   **🔁 健壮的错误处理 (Robust Error Handling):** 包含针对 API 调用失败的自动重试逻辑。更重要的是，如果自动重试失败，应用会提供一个**手动重试**按钮。此功能会从失败点继续，恢复整个对话的上下文（包括讨论历史、记事本内容和原始查询），无需用户重新开始整个流程。

## 🤖 工作原理 (How It Works)

该应用的核心是一个结构化的、多步骤的提示链，旨在模拟一个严谨的审查过程：
```
1. 👤 用户输入 (文本 + 可选图片)
   │
   └──> 🤖 Cognito: 进行初步分析，并向 Muse 提出观点。
   │
   └──> 💬 内部讨论循环 (Internal Discussion Loop):
   │   │
   │   ├──> 🤖 Muse: 挑战假设、质疑逻辑并探索替代方案。
   │   │
   │   └──> 🤖 Cognito: 以逻辑论证和支持数据回应挑战。
   │
   * (此循环根据配置的“讨论模式”重复进行)
   │
   └──> 🤖 Cognito: 综合整个讨论和记事本内容，形成最终的、全面的答案。
   │
   └──> 👤 用户接收最终答案
```

## 🛠️ 技术栈 (Tech Stack)

-   **前端框架 (Frontend Framework):** [React](https://react.dev/) 19
-   **语言 (Language):** [TypeScript](https://www.typescriptlang.org/)
-   **构建工具 (Build Tool):** [Vite](https://vitejs.dev/)
-   **AI 服务 (AI Service):** [Google Gemini API](https://ai.google.dev/) (`@google/genai`)
-   **样式 (Styling):** [Tailwind CSS](https://tailwindcss.com/) (通过 `index.html` 中的 CDN 引入)
-   **依赖管理 (Dependency Management):** 通过 `index.html` 中的 [Import Map](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/script/type/importmap) 直接加载 ES 模块，无需捆绑依赖。
-   **图标 (Icons):** [Lucide React](https://lucide.dev/)
-   **Markdown 处理 (Markdown Processing):** [Marked](https://marked.js.org/) & [DOMPurify](https://github.com/cure53/DOMPurify)


## 📁 项目结构 (Project Structure)

项目遵循一个清晰的、面向功能的结构，旨在分离关注点。

```
/
├── src/
│   ├── components/          # 共享的 React 组件
│   ├── hooks/               # 自定义 React hooks 业务逻辑
│   ├── services/            # 外部服务集成
│   ├── utils/               # 辅助函数和工具
│   ├── App.tsx              # 主应用组件
│   ├── constants.ts         # 存放系统提示、模型ID和其它配置常量
│   ├── index.tsx            # React 应用的入口点
│   └── types.ts             # 全局 TypeScript 类型定义
│
├── .env.local               # 本地环境变量 (用于 API 密钥)
├── index.html               # 应用的 HTML 入口
├── package.json             # 项目依赖和脚本
└── ...                      # 其他配置文件
```

## 📄 许可证 (License)

该项目采用 [MIT 许可证](LICENSE)授权。
