---
来源: https://www.nodeloc.com/t/topic/96596
标题: "Lunes Host 自动保活脚本"
作者: 七转八起
分类: 互联网服务
tags:
  - discourse
  - AFF
  - 求助
  - AI
  - VPS
  - 已完成
保存时间: 2026-07-05 08:01:49
评论数: 0
---

# Lunes Host 自动保活脚本

回复可见内容

[https://github.com/oyz8/Lunes](https://github.com/oyz8/Lunes)

# Lunes Host 自动保活脚本

#  **觉得有用？给个 Star 支持一下！**

>  注册地址：[https://betadash.lunes.host/](https://betadash.lunes.host/)

定时登录 Lunes Host 并访问服务器页面，保持账号活跃，避免因长时间不活动被暂停。支持 Cloudflare 绕过、Telegram 通知、代理等功能。

* * *

##  功能特性

*    单账号保活
*    自动登录（处理 Cloudflare 整页挑战 + Turnstile 验证）
*    自动跳转至服务器详情页完成保活
*    智能识别“无服务器”场景，仍视为保活成功
*    内建速率限制保护（20次/小时阈值自动停止）
*    支持 Hysteria2 代理
*    Telegram 通知（带截图）
*    定时 / 手动 / API 多种触发方式
*    浏览器状态自动清理，避免 Cookie 干扰

* * *

##  前置要求

### 1\. GitHub Secrets 配置

进入仓库 `Settings` → `Secrets and variables` → `Actions`，添加以下 Secrets：

| Secret 名称 | 必填 | 说明 | 示例 |
| --- | --- | --- | --- |
| LUNES |  | Lunes Host 账号信息 | 邮箱-----密码 |
| HY2_URL |  | Hysteria2 代理地址 | hysteria2://password@server:port?sni=example.com |
| TG_BOT_TOKEN |  | Telegram Bot Token | 123456:ABC-DEF1234ghIkl-zyx57W2v1u123ew11 |
| TG_CHAT_ID |  | Telegram Chat ID | 123456789 |

 **免责声明**：本脚本仅供学习与自动化运维研究，使用者须遵守 Lunes Host 的服务条款。因使用本脚本导致的账号限制或其他问题，作者不承担任何责任。