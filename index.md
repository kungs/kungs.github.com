---
layout: page
title: Kungs
tagline: Supporting tagline
---
{% include JB/setup %}

<ul class="posts">
  {% for post in site.posts %}
    <li>
    	<span class="post-title">{{ post.date | date_to_string }} &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a>
    	</span>{{ post.content | split: '<!-- more -->' | first }}
    </li>
  {% endfor %}
</ul>
