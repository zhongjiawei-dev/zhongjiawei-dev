# 部署与启用说明

这个目录是 **GitHub 个人主页仓库** 的源码，对应 `github.com/z1934642491-eng/z1934642491-eng`。

GitHub 有一个特殊机制：当仓库名与用户名完全一致时，仓库里的 `README.md` 会显示在你的个人主页顶部。

---

## 一、创建仓库

1. 打开 <https://github.com/new>
2. **Repository name** 填 `z1934642491-eng`（必须和用户名完全一致，大小写也要一致）
3. 勾选 **Public**，勾选 **Add a README file**（避免空仓库报错）
4. 创建后，把本目录的 `README.md` 与 `.github/` 一起上传覆盖

命令行方式：

```bash
cd github-profile
git init
git add .
git commit -m "feat: init profile README"
git branch -M main
git remote add origin https://github.com/z1934642491-eng/z1934642491-eng.git
git push -u origin main
```

推上去之后刷新 <https://github.com/z1934642491-eng>，README 就会出现在主页顶部。

---

## 二、启用两个 Actions

仓库 → **Settings → Actions → General** → 拉到最下面
**Workflow permissions** 选择 **Read and write permissions** → Save。
否则两个 workflow 都无法回写仓库。

然后在 **Actions** 标签页里手动各运行一次：

| Workflow | 作用 | 触发 |
| :--- | :--- | :--- |
| `blog-posts.yml` | 每天抓取博客 RSS，把最新文章写进 README 的 `BLOG-POST-LIST` 区块 | 每天 10:00（北京时间）/ 手动 |
| `snake.yml` | 生成贡献蛇形图，推到 `output` 分支 | 每天 / push / 手动 |

第一次运行 `snake.yml` 后，`output` 分支里会出现两个 svg：

- `github-contribution-grid-snake.svg`
- `github-contribution-grid-snake-dark.svg`

README 里的 `<picture>` 已经指向这两个地址，不需要改。

> ⚠️ 新账号的贡献图需要先有一些提交记录，蛇形图才会有内容显示。

---

## 三、需要你确认 / 替换的地方

| 位置 | 当前值 | 说明 |
| :--- | :--- | :--- |
| 用户名 | `z1934642491-eng` | 出现在所有徽章、统计卡片、Actions 配置里，如与实际不符需全局替换 |
| 博客地址 | `https://z1934642491-eng.github.io/My-Blog/` | 若改了仓库名，这里和 `feed.xml` 都要同步 |
| 邮箱 | `15216200398@163.com` | 来自简历 |
| 电话 | `15216200398` | 来自简历；如果不想公开，删掉 README 底部那个 Phone 徽章即可 |
| 项目链接 | 均指向 `github.com/z1934642491-eng` | 等 P2P / ECSHOP 项目整理成仓库后，换成具体仓库地址 |

---

## 四、视觉结构说明

README 的分区顺序参考了同类学生主页的通用做法（typing 头部 → 简介 → 技术栈 → 项目 → 数据 → 联系）：

1. **typing SVG 头部** —— 动态打字，第一眼给出身份与方向
2. **徽章行** —— 博客入口、方向、届别、访问量
3. **关于我** —— 一句话定位 + 5 条可验证事实（带数字）
4. **技术工具** —— skillicons 图标墙 + 测试工具专属徽章（Postman / JMeter / Selenium / pytest / Allure / Charles / 禅道 / XMind）
5. **项目展示** —— 表格形式，三列：项目 / 说明 / 技术要点
6. **最新文章** —— Actions 自动同步，带 `BLOG-POST-LIST` 注释标记
7. **GitHub 数据** —— 统计卡片 + 语言分布 + 连续贡献，深色主题配色与博客一致
8. **贡献蛇形图** —— 支持深色/浅色跟随
9. **保持联系** —— 邮箱 / 博客 / 电话三个大徽章

配色统一为靛蓝 `#6366f1` + 青 `#22d3ee`，与博客站点使用同一套设计令牌。
