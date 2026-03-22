# feishu-scheduled-message

配置飞书 MCP 实现定时自动发送消息到群聊的 WorkBuddy Skill。

## 功能

- 定时自动推送新闻早报
- 定时发送报告链接到飞书群
- 支持自定义定时任务（每天/每周/每小时）

## 前置要求

1. 飞书应用（App ID + App Secret）
2. 安装依赖：`pip install feishu-enhance-mcp`
3. 机器人已添加到目标群聊

## 配置步骤

详细配置见 [SKILL.md](./SKILL.md)

### 1. 配置 MCP

在 `~/.workbuddy/mcp.json` 添加两个 MCP 服务器：
- `feishu-mcp` - 文档处理
- `feishu-enhance-mcp` - 消息发送

### 2. 获取群 ID

飞书群 → 右上角设置 → 群设置 → 查看群 ID

### 3. 创建定时任务

告诉 WorkBuddy：
> 创建一个每天早上8点自动推送财经新闻到飞书群的任务

WorkBuddy 会自动调用本 Skill 完成配置。

## 使用示例

- "每天早上自动推送新闻早报到XX群"
- "每周一自动发送周报链接到群里"
- "每小时推送一次AI新闻"

## 相关文档

- [SKILL.md](./SKILL.md) - 完整配置指南
- [飞书开放平台](https://open.feishu.cn/) - 应用创建与管理
