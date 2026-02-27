# 来源：Cursor MCP 完整教程：让 AI 连接外部工具的配置方法
原始链接：https://eastondev.com/blog/zh/posts/dev/20260116-cursor-mcp-guide/

核心要点：
- MCP 类比：就像给 AI 装了一个 USB-C 接口，统一标准解决所有连接问题
- MCP vs 传统插件：传统插件是应用层功能扩展；MCP 是 AI 模型层能力扩展，AI 能智能判断何时调用工具（无需明确指令）
- MCP 核心架构：MCP Host（Cursor等AI应用）→ MCP Client（管理连接）→ MCP Server（包装外部服务）→ 外部服务（数据库/API/文件系统），使用 JSON-RPC 通信
- 配置文件位置：全局配置 `~/.cursor/mcp.json`，项目级配置 `.cursor/mcp.json`
- 配置文件结构：包含 command（执行命令）、args（参数）、env（环境变量传递敏感信息）
- 实战案例：GitHub MCP（创建PR、管理Issues、浏览分支）、Fetch Server（网页内容抓取）、MySQL MCP（查询实际数据库）
- 进阶技巧：stdio 适合本地单用户，SSE/HTTP 适合团队共享远程服务器；全局配置放通用 Server，项目配置放特有 Server
- 安全建议：mcp.json 加入 .gitignore 防止 Token 泄露；使用最小权限原则；定期更新 Token
- 性能优化：只启用需要的 Server，注意远程 Server 的网络延迟

关键概念：JSON-RPC、mcp.json 配置结构、stdio vs SSE 传输方式、环境变量安全、全局配置 vs 项目配置
