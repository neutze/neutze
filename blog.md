---
layout: page
title: Blog
description: Every article and lesson on neutze.de, newest first, with a filter by reader.
permalink: /blog/
---

{%- comment -%} The blog page (Publish Site, rule 4): newest first, one heading per year, a filter by reader for each reader with a published piece and a page. {%- endcomment -%}
{% assign dated = site.pages | where_exp: "p", "p.date" | sort: "date" | reverse %}

Every published article and lesson, newest first. The same pieces are kept by topic from the <a href="{{ "/" | relative_url }}">home page</a>, and a <a href="{{ "/feed.xml" | relative_url }}">feed</a> carries them too.

{% assign filters = "" | split: "" %}
{% for r in site.data.readers %}{% if r[1].primary %}{% assign u = "/for/" | append: r[0] | downcase | append: "/" %}{% assign fp = site.pages | where: "url", u | first %}{% assign theirs = dated | where: "for", r[0] %}{% if fp and theirs.size > 0 %}{% assign filters = filters | push: r %}{% endif %}{% endif %}{% endfor %}
{%- if filters.size > 0 %}
<nav aria-label="Filter by reader">
<ul class="reader-filter">
<li><a href="{{ "/blog/" | relative_url }}" aria-current="page">All</a></li>
{%- for r in filters %}
<li><a href="{{ "/for/" | append: r[0] | downcase | append: "/" | relative_url }}">{{ r[1].name }}</a></li>
{%- endfor %}
</ul>
</nav>
{%- endif %}

{%- if dated.size == 0 %}
<p>Nothing published yet.</p>
{%- endif %}
{% assign years = dated | group_by_exp: "p", "p.date | date: '%Y'" %}
{%- for year in years %}
<h2>{{ year.name }}</h2>
<ul class="piece-list">
{%- for p in year.items %}
{% include piece-item.html piece=p %}
{%- endfor %}
</ul>
{%- endfor %}
