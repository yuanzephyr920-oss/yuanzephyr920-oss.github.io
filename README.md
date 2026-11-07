# yuanzephyr 个人网页

手写 HTML + 一份 CSS，0 字节 JavaScript，无 Web 字体、无构建步骤、无第三方资源（连图片都只有项目页那一张）。
版面照 [brutalist-web.design](https://brutalist-web.design/) 的七条指南来；粗粝感取自 [brutalistwebsites.com](https://brutalistwebsites.com/) 那句话——「对舒适与易用的不在乎」——以及一份 Swiss Industrial Print 的参考稿（报纸底、细线格栅、巨型黑体、危险红）。七条怎么落地，逐条写在 `about.html` 的「为什么这样写网页」。

## 文件

| 文件 | 作用 |
| --- | --- |
| `index.html` | 首页：001 巨编号 + 正在造 / 三条做法 / 工作台 / 终端 |
| `projects.html` | 三个项目的细节、安装命令、ASCII 数据流、出处与许可 |
| `about.html` | 我、这台机器、七条指南的落地方式、怎么找我 |
| `style.css` | 全部样式（约 18 KB）。深浅两套配色与增强对比度都走媒体查询 |
| `favicon.svg` | 墨底磷光绿显像管方框 |
| `assets/idle-crt-plate.jpg` | 项目页的待机画面截图（1200×750） |

## 设计约定

- **字体**：只用系统字体。标题 `"Arial Black","Segoe UI Black",…,"Microsoft YaHei"`，等宽 `Cascadia Mono / Consolas` + 中文兜底，正文 `Segoe UI + PingFang / YaHei / Noto Sans SC`。**CJK 字体必须排在 `system-ui` 之前**——本机的 `system-ui` 会解析成外来宋体，正文会一路衬线下去，`getComputedStyle` 还看不出来，得用 CDP 的 `CSS.getPlatformFontsForNode` 才抓得到。
- **颜色**：报纸底的纸色 + 近黑墨色，强调色只有一个「危险红」（`#c1121f`；深色模式 `#ff4030`；增强对比度模式换成纯黑白与 `#ff7361`）。红只出现在编号、标签、斜章和内联强调上。
- **装饰只有五样**，且都指事：1 px 细线（格栅）、等宽大写小标签（元数据与登记条）、危险红、纸的颗粒（`body::after` 里一段内联 SVG `feTurbulence`，4.5% 不透明度，深色模式换成 `screen` 混合，打印时 `display:none`）、一条斜纹带（页脚上方）。
- **可点即链接**：正文链接一律 2 px 实线下划线，悬停反白，访问过的变灰；导航是带边框的等宽标签，当前页反白（`aria-current="page"`）。没有假按钮。
- **巨编号与斜章**：`001/` 这类编号用 `clamp()` 从 96 px 长到 300 px，`aria-hidden` 掉不进正文流；`.stamp` 是手写感斜章，窄屏（≤860 px）直接隐藏。

## 体积（实测）

首页 `index.html` + `style.css` + `favicon.svg` = 约 28 KB，3 个请求，gzip 之后约 9 KB；正文文字本身约 4 KB。
`pre` 与长命令在窄屏会折行（`white-space: pre-wrap` + `overflow-wrap: break-word`），不让读者横向拖。

## 本地看

直接双击 `index.html` 即可，`file://` 下没有跨域问题（页面不加载任何脚本或远程资源）。
需要本地服务时：`npx serve .` 或 `python -m http.server`。

## 线上

<https://yuanzephyr920-oss.github.io/> —— 仓库 `yuanzephyr920-oss/yuanzephyr920-oss.github.io`，发布源 `main` / 根目录，已强制 HTTPS。推完约半分钟构建完成。

## 改完怎么发

本机的 `git` 只要保留全局 `http.proxy = socks5://127.0.0.1:1080`（jamjams 在跑）就能直连 github.com；把它清空反而会挂住。

~~~powershell
cd G:\DSH\personal-site
git add index.html projects.html about.html style.css favicon.svg assets README.md
git commit -m "改了什么"
$token = (("protocol=https" + [char]10 + "host=github.com" + [char]10 + [char]10) | git credential fill | Select-String "^password=").ToString().Substring(9)
git push ("https://x-access-token:" + $token + "@github.com/yuanzephyr920-oss/yuanzephyr920-oss.github.io.git") main:main
~~~

首次建仓（仓库不存在时）走 API，token 同上，body 写成 `repo-body.json`：

~~~powershell
curl.exe -x socks5h://127.0.0.1:1080 -X POST https://api.github.com/user/repos `
  -H ("Authorization: Bearer " + $token) -H "Content-Type: application/json" `
  --data-binary "@repo-body.json"
~~~

注意 PowerShell 里不要用 `--data-raw $body` 传 JSON，引号会被打坏（API 回 `Problems parsing JSON`），一律写文件再用 `--data-binary "@file"`。

## `_check/`（本地验证用具，发布时不要带上）

| 脚本 | 作用 |
| --- | --- |
| `shoot.mjs` | Edge 的 CDP 在 12 种视口/配色下整页截图（320 / 390 / 820 / 1440 / 2560，深浅两套 + 增强对比度），并回传横向溢出探针 |
| `shoot-live.mjs` | 同一套截图，但拍的是线上地址，结果落在 `_check/live/` |
| `verify2.mjs` | 查 lang / 标题层级 / 脚本数 / 真实请求 / 实际平台字体（`CSS.getPlatformFontsForNode`）/ 打印样式（导航、登记条、颗粒、外链补 URL）/ 四套配色 |
| `measure.mjs` | 量正文字号、行高比与每行字数（1440 下 17 px / 1.66 / 40 字） |
| `probe3.mjs` | 量 hero 与章节编号的几何，用来定标题字号与列宽 |
| `chrome-check.mjs` | 同样的页面在 Chrome 里复查一遍，并抓控制台报错 |

跑法：`node _check/shoot.mjs` 等，需要装了 Edge / Chrome 的 Windows。
