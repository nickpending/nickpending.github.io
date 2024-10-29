---
layout: default
title: The Lab
---

## About
This is my workspace for security research, tool development, and technical analysis. I focus on building practical security tools, breaking down complex problems with data, and documenting interesting findings along the way.

## Focus Areas
Expect posts about:
- Vulnerability research
- Custom security tools
- Technical deep-dives
- Data analysis 

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
