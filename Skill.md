***

name: feishu-scheduled-message
description: 配置飞书MCP实现定时自动发送消息到群聊。用于用户要求创建定时任务自动推送新闻早报、报告到指定飞书群等场景。
-------------------------------------------------------------------

# 飞书定时消息推送 Skill

## 概述

本Skill记录了成功配置飞书MCP（feishu-mcp + feishu-enhance-mcp）实现定时自动发送消息到飞书群的完整流程。

## 适用场景

- 用户要求创建定时自动推送任务
- 需要发送到飞书群
- 定时发送新闻早报、报告链接等

## 关键配置信息

### MCP配置

**文件位置**: `~/.workbuddy/mcp.json`

```json
{
  "mcpServers": {
    "feishu-mcp": {
      "command": "npx",
      "args": ["-y", "feishu-mcp", "--stdio", "--enabled-modules", "all"],
      "env": {
        "FEISHU_APP_ID": "<YOUR_APP_ID>",
        "FEISHU_APP_SECRET": "<YOUR_APP_SECRET>",
        "FEISHU_AUTH_TYPE": "tenant"
      }
    },
    "feishu-enhance-mcp": {
      "command": "<PYTHON_PATH>",
      "args": ["<PATH_TO_SCRIPT>/run_feishu_enhance_mcp.py"],
      "env": {
        "APP_ID": "<YOUR_APP_ID>",
        "APP_SECRET": "<YOUR_APP_SECRET>"
      }
    }
  }
}
```

**参数说明**:

- `<YOUR_APP_ID>`: 飞书应用的App ID
- `<YOUR_APP_SECRET>`: 飞书应用的App Secret
- `<PYTHON_PATH>`: Python解释器路径，如 `C:\Python\python.exe`
- `<PATH_TO_SCRIPT>`: 启动脚本所在目录

### 步骤0: 安装 feishu-enhance-mcp

**安装命令**:

```bash
pip install feishu-enhance-mcp
```

**验证安装**:

```bash
pip show feishu-enhance-mcp
```

### 增强版MCP启动脚本

**文件位置**: `<工作目录>/run_feishu_enhance_mcp.py`

```python
#!/usr/bin/env python
import sys
import os

os.environ["APP_ID"] = "<YOUR_APP_ID>"
os.environ["APP_SECRET"] = "<YOUR_APP_SECRET>"

from feishu_enhance_mcp.server import run
run()
```

**注意**: 必须先执行 `pip install feishu-enhance-mcp` 安装依赖包

## 执行流程

### 步骤1: 确认MCP连接

使用mcp\_get\_tool\_description确认feishu-enhance-mcp已连接。

### 步骤2: 获取群ID

如果不知道群ID，让用户在飞书中获取：

1. 打开飞书群 → 点击右上角设置 → 群设置 → 查看群ID

### 步骤3: 测试发送消息

使用lark\_send\_message测试：

```
mcp_call_tool: lark_send_message
- chat_id: <YOUR_CHAT_ID>
- text: 测试消息内容
```

### 步骤4: 创建定时任务

使用automation\_update创建：

```
automation_update mode: suggested create
- name: 任务名称（如"新闻早报自动推送"）
- prompt: 具体任务描述，包含搜索新闻、生成早报、使用lark_send_message发送到群
- rrule: 定时规则
  - 每天8点: FREQ=DAILY;BYHOUR=8;BYMINUTE=0
  - 每周一: FREQ=WEEKLY;BYDAY=MO;BYHOUR=9;BYMINUTE=0
- cwds: 工作目录
- status: ACTIVE
```

## 常见问题

### Q1: 增强版MCP启动失败

**错误**: `No module named feishu_enhance_mcp.__main__`

**解决**: 使用启动脚本方式，不要用 `-m feishu_enhance_mcp`，改用Python脚本启动

### Q2: 发送消息失败

**错误**: `Bot/User can NOT be out of the chat`

**解决**: 确认机器人已被添加到目标群聊中

### Q3: MCP连接问题

需要重启MCP连接使配置生效

## 相关权限

飞书应用需要以下权限：

- `im:message:send_as_bot` - 发送消息
- `im:message:send_multi_users` - 发送给多个用户
- `im:chat:read` - 读取群聊信息

