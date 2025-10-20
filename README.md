<div align="center">

## 使用 [Mastra](https://mastra.ai) 构建的强大 AI 驱动网页代理，可轻松在开放网络中进行搜索、抓取与数据提取。

<img width="656" height="203" alt="Screenshot 2025-10-16 at 10 33 20" src="https://github.com/user-attachments/assets/05bedc40-29be-4519-843d-c1f5012af9ad" />
</div>

## 概览

该代理将 OpenAI 的 GPT-4o-mini 模型与 Bright Data 强大的 SDK 相结合，打造一名智能网页研究助手，能够：

- 使用 Google、Bing 或 Yandex 进行网页搜索，并具备反机器人/反爬虫保护
- 以干净的 Markdown 格式抓取网站内容
- 提取亚马逊产品详细信息（价格、评论、规格）
- 收集 LinkedIn 个人资料数据（经历、教育、技能）
- 通过持久化记忆维持对话上下文
- 提供准确且有据可查的回答，并附带引用

开始构建具备网页访问能力的 Mastra 代理的最简方式——只需添加你的 API 密钥，即可开启探索之旅！

## 特性

- AI 驱动智能：使用 OpenAI GPT-4o-mini 进行智能推理与回答
- Bright Data SDK 集成：与 Bright Data 强大的网络数据工具直接集成
- 多数据源：搜索引擎、网页抓取、亚马逊产品、LinkedIn 个人资料
- 反机器人保护：自动处理 CAPTCHA 与机器人检测绕过
- 持久化记忆：使用 LibSQL 维护会话历史
- 来源引用：始终提供 URL 与来源标注
- 结构化日志：内置 Pino 日志，便于监控与调试
- TypeScript 支持：完整类型定义，提升开发体验

## 架构

### 核心组件

- Web Agent（[src/mastra/agents/web-agent.ts](src/mastra/agents/web-agent.ts)）：具备网页研究能力的主 AI 代理
- Bright Data 工具（[src/mastra/tools/web-tools.ts](src/mastra/tools/web-tools.ts)）：用于网页抓取、搜索与数据提取的 SDK 集成
- Mastra 核心（[src/mastra/index.ts](src/mastra/index.ts)）：集中配置与编排

### 关键依赖

- `@mastra/core` - 代理与工作流的主框架
- `@brightdata/sdk` - Bright Data 的网页抓取与数据采集 SDK
- `@mastra/memory` - 持久化记忆管理
- `@mastra/libsql` - LibSQL 数据库适配器
- `@mastra/loggers` - 日志工具
- `zod` - 工具输入的模式校验

## 要求

### 系统要求

- Node.js：>= 20.9.0
- npm：最新版本

### 所需 API 密钥

1. Bright Data API Key：用于网页抓取与数据采集能力
2. OpenAI API Key：用于访问 GPT-4o-mini 模型

## 安装与配置

### 1. 克隆并安装

```bash
git clone https://github.com/brightdata/brightdata-mastra-tools
cd brightdata-tools-test
npm install
```

### 2. 环境配置

复制示例环境文件并添加你的 API 密钥：

```bash
cp .env.example .env
```

编辑 `.env` 并添加你的 API 密钥：

```env
OPENAI_API_KEY=your_openai_api_key_here
BRIGHTDATA_API_KEY=your_brightdata_api_key_here
```

#### 获取 API 密钥

