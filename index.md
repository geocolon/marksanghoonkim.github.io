---
layout: page
title: Mark S Kim - Home
tagline: Home of Mark's Thoughts
---
{% include JB/setup %}
    
## Blog Posts

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

## Web Development Projects

This section is still unfinished. 

## Interesting Links

This section is still unfinished. 


