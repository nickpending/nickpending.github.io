---
layout: default
title: "The Lab"
header:
  title: "The Lab"
  headline: "Security research and analysis"
---

This is my workspace for security research, tool development, and technical analysis. I focus on building practical security tools, breaking down complex problems with data, and documenting interesting findings along the way.

Expect posts about vulnerability research, custom security tools, technical deep-dives, and data analysis. 

## Recent Posts

{% for post in site.posts %}
<article class="post">
  <h2 class="post-title">
    <a href="{{ post.url }}">{{ post.title }}</a>
  </h2>
  <span class="post-date">{{ post.date | date: "%B %-d, %Y" }}</span>
  <div class="post-excerpt">
    {{ post.excerpt }}
  </div>
</article>
{% endfor %}
