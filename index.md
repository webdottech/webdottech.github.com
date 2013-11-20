---
layout: page
title: Web.Tech
tagline: 最新Web技術を勉強してまとめるブログ
---
{% include JB/setup %}

<div class="row-fluid">
  <div class="span8">
    {% for post in site.posts limit:5 %}
      <h3>{{ post.title }}</h3>
      <p class="date">{{ post.date | date: "%Y年%m月%d日" }}</p>

      {{ post.excerpt }}
      <a href="{{ post.url }}">続きを読む</a></h1>

      {% unless post.categories == empty %}
      <ul class="tag_box inline">
        <li><i class="icon-folder-open"></i></li>
        {% assign categories_list = post.categories %}
        {% include JB/categories_list %}
      </ul>
      {% endunless %}

      {% unless post.tags == empty %}
        <ul class="tag_box inline">
          <li><i class="icon-tags"></i></li>
          {% assign tags_list = post.tags %}
          {% include JB/tags_list %}
        </ul>
      {% endunless %}

    {% endfor %}
  </div>
  <div class="span4">
	<i class="icon-folder-open"></i>
	<span class="label label-info">Categories</span>
    <ul class="tag_box inline">
      {% assign categories_list = site.categories %}
      {% include JB/categories_list %}
    </ul>
	<i class="icon-tags"></i>
	<span class="label label-info">Tags</span>
	<ul class="tag_box inline">
      {% assign tags_list = site.tags %}  
      {% include JB/tags_list %}
    </ul>
  </div>
</div>
{% include JB/setup %}

