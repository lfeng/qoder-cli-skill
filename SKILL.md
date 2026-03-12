---
name: qoder-cli
description: OpenClaw 原生 ACP 集成 Qoder CLI。通过 @openclaw/acpx 插件实现，支持聊天中直接调用 Qoder 进行代码开发。
metadata:
  {
    "openclaw":
      {
        "emoji": "🤖",
        "requires": { "anyBins": ["qodercli"] },
        "install":
          [
            {
              "id": "qodercli",
              "kind": "node",
              "package": "@qoder-ai/qodercli",
              "bins": ["qodercli"],
            },
            {
              "id": "acpx-plugin",
              "kind": "openclaw",
              "command": "openclaw plugins install @openclaw/acpx",
            },
          ],
      },
  }
---

# Qoder CLI - OpenClaw ACP 集成

通过 ACP 协议对接 Qoder CLI，在聊天中直接调用 Qoder 进行代码开发。

---

## 🎯 使用场景

**适合使用 Qoder：**

- 💬 描述需求，自动编写代码
- 🔄 多轮对话的持久化开发任务
- 🔍 代码审查和重构

**不适合：**

- 简单的单行代码修复（直接 `edit`）
- 仅读取代码（用 `read` 工具）

---

## 🚀 快速开始

### 自然语言请求（推荐）

直接在聊天中描述需求：

```
用 qoder 帮我开发一个博客系统
用 qoder 审查 ./src 目录的 TypeScript 类型安全
用 qoder 创建一个持久会话，帮我重构路由模块
```

### 通过 sessions_spawn（编程调用）

```json
// 持久会话（推荐用于长任务）
{
  "task": "重构 InkBoard 的路由模块",
  "runtime": "acp",
  "agentId": "qoder",
  "thread": true,      // Discord 线程绑定
  "mode": "session"    // 持久会话
}

// 一次性任务
{
  "task": "写个 Python 脚本批量重命名图片",
  "runtime": "acp",
  "agentId": "qoder",
  "mode": "run"
}
```

---

## 📋 会话模式

| 模式                          | 适用场景             | 说明                         |
| ----------------------------- | -------------------- | ---------------------------- |
| `run` + `mode: "run"`         | 简单问答、一次性任务 | 执行完即关闭，无历史         |
| `session` + `mode: "session"` | 代码修改、系统开发   | 持久会话，多轮对话保持上下文 |
| `session` + `thread: true`    | Discord 线程绑定开发 | 隔离上下文，适合长期项目     |

---

## 🔄 会话恢复 (resumeSessionId)

使用 `resumeSessionId` 可以恢复之前的 ACP 会话，继续之前的上下文。

### 使用场景

- 🖥️ **跨设备继续**：在电脑上开启的 Qoder 会话，切到手机继续
- ⏸️ **中断恢复**：会话因网关重启或空闲超时中断，从断点继续
- 🔁 **CLI 到 Agent**：在 CLI 开始的开发任务，交给 Agent 继续

### 用法

```json
{
  "task": "继续之前的任务 — 修复剩余的测试失败",
  "runtime": "acp",
  "agentId": "qoder",
  "resumeSessionId": "<之前的会话ID>"
}
```

### 获取会话 ID

```bash
# 列出最近的 ACP 会话
/acp sessions
```

### 注意事项

- `resumeSessionId` 需要 `runtime: "acp"`，子代理运行时不支持
- 恢复后会重新加载对话历史，agent 会获得完整的上下文
- `thread` 和 `mode` 对新会话仍然有效（`mode: "session"` 仍需 `thread: true`）
- 目标 agent 必须支持 `session/load`（Qoder 支持）
- 如果会话 ID 不存在，会报错而非静默创建新会话

---

## 🎯 使用示例

### 示例 1：快速开发

```
用 qoder 帮我写个 Python 脚本，批量重命名文件夹里的图片
```

### 示例 2：Spec 模式开发

```
用 qoder 开发一个个人博客系统，技术栈 Next.js + MDX
```

### 示例 3：代码审查

```
用 qoder 审查 ./src 目录的代码，检查 TypeScript 类型安全
```

### 示例 4：持久会话开发

```
用 qoder 创建一个持久会话
→ 先设计数据库模型
→ 然后写后端 API
→ 最后做前端页面
```

---

## ⚙️ 前置要求

确保以下组件已安装并配置：

1. **Qoder CLI**

   ```bash
   npm install -g @qoder-ai/qodercli
   ```

2. **ACPX 插件**

   ```bash
   openclaw plugins install @openclaw/acpx
   ```

