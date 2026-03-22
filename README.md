# feishu-scheduled-message

配置飞书 MCP 实现定时自动发送消息到群聊的 WorkBuddy Skill。

## 功能

- 定时自动推送新闻早报
- 定时发送报告链接到飞书群
- 支持自定义定时任务（每天/每周/每小时）

## 前置要求

1. 飞书应用（App ID + App Secret）
2. Python 环境
3. 安装 feishu-enhance-mcp 包

## 安装步骤

### 步骤1: 安装 feishu-enhance-mcp

打开终端/命令行，执行：

```bash
pip install feishu-enhance-mcp
```

验证安装成功：

```bash
pip show feishu-enhance-mcp
```

### 步骤2: 配置 MCP

在 `~/.workbuddy/mcp.json` 中添加配置（详见 SKILL.md）

### 步骤3: 获取飞书群 ID

飞书群 → 右上角设置 → 群设置 → 查看群 ID

### 步骤4: 测试发送

告诉 WorkBuddy：发送测试消息到 XX 群

### 步骤5: 创建定时任务

告诉 WorkBuddy：创建一个每天早上8点推送新闻早报的任务

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

## 许可证
MIT License

## 相关文档

- [SKILL.md](./SKILL.md) - 完整配置指南
- [飞书开放平台](https://open.feishu.cn/) - 应用创建与管理
