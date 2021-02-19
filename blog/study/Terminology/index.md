---
layout: default
title: "Terminology"
subtitle: "공부하면서 몰랐던 용어들 찾아보고 정리해두는 곳"
project-header: true
type: "Archive"
category: study
---

<ul class="catalogue">
{% assign sorted = site.posts | sort: 'order' | reverse %}
{% for post in sorted %}
{% if post.category == "term" %}
{% include post-list.html %}
{% endif %}
{% endfor %}
</ul>