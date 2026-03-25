# 项目背景

这是一个从 [firecrawl/firecrawl](https://github.com/firecrawl/firecrawl) fork 的项目。Firecrawl 是一个网页爬虫 API，能够将任意网站爬取并提取出适合 LLM/RAG 使用的结构化数据。

**核心功能**：通过 AI 爬取网页内容，输出格式化内容供 LLM 和 RAG 使用。

# 本地部署目标

将项目通过 Docker 本地部署到 Mac 电脑，配置要求：
- 项目代码运行在本地 Docker 容器中
- 大模型（LLM）调用使用火山引擎（兼容 OpenAI 协议的云服务）

# 开发指南（原始翻译）

Firecrawl 是一个网页爬虫 API。当前代码库是一个 monorepo（单体仓库）：
 - `apps/api` 包含实际的 API 和 Worker 代码
 - `apps/js-sdk`、`apps/python-sdk` 和 `apps/rust-sdk` 是各个语言的 SDK

当你需要修改 API 时，请遵循以下通用步骤：
1. 如果你想要的测试还不存在，编写一些端到端（E2E）测试来断言你的预期结果
  - 至少编写 1 个成功路径（如果有多个代码路径差异显著的成功场景，鼓励编写更多）
  - 至少编写 1 个失败路径
  - 一般来说，端到端测试（在 API 中称为 `snips`）比单元测试更受欢迎
  - 在 API 中，请始终使用 `./lib` 提供的 `scrapeTimeout` 来设置爬取超时
  - 这些测试会在多种配置下运行，你应该按以下方式对测试进行条件控制：
    - 如果测试需要 fire-engine：`!process.env.TEST_SUITE_SELF_HOSTED`
    - 如果测试需要 AI：`!process.env.TEST_SUITE_SELF_HOSTED || process.env.OPENAI_API_KEY || process.env.OLLAMA_BASE_URL`
2. 编写代码实现你的目标
3. 使用 `pnpm harness jest ...` 运行测试
  - `pnpm harness` 是一个命令，它会帮你启动 API 服务器和 Worker 来运行测试。不要手动尝试 `pnpm start`
  - 完整测试套件运行时间很长，所以你应该在本地只运行相关测试，让 CI 运行完整测试套件
4. 推送到分支，打开 PR，让 CI 运行验证你的结果
在构建 TODO 列表时请记住这些步骤。