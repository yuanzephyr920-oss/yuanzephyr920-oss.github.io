# yuanzephyr 个人站 · 第 4 版「粗野浇筑」

手写 HTML，一份样式表，零脚本。图纸照 [brutalist-web.design](https://brutalist-web.design/)
的七条，样式照 [brutalistwebsites.com](https://brutalistwebsites.com/) 的排法，
再掺一点 2000 年中文个人站的零件和 2010 年代反 UX 的站姿。

线上：<https://yuanzephyr920-oss.github.io/>

## 文件

| 文件 | 是什么 |
|---|---|
| `index.html` | 首页 / 车间：在产三件、车间规矩、原始肌理、材料单、怎么找我 |
| `projects.html` | 三个包 / 工单：装法、代价、钩子、出处与许可 |
| `about.html` | 自述 / 材料：这台机器、怎么浇的（七条逐条对）、参考墙、皮下自检、更新记录 |
| `404.html` | 「这条走廊没有门」，GitHub Pages 自动接管坏链接 |
| `style.css` | 全部样式，约 14 KB，一个 `@media` 三层，外加打印样式 |
| `favicon.svg` | 纸底墨框 + 黄三角 + Impact 的 Z |
| `assets/idle-crt-plate.jpg` | 显像管屏保的实拍截图（工单里那张） |

## 本地看

直接双击 `index.html` 就行——没有构建、没有依赖、断网也照开。

要按线上路径试，随便起个静态服务：

```powershell
cd G:\DSH\personal-site
python -m http.server 8080
# http://127.0.0.1:8080/
```

截图（Edge headless + 手写 CDP，宽 320–2560 全页，写进 `_check/v4/`）：

```powershell
cd G:\DSH\personal-site\_check
node shoot4.mjs
```

## 发布

仓库 `yuanzephyr920-oss/yuanzephyr920-oss.github.io`，Pages 源是 `main` 根目录，强制 HTTPS。
本机推之前保留全局 socks 代理（清空会挂住），token 从 `git credential fill` 取：

```powershell
cd G:\DSH\personal-site
git add -A
git commit -m "重做版面 v4：粗野浇筑"
$t = (("protocol=https`nhost=github.com`n`n" | git credential fill) | Select-String 'password=') -replace 'password=',''
git push "https://x-access-token:$t@github.com/yuanzephyr920-oss/yuanzephyr920-oss.github.io.git" main:main
```

推完约 40 秒构建。验：

```powershell
curl.exe -x socks5h://127.0.0.1:1080 -s https://yuanzephyr920-oss.github.io/ | Select-Object -First 20
```

## 这一版守的规矩

- **只有链接能点**：零 JavaScript，没有卡整块跳转、没有假按钮。
- **链接有下划线，访问过变紫**：浏览器默认长什么样就什么样，只改颜色变量。
- **后退键好使**：没有 hash 路由、没有重定向、没有拦 `history`。
- **靠滚动看内容**：不折叠、不分页、不弹「阅读更多」。
- **该装饰才装饰**：纸纹、粗线、骑缝章、施工带、走马灯——每样都有出处。
  「最佳浏览 1024×768 · IE 6.0」只是族谱，不是要求。
- **快**：无 Web 字体、无图标库、无统计脚本，外部请求 0 个。

**暗色跟着系统走**：`prefers-color-scheme: dark` 下纸墨对调（`#e9e5da` ⇄ `#14120c`）、
纸纹从「乘一层脏」改成「筛一层亮」、橙色提亮到 `#ff5a2b`、黄底永远黑字、图片不反相。
两套颜色是同一份样式表里的一个媒体查询，没有脚本也没有开关——做法抄的是
[brutalist-web.design](https://brutalist-web.design/) 自己那层 Tachyons 颜色映射。
**有打印样式**：走带、侧栏、施工带不进纸，外链后面补真实地址。

## 参考站的截图

`about.html` 的「现场」一节放了两张 2026-11-07 的首页截图（`assets/ref-*.jpg`，1200 宽 JPEG）：
`brutalistwebsites.com`（灰底等宽、三列 400×250）与 `brutalist-web.design`（暗底、21 px 正文、每节一个 Permalink）。
它们用 `figure.shot img{filter:none}` 逃过全站灰度，是全站唯一不跟系统配色走的东西。

## 版本

- `v1` 第一版，三页（2026-11-07）
- `v2` 报纸底、巨编号、危险红（2026-11-07，已废）
- `v3` 拆掉报纸版式，改手搭：默认字体、方框、表格、虚线（2026-11-07，已废）
- `v4` 粗野浇筑（2026-11-07）
- `v4.1` 加暗色、每节 § 永久链接、参考墙补两张现场截图（2026-11-07）

旧版都在 git 历史里；线上只挂最新一版。

## 版权

文字 CC BY 4.0，代码 MIT。显像管画面派生自
[justawebsite/dsh-splash-crt](https://github.com/justawebsite/dsh-splash-crt)（MIT）。
七条规矩原文属于 [David Bryant Copeland](https://brutalist-web.design/)，CC BY-NC-SA 4.0；
这里的做法是自己的。
