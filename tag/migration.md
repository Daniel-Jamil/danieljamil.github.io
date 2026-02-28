---
layout: tag
title: Migration
tag: migration
permalink: /tag/migration/
---

<h1 style="margin-top:10px;">Migration</h1>
<p class="muted" style="margin-top:6px;">Posts tagged <strong>migration</strong>.</p>

{% for post in site.posts %}
  {% if post.tags contains "migration" %}
    <div class="post-card">
      <div class="post-meta">{{ post.date | date: "%Y-%m-%d" }}</div>
      <h3><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h3>
      {% if post.excerpt %}
        <p>{{ post.excerpt | strip_html | truncate: 180 }}</p>
      {% endif %}
    </div>
  {% endif %}
{% endfor %}
