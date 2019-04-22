---
title:  "Blogs"
layout: archive
permalink: /Blogs/
author_profile: true
comments: true
---


<ul>
  {% for post in site.posts %}
    {% unless post.next %}
      <font color="#708090" size="5"><h2>{{ post.date | date: '%Y' }}</h2></font>
      <hr>
    {% else %}
      {% capture year %}{{ post.date | date: '%Y' }}{% endcapture %}
      {% capture nyear %}{{ post.next.date | date: '%Y' }}{% endcapture %}
      {% if year != nyear %}
        <font color="#708090"><h2>{{ post.date | date: '%Y' }}</h2></font>
        <hr>
      {% endif %}

    {% endunless %}
   {% include archive-single.html %}
  {% endfor %}
</ul>
