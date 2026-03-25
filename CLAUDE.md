# 项目背景

这是一个从 [firecrawl/firecrawl](https://github.com/firecrawl/firecrawl) fork 的项目。Firecrawl 是一个网页爬虫 API，能够将任意网站爬取并提取出适合 LLM/RAG 使用的结构化数据。

Firecrawl 是一个网页爬虫 API。当前代码库是一个 monorepo（单体仓库）：
 - `apps/api` 包含实际的 API 和 Worker 代码
 - `apps/js-sdk`、`apps/python-sdk` 和 `apps/rust-sdk` 是各个语言的 SDK


**核心功能**：通过 AI 爬取网页内容，输出格式化内容供 LLM 和 RAG 使用。

# 本地部署目标

将项目通过 Docker 本地部署到 Mac 电脑，配置要求：
- 项目代码运行在本地 Docker 容器中
- 大模型（LLM）调用使用火山引擎（兼容 OpenAI 协议的云服务）