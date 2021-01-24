---
layout: default
title: "Blog"
description: 아주 가끔씩 관심 분야의 글을 올려요.
main: true
project-header: true
header-img: img/blog_main.png
---

<ul class="catalogue">
{% for page in site.pages %}
{% if page.blog == true %}
{% include category-list.html %}
{% endif %}
{% endfor %}
</ul>