3. **ACP 配置**（在 OpenClaw 配置中启用）
   - `acp.enabled: true`
   - `acp.backend: "acpx"`
   - `acp.defaultAgent: "qoder"`
   - `acp.allowedAgents` 包含 `"qoder"`

4. **acpx 配置**（`~/.acpx/config.json`）
   ```json
   {
     "defaultAgent": "qoder",
     "auth": {
       "QODER_PERSONAL_ACCESS_TOKEN": "YOUR_QODER_PERSONAL_ACCESS_TOKEN"
     },
     "agents": {
       "qoder": {
         "command": "qodercli --acp"
       }
     }
   }
   ```

---

## 🔧 故障排查

### 验证配置

```bash
# 检查 Qoder CLI 安装
qodercli --version

# 检查 ACPX 插件
openclaw plugins list | grep acpx

# 检查 ACP 配置
openclaw config get acp --raw
```

### 常见问题

| 问题                | 原因               | 解决方案                                |
| ------------------- | ------------------ | --------------------------------------- |
| `Agent not allowed` | qoder 不在允许列表 | 将 `"qoder"` 添加到 `acp.allowedAgents` |
| `Qoder 提示未登录`  | API Key 未配置     | 在 `~/.acpx/config.json` 中配置 Token   |
| `ACP 会话立即关闭`  | Gateway 配置问题   | 查看 `openclaw logs --tail 50`          |

---

## 📐 架构原理

```
你 (飞书/钉钉/Discord)
  ↓
OpenClaw Gateway
  ↓
ACP 协议 → ACPX 插件
  ↓ stdio
Qoder CLI (--acp)
  ↓ HTTP
Qoder AI 服务
```

- **ACP**: 标准化代理通信协议
- **ACPX**: OpenClaw 的 ACP 后端实现
- **Qoder CLI**: 接收 ACP 指令，调用 Qoder AI 服务

---

## ⚠️ 注意事项

1. **权限控制**：ACP 会话是非交互式的，无 TTY 界面批准权限提示。通过 acpx 插件配置权限模式：

### permissionMode

控制 harness agent 无需提示即可执行的操作：

| 值              | 行为                                          |
| --------------- | --------------------------------------------- |
| `approve-all`   | 自动批准所有文件写入和 shell 命令             |
| `approve-reads` | 只自动批准读取；写入和执行需要交互式 TTY 确认 |
| `deny-all`      | 拒绝所有权限提示                              |

### nonInteractivePermissions

控制当权限提示应该显示但无可用 TTY 时的行为：

| 值     | 行为                                         |
| ------ | -------------------------------------------- |
| `fail` | 中止会话，抛出 `AcpRuntimeError`（**默认**） |
| `deny` | 静默拒绝权限并继续（优雅降级）               |

### 配置方式

```bash
# 设置权限模式
openclaw config set plugins.entries.acpx.config.permissionMode approve-all

# 设置无交互时的行为（可选）
openclaw config set plugins.entries.acpx.config.nonInteractivePermissions deny
```

> ⚠️ **重要**：OpenClaw 默认 `permissionMode=approve-reads` + `nonInteractivePermissions=fail`。在 ACP 非交互式会话中，任何触发权限提示的写入或执行操作都可能失败并抛出 `AcpRuntimeError: Permission prompt unavailable in non-interactive mode`。
>
> 如果需要限制权限，建议将 `nonInteractivePermissions` 设为 `deny`，让会话优雅降级而非崩溃。

### 任务类型推荐

| 任务类型          | 推荐 permissionMode | 推荐 nonInteractivePermissions |
| ----------------- | ------------------- | ------------------------------ |
| 快速原型开发      | `approve-all`       | `fail`                         |
| 正式项目开发      | `approve-all`       | `deny`                         |
| 代码审查/只读任务 | `approve-reads`     | `fail`                         |
| 高安全要求环境    | `deny-all`          | `deny`                         |

2. **超时设置**：长任务建议设置合理的 `timeoutSeconds`
3. **上下文隔离**：使用 `thread: true` 将不同项目的开发会话隔离

---

## 🔗 相关资源

- [OpenClaw 文档](https://docs.openclaw.ai)
- [Qoder 官网](https://qoder.com)
- [ACP 协议](https://agentclientprotocol.com)
- [Qoder CLI GitHub](https://github.com/qoder-ai/qoder-cli)

---

**状态:** ✅ OpenClaw 原生 ACP 集成  
**版本:** 2026-03-12 (新增 permissionMode + resumeSessionId 配置说明)
