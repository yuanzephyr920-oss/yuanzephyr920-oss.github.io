# SHEET 920 — 个人站（连续打印纸版）

手写的个人站。四张纸、一份样式表、零脚本、零字体包、零外部请求。
线上地址：<https://yuanzephyr920-oss.github.io/>

## 这一版是什么

v5「连续打印纸」。前面的 v1–v4 已经从仓库里删掉，只留在 git 历史里。

把网页当成一叠打印机吐出来的连续纸：

- 纸是白的，两边各一排齿轮孔（`radial-gradient` 打的，全 CSS）；
- 页眉是机器的黑头条（`JOB 920-A · SHEET 1/4`）；
- 正文全是等宽字，因为这是打印机打出来的东西；
- 顶上一道黑菜单，四格，像纸上的四栏；
- 右上角一枚方形红章「已阅 · 920 · IN USE」，`mix-blend-mode: multiply`，压进纸里；
- 标题后面一个会闪的方块光标，全站唯一的动效；
- 页脚一条黑底 `END OF SHEET N — 共 4 张`。

## 文件

| 文件 | 作用 |
| --- | --- |
| `index.html` | 车间（首页）· SHEET 1/4 |
| `projects.html` | 三个包（三张工单）· SHEET 2/4 |
| `about.html` | 自述（机器、七条、参考墙、皮下、更新记录）· SHEET 3/4 |
| `404.html` | 错误页（GitHub Pages 会自动拿它当 404）· SHEET 4/4 |
| `style.css` | 全套样式：纸、孔、黑头条、表格、暗色、打印 |
| `favicon.svg` | 黑底 + 两排白孔 + 三道红杠 |
| `assets/idle-crt-plate.jpg` | dsh-idle-crt 的显像管画面（1024×576） |
| `assets/ref-brutalistwebsites.jpg` | 参考站实拍（1200×750） |
| `assets/ref-brutalist-web-design.jpg` | 参考站实拍（1200×750） |

## 本地看

```powershell
cd G:\DSH\personal-site
python -m http.server 8080
# 然后开 http://127.0.0.1:8080/
```

`file://` 直接打开也能看，只是 404 页不会被自动调用。

## 截图自检

截图脚本在源工程 `G:\DSH\psite-v5\_check\shoot5.mjs`（Edge headless + 手写 CDP，19 张：四个页面 1440，首页 820/390/320/2560，projects/about/404 的 390，about 320，减少动效一张，暗色六张）。产物在 `_check\v5\`，探针数据在 `report.json`。

```powershell
cd G:\DSH\psite-v5\_check
node shoot5.mjs
node probe4.mjs   # 单个盒子量一次，例如 pre 的实际宽度
```

注意：Edge 全页截图（`captureBeyondViewport`）**不遵守内层元素的 `overflow` 裁剪**，`pre` 里的长代码在图上会画到纸外；实际几何以探针的 `getBoundingClientRect` 为准。

## 改一个字的流程

```powershell
cd G:\DSH\personal-site
git add -A
git commit -m "改了什么"
```

发布（token 从 git 凭据管理器取）：

```powershell
$t = (echo "protocol=https`nhost=github.com`n" | git credential fill | Select-String '^password=') -replace 'password=',''
git push https://x-access-token:$t@github.com/yuanzephyr920-oss/yuanzephyr920-oss.github.io.git main:main
```

本机 git 的全局 socks 代理（`socks5://127.0.0.1:1080`）必须留着，否则 push 会挂住。线上验证：

```powershell
curl.exe -x socks5h://127.0.0.1:1080 -sI https://yuanzephyr920-oss.github.io/
curl.exe -x socks5h://127.0.0.1:1080 -s  https://yuanzephyr920-oss.github.io/this-path-does-not-exist | Select-String "错页"
```

## 七条

这套东西按 <https://brutalist-web.design/> 的七条来，自述页第 03 节逐条写了这一版怎么兑现：

内容先于版面 · 只有链接和按钮能被点 · 链接带下划线 · 后退键必须好用 · 长内容靠滚 · 该装饰才装饰 · 快是设计的一部分。

## 契约

- **零 JS**：全站没有一个 `<script>`，也没有事件处理器。
- **零外部请求**：没有 CDN、没有外链字体、没有统计。断网刷新照开。
- **系统字体**：Courier New / Consolas / 宋体 / 黑体，一个都不下载。
- **暗色**：`prefers-color-scheme: dark` 下纸变黑纸，绿道淡下去；不反相图片。
- **窄屏**：320–2560 实测无横向滚动；表格整行变方块（`table.stack`）。

## 版本

| 版本 | 是什么 | 状态 |
| --- | --- | --- |
| v5 | 连续打印纸（当前） | 线上 |
| v4.1 | 水泥浇筑 + 暗色 + § 链接 | 已删，git 历史 |
| v4 | 水泥浇筑 / 报纸 | 已删，git 历史 |
| v3 | 手搭：默认字体、方框、虚线 | 已删，git 历史 |
| v2 | 报纸底、巨编号、危险红 | 已删，git 历史 |
| v1 | 第一版，三页 | 已删，git 历史 |

文字 CC BY 4.0，代码 MIT。七条规矩原文属于 David Bryant Copeland（CC BY-NC-SA 4.0）。
