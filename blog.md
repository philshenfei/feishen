---
layout: default
title: Blog
---

# My Blogs

{% for post in site.posts %}

{% unless post.next %}
## {{ post.date | date: '%Y' }}

### {{ post.date | date: '%b.' }}
{% else %}
  {% capture year %}{{ post.date | date: '%Y' }}{% endcapture %}
  {% capture nyear %}{{ post.next.date | date: '%Y' }}{% endcapture %}
  {% if year != nyear %}
## {{ post.date | date: '%Y' }}
  {% endif %}
  
  {% capture month %}{{ post.date | date: '%m' }}{% endcapture %}
  {% capture nmonth %}{{ post.next.date | date: '%m' }}{% endcapture %}
  {% if month != nmonth %}
### {{ post.date | date: '%b.' }}
  {% endif %}
  
{% endunless %}

<p><a href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a><small> • {{ post.date | date: site.date_format }}</small></p>
{% endfor %}
