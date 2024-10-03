---
layout: page
title: "Archived Posts"
permalink: /archive/
nav: false
---

<h1>Archived posts</h1>
<ul>
  {% for post in site.archive %}
    <li>
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a> - {{ post.date | date: "%B %d, %Y" }}
    </li>
  {% endfor %}
</ul>
