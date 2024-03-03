---
layout: page
title: About
description: 这不操作一下？
keywords: About me
comments: true
menu: 关于
permalink: /about/
---

把以前的介绍都删了，随便写写了。

## 联系

<ul>
{% for website in site.data.social %}
<li>{{website.sitename }}：<a href="{{ website.url }}" target="_blank">@{{ website.name }}</a></li>
</ul>


## Skill Keywords

下面的东西我都在项目里或多或少的专门用过，有兴趣的话可以一起讨论，交流经验。

{% for skill in site.data.skills %}
### {{ skill.name }}
<div class="btn-inline">
{% for keyword in skill.keywords %}
<button class="btn btn-outline" type="button">{{ keyword }}</button>
{% endfor %}
</div>
{% endfor %}
