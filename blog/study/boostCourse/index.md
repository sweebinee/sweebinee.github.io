---
layout: default
title: "BoostCourse"
description: 부스트코스 강의 듣고 정리하는 곳
project-header: true
type: "Archive"
category: study
---

<ul class="catalogue">
{% assign sorted = site.pages | sort: 'order' | reverse %}
{% for page in sorted %}
{% if page.category == "study/boostCourse" %}
{% include post-list.html %}
{% endif %}
{% endfor %}
</ul>