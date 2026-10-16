---
title: Johannes Neutze
---

# Johannes Neutze

Engineering Manager at DoorDash / Wolt in Munich.
Long-form guides on building a career on evidence, and on building software with AI agents.

{% assign tops = "" | split: "" %}
{% for entry in site.data.sections %}{% unless entry[0] contains "/" %}{% assign url = "/" | append: entry[0] | append: "/" %}{% assign found = site.pages | where: "url", url | first %}{% if found %}{% assign tops = tops | push: entry %}{% endif %}{% endunless %}{% endfor %}
{% if tops.size > 0 %}
## Sections
{% for entry in tops %}
- <a href="{{ "/" | append: entry[0] | append: "/" | relative_url }}">{{ entry[1].title }}</a>: {{ entry[1].summary }}{% endfor %}
{% endif %}

## Latest

{% assign dated = site.pages | where_exp: "p", "p.date" | sort: "date" | reverse %}
{% if dated.size == 0 %}Nothing published yet.{% endif %}
{% for p in dated limit: 3 %}
- <a href="{{ p.url | relative_url }}">{{ p.title }}</a>, {{ p.date | date: "%B %-d, %Y" }}{% endfor %}

<a href="{{ "/blog/" | relative_url }}">Everything by date</a> · <a href="https://www.linkedin.com/in/johannes-neutze">LinkedIn</a> · <a href="https://github.com/neutze">GitHub</a>
