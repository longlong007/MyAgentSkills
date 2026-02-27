# 来源：Cursor MCP使用教程：完整配置指南与推荐Server【2025最新实测】
原始链接：https://www.cursor-ide.com/blog/cursor-mcp-guide-2025

核心要点：
- MCP（Model Context Protocol）是 Anthropic 推出的开放协议，让 Cursor 中的 AI 能访问外部工具和服务
- 传统 AI 编程助手无法访问外部工具，启用 MCP 的 Cursor 可无限扩展能力
- MCP 工作流程：用户输入 → AI 解析意图 → 请求工具列表 → AI 选择工具 → MCP Server 执行 → 返回结果 → AI 生成最终响应（对用户透明）
- 三种配置方式：① 内置 MCP Server（新手推荐）② 连接开源 MCP Server（npm 安装）③ 自建 MCP Server（进阶）
- 支持三种传输方式：stdio（本地单用户）、SSE（多用户）、Streamable HTTP（远程部署）
- 推荐 MCP Server：Sequential Thinking（思维增强）、Tavily Search（网络搜索）、Database Connector（数据库）、File System Manager（文件管理）、API Gateway（API网关）
- 实战场景：连接 MySQL/PostgreSQL 数据库、外部 API 调用、文件自动化操作
- 最佳实践：明确指示工具使用意图、组合多个 MCP 工具、提供足够上下文
- 常见问题：连接失败检查防火墙和URL；工具不被调用时使用更明确提示；保护敏感数据用环境变量传递

关键概念：MCP Server、stdio/SSE/Streamable HTTP、工具调用链、mcp.json 配置文件、环境变量安全
