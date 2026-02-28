---
layout: default
title: Home
---

<div class="hero">
  <h1>Cloud Architecture. Migration. Strategy.</h1>
  <p>
    OCI, OCVS and VMware deep dives — architecture patterns, troubleshooting and field notes.
  </p>

  <div class="hero-badges">
    <span class="badge">OCI</span>
    <span class="badge">OCVS</span>
    <span class="badge">VMware</span>
    <span class="badge">HCX</span>
    <span class="badge">DR</span>
  </div>
</div>

<h2 id="articles" class="section-title">Latest Articles</h2>

{% for post in site.posts %}
<div class="post-card">
  <div class="post-meta">{{ post.date | date: "%Y-%m-%d" }}</div>
  <h3><a href="{{ post.url }}">{{ post.title }}</a></h3>
  <p>{{ post.excerpt }}</p>
</div>
{% endfor %}
