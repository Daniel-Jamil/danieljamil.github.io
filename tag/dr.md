---
layout: tag
title: "Tag: DR"
permalink: /tag/dr/
---

<h1 style="margin-top:10px;">DR</h1>
<p class="muted" style="margin-top:6px;">Posts tagged <strong>dr</strong>.</p>

{% for post in site.posts %}
  {% if post.tags contains "dr" %}
    <div class="post-card">
      <div class="post-meta">{{ post.date | date: "%Y-%m-%d" }}</div>
      <h3><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h3>
      {% if post.excerpt %}
        <p>{{ post.excerpt | strip_html | truncate: 180 }}</p>
      {% endif %}
    </div>
  {% endif %}
{% endfor %}
