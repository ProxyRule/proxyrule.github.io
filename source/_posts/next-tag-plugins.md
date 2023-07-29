---
title: NexT 主题之 Tag Plugins
date: 2020-08-26 11:50:01
tags:
  - Hexo
  - NexT
---

NexT 主题通过一些标签「插件」来增强页面显示效果和提供额外信息，如按钮、标注、流程图等。以下是我所需要的部分，完整文档可查阅[这里](https://theme-next.js.org/docs/tag-plugins/)。

<!-- more -->

### 居中引用

用法

```markdown
{% centerquote %}最好只有一行文字{% endcenterquote %}

// 简写形式
{% cq %}最好只有一行文字{% endcq %}
```

示例

{% centerquote %}最好只有一行文字{% endcenterquote %}

### 按钮

用法

```markdown
{% button url, text, icon [class], [title] %}

// 简写形式
{% btn url, text, icon [class], [title] %}

url     : URL 绝对或者相对路径。
text    : 按钮显示的文字。text 和 icon 至少指定一个。
icon    : Font Awesome icon 名。text 和 icon 至少指定一个。
[class] : 可选参数。icon 样式：fa-fw | fa-lg | fa-2x | fa-3x | fa-4x | fa-5x
[title] : 可选参数。鼠标悬停时显示的文字。
```

示例

```markdown
{% button https://zs.fyi, ZS.FYI, home fa-fw fa-lg, 知识参考 %}
```

{% button https://zs.fyi, ZS.FYI, home fa-fw fa-lg, 知识参考 %}

更多示例可查阅该[文档](https://theme-next.js.org/docs/tag-plugins/button.html)。

### 标注

用法

```markdown
{% label [class]@text %}

[class] : 可选参数，未指定则使用浏览器默认样式。支持的值有：default | primary | success | info | warning | danger
text    : 'success @text' 和 'success@text' 的效果是一样的。
```

示例

```markdown
{% label default @default %}
{% label primary @primary %}
{% label success @success %}
*{% label info @italic + info %}*
**{% label warning @bold + warning %}**
~~{% label danger @strikethrough + danger %}~~
<mark>mark</mark>
```

- {% label default @default %}
- {% label primary @primary %}
- {% label success @success %}
- *{% label info @italic + info %}*
- **{% label warning @bold + warning %}**
- ~~{% label danger @strikethrough + danger %}~~
- <mark>mark</mark>

### Mermaid

Mermaid 是一个用于画流程图、状态图、时序图、甘特图等的库，使用 JS 进行本地渲染。

用法

```markdown
{% mermaid type %}
something
{% endmermaid %}

type : 类型
```

可以在[这里](https://mermaid-js.github.io/mermaid/)查阅支持哪些图及其详细用法。

示例

```markdown
{% mermaid pie %}
"Dogs" : 386
"Cats" : 85
"Rats" : 15
{% endmermaid %}
```

{% mermaid pie %}
"Dogs" : 386
"Cats" : 85
"Rats" : 15
{% endmermaid %}

### 注释说明

用法

```markdown
{% note [class] [no-icon] [summary] %}
任何内容
{% endnote %}

[class]   : 可选参数。支持的值有：default | primary | success | info | warning | danger
[no-icon] : 可选参数。不显示 icon
[summary] : 可选参数。
```

示例

```markdown
{% note %}
假装是四级标题，{% label @未定义 %}样式
{% endnote %}
```

{% note %}
假装是四级标题，{% label @未定义 %}样式
{% endnote %}

```markdown
{% note default %}
假装是四级标题，{% label default @default %} 样式
{% endnote %}
```

{% note default %}
假装是四级标题，{% label default @default %} 样式
{% endnote %}

```markdown
{% note primary %}
假装是四级标题，{% label primary @primary %} 样式
{% endnote %}
```

{% note primary %}
假装是四级标题，{% label primary @primary %} 样式
{% endnote %}

```markdown
{% note success %}
假装是四级标题，{% label success @success %} 样式
{% endnote %}
```

{% note success %}
假装是四级标题，{% label success @success %} 样式
{% endnote %}

```markdown
{% note info %}
假装是四级标题，{% label info @info %} 样式
{% endnote %}
```

{% note info %}
假装是四级标题，{% label info @info %} 样式
{% endnote %}

```markdown
{% note warning %}
假装是四级标题，{% label warning @warning %} 样式
{% endnote %}
```

{% note warning %}
假装是四级标题，{% label warning @warning %} 样式
{% endnote %}

```markdown
{% note danger %}
假装是四级标题，{% label danger @danger %} 样式
{% endnote %}
```

{% note danger %}
假装是四级标题，{% label danger @danger %} 样式
{% endnote %}

### 嵌入 PDF

用法

```markdown
{% pdf url [height] %}

url      : PDF 文件的绝对路径。
[height] : 可选参数。高度（单位：px）。
```

示例

暂无
