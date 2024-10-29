---
layout: default
title: Posts
---

## All Posts

{% for post in site.posts %}
  <div class="post">
    <h2 class="post-title">
      <a href="{{ post.url }}">{{ post.title }}</a>
    </h2>
    <span class="post-date">{{ post.date | date_to_string }}</span>
    {{ post.excerpt }}
  </div>
{% endfor %}
