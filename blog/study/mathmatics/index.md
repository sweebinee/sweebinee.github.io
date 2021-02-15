---
layout: default
title: "Math things"
subtitle: 수학 짱시룸
project-header: true
type: "Archive"
category: study
---

<ul class="catalogue">
{% assign sorted = site.posts | sort: 'order' | reverse %}
{% for post in sorted %}
{% if post.category == "math" %}
{% include post-list.html %}
{% endif %}
{% endfor %}
</ul>