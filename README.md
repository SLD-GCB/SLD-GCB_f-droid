# SLD-GCB 的自建 F-Droid 仓库

这里分发 [SLD-GCB](https://github.com/SLD-GCB) 自研的安卓应用，全部开源。

<a href="https://sld-gcb.github.io/SLD-GCB_f-droid/repo/">
  <img alt="Get it on F-Droid" src="https://f-droid.org/badge/get-it-on.svg" width="200">
</a>

---

## 在 F-Droid 里添加本仓库

1. 打开 F-Droid，进入 **设置 → 仓库**
2. 点 **加号**，在地址栏填入：

   ```
   https://sld-gcb.github.io/SLD-GCB_f-droid/repo/
   ```

3. 指纹填下面这串（用于核对仓库没被篡改）：

   ```
   40:18:8E:27:12:DB:58:52:19:B8:9E:56:48:2A:0E:FB:FB:2D:D7:63:76:97:E9:01:C9:9E:0A:AB:6A:C4:4A:54
   ```

4. 保存后回到首页下拉刷新，就能看到下面这三个应用。

> 指纹必须**完全一致**。如果 F-Droid 提示的指纹和上面不同，说明地址被劫持了或你填错了，请立刻停止。

---

## 已收录的应用

### 大司农

局域网文件互传与聊天。无服务器、不走互联网，靠热点或局域网直连，Android 与 Windows 可任意组合互通。

- 包名 `com.dasinong` ｜ 版本 **6.40** ｜ 许可证 AGPL-3.0
- 源码：<https://github.com/SLD-GCB/DSN-android-windows>

### 鸿胪寺

本地优先的 AI 角色扮演聊天。所有角色、对话、模型都放在手机本地，不经过任何服务器。

- 包名 `com.honglusi.tavern` ｜ 版本 **0.6.1** ｜ 许可证 AGPL-3.0
- 源码：<https://github.com/SLD-GCB/HLS-AI-Android>

### 梼杌

网页视频抓取与下载。内置浏览器走到目标页面，一键解析 m3u8 / mp4 / DASH 流，下载成可离线播放的本地文件。

- 包名 `com.taowu` ｜ 版本 **5.6** ｜ 许可证 AGPL-3.0
- 源码：<https://github.com/SLD-GCB/Taowu-spider-Android>

---

## 不想用 F-Droid？

也可以直接装 APK：打开 <https://sld-gcb.github.io/SLD-GCB_f-droid/repo/> ，页面上列出了所有可下载的安装包和二维码。

> 直接装的话不会自动收到更新，需要自己盯着新版本。

---

## 关于本仓库

- 由 [AoEiuV020/f-droid-repo](https://github.com/AoEiuV020/f-droid-repo) 模板搭建
- 管理仓库（发布入口、应用元数据）：<https://github.com/SLD-GCB/SLD-GCB_f-droid>
- 所有应用代码以 **AGPL-3.0** 授权
