---
layout: default
title: "R"
description: 자주쓰는 R code 조각 모아두는 곳
project-header: true
type: "Archive"
category: study
---

<ul class="catalogue">
{% assign sorted = site.posts | sort: 'order' | reverse %}
{% for post in sorted %}
{% if post.category == "Rcode" %}
{% include post-list.html %}
{% endif %}
{% endfor %}
</ul>