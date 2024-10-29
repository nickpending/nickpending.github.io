---
layout: default
title: The Lab
---

## Welcome
This is my workspace for security research, tool development, and technical analysis. Here you'll find documentation of my experiments, security tools I've built, and observations about interesting security issues.

## Recent Posts
{% for post in site.posts %}
  <div class="post">
    <h2 class="post-title">
      <a href="{{ post.url }}">{{ post.title }}</a>
    </h2>
    <span class="post-date">{{ post.date | date_to_string }}</span>
    {{ post.excerpt }}
  </div>
{% endfor %}
