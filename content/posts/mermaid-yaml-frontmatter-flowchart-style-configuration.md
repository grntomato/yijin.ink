+++
date = '2026-02-15T12:22:28+08:00'
draft = false
title = 'Mermaid：利用 YAML 轻松配置流程图样式'
author = "Yijin Wang"
tags = ['Mermaid', 'Markdown', '图表', '流程图','YAML', '技术写作']
categories = ["学习录"]
+++

> 太长不看
>
> - init 指令块已弃用，改用 config 来配置。
> - 开启手绘风格 `look: handDrawn`
> - 手绘风格来源于 rough.js，通过改变多种变量参数改变手绘细节

## 背景与配置方式变迁

上上周，帮同事解决如何绘制美观些的组织架构图。我记得之前曾用过一次 mermaid 来绘制流程图，感觉流程图和领导需要的组织架构图也差不多，学会的话以后再做什么图就容易很多。

回温 mermaid 语法，绘制出我们想要的组织架构图，但样式单一。查询后发现其实是有多种不同的方法更改图表样式的。在最初，我搜到的教程都告诉我要使用 init 指令块，花了些时间弄清 init 指令块大致语法后，才发现官方的文档直接指出**已弃用 init 指令块配置**， v10.5.0+ 可以**直接使用 Frontmatter 配置**来更改样式[^1]。

[^1]: mermaid 官方文档 - mermaid-Configeration <https://mermaid.js.org/config/configuration.>

> - Frontmatter (v10.5.0+) - diagram authors can update selected configuration parameters in the frontmatter of the diagram. These are applied to the render config.
> - Directives (Deprecated by Frontmatter) - diagram authors can update selected configuration parameters directly in the diagram code via directives. These are applied to the render config.

这么一来感觉轻松了好些, 因为针对 mermaid 的 Frontmatter 可利用 YAML 语法，只需在图表前加入一段 YAML 代码块即可自定义图图表样式。而 YAML 语法，在本小白看来比起 init 指令块内的像是 JSON 的语法更清晰明了。

所以我记录了一些日后也许会用到的参数配置，组成较为美观、简洁大方的样式，好供以后应用。

## EXAMPLE (详解在后)

总示例 (含主题样式、字体样式、多种节点形状样式、链接样式等)

```markdown
---
config:
  theme: neutral
  themeVariables:
    fontSize: 18px
    fontFamily: FangSong, STFangsong, serif
  flowchart:
    curve: cardinal
    htmlLabels: false
  markdownAutoWrap: true
  look: handDrawn
---

flowchart TD;
  A[节点1]-->B{节点2};
  A---|文本1|C[节点3];
  B-- 文本2 -->D(节点4);
  B-- 文本3 -->E([节点5]);
  D-->F{节点6};
  C-- 文本4 ---->F;
  F-->G((结束));

```

以下是上行代码的效果：

{{< mermaid >}}
%%{init: {
  'theme': 'neutral',
  'themeVariables': {
    'fontSize': '16px',
    'fontFamily': 'FangSong, STFangsong, serif'
  },
  'flowchart': {
    'curve': 'cardinal',
    'htmlLabels': false
  },
  'look': 'handDrawn'
}}%%
flowchart TD;
  A[节点1]-->B{节点2};
  A---|文本1|C[节点3];
  B-- 文本2 -->D(节点4);
  B-- 文本3 -->E([节点5]);
  D-->F{节点6};
  C-- 文本4 ---->F;
  F-->G((结束));
{{< /mermaid >}}

## 样式配置详解

### 选择主题 (`theme`)

