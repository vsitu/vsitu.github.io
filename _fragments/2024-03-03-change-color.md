---
layout: fragment
title: 尝试修改 Github Page 模板的标题颜色
tags: [Github, html, css]
description: 学习编辑 Github Pages 的 html 和 css 文件
keywords: Github, html, css
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---

首先，修改了 index.html 里面的 class="collection-head" 的属性，改成 class="collection-head geopattern"，这个和其他带背景花纹的页面的叫法一样。
改了之后再在这个尖括号里添加指定纹理id的字段，确保纹理固定，不会一刷新就变。之后看起来是这个样子：
class="collection-head geopattern" data-pattern-id="{{ page.title | truncate: 15}}"


然后，修改了颜色，搜索 4183c4，在index.css 里面改，这个是 site-header 的背景颜色，改了之后就变成其他颜色了。
新颜色的效果可以去这里看：[Hex to RGB Converter](https://www.rapidtables.com/convert/color/hex-to-rgb.html)