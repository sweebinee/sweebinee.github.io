---
layout: default
title: "scRNAseq analysis"
subtitle: single cell 분석 기법 및 공부 내용
project-header: true
type: "Archive"
category: study
---

<ul class="catalogue">
{% assign sorted = site.pages | sort: 'order' | reverse %}
{% for page in sorted %}
{% if page.category == "study/singleCell" %}
{% include post-list.html %}
{% endif %}
{% endfor %}
</ul>