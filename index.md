---
layout: default
title: Home
header_title: The Lab
header_text: Security research, tools, and random observations
---

# Welcome to The Lab

This is my workspace for security research, tool development, and technical analysis. I focus on building practical security tools, breaking down complex problems with data, and documenting interesting findings along the way.

Expect posts about vulnerability research, custom security tools, technical deep-dives, and data analysis. 

## Recent Posts

<div class="posts-list">
  {% for post in site.posts %}
    <article class="post-preview">
      <h2 class="post-title">
        <a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a>
      </h2>
      <p class="post-meta">
        Posted on {{ post.date | date: "%B %-d, %Y" }}
      </p>
      <div class="post-entry">
        {{ post.excerpt }}
      </div>
      <a href="{{ site.baseurl }}{{ post.url }}" class="read-more">Read More</a>
    </article>
  {% endfor %}
</div>
