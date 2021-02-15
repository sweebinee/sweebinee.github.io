---
layout: default
title: "Daily"
subtitle: 대학원생의 일상이야기
description: 일상이야기
project-header: true
header-img: /blog/img/about.jpg
type: "Daily"
blog: true
---

<ul class="catalogue">
{% assign sorted = site.posts | sort: 'order' | reverse %}
{% for post in sorted %}
{% if post.category == "daily" %}
{% include post-list.html %}
{% endif %}
{% endfor %}
</ul>