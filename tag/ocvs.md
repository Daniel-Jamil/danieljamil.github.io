---
layout: tag
title: OCVS
tag: ocvs
permalink: /tag/ocvs/
---

<h1 style="margin-top:10px;">OCVS</h1>
<p class="muted" style="margin-top:6px;">Posts tagged <strong>ocvs</strong>.</p>

{% for post in site.posts %}
  {% if post.tags contains "ocvs" %}
    <div class="post-card">
      <div class="post-meta">{{ post.date | date: "%Y-%m-%d" }}</div>
      <h3><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h3>
      {% if post.excerpt %}
        <p>{{ post.excerpt | strip_html | truncate: 180 }}</p>
      {% endif %}
    </div>
  {% endif %}
{% endfor %}
