---
layout: page
title: Web.Tech
tagline: Web開発技術を勉強してメモするブログ
---
{% include JB/setup %}
{% for post in site.posts limit:10 %}
<h2>{{ post.title }}</h2>
<p class="date">{{ post.date | date: "%Y年%m月%d日" }}</p>

{{ post.excerpt }}
<a href="{{ post.url }}">続きを読む</a></h1>

<ul class="tag_box inline">
{% unless post.categories == empty %}
  <li><span class="icon-folder-open"></span></li>
  {% assign categories_list = post.categories %}
  {% include JB/categories_list %}
{% endunless %}

{% unless post.tags == empty %}
  <li><span class="icon-tags"></span></li>
  {% assign tags_list = post.tags %}
  {% include JB/tags_list %}
{% endunless %}
</ul>
<hr>

{% endfor %}

