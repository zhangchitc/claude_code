# claude_code 可视化 demo 仓库

"Claude Code 工作原理" 小红书视频系列的配套可视化 demo 页。每期视频一个交互式 HTML，`index.html` 是列表首页。

## 结构

- `NNN_topic/` — 每期视频一个文件夹，内含该期的 demo HTML（文件名不统一，如 `cch_demo.html`、`visual.html`、`flow.html`）
- `index.html` — 列表首页，`.list` 里每期一个 `<a class="row">` 条目
- `thumbs-tight/NNN.jpg` — 列表缩略图，**是 demo 页本身的截图**，16:10 比例（不是视频封面）

## 添加一期新 demo

1. 建 `NNN_topic/` 文件夹，把 demo HTML 放进去
2. 在 repo 根目录跑下面的命令生成缩略图 `thumbs-tight/NNN.jpg`。截 demo 信息量最大的那一屏，有 tab 的先点到对应 tab：
   ```bash
   uv run --with playwright python3 -c "
   from playwright.sync_api import sync_playwright
   import os
   with sync_playwright() as p:
       b = p.chromium.launch()
       page = b.new_page(viewport={'width':1440,'height':900}, device_scale_factor=2)
       page.goto(f'file://{os.path.abspath(\"NNN_topic/xxx.html\")}')
       page.wait_for_timeout(400)
       # page.click('button[data-view=xxx]'); page.wait_for_timeout(400)  # 如有 tab
       page.screenshot(path='thumbs-tight/NNN.jpg', type='jpeg', quality=90)
       b.close()
   "
   ```
3. 在 `index.html` 的 `.list` 末尾加一个条目，照抄已有 `<a class="row">` 结构，改 `href` / 缩略图路径 / `title` / `desc`
4. commit + push 到 `origin/main`

## 约定

- 列表标题尽量短（放不下会换行），细节放 `desc`
- Python 工具一律用 `uv`