Bright Data API Key：
1. 在 [Bright Data](https://www.bright.cn) 注册
2. 进入你的控制台
3. 生成用于 SDK 访问的 API 密钥
4. 启用所需的 Zone（若设置了 `autoCreateZones: true` 会自动创建）

OpenAI API Key：
1. 前往 [OpenAI Platform](https://platform.openai.com)
2. 进入 API keys 区域
3. 创建新的 API 密钥
4. 确保你拥有 GPT-4o-mini 模型的访问权限

### 3. 数据库设置

该代理使用 LibSQL 进行持久化记忆：
- 开发环境：在 [src/mastra/index.ts](src/mastra/index.ts) 中使用内存数据库（`:memory:`）
- 代理记忆：使用基于文件的存储（`file:../mastra.db`）保存会话历史

无需额外配置——数据库会自动创建。

## 使用方法

### 开发模式

以热重载方式启动开发服务器：

```bash
npm run dev
```

### 生产构建

构建应用：

```bash
npm run build
```

### 启动生产服务器

```bash
npm run start
```

## 代理能力

该 Web 代理旨在提供全面的网页研究能力：

### 搜索操作
- 网页搜索：在 Google、Bing 或 Yandex 上进行搜索
- 本地化结果：可指定国家代码以获取区域化结果
- 多种格式：以 HTML 或干净的 Markdown 返回结果
- 反机器人保护：自动绕过 CAPTCHA 与机器人检测

### 网页抓取
- 干净 Markdown：提取可读性强的 Markdown 内容
- 代理支持：使用特定国家代理获取有地域限制的内容
- CAPTCHA 处理：自动应对反爬保护
- 原始内容：可访问未处理的站点数据

### 亚马逊产品数据
- 产品详情：获取价格、评分、评论与规格
- 区域特定：可提供邮编获取区域定价与库存
- 全量数据：访问详尽的产品与评论信息
- 结构化输出：以干净的 JSON 返回数据

### LinkedIn 资料收集
- 专业信息：收集工作经历、教育背景与技能
- 批量处理：一次请求抓取多个个人资料
- 多种格式：以 JSON 或 JSONL 返回
- 详细资料：访问全面的职业信息

### 记忆与上下文
- 对话历史：记住先前的交互
- 上下文感知：基于已有研究结果继续深化
- 会话持久化：跨会话保持上下文
- 研究留痕：存储搜索与抓取记录以供参考

## 可用工具

该代理可使用四种强大的 Bright Data 工具：

### 1. 搜索工具
```typescript
searchTool({
  query: "your search query",
  searchEngine: "google" | "bing" | "yandex",
  country: "us",  // optional 2-letter country code
  dataFormat: "markdown" | "html"
})
```

### 2. 抓取工具
```typescript
scrapeTool({
  url: "https://example.com",
  country: "us"  // optional 2-letter country code
})
```

### 3. 亚马逊产品工具
```typescript
amazonProductTool({
  url: "https://amazon.com/dp/PRODUCTID",
  zipcode: "10001"  // optional for location-specific data
})
```

### 4. LinkedIn 批量收集资料工具
```typescript
linkedinCollectProfilesTool({
  urls: ["https://www.linkedin.com/in/profile1", "https://www.linkedin.com/in/profile2"],
  format: "json" | "jsonl"
})
```

## 项目结构

```
brightdata-tools-test/
├── src/
│   └── mastra/
│       ├── agents/
│       │   └── web-agent.ts           # Main AI agent
│       ├── tools/
│       │   └── web-tools.ts           # Bright Data SDK tools
│       └── index.ts                   # Mastra configuration
├── .env.example                       # Environment template
├── .gitignore                        # Git ignore rules
├── package.json                      # Dependencies and scripts
├── tsconfig.json                     # TypeScript configuration
└── README.md                         # This file
```

## 配置

### 代理设置

在 [src/mastra/agents/web-agent.ts](src/mastra/agents/web-agent.ts) 中进行配置：

- 模型：OpenAI GPT-4o-mini
- 记忆：基于文件的 LibSQL 持久化存储（`file:../mastra.db`）
- 工具：四个 Bright Data 工具（搜索、抓取、亚马逊、LinkedIn）
- 指令：通用网页研究，包含明确的引用规范

### 工具配置

在 [src/mastra/tools/web-tools.ts](src/mastra/tools/web-tools.ts) 中配置工具：

- API Key：所有工具均需
- 自动创建 Zone：需要时自动创建 Bright Data Zone
- 输入校验：使用 Zod 确保输入格式正确
- 错误处理：提供全面的错误信息便于调试

### 数据库配置

在 [src/mastra/index.ts](src/mastra/index.ts) 中的存储设置：

- 可观测性：可观测性数据使用内存数据库
- 代理记忆：会话历史使用基于文件的存储（相对于 `.mastra/output` 目录）

## 故障排除

### 常见问题

“Bright Data API key is required to initialize tools”
- 确保存在包含有效 `BRIGHTDATA_API_KEY` 的 `.env` 文件
- 检查 API 密钥是否过期
- 验证该密钥具备 SDK 访问权限

“OpenAI API key not found”
- 核对 `.env` 中的 `OPENAI_API_KEY`
- 确保该密钥可访问 GPT-4o-mini 模型
- 检查你的 OpenAI 账户是否有可用额度

工具初始化失败
- 查看具体失败的工具（错误信息会列出）
- 确认你的 Bright Data 账户拥有所需数据集的访问权限
- 确保 Zone 配置正确（或设置 `autoCreateZones: true`）

搜索或抓取超时
- 检查网络连接
- 查看 Bright Data 服务状态
- 考虑网络延迟与代理位置

亚马逊或 LinkedIn 工具报错
- 确保 URL 格式正确（亚马逊：必须包含 `/dp/` 或 `/gp/product/`）
- 验证 LinkedIn URL 为有效的个人资料链接
- 检查你的 Bright Data 套餐是否包含所需数据集访问

### 日志

应用使用 Pino 进行结构化日志，包含：
- 代理响应与推理
- 工具调用与返回
- 错误详情与堆栈
- 性能指标
- API 交互

在开发模式下查看日志以获取详细调试信息。

## 开发

### 添加新功能

1. 新工具：添加至 [src/mastra/tools/](src/mastra/tools/) 目录
2. 代理修改：更新 [src/mastra/agents/web-agent.ts](src/mastra/agents/web-agent.ts)
3. 配置变更：修改 [src/mastra/index.ts](src/mastra/index.ts)

### 扩展 Bright Data 工具

[web-tools.ts](src/mastra/tools/web-tools.ts) 中的 `brightDataTools` 支持：
- 选择性加载工具：使用 `excludeTools` 禁用特定工具
- 自定义客户端：修改 `createBrightDataClient` 以满足自定义配置
- 新增工具：按既有模式添加新的工具创建函数

示例：
```typescript
const tools = brightDataTools({
  apiKey: process.env.BRIGHTDATA_API_KEY!,
  excludeTools: ['linkedinCollectProfiles']  // Disable LinkedIn tool
});
```

### 测试

当前未配置测试。若要添加测试：

```bash
npm install --save-dev jest @types/jest ts-jest
```

创建 `jest.config.js`：
```javascript
module.exports = {
  preset: 'ts-jest',
  testEnvironment: 'node',
};
```

## 示例用例

### 网页研究
让代理研究某个话题，它会通过搜索与抓取工具收集最新信息：
```
"What are the latest developments in quantum computing?"
```

### 产品研究
获取亚马逊产品的详细信息：
```
"Compare the features and reviews of the top 3 noise-cancelling headphones"
```

### 竞品分析
抓取竞品网站并分析其产品与定价：
```
"Analyze the pricing structure on example.com and compare it to industry standards"
```

### 职业研究
收集用于研究的 LinkedIn 个人资料：
```
"Get the professional background of executives at [company name]"
```

## 安全与隐私

- API 密钥：切勿将 `.env` 提交到版本控制
- 数据隐私：负责任地使用工具，遵守隐私法规
- 速率限制：Bright Data 会自动处理限速
- 个人数据：避免收集不必要的个人信息

## 许可

ISC

## 支持

如遇问题：
- Mastra：参阅 [Mastra 文档](https://mastra.ai)
- Bright Data SDK：参阅 [Bright Data 文档](https://docs.brightdata.com)
- 本项目：在仓库中提交 Issue

## 参与贡献

欢迎贡献！请：
1. Fork 本仓库
2. 创建功能分支
3. 完成修改
4. 提交 Pull Request

---

使用 [Mastra](https://mastra.ai) 与 [Bright Data](https://www.bright.cn) 构建。
