---
layout: default
title: Home
---

This is my workspace for security research, tool development, and technical analysis. I focus on building practical security tools, breaking down complex problems with data, and documenting interesting findings along the way.

Expect posts about vulnerability research, custom security tools, technical deep-dives, and data analysis. 

<div class="posts">
  {% for post in site.posts %}
  <div class="post">
    <h2 class="post-title">
      <a href="{{ post.url }}">{{ post.title }}</a>
    </h2>
    <span class="post-date">{{ post.date | date_to_string }}</span>
    {{ post.excerpt }}
  </div>
  {% endfor %}
</div>
