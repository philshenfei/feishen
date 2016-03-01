---
layout: default
title: Blog
---

# My Blogs

{% for post in site.posts %}

<div class="mainpage2">

{% unless post.next %}
<h2>{{ post.date | date: '%Y' }}</h2>

<div class="leftcolumn2">
<h3>{{ post.date | date: '%b.' }}</h3> 
</div>

{% else %}
  {% capture year %}{{ post.date | date: '%Y' }}{% endcapture %}
  {% capture nyear %}{{ post.next.date | date: '%Y' }}{% endcapture %}
  {% if year != nyear %}
  <h2>{{ post.date | date: '%Y' }}</h2>
  {% endif %}
  
  {% capture month %}{{ post.date | date: '%m' }}{% endcapture %}
  {% capture nmonth %}{{ post.next.date | date: '%m' }}{% endcapture %}
  {% if month != nmonth %}
  <div class="leftcolumn2">
  <h3>{{ post.date | date: '%b.' }}</h3> 
  </div>
  {% endif %}
  
{% endunless %}

<div class="rightcolumn2">
<p><a href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a><small> • {{ post.date | date: site.date_format }}</small></p>
</div>

</div>

{% endfor %}


