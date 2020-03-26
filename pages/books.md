---
layout: page
title: Books
description: books
keywords: Books
comments: false
menu: books
permalink: /books/
---

> some books

<ul class="listing">
{% for book in site.books %}
{% if book.title != "Book Template" %}
<li class="listing-item"><a href="{{ site.url }}{{ book.url }}">{{ book.title }}</a></li>
{% endif %}
{% endfor %}

</ul>
