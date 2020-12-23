---
layout: categories
title: Categories
permalink: /categories/
includelink: true
---

<section class="container posts-content">
{% assign sorted_categories = site.categories | sort %}
{% for category in sorted_categories %}
<h3 id="{{ category[0] }}"><b>{{ category | first }}</b></h3>
<ul class="posts-list">
{% for post in category.last %}
<li class="posts-list-item">
<span class="posts-list-meta">{{ post.date | date:"%Y-%m-%d" }}</span>
<a class="posts-list-name" href="{{ site.url }}{{ post.url }}">{{ post.title }}</a>
</li>
{% endfor %}
</ul>
{% endfor %}
</section>
