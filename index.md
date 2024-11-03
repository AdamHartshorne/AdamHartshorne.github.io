---
layout: default  # or home, depending on your theme
title: Welcome!
---

# Adam Hartshorne's Portfolio 

### Recent Posts

{% for post in site.posts %}
  * [{{ post.title }}]({{ post.url }}) - {{ post.date | date: "%B %d, %Y" }}
{% endfor %}
