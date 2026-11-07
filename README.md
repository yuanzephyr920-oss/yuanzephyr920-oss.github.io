# yuanzephyr 个人网页

手写 HTML + 一份 CSS，0 字节 JavaScript，无 Web 字体、无构建步骤、无第三方资源。
版面依照 [brutalist-web.design](https://brutalist-web.design/) 的七条指南，逐条落地方式写在 `about.html` 的「为什么这样写网页」一节。

## 文件

| 文件 | 作用 |
| --- | --- |
| `index.html` | 首页：正在造 / 三条做法 / 工作台 / 终端 |
| `projects.html` | 三个项目的细节、安装命令、出处与许可 |
| `about.html` | 我、这台机器、七条指南的落地方式 |
| `style.css` | 全部样式。深浅两套配色与增强对比度都走媒体查询 |
| `favicon.svg` | 墨底磷光绿显像管方框 |
| `assets/idle-crt-plate.jpg` | 项目页的待机画面截图（1200×750） |

## 本地看

直接双击 `index.html` 即可，`file://` 下没有跨域问题（页面不加载任何脚本或远程资源）。
需要本地服务时：`npx serve .` 或 `python -m http.server`。

## 发布到 GitHub Pages

仓库名要用 `<账号>.github.io`（用户主页）或任意仓库的 `main` 分支根目录（项目页）：

```powershell
cd G:\DSH\personal-site
git init -b main
git add index.html projects.html about.html style.css favicon.svg assets
git commit -m "个人主页：手写 HTML/CSS，依照 brutalist-web.design 七条"
git remote add origin https://github.com/yuanzephyr920-oss/yuanzephyr920-oss.github.io.git
git push -u origin main
```

本机 git 走 github.com 需要先清掉全局 socks 代理与 hosts 劫持，可用一次性的：

```powershell
git -c http.proxy= -c https.proxy= -c http.sslVerify=false push -u origin main
```

推完后在仓库 Settings → Pages 里确认发布源是 `main` / 根目录，几分钟后地址为
`https://yuanzephyr920-oss.github.io/`。

## `_check/`（本地验证用具，发布时不要带上）

- `shoot.mjs`：用 Edge 的 CDP 在 11 种视口/配色下整页截图，输出到本目录，并回传横向溢出探针。
- `verify.mjs`：查打印样式、对比度、外部请求数、实际使用的平台字体、标题层级。
- `chrome-check.mjs`：同样的页面在 Chrome 里复查一遍，并抓控制台报错。

跑法：`node _check/shoot.mjs` 等，需要装了 Edge/Chrome 的 Windows。
