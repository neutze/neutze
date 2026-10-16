---
layout: page
title: Blog
permalink: /blog/
---

Every published article and lesson, newest first, by the day it went out.
The same pieces are kept by topic, starting from the <a href="{{ "/" | relative_url }}">home page</a>.
A <a href="{{ "/feed.xml" | relative_url }}">feed</a> carries the same list.

{% assign dated = site.pages | where_exp: "p", "p.date" | sort: "date" | reverse %}
{% if dated.size == 0 %}
Nothing published yet.
{% endif %}
{% assign days = dated | group_by_exp: "p", "p.date | date: '%Y-%m-%d'" %}
{% for day in days %}
## {{ day.name | date: "%B %-d, %Y" }}
{% for p in day.items %}
- <a href="{{ p.url | relative_url }}">{{ p.title }}</a>{% if p.series %}, {{ p.series }}{% if p.part %}, part {{ p.part }}{% endif %}{% endif %}
{% endfor %}
{% endfor %}
