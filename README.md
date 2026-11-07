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

## 线上

<https://yuanzephyr920-oss.github.io/> —— 仓库 `yuanzephyr920-oss/yuanzephyr920-oss.github.io`，发布源 `main` / 根目录，已强制 HTTPS。推完约半分钟构建完成。

## 改完怎么发

本机的 `git` 只要保留全局 `http.proxy = socks5://127.0.0.1:1080`（jamjams 在跑）就能直连 github.com；把它清空反而会挂住。

```powershell
cd G:\DSH\personal-site
git add index.html projects.html about.html style.css favicon.svg assets README.md
git commit -m "改了什么"
$token = (("protocol=https" + [char]10 + "host=github.com" + [char]10 + [char]10) | git credential fill | Select-String "^password=").ToString().Substring(9)
git push ("https://x-access-token:" + $token + "@github.com/yuanzephyr920-oss/yuanzephyr920-oss.github.io.git") main:main
```

首次建仓（仓库不存在时）走 API，token 同上，body 写成 `repo-body.json`：

```powershell
curl.exe -x socks5h://127.0.0.1:1080 -X POST https://api.github.com/user/repos `
  -H ("Authorization: Bearer " + $token) -H "Content-Type: application/json" `
  --data-binary "@repo-body.json"
```

注意 PowerShell 里不要用 `--data-raw $body` 传 JSON，引号会被打坏（API 回 `Problems parsing JSON`），一律写文件再用 `--data-binary "@file"`。

## `_check/`（本地验证用具，发布时不要带上）

- `shoot.mjs`：用 Edge 的 CDP 在 12 种视口/配色下整页截图（320 / 390 / 820 / 1440 / 2560，深浅两套 + 增强对比度），输出到本目录，并回传横向溢出探针。
- `shoot-live.mjs`：同一套截图，但拍的是线上 `https://yuanzephyr920-oss.github.io/`，结果落在 `_check/live/`。
- `verify.mjs`：查打印样式、对比度、外部请求数、实际使用的平台字体、标题层级。
- `chrome-check.mjs`：同样的页面在 Chrome 里复查一遍，并抓控制台报错。

跑法：`node _check/shoot.mjs` 等，需要装了 Edge/Chrome 的 Windows。
