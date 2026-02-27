# 来源：搜索结果综合整理（多来源）
原始链接：https://docs.cursor.com/zh-Hant/context/mcp / https://juejin.cn/post/7488168037675139082 等

核心要点：
- MCP 官方文档确认：Cursor 支持 stdio、SSE、Streamable HTTP 三种传输方式
- 前置要求：Cursor 版本需 0.45.6 或更高；Node.js 18 或更高版本；部分工具需要 Git
- 配置入口（Windows）：Ctrl+Shift+J → Tools & Integrations → New MCP Servers
- 配置入口（macOS）：Cmd+Shift+J → 同上
- 推荐 MCP Server 清单（入门必装）：
  - `@modelcontextprotocol/server-fetch`：网页内容抓取（无需 Token，最适合入门）
  - `@modelcontextprotocol/server-github`：GitHub 集成（需 Personal Access Token）
  - `@modelcontextprotocol/server-filesystem`：文件系统操作
  - `@modelcontextprotocol/server-sequential-thinking`：增强 AI 推理能力
- Cursor MCP 目录（一键安装）：cursor.com/cn/docs/context/mcp/directory
- 掘金社区实践：技术方案先行再编码；接口调试时提供 demo 代码；活用 Docs 和 Rules 工具
- Windows 小白注意：路径分隔符用双反斜杠或正斜杠；重启 Cursor 后配置才生效

关键概念：Cursor MCP 目录、一键安装 Install Links、版本要求、Windows 路径配置
