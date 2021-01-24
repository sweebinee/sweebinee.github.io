---
layout: default
title: "Daily"
description: 일상이야기
project-header: true
header-img: /blog/img/about.jpg
type: "Daily"
blog: true
---

<ul class="catalogue">
{% assign sorted = site.pages | sort: 'order' | reverse %}
{% for page in sorted %}
{% if page.category == "daily" %}
{% include post-list.html %}
{% endif %}
{% endfor %}
</ul>