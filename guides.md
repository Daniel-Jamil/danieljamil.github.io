---
layout: default
title: Guides
permalink: /guides/
---

<h1 style="margin-top:10px;">Architecture Guides</h1>
<p class="muted" style="margin-top:6px;">Evergreen reference architectures and blueprints.</p>

{% assign guides = site.posts | where: "guide", true %}
{% for post in guides %}
  <div class="post-card">
    <div class="post-meta">Guide • {{ post.date | date: "%Y-%m-%d" }}</div>
    <h3><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h3>
    {% if post.summary %}
      <p>{{ post.summary }}</p>
    {% elsif post.excerpt %}
      <p>{{ post.excerpt | strip_html | truncate: 180 }}</p>
    {% endif %}
  </div>
{% endfor %}
