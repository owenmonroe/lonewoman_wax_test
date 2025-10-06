---
layout: page
title: "All Items"
permalink: /items/
---
<ul>
  {% assign sorted = site.items | sort: 'title' %}
  {% for doc in sorted %}
    <li><a href="{{ doc.url | relative_url }}">{{ doc.title }}</a></li>
  {% endfor %}
</ul>