mermaid 内置 5 种[主题](https://mermaid.js.org/config/theming.html) (配色方案)，可直接在 Frontmatter 中指定：

- [default](https://github.com/mermaid-js/mermaid/blob/develop/packages/mermaid/src/themes/theme-default.js) - 这是所有图表 的默认主题。

- [neutral](https://github.com/mermaid-js/mermaid/blob/develop/packages/mermaid/src/themes/theme-neutral.js) - 此主题非常适合黑白打印文档。

- [dark](https://github.com/mermaid-js/mermaid/blob/develop/packages/mermaid/src/themes/theme-dark.js) - 此主题与深色元素或暗模式很搭配。

- [forest](https://github.com/mermaid-js/mermaid/blob/develop/packages/mermaid/src/themes/theme-forest.js) - 此主题包含绿色阴影。

- [base](https://github.com/mermaid-js/mermaid/blob/develop/packages/mermaid/src/themes/theme-base.js) - **这是唯一可以修改的主题。使用此主题作为自定义的基础。且需通过 `frontmatter` 配置修改 [`themeVariables`](#themeVaribles)。**

```YAML
---
config:
  theme: default
---
  graph LR
    a --> b
```

{{< mermaid >}}

---

config:

  theme: default

---
  graph LR
    a --> b
{{< /mermaid >}}

```YAML
---
config:
  theme: neutral
---
  graph LR
    a --> b
```

{{< mermaid >}}

---

config:

  theme: neutral

---
  graph LR
    a --> b
{{< /mermaid >}}

```YAML
---
config:
  theme: dark
---
  graph LR
    a --> b
```

{{< mermaid >}}

---
config:

  theme: dark

---
  graph LR
    a --> b
{{< /mermaid >}}

```YAML
---
config:
  theme: forest
---
  graph LR
    a --> b
```

{{< mermaid >}}

---
config:

  theme: forest

---
  graph LR
    a --> b
{{< /mermaid >}}

### 深度自定义主题 (`themeVariables`) {#themeVaribles}

- 要创建自定义主题，需要先修改 `themeVariables` 变量。

- 必须使用 base 主题，它是唯一可修改的主题。_**(个人实验：如果只修改字体，可以不用 base 主题，但涉及颜色时，必须修改为 base)**_

`themeVariables` 里包含 `fontfamily`、`fontSize`、`primaryColor`、`primaryTextColor`、`lineColor` 等变量的设置。

- [themeVariables 变量完整列表 官方文档站](https://mermaid.ai/open-source/config/theming.html#theme-variables)
- [themeVariables 变量完整列表 中文社区文档](https://mermaid.npmjs.net.cn/config/theming.html#theme-variables)

### 开启手绘风格 (`look: handDrawn`)

开启手绘风格语法：`look: handDrawn`
之前搜到的好些写的是过去的语法，在使用 typora 书写时发现无效，以为是 typora 支持的 mermaid 功能不全，后来才发现是搜到的语法过于老旧。

手绘风格 (handDrawn) 由 rough.js 库驱动，可实现线条抖动等效果。若需深度定制 (如调整粗糙度、线条弯曲度)，可传入 roughness、bowing 等参数，具体配置方法详见下方链接 (未完全验证准确性，文档内容为 init 指令块，但其参数与 config 配置（YAML) 相同。

- [如何在 Mermaid 中使用 handdraw 风格？](https://ask.csdn.net/questions/8490397)

## 利用 mermaid AI 学习 mermaid 语法

在查找 mermaid 文档时，我发现 mermaid 现在推出 AI 在线编辑器，注册后有 15 次免费 AI 修改，可以与其对话，她会给出比起其他 AI 在 mermaid 方面更专业更新的知识拓扑，我有不确定的时候，我会问她，发现还是很好用的，并且能从她改的语法中学习 mermaid 标准的语法。

- mermaidd AI 在线编辑器地址：<https://mermaid.ai/>

## 推荐文档

一些可参考的文档页面

**mermaid 官方文档**：

- Mermaid Theme Configuration <https://mermaid.ai/open-source/config/theming.html#theme-configuration>
- Flowcharts - Basic Syntax <https://mermaid.ai/open-source/syntax/flowchart.html#flowcharts-basic-syntax>

**中文社区翻译 (供参考，信息会有滞后)**：

- mermaid 中文文档 <https://docs.min2k.com/zh/mermaid/intro/>
- mermaid 流程图语法 <https://mermaid.npmjs.net.cn/syntax/flowchart.html>
- mermaid 主题配置、主题变量 <https://mermaid.npmjs.net.cn/config/theming.html#theme-variables>
- mermaid 配置属性 <https://mermaid.npmjs.net.cn/config/schema-docs/config.html>
- 一些配色参考 (未实验) <https://blog.csdn.net/u010702254/article/details/147235602>
