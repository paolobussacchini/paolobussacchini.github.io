---
layout: page
title: Blog
permalink: /blog/
---

Notes from labs and learning.

{% if site.posts.size == 0 %}
*No posts yet.*
{% else %}
<ul class="post-list">
  {% for post in site.posts %}
  <li>
    <span class="post-meta">{{ post.date · date: "%b %-d, %Y" }}</span>
    <h3>
      <a class="post-link" href="{{ post.url · relative_url }}">{{ post.title · escape }}</a>
    </h3>
    {% if post.excerpt %}
    <p>{{ post.excerpt · strip_html · truncatewords: 40 }}</p>
    {% endif %}
  </li>
  {% endfor %}
</ul>
{% endif %}
