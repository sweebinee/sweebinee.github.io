---
layout: default
title: "Seminar note"
subtitle: 세미나, 강연, 수업 듣고 정리하는 곳
project-header: true
type: "Archive"
category: study
---

<ul class="catalogue">
{% assign sorted = site.posts | sort: 'order' | reverse %}
{% for post in sorted %}
{% if post.category == "seminar" %}
{% include post-list.html %}
{% endif %}
{% endfor %}
</ul>