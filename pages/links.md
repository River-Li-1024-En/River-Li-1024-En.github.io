---
layout: page
title: Links
description: Share links
keywords: Share links
comments: true
menu: Share Links
permalink: /links/
---

> Some awesome articles.

{% for link in site.data.links %}
  {% if link.src == 'article' %}
* [{{ link.name }}]({{ link.url }})
  {% endif %}
{% endfor %}

> Some awesome tools.

{% for link in site.data.links %}
  {% if link.src == 'tool' %}
* [{{ link.name }}]({{ link.url }})
  {% endif %}
{% endfor %}

> some awesome blog.

{% for link in site.data.links %}
  {% if link.src == 'blog' %}
* [{{ link.name }}]({{ link.url }})
  {% endif %}
{% endfor %}
