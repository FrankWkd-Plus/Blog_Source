---
title: 🦊Snap版Firefox切换到APT版迁移指南
date: 2025-08-05
tags:
  - 工程
licenseName: CC-BY-NC-SA 4.0
---
By 某位LLM同志

---

###  🔄 **一、核心流程概述**
1. **备份 Snap 版配置** → 复制配置文件到安全位置
2. **彻底移除 Snap 版 Firefox** → 避免冲突
3. **通过官方 APT 库安装 .deb 版** → 原生支持最佳兼容性
4. **恢复配置文件到 APT 版路径** → 迁移用户数据
5. **验证数据完整性与锁定更新** → 防止回退到 Snap

---

### ⚠️ **二、操作前准备**
1. **备份现有配置（关键步骤！）**
```bash
# 退出所有 Firefox 进程后执行
cp -r ~/snap/firefox/common/.mozilla/firefox/ ~/firefox_snap_backup
```
- 所有用户数据（包括扩展、密码、Cookie）均存储在 `~/snap/firefox/common/.mozilla/firefox/` 目录下。
- 此备份可防止迁移失败导致数据丢失。

---

### 🔧 **三、移除 Snap 版并安装 APT 版**
#### 步骤 1：卸载 Snap 版 Firefox
```bash
sudo snap remove --purge firefox# 彻底删除 Snap 版及数据
```

#### 步骤 2：禁用 Ubuntu 自动回退 Snap
```bash
# 创建配置文件阻止安装 Snap 版
echo 'Package: firefox*
Pin: release o=Ubuntu
Pin-Priority: -1' | sudo tee /etc/apt/preferences.d/firefox-no-snap
```

#### 步骤 3：添加 Mozilla 官方 APT 源
```bash
# 导入密钥并验证指纹（必须匹配 35BAA0B33E9EB396F59CA838...）
sudo mkdir -p /etc/apt/keyrings
wget -q https://packages.mozilla.org/apt/repo-signing-key.gpg -O- | sudo tee /etc/apt/keyrings/packages.mozilla.org.asc > /dev/null

# 添加 Mozilla 软件源
echo "deb [signed-by=/etc/apt/keyrings/packages.mozilla.org.asc] https://packages.mozilla.org/apt mozilla main" | sudo tee /etc/apt/sources.list.d/mozilla.list

# 设置优先级高于 Ubuntu 官方源
echo 'Package: *
Pin: origin packages.mozilla.org
Pin-Priority: 1000' | sudo tee /etc/apt/preferences.d/mozilla
```

#### 步骤 4：安装 APT 版 Firefox
```bash
sudo apt update && sudo apt install firefox
```

---

### 📦 **四、迁移用户配置到 APT 版**
#### 恢复备份数据到标准路径
```bash
# 复制配置到 APT 版的默认存储位置
mkdir -p ~/.mozilla/firefox/
cp -r ~/firefox_snap_backup/* ~/.mozilla/firefox/
```
- **路径说明**：
- Snap 配置位置： `~/snap/firefox/common/.mozilla/firefox/`
- APT 版配置位置： `~/.mozilla/firefox/`
- 此操作覆盖了所有个人数据（包括登录会话、Cookie、扩展状态）。

---

### ✔️ **五、验证与后续配置**
1. **启动 Firefox 检查数据完整性**
- 访问 `about:support` → **应用基础** → 确认 **配置文件夹** 路径为 `~/.mozilla/firefox/...`
- 检查扩展、保存的密码、历史记录是否完整。

2. **防止未来自动更新干扰**
```bash
# 禁用 Snap 自动刷新（避免后台重装）
sudo snap set system refresh.hold="$(date +%Y-%m-%dT%H:%M:%S%z -d 'now+365 days')"
```

---

### ⚠️ **六、常见问题处理**
- **迁移后扩展无法加载？**
在终端运行 `firefox -P`，选择原配置文件夹并设为默认。
- **登录信息丢失？**
检查 `~/.mozilla/firefox/` 下是否有多个配置文件，恢复备份到正确的 `*.default-release` 目录。
- **Snap 残留导致冲突？**
彻底清理：
```bash
sudo apt purge snapd# 可选（仅需 APT 版时）
sudo rm -rf /etc/apt/preferences.d/snapd
```

---

### 💎 **总结建议**
- **推荐使用官方 APT 源**：比 PPA 更新更及时，且经过 Mozilla 签名校验。
- **备份至关重要**：操作前务必备份 `~/snap/firefox` 目录，避免因权限问题导致数据丢失。
- **扩展兼容性**：99% 的扩展可直接迁移，少数依赖 Snap 环境的需重新安装。

> 执行完毕后，你的 Firefox 将切换为无沙盒的原生 .deb 版，解决 Snap 版文件访问限制问题（如无法访问挂载分区），同时保留全部用户数据。