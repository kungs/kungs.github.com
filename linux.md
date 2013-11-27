---
layout: page
title: "Linux"
description: ""
---
{% include JB/setup %}

<ul class="posts">
  {% for post in site.posts %}
    {% if post.category == 'linux' %}
      <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
    {% endif %}
  {% endfor %}
</ul>
