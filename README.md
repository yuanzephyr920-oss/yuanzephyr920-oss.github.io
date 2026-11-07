# 车间 920 — 六行 CSS 的个人站

站点：<https://yuanzephyr920-oss.github.io/>

整站样式表 **六行**，三条规则，182 字节。没有第七行、没有脚本、没有外部请求。

## 文件

| 文件 | 是什么 |
|---|---|
| `index.html` | 车间：这一张纸、在产三件、七条、原始肌理 |
| `projects.html` | 三个包：待机显像管、机神祷文、插件目录 |
| `about.html` | 自述：参考站、取舍、皮下、更新记录 |
| `404.html` | 错页（GitHub Pages 自动使用） |
| `style.css` | 六行样式表，全文见下 |
| `favicon.svg` | 纸色方块 + 黑框 + 一个 Z |
| `assets/` | 三张 444×250 的图，由 `.screenshot` 裁到 400 |

## 样式表全文

```css
body{font-family:monospace;
background-color:#eee;margin:40px}
.box{width:400px;height:250px;float:left;
padding:0 40px 80px 0}
.screenshot{width:400px;height:250px;
overflow:hidden}
```

三条规则与 <https://brutalistwebsites.com/> 一致，只把每条拆成两行，凑满六行。核对：

```powershell
(Get-Content style.css).Count   # 6
(Get-Item style.css).Length     # 182
```

## 本地预览

```powershell
cd G:\DSH\personal-site
python -m http.server 8080
# 打开 http://127.0.0.1:8080/
```

`file://` 直接开也行；用服务器只是为了和线上一致。

## 版面规则

- 一格 `.box`：400 宽、250 高、下留 80 空隙。三格一行（1440 下），第二行从 330px 处开始。
- 字少了就空着。不居中、不拉伸、不拿图片补——这是页面上写明的规矩。
- 图进 `.screenshot`：源图 444×250，右边 44px 被裁掉，所以每张图都是「裁进 400×250」。
- 组与组之间用 `<br clear="all">`（HTML 属性，不是 CSS）清浮动，然后接全宽的 `<h2>`。
- `viewport` 定 `width=500`：400 的格子 + 两边 40 的边距。手机上整页缩放，不横滚。

## 七条怎么兑现

1. 内容先于版面 —— 去掉样式表，页面照样从上读到下（截图：`_check/v6/home-nocss.png`）。
2. 只有链接能点 —— 零 JavaScript，没有假按钮。
3. 链接带下划线 —— 浏览器默认的蓝与紫，一个色值没改。
4. 后退键 —— 无 hash 路由、无重定向。
5. 长内容靠滚 —— 不分页、不折叠。
6. 该装饰才装饰 —— 灰底、40px 边距、横线，全部来自那六行。
7. 快 —— 样式表 182 B，首页 3.7 KB，图标 286 B，请求 2 个。

## 取舍（写下来，免得以后自己骗自己）

- **字号 13px**：`font-family:monospace` 让浏览器用「等宽默认字号」，Chrome/Edge 是 13px。参考站也是 13px。要改就得动样式表，不改。
- **没有暗色**：加 `@media (prefers-color-scheme: dark)` 就是第 7 行。
- **没有窄屏断点**：`.box` 固定 400，只能靠 `viewport=500` 让手机整体缩放。
- **正文都在格子里**：全宽段落实测 1360px 宽，读不了；所以每段话都住进 400px 的格子。

## 截图自检

```powershell
cd G:\DSH\psite-v6\_check
node shoot6.mjs     # 14 张图 + report.json（含每格 over 值：内容超出 250 多少像素）
node probe6.mjs     # 浮动位置、<br clear> 是否生效、盒子填充量
```

判溢出只看 `getBoundingClientRect`，别信全页截图：`captureBeyondViewport` 不遵守内层 `overflow` 裁剪。

## 发布

```powershell
cd G:\DSH\personal-site
git add -A
git -c user.name=yuanzephyr -c user.email=yuanzephyr920-oss@users.noreply.github.com commit -m "…"
$t = (git credential fill) # 取 token（需 socks5h 代理）
git push https://x-access-token:$t@github.com/yuanzephyr920-oss/yuanzephyr920-oss.github.io.git main:main
```

线上验证：

```powershell
curl.exe -s -x socks5h://127.0.0.1:1080 -o NUL -w "%{http_code} " https://yuanzephyr920-oss.github.io/
```

## 版本史

| 版 | 载体 |
|---|---|
| v6 | 六行 CSS，灰底等宽格子（这一版） |
| v5 | 连续打印纸：齿轮孔、机头条、方章、暗色 |
| v4 | 水泥浇筑：报纸版式、走马灯、骑缝章 |
| v3 | 反 UX 报纸：Impact 巨题、黄走马灯 |
| v2 | 信纸：纸纹、编号、危险红 |
| v1 | 第一版 |

都在 git 历史里。
