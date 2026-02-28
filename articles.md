---
layout: default
title: Articles
permalink: /articles/
---

<h1 style="margin-top:10px;">Articles</h1>
<p class="muted" style="margin-top:6px;">All posts — newest first.</p>

{% for post in site.posts %}
  <div class="post-card">
    <div class="post-meta">{{ post.date | date: "%Y-%m-%d" }}</div>
    <h3><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h3>
    {% if post.excerpt %}
      <p>{{ post.excerpt | strip_html | truncate: 180 }}</p>
    {% endif %}
  </div>
{% endfor %}
