# Cursor 高级用法：MCP 让 AI 连接外部世界

本节课带你从零掌握 MCP（Model Context Protocol）——Cursor 最强大的扩展能力，让 AI 助手从「代码建议者」进化为「主动执行者」。

**学完你能做到：**
- 理解 MCP 的核心原理和架构
- 独立配置 Fetch、GitHub、数据库等常用 MCP Server
- 掌握安全使用和高效提示的最佳实践

---

## 你有没有遇到过这些痛点？

> 让 AI 帮你处理项目任务，却要不停地解释：数据库表结构是什么、文件在哪里、接口怎么调用……

- AI 不知道你的数据库里有什么数据
- AI 看不到你的文件系统有哪些文件  
- AI 无法直接调用你的 API 获取实时数据
- 每次都要手动复制粘贴信息，**效率极低**

**MCP 就是为解决这些问题而生的。**

---

## 什么是 MCP？

**MCP = Model Context Protocol**（模型上下文协议）

- 由 **Anthropic** 公司推出的开放协议
- 为 AI 连接外部工具提供**统一标准接口**

> 💡 **一个类比帮你秒懂：**  
> MCP 就像给 AI 装了一个 **USB-C 接口**。  
> 以前每个设备充电口都不一样，USB-C 出现后一口通吃。  
> MCP 也是这个思路——一套标准，连接所有工具。

---

## 传统 AI vs 启用 MCP 的 Cursor

| 能力 | 传统 AI 助手 | 启用 MCP 的 Cursor |
|------|------------|-----------------|
| 访问外部工具 | ❌ 无法访问 | ✅ 连接任意工具 |
| 获取实时数据 | ❌ 无法实时获取 | ✅ 实时查询 |
| 执行系统操作 | ❌ 不支持 | ✅ 文件/命令执行 |
| 自定义工作流 | ❌ 固定能力 | ✅ 无限扩展 |

**核心变化：AI 从「被动建议者」→「主动执行者」**

---

## MCP 工作原理

**三层架构：**

```
用户（Cursor）→ MCP Client → MCP Server → 外部服务
                                          （数据库/API/文件系统）
```

**完整调用链（对用户透明）：**

1. 用户输入需求
2. AI 解析意图，判断需要调用外部工具
3. 向 MCP Server 请求可用工具列表
4. AI 选择合适工具，发起请求
5. MCP Server 执行操作，返回结果
6. AI 整合结果，生成最终回复

> 你只需要正常对话，AI 自动决定何时调用工具——**整个过程完全透明**。

---

## 两种传输方式，如何选择？

| 传输方式 | 特点 | 适合场景 |
|---------|------|---------|
| **stdio** | 本地运行，Cursor 自动管理 | 个人开发、新手入门 ✅ |
| **SSE / HTTP** | 网络通信，支持远程部署 | 团队共享、远程数据库 |

**推荐新手：优先使用 stdio 方式，配置最简单。**

---

## 配置 MCP：前置准备

在开始配置之前，确认以下条件：

- ✅ **Cursor 版本** ≥ 0.45.6（在设置中检查版本）
- ✅ **Node.js** ≥ 18（命令行输入 `node -v` 检查）
- ✅ **配置文件位置**已知：

```
全局配置（所有项目生效）：~/.cursor/mcp.json
项目配置（仅当前项目）：.cursor/mcp.json
```

**打开配置界面：**  
Windows/Linux：`Ctrl+Shift+J` → Tools & Integrations → New MCP Servers  
macOS：`Cmd+Shift+J` → 同上

---

## 配置文件结构

所有 MCP Server 的配置都遵循同一套格式：

```json
{
  "mcpServers": {
    "服务器名称": {
      "command": "npx",
      "args": ["-y", "@包名/server-xxx"],
      "env": {
        "API_TOKEN": "你的密钥"
      }
    }
  }
}
```

- `command`：执行的命令（通常是 `npx`）
- `args`：参数，填 MCP Server 的包名
- `env`：环境变量，用来传递 **Token、密码等敏感信息**

---

## 实战 1：配置 Fetch Server（最简单，立即上手）

**功能：** 让 AI 直接抓取任意网页内容，无需手动复制粘贴

**无需 Token，5 分钟完成配置：**

```json
{
  "mcpServers": {
    "fetch": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-fetch"]
    }
  }
}
```

