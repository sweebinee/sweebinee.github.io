---
layout: default
title: "Study"
description: 배운것들 정리
project-header: true
header-img: study/img/header.jpg
type: "Archive"
blog: true
---

<ul class="catalogue">
{% assign sorted = site.pages | sort: 'order' | reverse %}
{% for page in sorted %}
{% if page.category == "study" %}
{% include post-list.html %}
{% endif %}
{% endfor %}
</ul>