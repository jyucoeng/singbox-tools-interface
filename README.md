# singbox-tools-interface

为 [jyucoeng/singbox-tools](https://github.com/jyucoeng/singbox-tools) 制作的一键 SSH 命令生成快捷界面（界面风格借鉴 [yonggekkk/argosbx](https://yonggekkk.github.io/argosbx/) 命令生成器），页头提供 [pkg.tbbbk.com](https://pkg.tbbbk.com/)（Linux 应用安装器）查询链接。

一个项目，三种部署：**Cloudflare Workers**、**Cloudflare Pages**、**GitHub Pages** 任选，界面完全一致。

## 功能

- **📦 Singbox 一键脚本（sb.sh，最新主力脚本）**：Hysteria2 / Vless-Reality / Tuic / AnyTLS 直连协议自由组合，VMess-WS-TLS / Trojan-WS-TLS / Vless-WS-TLS Argo 三选一，Socks5（含 IP 白名单 socks5_wl_flag / socks5_ips），UUID / 四协议独立 SNI（hy_sni / vl_sni / tu_sni / any_sni）/ Reality 私钥 / Argo 固定隧道（agn 手输，留空为临时隧道）/ CF 优选域名（cdn_host，内置优选列表）/ Nginx 订阅等全局配置，rep / ins / del / delall / list / list key / ups 动作一键生成，附 25 条 sb 快捷指令速查。
- **✈️ MTProxy 新版（mtp-new.sh）**：交互菜单 / Go 版 / Telemt 高性能版三种安装模式，配额、到期、限速、重置日参数，Telemt 多用户管理命令（adduser / users / getuser / moduser / deluser）实时生成，32 位 hex 密钥一键生成。
- 全部端口输入框支持 🎲 一键随机端口；UUID / Socks5 密码 / Reality 私钥 / Telemt 密钥支持一键随机生成（Reality 私钥采用与脚本 `gen_reality_private()` 相同的 base64(32 字节) 算法）。
- 深色 / 浅色主题切换（本地记忆），curl / wget 双下载方式，一键复制。

## 文件说明

| 文件 | 用途 |
|---|---|
| `index.html` | 界面本体（唯一源文件，纯静态单文件，无外部依赖） |
| `worker.js` | Cloudflare Workers 部署文件（由 `build.py` 从 index.html 生成） |
| `wrangler.toml` | Wrangler 部署配置 |
| `build.py` | 修改 index.html 后重新生成 worker.js：`python build.py` |
| `.github/workflows/deploy-pages.yml` | GitHub Pages 自动部署（push 到 main 即部署） |
| `.nojekyll` | GitHub Pages 跳过 Jekyll 处理 |

## 部署方式一：Cloudflare Workers

**① 控制台粘贴（最简单，无需本地环境）**

1. 登录 [Cloudflare Dashboard](https://dash.cloudflare.com/) → **Workers & Pages** → **Create application** → **Create Worker**；
2. 随意命名（如 `singbox-tools-interface`）→ **Deploy** → **Edit code**；
3. 删除默认代码，把 `worker.js` 的**全部内容**粘贴进去 → **Deploy**；
4. 访问 `https://singbox-tools-interface.你的子域.workers.dev`。

**② Wrangler CLI**

```bash
npm install -g wrangler
wrangler login
cd singbox-tools-interface
wrangler deploy
```

## 部署方式二：Cloudflare Pages（推荐）

纯静态部署，直接用 `index.html`（无需 worker.js），免费额度充足，访问地址为 `https://<项目名>.pages.dev`。

**① 连接 GitHub 仓库（推荐，push 自动部署）**

1. 登录 [Cloudflare Dashboard](https://dash.cloudflare.com/) → **Workers & Pages** → **Create application** → 顶部切到 **Pages** 标签 → **Connect to Git**；
2. 授权 GitHub 并选择本仓库（`singbox-tools-interface`）；
3. **Project name** 随意（如 `singbox-tools-interface`），**Production branch** 选 `main`；
4. 构建配置：**Framework preset** 选 **None**，**Build command** 与 **Build output directory** 留空（纯静态单文件，无构建步骤）→ **Save and Deploy**；
5. 之后每次 push 到 `main` 自动部署，地址为 `https://singbox-tools-interface.pages.dev`。或者增加自定义域名。



## 部署方式三：GitHub Pages

**① Actions 自动部署**

1. 把本目录推送到 GitHub 仓库：

   ```bash
   git init
   git add .
   git commit -m "init: singbox-tools-interface"
   git branch -M main
   git remote add origin https://github.com/<你的用户名>/singbox-tools-interface.git
   git push -u origin main
   ```

2. 仓库 **Settings → Pages → Build and deployment → Source** 选择 **GitHub Actions**（只需设置一次）；
3. 之后每次 push 到 `main` 自动部署，地址为 `https://<你的用户名>.github.io/<仓库名>/`。

**② 分支部署（不用 Actions）**

Settings → Pages → Source 选 **Deploy from a branch** → Branch 选 `main`、目录选 `/(root)` → Save。

**③ 挂到现有仓库子目录（可选）**

把 `index.html` 复制到已有仓库的 `docs/` 目录，Pages 设置 Branch 选 `main`、目录选 `/docs`，访问 `https://<用户名>.github.io/<仓库名>/`。

> 只用其中一种部署方式时，删除 `.github/workflows/` 下不需要的 workflow 文件即可（例如只用 Workers 就删 `deploy-pages.yml`，免得每次 push 都跑 Pages 构建）。Cloudflare Pages 是在控制台直接关联仓库或上传部署的，**无需**任何 workflow 文件。

## 修改界面

Cloudflare Pages / GitHub Pages 只用 `index.html`，改完直接 push（或重新上传）即可自动部署；Cloudflare Workers 需先运行 `python build.py` 重新生成 `worker.js` 再部署（push 时 worker.js 记得一并提交）。

## 免责声明

本界面仅为命令生成辅助工具，所有脚本功能与版权归 [jyucoeng/singbox-tools](https://github.com/jyucoeng/singbox-tools) 原项目所有，仅供学习交流，请遵守所在地法律法规。
