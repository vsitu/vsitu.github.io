---
layout: page
title: Links
description: 其他人的各种链接
keywords: 友情链接
comments: true
menu: 链接
permalink: /links/
---

> 生活 

<ul>
{% for link in site.data.links %}
  {% if link.src == 'life' %}
  <li><a href="{{ link.url }}" target="_blank">{{ link.name}}</a></li>
  {% endif %}
{% endfor %}
</ul>

> 数据资料整理

<ul>
{% for link in site.data.links %}
  {% if link.src == 'data' %}
  <li><a href="{{ link.url }}" target="_blank">{{ link.name}}</a></li>
  {% endif %}
{% endfor %}
</ul>

> 朋友们

<ul>
{% for link in site.data.links %}
  {% if link.src == 'friend' %}
  <li><a href="{{ link.url }}" target="_blank">{{ link.name}}</a></li>
  {% endif %}
{% endfor %}
</ul>

> 学术界

<ul>
{% for link in site.data.links %}
  {% if link.src == 'academia' %}
  <li><a href="{{ link.url }}" target="_blank">{{ link.name}}</a></li>
  {% endif %}
{% endfor %}
</ul>

> 程序相关大佬

<ul>
{% for link in site.data.links %}
  {% if link.src == 'code' %}
  <li><a href="{{ link.url }}" target="_blank">{{ link.name}}</a></li>
  {% endif %}
{% endfor %}
</ul>