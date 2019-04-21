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
      <font color="#778899"><h2><u>{{ post.date | date: '%Y' }}</u></h2></font>
    {% else %}
      {% capture year %}{{ post.date | date: '%Y' }}{% endcapture %}
      {% capture nyear %}{{ post.next.date | date: '%Y' }}{% endcapture %}
      {% if year != nyear %}
        <font color="#778899"><h2><u>{{ post.date | date: '%Y' }}</u></h2></font>
      {% endif %}

    {% endunless %}
   {% include archive-single.html %}
  {% endfor %}
</ul>