**保存后重启 Cursor，测试：**
> 「帮我抓取 https://docs.cursor.com 的主要内容并总结」

AI 会自动调用 Fetch 工具获取内容——**你只需要给个链接。**

---

## 实战 2：配置 GitHub MCP（开发者必装）

**功能：** 直接在 Cursor 里管理 GitHub 仓库、Issue、PR

**第一步：获取 GitHub Token**
1. GitHub → Settings → Developer settings → Personal access tokens
2. 生成 Token，勾选 `repo` 权限

**第二步：配置 mcp.json**
```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_你的token"
      }
    }
  }
}
```

**配置后你可以直接说：**
- 「帮我创建一个新分支 feature/login」
- 「查看仓库里标记为 bug 的未解决 Issue」

---

## 实战 3：数据库直连（效率提升最明显）

**功能：** AI 自动分析真实表结构，生成精确 SQL，无需手动解释数据库设计

**以 MySQL 为例：**
```json
{
  "mcpServers": {
    "mysql": {
      "command": "node",
      "args": ["/path/to/mysql-mcp-server/index.js"],
      "env": {
        "DB_HOST": "localhost",
        "DB_USER": "root",
        "DB_PASSWORD": "your_password",
        "DB_NAME": "your_database"
      }
    }
  }
}
```

**配置后的对话示例：**
> 「查询用户表中最近 7 天注册的用户，生成一个列表展示组件」

AI 会自动连接数据库、分析表结构、生成精确 SQL + 完整组件代码。

---

## 4 个必装 MCP Server 清单

| MCP Server | 用途 | 是否需要 Token |
|-----------|------|-------------|
| `server-fetch` | 网页内容抓取 | ❌ 无需 |
| `server-github` | GitHub 集成 | ✅ Personal Token |
| `server-filesystem` | 文件系统操作 | ❌ 无需 |
| `server-sequential-thinking` | 增强 AI 推理能力 | ❌ 无需 |

> **推荐入门路径：** fetch → filesystem → github → 数据库

---

## 高效使用技巧：让 AI 100% 调用工具

**模糊提示 vs 明确提示：**

❌ 不够明确：「帮我获取一些天气数据」

✅ 明确有效：「使用 OpenWeatherMap API 获取北京未来 5 天天气预报，以表格形式展示」

**核心原则：**
- 说清楚你想用**哪个工具**或**哪个数据源**
- 提供足够的**上下文**（数据库名、仓库名、文件路径）
- 复杂任务可以**组合多个工具**：「先搜索最新方案，然后在项目里实现它」

---

## 安全使用三原则

使用 MCP 会涉及 Token、数据库密码等敏感信息，一定要注意：

**① 防止 Token 泄露**
```bash
# 在 .gitignore 中添加
.cursor/mcp.json
```

**② 用环境变量传递敏感信息**  
密钥永远放在 `env` 字段，不要硬编码在 args 里

**③ 最小权限原则**  
- GitHub Token：只勾选需要的权限
- 数据库账号：如只需查询，仅开读权限
- 定期轮换 Token，设置过期时间

---

## 常见问题排查

| 问题 | 原因 | 解决方法 |
|------|------|---------|
| MCP 配置不生效 | 未重启 Cursor | 保存配置后必须**重启 Cursor** |
| AI 不调用工具 | 提示不够明确 | 在对话中**明确说明**要用的工具 |
| JSON 格式错误 | 多余逗号/引号 | 用 JSON 验证工具检查格式 |
| Token 权限不足 | 权限范围太小 | 检查 Token 的权限范围 |
| Windows 路径问题 | 路径分隔符 | 用 `/` 或 `\\` 代替单个 `\` |

---

## 本节总结

**MCP 的核心价值：**

- 🔌 **统一接口**：一套协议连接所有外部工具
- 🤖 **智能调用**：AI 自动判断何时使用工具，无需手动触发
- ♾️ **无限扩展**：从数据库到 GitHub，从文件系统到 API，能力边界由你定义

**推荐学习路径：**
1. 先配置 Fetch Server，感受 MCP 的威力
2. 添加 GitHub MCP，提升日常开发效率
3. 根据项目需求，逐步接入数据库等专业 Server

**延伸资源：**
- Cursor MCP 官方文档：cursor.com/docs/context/mcp
- MCP Server 目录（一键安装）：cursor.com/cn/docs/context/mcp/directory
- MCP 协议仓库：github.com/modelcontextprotocol/mcp
