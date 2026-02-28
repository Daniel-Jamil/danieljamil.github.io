---
layout: default
title: Home
---

<div class="hero">
  <h1>Cloud Architecture. Migration. Strategy.</h1>
  <p>
    Technical deep dives on OCI, OCVS, VMware and enterprise migration design.
    Real-world architecture patterns and field-tested solutions.
  </p>
</div>

<h2 id="articles" class="section-title">Latest Articles</h2>

{% for post in site.posts %}
<div class="post-card">
  <h3><a href="{{ post.url }}">{{ post.title }}</a></h3>
  <p>{{ post.excerpt }}</p>
</div>
{% endfor %}
