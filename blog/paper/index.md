---
layout: post
title: "Paper list"
subtitle: 읽은 논문 정리
post-header: true
header-img: img/paper_header.png
type: "Archive"
blog: true
---

**Title** | **Journal** | **Publish date** | **keywords** | **Status**
----------|:-----------:|:----------------:|--------------|:----------:
Identifying cell populations with scRNASeq | Molecular Aspects of Medicine | 25 July 2017 | scRNAseq, cell type annotaion | `Reading` 


<ul class="catalogue">
{% assign sorted = site.pages | sort: 'order' | reverse %}
{% for page in sorted %}
{% if page.category == "study" %}
{% include category-list.html %}
{% endif %}
{% endfor %}
</ul>