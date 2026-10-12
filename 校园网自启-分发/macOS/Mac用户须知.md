# macOS 用户须知

**当前版本**：v1.0.0  
**适用系统**：macOS 11 Big Sur 及以上

> ⚠️ 本文件夹当前**暂无 `校园网自启.app`**。macOS 版需开发者在一台 Mac 上打包一次后放入此文件夹分发。以下为完整操作指南。

---

## 一、开发者打包步骤（仅需一次，约 5 分钟）

Windows 上无法交叉打包 macOS 的 `.app`，必须在**一台 Mac** 上执行：

```bash
# 1. 确认 Python 3.11+（Mac 一般自带，或从 python.org 下载）
python3 --version

# 2. 安装依赖和打包工具
pip3 install pystray pillow pyinstaller

# 3. 把项目源码拷到 Mac 上（整个 校园网自启 文件夹），进入目录
cd /path/to/校园网自启

# 4. 打包
python3 -m PyInstaller 校园网自启-mac.spec --noconfirm

# 5. 产物在 dist/校园网自启.app
#    将 校园网自启.app 复制到本 macOS 文件夹，即可分发给 Mac 用户
```

打包完成后，把 `校园网自启.app` 放进本文件夹，连同本说明一起发给 Mac 用户。

---

## 二、Mac 用户使用步骤（拿到 .app 后）

1. 把 `校园网自启.app` 拖到「应用程序」文件夹（可选，便于管理）
2. **首次打开**：macOS 会提示"无法打开，因为来自未识别开发者"（Gatekeeper 安全机制）
   - 方法一：右键点击 `校园网自启.app` → 选「打开」→ 弹窗再点「打开」
   - 方法二：「系统设置」→「隐私与安全性」→ 点「仍要打开」
   - 后续打开不再提示
3. 连上校园网 WiFi（如 `JXAU-Stu`）
4. 双击 `校园网自启.app`，弹出主窗口
5. 填写账号、密码，选择运营商（移动/电信/联通），点「保存」
6. 登录成功后按提示开启开机自启

---

## 三、开机自启说明（macOS 专用）

- 勾选「开机自启」后，程序在 `~/Library/LaunchAgents/com.campusnet.autologin.plist` 注册 launchd 项
- 开机后自动后台启动（`--hidden` 模式，仅托盘图标，无窗口）
- 取消勾选即移除 launchd 项
- 与 Windows 的注册表自启机制对应，功能一致

---

## 四、Mac 专属常见问题

**Q：提示"无法打开，来自未识别开发者"？**
A：macOS Gatekeeper 安全机制（与 Windows 的 SmartScreen 类似）。右键 → 打开 → 确认即可。程序无代码签名证书，属正常现象。

**Q：托盘图标不显示？**
A：macOS 顶部菜单栏空间有限，检查是否有图标被挤到「控制中心」里；或重启程序。

**Q：开机自启不生效？**
A：确认 `~/Library/LaunchAgents/com.campusnet.autologin.plist` 存在；终端执行 `launchctl list com.campusnet.autologin` 查看状态。

**Q：提示"未能从校园网重定向获取登录参数"？**
A：断开 WiFi 重连一次，再点「立即连接」。

---

## 五、数据与隐私

- 配置文件：`校园网自启.app` 同目录下的 `config.json`（base64 编码，非加密）
- 登录请求只发往校园网内网认证服务器，与浏览器手动登录同一接口
- 分发时**只发 .app**，切勿附带 `config.json`（含个人凭据）

---

## 六、卸载

1. 托盘右键 → 退出
2. 取消勾选「开机自启」（或删除 `~/Library/LaunchAgents/com.campusnet.autologin.plist`）
3. 删除 `校园网自启.app` 及同目录的 `config.json`、`log.txt`
