---
layout: page
title: About
description: About Me
keywords: River
comments: true
menu: About
permalink: /about/
---

I'm River, a video game engineer.

Have been working for 12 years and accumulated some experience in game development.

Hard-working, but still learning.

Engenitic and passionate about my work, and still pursuing better opportunities.

Take pride in being objective about my duty and in being proactive on projects with the goal of delivering excellence.


## Contact

{% for website in site.data.social %}
* {{ website.sitename }}：[@{{ website.name }}]({{ website.url }})
{% endfor %}

## Skill Keywords

{% for category in site.data.skills %}
### {{ category.name }}
<div class="btn-inline">
{% for keyword in category.keywords %}
<button class="btn btn-outline" type="button">{{ keyword }}</button>
{% endfor %}
</div>
{% endfor %}
