---
layout: default
title: "Tools: installation & simple guide"
subtitle: "분석 툴 설치 가이드 + 에러 정리"
project-header: true
type: "Archive"
category: study
---

<ul class="catalogue">
{% assign sorted = site.posts | sort: 'order' | reverse %}
{% for post in sorted %}
{% if post.category == "Tools" %}
{% include post-list.html %}
{% endif %}
{% endfor %}
</ul>