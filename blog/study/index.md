---
layout: post
title: "Study"
subtitle: 배운것들 정리
post-header: true
header-img: img/header.jpg
type: "Archive"
blog: true
---


<ul class="catalogue">
{% assign sorted = site.pages | sort: 'order' | reverse %}
{% for page in sorted %}
{% if page.category == "study" %}
{% include category-list.html %}
{% endif %}
{% endfor %}

{% assign sorted = site.posts | sort: 'order' | reverse %}
{% for post in sorted %}
{% if post.category == "study" %}
{% include post-list.html %}
{% endif %}
{% endfor %}
</ul>