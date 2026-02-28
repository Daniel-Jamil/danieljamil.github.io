---
layout: default
title: Home
---

<section class="hero">
  <h1>Cloud Architecture. Migration. Strategy.</h1>
  <p>Deep dives into OCI, OCVS, VMware architecture patterns, Migration and DR guides.</p>

<div class="hero-badges">
  <a class="badge" href="{{ '/tag/oci/' | relative_url }}">OCI</a>
  <a class="badge" href="{{ '/tag/ocvs/' | relative_url }}">OCVS</a>
  <a class="badge" href="{{ '/tag/vmware/' | relative_url }}">VMware</a>
  <a class="badge" href="{{ '/tag/hcx/' | relative_url }}">HCX</a>
  <a class="badge" href="{{ '/tag/migration/' | relative_url }}">Migration</a>
  <a class="badge" href="{{ '/tag/dr/' | relative_url }}">DR</a>
</div>
</section>

<h2 class="section-title">Workload migration framework</h2>

{% assign guides = site.posts | where: "guide", true %}
{% for post in guides limit:4 %}
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

<div class="hero-badges section-spacing">
  <a class="badge badge-cta" href="{{ '/guides/' | relative_url }}">View all guides</a>
</div>

<h2 id="articles" class="section-title">Latest articles</h2>

{% for post in site.posts limit:6 %}
<div class="post-card">
  <div class="post-meta">{{ post.date | date: "%Y-%m-%d" }}</div>
  <h3><a class="post-link" href="{{ post.url }}">{{ post.title }}</a></h3>
  {% if post.excerpt %}
    <p>{{ post.excerpt | strip_html | truncate: 160 }}</p>
  {% endif %}
</div>
{% endfor %}

<h2 class="section-title">About</h2>
<div class="post-card">
  <p>
    I’m Daniel — a virtualization & cloud architect at Oracle focused on <strong>Oracle Cloud VMware Solution (OCVS)</strong>,
    <strong>OCI</strong> and enterprise migrations. This blog is a practical notebook: design decisions, lessons learned,
    and repeatable patterns you can apply in real projects.
  </p>
  <p class="muted" style="margin:0;">
    Expect content on: landing zones, connectivity, DNS, vSphere/NSX, HCX, DR architectures, sizing and performance.
  </p>
</div>

<h2 class="section-title">Popular topics</h2>
<div class="hero-badges section-spacing">
  <a class="badge" href="/tag/ocvs/">OCVS</a>
  <a class="badge" href="/tag/oci/">OCI</a>
  <a class="badge" href="/tag/vmware/">VMware</a>
  <a class="badge" href="/tag/hcx/">HCX</a>
  <a class="badge" href="/tag/nsx/">NSX</a>
  <a class="badge" href="/tag/migration/">Migration</a>
  <a class="badge" href="/tag/dr/">DR</a>
</div>

<h2 class="section-title">Get updates</h2>
<div class="post-card">
  <p class="lead" style="margin-top:0;">
    New posts are published as I work through real customer scenarios. Bookmark the site or connect on LinkedIn.
  </p>
  <div class="hero-badges">
    <a class="badge badge-cta" href="https://github.com/" target="_blank" rel="noopener">GitHub</a>
    <a class="badge badge-cta" href="https://www.linkedin.com/" target="_blank" rel="noopener">LinkedIn</a>
  </div>
</div>
