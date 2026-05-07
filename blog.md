---
layout: default
title: Blog — Cenfracee
permalink: /blog/
---

<section class="section">
  <div class="container">

    <div style="max-width: 800px;">
      <p class="label teal anim-1" style="margin-bottom:14px;">// Lab Notes &amp; Updates</p>
      <h1 class="d-lg anim-2" style="margin-bottom:20px;">FROM THE<br>LAB.</h1>
      <p class="anim-3" style="font-size:.95rem;color:var(--muted);max-width:48ch;line-height:1.85;margin-bottom:72px;">
        Progress dispatches, technology breakdowns, and field notes from
        Cenfracee's operations across energy, chemistry, and mining in Sri Lanka.
      </p>
    </div>

    <div style="border-top:1px solid var(--border);">
      {% for post in site.posts %}
      <a href="{{ post.url }}" style="display:grid;grid-template-columns:110px 1fr;gap:32px;padding:36px 0;border-bottom:1px solid var(--border);text-decoration:none;transition:padding-left .25s,background .2s;" onmouseenter="this.style.paddingLeft='12px'" onmouseleave="this.style.paddingLeft='0'">
        <div>
          <p class="mono muted" style="font-size:.62rem;letter-spacing:.12em;line-height:1.9;">
            {{ post.date | date: "%b %d" }}<br>
            <span style="color:var(--dim);">{{ post.date | date: "%Y" }}</span>
          </p>
        </div>
        <div>
          <h2 style="font-family:var(--display);font-size:2rem;margin-bottom:10px;color:var(--text);transition:color .25s;" onmouseenter="this.style.color='var(--amber)'" onmouseleave="this.style.color='var(--text)'">{{ post.title }}</h2>
          <p style="font-size:.88rem;color:var(--muted);line-height:1.75;max-width:62ch;">
            {{ post.excerpt | default: post.content | strip_html | truncate: 180 }}
          </p>
          {% if post.tags %}
          <div style="margin-top:14px;display:flex;gap:6px;flex-wrap:wrap;">
            {% for tag in post.tags limit:3 %}
              <span class="tag tag-dim" style="font-size:.55rem;">{{ tag }}</span>
            {% endfor %}
          </div>
          {% endif %}
          <span class="mono" style="font-size:.62rem;letter-spacing:.15em;color:var(--amber);text-transform:uppercase;margin-top:14px;display:inline-block;">Read → </span>
        </div>
      </a>
      {% endfor %}

      {% if site.posts.size == 0 %}
      <div style="padding:80px 0;text-align:center;">
        <p class="mono muted" style="font-size:.75rem;letter-spacing:.2em;">NO POSTS YET — CHECK BACK SOON.</p>
      </div>
      {% endif %}
    </div>

  </div>
</section>
