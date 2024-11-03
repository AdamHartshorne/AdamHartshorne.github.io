---
layout: default  # or home, depending on your theme
title: Welcome!
---

# Welcome to My Blog
## Testing

### Recent Posts

{% for post in site.posts %}
  * [{{ post.title }}]({{ post.url }}) - {{ post.date | date: "%B %d, %Y" }}
{% endfor %}
