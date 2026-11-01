# 维护手册

这个目录是 **GitHub 个人主页仓库** 的源码，已经上线在 <https://github.com/z1934642491-eng>。

GitHub 的机制：仓库名与用户名完全一致时，仓库里的 `README.md` 会渲染在个人主页顶部。仓库名一旦改动，这个效果就没了。

---

## 一、当前线上状态

| 项目 | 地址 | 说明 |
|---|---|---|
| 个人主页 | <https://github.com/z1934642491-eng> | README 已渲染，含 typing 头部、技能墙、项目表、自动同步的文章列表 |
| 技术博客 | <https://z1934642491-eng.github.io/My-Blog/> | GitHub Pages，`main` 分支根目录发布 |
| 博客仓库 | <https://github.com/z1934642491-eng/My-Blog> | 静态站源码 |

首次部署已完成，不需要再走一遍建仓流程。

---

## 二、两个自动任务

`.github/workflows/` 下有两个定时任务，都在 **Actions** 标签页里能看到：

| 文件 | 频率 | 作用 |
|---|---|---|
| `blog-posts.yml` | 每天 UTC 02:00（北京时间 10:00） | 抓取博客 `feed.xml`，把最新文章写进 README 的 `BLOG-POST-LIST` 区块 |
| `snake.yml` | 每天 UTC 00:00 | 生成贡献蛇形图，推到 `output` 分支 |

两个任务都能在 Actions 页手动触发（**Run workflow** 按钮），改完无需等定时。

**前提**：Settings → Actions → General → Workflow permissions 必须是 **Read and write**。当前已开启。如果哪天自动同步失效，先查这里。

---

## 三、写一篇新文章

1. 复制 `my-blog/posts/defect-triage.html`，改文件名（英文短横线，别用中文）
2. 改正文标题、日期、内容
3. 三处入口要同步更新：
   - `my-blog/blog.html` — 文章列表加一条
   - `my-blog/index.html` — 首页「最新文章」加一条
   - `my-blog/feed.xml` — 加一个 `<item>`，**否则主页 README 不会同步到这篇**
4. 推送 `main` 分支，30 秒后 Pages 生效

`feed.xml` 里的 `<link>` 必须写完整线上地址，否则 Actions 抓过去是死链。

---

## 四、如果要改 GitHub 用户名

`z1934642491-eng` 这个账号名会出现在所有徽章和链接里。改名后 GitHub 会保留旧地址重定向，但有两件事要跟着改：

1. 两个仓库里的所有硬编码用户名（README 徽章、stats 卡片、feed.xml 的 `<link>`、博客页脚）
2. 仓库 `z1934642491-eng` 本身要改名成新用户名，否则主页 README 不再显示

改完刷新 Pages 即可，`zhongjiawei.github.io` 这类地址会自动跟着变。

---

## 五、徽章里的坑

`img.shields.io/badge/` 用 `-` 分隔 label、message、color 三段。用户名里带连字符时，必须写成双连字符 `--`，否则 shields.io 会把连字符后面的内容当成颜色值，徽章直接显示 **404 Badge not found**。

例如：

```text
❌ img.shields.io/badge/Blog-z1934642491-eng.github.io-6366f1
✅ img.shields.io/badge/Blog-z1934642491--eng.github.io-6366f1
```

中文和 `@` 等其他特殊字符用 URL 编码（`%E4%B8%AD`、`%40`）。

---

## 六、排查顺序

显示不对时按这个顺序查：

1. `https://<用户名>.github.io/<仓库名>/` 是否 200 — 打不开多半是 Pages 还在构建（Repository → Actions → pages build and deployment）
2. 样式丢失 — 检查 `index.html` 里是否写了 `/assets/...` 这种绝对路径，项目站点的根目录是 `/<仓库名>/`，必须用相对路径
3. README 文章列表不更新 — 手动触发 `blog-posts.yml`，看日志里 feed 是否抓取成功
4. 蛇形图不显示 — `raw.githubusercontent.com` 在国内常被墙，属于网络问题，不代表配置错
