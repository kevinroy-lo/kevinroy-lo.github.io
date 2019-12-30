---
title: { { title } }
copyright: true
reward: true
rating: true
related_posts: false
tags:
categories:
---

<!-- more -->

## 文本居中的引用

此标签将生成一个带上下分割线的引用，同时引用内文本将自动居中。 文本居中时，多行文本若长度不等，视觉上会显得不对称，因此建议在**引用单行文本的场景下使用**。 例如作为文章开篇引用 或者 结束语之前的总结引用。

``` 
<!-- HTML方式: 直接在 Markdown 文件中编写 HTML 来调用 -->
<!-- 其中 class="blockquote-center" 是必须的 -->
<blockquote class="blockquote-center">blah blah blah</blockquote>

<!-- 标签 方式，要求版本在0.4.5或以上 -->
{% centerquote %}blah blah blah{% endcenterquote %}

<!-- 标签别名 -->
{% cq %} blah blah blah {% endcq %}
```

## 突破容器宽度限制的图片

当使用此标签引用图片时，图片将自动扩大 26%，并突破文章容器的宽度。 此标签使用于需要突出显示的图片, 图片的扩大与容器的偏差从视觉上提升图片的吸引力。

``` 
<!-- HTML方式: 直接在 Markdown 文件中编写 HTML 来调用 -->
<!-- 其中 class="full-image" 是必须的 -->
<img src="/image-url" class="full-image" />

<!-- 标签 方式，要求版本在0.4.5或以上 -->
{% fullimage /image-url, alt, title %}

<!-- 别名 -->
{% fi /image-url, alt, title %}
```

## Bootstrap Callout

``` 
{% note class_name %} Content (md partial supported) {% endnote %}
```

其中，class_name 可以是以下列表中的一个值：

* [ ] default
* [ ] primary
* [ ] success
* [ ] info
* [ ] warning
* [ ] danger

## 指定七牛云图片的大小

``` js
? imageView / 2 / w / 300
```

<div class="reference-linking">参考链接</div>
-[]()

