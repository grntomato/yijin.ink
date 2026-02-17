+++
draft = true
title = 'Timeline'

+++

<!-- ## 时光留痕 -->

<!-- --- -->

2026 年 2 月 15 日晚，决定建立一个『时光留痕』页面，记录我对博客的步步完善。希望自己投入的时间与精力能被『看见』。

---

## 2026

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
