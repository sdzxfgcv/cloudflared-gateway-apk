# Cloudflared Gateway APK（本地 HTTP 变公网）

基于 [roufsweb/cloudflared-android](https://github.com/roufsweb/cloudflared-android)（MIT）二次开发，内置 cloudflared 原生二进制，由 GitHub Actions 自动编译。

## 两种模式

### 1. 快捷模式（默认，无需账号）
- 打开 App，保持「快捷模式」开启。
- 在「本地 HTTP 地址」填入要暴露的地址，例如 `http://127.0.0.1:8080`（也可以是局域网地址 `http://192.168.1.10:8080`）。
- 点「启动隧道」，等待日志解析出 `https://xxxx.trycloudflare.com`。
- 复制该地址即可从公网访问。地址在 App 重启后会变化（临时域名）。

### 2. 固定域名模式（需要 Cloudflare 账号）
- 关闭「快捷模式」开关。
- 在 Cloudflare Zero Trust → Networks → Tunnels 创建隧道。
- 复制安装命令或 token 粘贴到输入框，点「启动隧道」。
- 在 Cloudflare 面板把域名的 DNS 记录指向该隧道。

## 注意事项
- 需要 Android 5.0+，仅支持 **arm64-v8a** 架构（绝大多数现代手机）。
- 本地服务必须真的在监听对应端口；`127.0.0.1` 指的是**手机本机**。
- 会把本地服务暴露到公网，请务必给服务本身加认证，不要暴露敏感后台。
- 系统可能杀后台，建议关闭电池优化并保持通知权限。

## 构建
Fork 后推送到 `main` 或在 Actions 页面手动触发 `Build Cloudflared Gateway APK`，产物在 Artifacts 中。

原生二进制来源：MIT 项目内置 `libcloudflared.so`（已针对 Android 无 `/etc/resolv.conf` 的问题做 DNS stdin 补丁）。
