---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults
layout: default
title: Le blog de Usawa
---

# Mes articles

{% for post in site.posts %}
<ul>
<span>{{ post.date | date: "%d/%m/%Y à %H:%M" }}</span> 
<li><h3><a href="{{ post.url | relative_url }}">{{ post.title }} </a></h3></li>
 
</ul>
{% endfor %}