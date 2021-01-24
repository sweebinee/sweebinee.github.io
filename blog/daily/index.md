---
layout: default
title: "Daily"
description: 일상이야기
project-header: true
header-img: img/about.jpg
type: "Daily"
blog: true
order: 11
---

<ul class="catalogue">
{% assign sorted = site.pages | sort: 'order' | reverse %}
{% for page in sorted %}
{% if page.category == "daily" %}
{% include post-list.html %}
{% endif %}
{% endfor %}
</ul>