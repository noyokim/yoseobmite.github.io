---
layout: default
title: home
permalink: /blog/
robots: noindex
---
<div class="tags-expo">
  <div class="tags-expo-list">
    {% for tag in site.categories %}
    <a href="#{{ tag[0] | slugify }}" class="post-tag">{{ tag[0] }}</a>
    {% endfor %}
  </div>
  <hr/>
  <div class="tags-expo-section">
    {% for tag in site.categories %}
    <h2 id="{{ tag[0] | slugify }}">{{ tag[0] }}</h2>
    <ul class="tags-expo-posts">
      {% for post in tag[1] %}
        <a class="post-title" href="{{ site.baseurl }}{{ post.url }}">
      <li>
        {{ post.title }}
      <small class="post-date">{{ post.date | date_to_string }}</small>
      </li>
      </a>
      {% endfor %}
    </ul>
    {% endfor %}
  </div>
</div>
 <div class="posts">
  {% for post in paginator.posts %}
  <div class="post">
    <h1 class="post-title">
      <a href="{{ site.baseurl }}{{ post.url }}">
        {{ post.title }}
      </a>
    </h1>

    <span class="post-date">{{ post.date | date_to_string }}</span>
    {% if post.tags %} | 
    {% for tag in post.tags %}
    <a href="{{ site.baseurl }}{{ site.tag_page }}#{{ tag | slugify }}" class="post-tag">{{ tag }}</a>
    {% endfor %}
    {% endif %}

    <article>
      {{ post.excerpt | markdownify }}
    <article>
    <div class="post-more">
      {% if site.disqus_short_name %}
      <a href="{{ site.baseurl }}{{ post.url }}#disqus_thread"> <i class="fa fa-comments" aria-hidden="true"></i>Comment</a>&nbsp;
      {% endif %}
      <a href="{{ site.baseurl }}{{ post.url }}"><i class="fa fa-plus-circle" aria-hidden="true"></i>Read more</a>
    </div>
  </div>
  {% endfor %}
</div>
