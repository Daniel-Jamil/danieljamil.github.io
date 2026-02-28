/* ===== Home polish ===== */
.muted { color: var(--muted); }

.lead {
  color: var(--text);
  opacity: 0.92;
  line-height: 1.65;
}

.section-spacing { margin-bottom: 10px; }

.post-link{
  color: var(--accent);
  text-decoration: none;
}
.post-link:hover{
  text-decoration: underline;
  text-decoration-color: rgba(56,189,248,0.55);
  text-underline-offset: 3px;
}

/* Better typography in cards */
.post-card p{ max-width: 78ch; }

/* Slightly nicer separators between sections */
.section-title{
  position: relative;
}
.section-title::after{
  content:"";
  display:block;
  height: 1px;
  margin-top: 10px;
  background: linear-gradient(90deg, rgba(148,163,184,0.22), transparent);
}

/* CTA badges for external links */
.badge-cta{
  border-color: rgba(56,189,248,0.28);
}
.badge-cta:hover{
  border-color: rgba(56,189,248,0.45);
}

/* Reduce huge empty space feeling on tall screens */
.hero{ padding: 52px 0 28px 0; }
.container{ padding-bottom: 64px; }
