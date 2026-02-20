+++
draft = false
title = 'Timeline'

+++

<!-- ## 时光留痕 -->

<!-- --- -->

2026 年 2 月 15 日晚，决定建立一个『时光留痕』页面，记录了博客的点滴成长。希望自己投入的时间与精力能被『看见』。

---

## 2026

### 02/20

- 解决 Cloudflare Pages 自定义域名总是 “正在验证” 状态，删除域又重新关联了一次，不清楚是不是网络问题还是啥问题，竟然好了
- 增加 `layouts/about.html`；修改 about 页面的模版，去掉 `H1` 的显示，增加头像，将头像嵌入 `article-meta`，使头像应用于 `article-meta` 样式

### 02/16 - 02/18

1. 解决本地无法 git push 到 GitHub 仓库 (利用 Gitee 同步到 GitHub)
2. Fork 原主题仓库发现混乱，采用建立自己的网站仓库，但主题引用自己 Fork 原主题仓库后的仓库
3. 发现 push 后 Gitee 和 GitHub 仓库内推送动态的头像和我的账户不相符，了解需要对 Git 设置 User.name 和 User.email，并且 User.email 需设置为和 Gitee 与 GitHub 注册账户的邮箱一致才行
4. 更换 User.email 后，git push 失败，发现 Gitee 和 GitHub 都需取消勾选 "不公开我的邮箱地址"，否则当发现用户邮箱和保护邮箱一致时，会拒绝 push 请求
5. 购买域名，转移 DNS 服务器到 CF，设置 DNS 域名解析，建立 CF pages，设置自定义域名，网站上线完成
6. 发现构建页面错误，怀疑是 hugo --minify 指令问题
7. 整改我的网站页面：取消主页的头像显示，只留一句话：Where thoughts flow and moments stay. 这样可以给 Recent Posts 留更多的展示，意识到文章是最关键的，而不是自我介绍

### 02/15

1. 解决了 Mermaid 语法无法被识别的问题
   1. 在 `head_custome.css` 文件中加入 mermaid.js 库并初始化
   2. 在 layout/shortcodes/ 下建立 Mermaid 短代码，可直接在 Markdown 中通过 `{{ </mermaid}}` 引用, 而不用开启 `Goldmark`
2. 建立了『时光留痕』标签与页面
   - 在 `hugo.toml` 的 `[menu]` 下建立新的数组标签，若未制定标签路径，Hugo 的 catagories 和 tags 页面的时间轴会混乱
3. 试了好几个头像，暂时敲定此头像
4. 查看多位博主的字体引用方式，了解多种网站字体引用方；了解 Mac 与 Win 自带字体的不同

### 02/14

1. 完成了文章 [Mermaid：利用 YAML 轻松配置流程图样式](http://yijin.ink/posts/mermaid-yaml-frontmatter-flowchart-style-configuration/)，询问 AI 不足之处，更改了多级标题的结构与名称

### 02/13

- 完善文章 [Mermaid：利用 YAML 轻松配置流程图样式](http://yijin.ink/posts/mermaid-yaml-frontmatter-flowchart-style-configuration/)，了解 Markdown 中西文排版要求

### 01/31 - 02/13

- 01/31-02/13 之前具体做了什么难以一一对应，可能是熟悉 Hugo 框架、了解 Go 语言模版、了解 Xmin 主题结构、不断回忆 HTML 语言等

### 01/31

- 2026 年 1 月 31 日晚，应用益辉兄的 [hugo-xmin](https://github.com/yihui/hugo-xmin) 主题重新开启 Hogo，写下饱含真情的契机，详见[我的第一篇 HUGO 文章](http://yijin.ink/posts/my-first-post/)
