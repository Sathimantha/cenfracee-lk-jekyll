---
layout: default
title: Blog — Cenfracee
permalink: /blog/
---

<div class="max-w-7xl mx-auto px-6 py-20">
  <div class="text-center mb-16">
    <h1 class="heading text-5xl font-bold tracking-tighter">Blog</h1>
    <p class="text-xl text-slate-400 mt-4">Latest updates, insights, and breakthroughs from Cenfracee</p>
  </div>

  <div class="grid gap-8 max-w-3xl mx-auto">
    {% for post in site.posts %}
    <a href="{{ post.url }}" class="group block glass border border-slate-700 hover:border-emerald-400 rounded-3xl p-8 transition-all">
      <div class="flex justify-between items-start mb-4">
        <h2 class="heading text-3xl font-semibold group-hover:text-emerald-400 transition">{{ post.title }}</h2>
        <span class="text-sm text-slate-400 font-mono whitespace-nowrap ml-6">{{ post.date | date: "%b %d, %Y" }}</span>
      </div>
      <p class="text-slate-300 line-clamp-3">{{ post.excerpt | default: post.content | strip_html | truncate: 180 }}</p>
      <div class="mt-6 text-emerald-400 text-sm font-medium flex items-center gap-2">
        Read more 
        <i class="fa-solid fa-arrow-right transition group-hover:translate-x-1"></i>
      </div>
    </a>
    {% endfor %}
  </div>

  {% if site.posts.size == 0 %}
    <p class="text-center text-slate-400 py-12">No posts yet. Check back soon!</p>
  {% endif %}
</div>