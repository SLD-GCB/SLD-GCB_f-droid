# SLD-GCB · 自建 F-Droid 仓库

[SLD-GCB](https://github.com/SLD-GCB) 自研安卓应用的 F-Droid 仓库。
基于 [AoEiuV020/f-droid-repo](https://github.com/AoEiuV020/f-droid-repo) 模板搭建。

<a href="https://sld-gcb.github.io/SLD-GCB_f-droid/repo/">
  <img alt="Get it on F-Droid" src="https://f-droid.org/badge/get-it-on.svg" width="200">
</a>

> 从零搭建这个仓库的完整过程、踩过的坑，见 **[docs/搭建全过程.md](docs/搭建全过程.md)**。

---

## 给用户

**仓库地址**

```
https://sld-gcb.github.io/SLD-GCB_f-droid/repo/
```

**签名指纹（SHA-256）**

```
40:18:8E:27:12:DB:58:52:19:B8:9E:56:48:2A:0E:FB:FB:2D:D7:63:76:97:E9:01:C9:9E:0A:AB:6A:C4:4A:54
```

添加方法：F-Droid → **设置 → 仓库 → 加号**，填入上面的地址和指纹。
直接下载 APK 的页面：<https://sld-gcb.github.io/SLD-GCB_f-droid/repo/>

---

## 已收录应用

| 应用 | 包名 | 版本 | 分类 | 许可证 | 源码 |
|---|---|---|---|---|---|
| 大司农 | `com.dasinong` | 6.40 (46) | Connectivity | AGPL-3.0-only | [DSN-android-windows](https://github.com/SLD-GCB/DSN-android-windows) |
| 鸿胪寺 | `com.honglusi.tavern` | 0.6.1 (10) | Internet | AGPL-3.0-only | [HLS-AI-Android](https://github.com/SLD-GCB/HLS-AI-Android) |
| 梼杌 | `com.taowu` | 5.6 (38) | Internet / Multimedia | AGPL-3.0-only | [Taowu-spider-Android](https://github.com/SLD-GCB/Taowu-spider-Android) |

---

## 分支说明

| 分支 | 内容 | 什么时候动 |
|---|---|---|
| `main` | 本说明 + 模板脚本 + workflow | 基本不动 |
| `actions` | **发布入口**，`apk/` 目录放东西即触发发布 | 每次发新版 |
| `gh-pages` | 真正的仓库内容：`repo/`（APK + 索引）、`metadata/`（应用信息） | 只在改应用信息时 |

---

## 发新版本

在 `actions` 分支的 `apk/` 目录丢一个 `.txt`，内容是新 APK 的下载地址：

```bash
git checkout actions
git fetch origin && git reset --hard origin/actions   # CI 也会往这个分支提交，先同步

echo "https://github.com/SLD-GCB/<仓库>/releases/download/<tag>/<文件名>.apk" > apk/<名字>.txt
git add -f apk/<名字>.txt        # 注意：apk/.gitignore 是 /*，必须加 -f
git commit -m "添加 <应用> <版本> 的下载链接"
git push origin actions
```

推送后 `Process APK Updates` 会自动执行：

```
下载 APK → 生成索引 → 提交到 gh-pages → 触发 Pages 重新部署
```

约 1~2 分钟后生效，用户端刷新即可看到新版本。

也可以直接放 `.apk` 文件本身（同样要 `-f`）。想手动触发就去 Actions 页跑 `Process APK Updates`。

> 脚本处理完会**删掉** `apk/` 里的文件并推回 `actions` 分支，所以下次推送前一定要先 `fetch` + `reset`，否则会被拒。

---

## 修改应用信息

应用在 F-Droid 里显示的名称、简介、描述、分类、许可证、源码链接，全部来自 `gh-pages` 分支的 `metadata/<包名>.yml`。

```bash
cd fdroid
git fetch origin && git checkout gh-pages && git reset --hard origin/gh-pages
# 编辑 metadata/com.taowu.yml 等
git commit -am "更新应用信息"
git push origin gh-pages
```

**改完必须再触发一次发布**（往 `apk/` 丢个 txt）索引才会重新生成 —— 索引是发布时算出来的，不是实时读元数据。

> `fdroid update` 只在元数据**不存在**时用 `--create-metadata` 生成占位文件。三个应用的元数据都已写好，**别删**；删了会退回成 `License: Unknown`、分类错填的空壳。

---

## 签名密钥（重要）

仓库用一把独立密钥给索引签名，客户端靠它的指纹确认仓库没被篡改。

- 指纹：见上方
- 密钥文件 `keystore.p12` **不在本仓库内**，以 base64 存放在 Actions Secrets
- 需要的 5 个 Secrets：`KEYSTORE`、`KEYSTOREPASS`、`KEYPASS`、`REPO_KEYALIAS`、`KEYDNAME`

**这把密钥务必在仓库之外离线备份。**

一旦丢失，老用户就无法从本仓库继续更新 —— Android 不允许换签名升级，他们只能卸载重装。密钥换了，指纹也会变，所有用户都得重新添加仓库。

---

## 目录结构

```
├── .github/workflows/
│   ├── process-apk.yml        # 主流程：apk/ 有变化就发布
│   └── manual-push-apk.yml    # 手动触发入口
├── script/                    # 模板脚本（下载 APK、生成索引、提交推送）
├── docker/fdroidserver/       # CI 里构建 fdroidserver 镜像
├── actions/push-fdroid/       # 可复用 Action：从别的仓库推送 APK 过来
├── apk/                       # （actions 分支）发布入口
└── fdroid/                    # 子模块 → 本仓库的 gh-pages 分支
```

---

## 备注

- **fork 来的仓库 GitHub Actions 默认是关的**。如果推送后什么反应都没有，去 Actions 页点一次 *"I understand my workflows, go ahead and enable them"*。
- CI 里跑的是 `fdroidserver` 官方 Docker 镜像，跟本机环境无关，不需要在本地装 Docker。
