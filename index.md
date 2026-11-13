---
title: Johannes Neutze
description: An engineering manager who still builds. Guides for engineers stepping up to lead, new managers and the leaders they work with, from real decisions and what they cost.
---

{%- comment -%}
The home page (Publish Site, rule 11): teach first, the brand follows (Blog Strategy, rule 1).
The readers come from _data/readers.json, generated from Blog Strategy rule 3; nothing about
them is written here. Every block that has nothing to show is left out.
{%- endcomment -%}
{% assign dated = site.pages | where_exp: "p", "p.date" | sort: "date" | reverse %}
{% assign primary = "" | split: "" %}
{% for r in site.data.readers %}{% if r[1].primary %}{% assign primary = primary | push: r %}{% endif %}{% endfor %}

<div class="home">
<section class="hero" aria-labelledby="name">
<h1 id="name">Johannes Neutze</h1>
<p class="lede">An engineering manager who still builds. I write down my decisions, the fix I turned down, and what it cost.</p>
{%- if primary.size > 0 %}
<p class="for-whom">Written for {% for r in primary %}{{ r[1].name | downcase }}{% if forloop.rindex == 2 %} and {% elsif forloop.last == false %}, {% endif %}{% endfor %}: rules you can use tomorrow, with the story of who paid for them.</p>
{%- endif %}
<p class="actions"><a class="button-link" href="{{ "/blog/" | relative_url }}">Read the blog</a></p>
</section>

{%- comment -%} Start here waits for the first published piece (Publish Site rule 11, DEC-0065). {%- endcomment %}
{%- if primary.size > 0 and dated.size > 0 %}
<section aria-labelledby="start-here">
<h2 id="start-here">Start here</h2>
<ul class="reader-cards">
{%- for r in primary %}
{%- assign slug = r[0] | downcase %}
{%- assign mine = dated | where: "for", r[0] %}
{%- assign reader_url = "/for/" | append: slug | append: "/" %}
{%- assign reader_page = site.pages | where: "url", reader_url | first %}
<li class="reader-card">
<h3>{{ r[1].name }}</h3>
{%- if r[1].who %}<p class="who">{{ r[1].who }}</p>{% endif %}
<p>What you'll find: {{ r[1].comes_for }}.</p>
{%- if mine.size > 0 %}
<ul>{% for p in mine limit: 3 %}<li><a href="{{ p.url | relative_url }}">{{ p.title | escape }}</a></li>{% endfor %}</ul>
{%- endif %}
{%- if reader_page and mine.size > 0 %}
<a class="more" href="{{ reader_url | relative_url }}">Everything for this reader</a>
{%- endif %}
</li>
{%- endfor %}
</ul>
</section>
{%- endif %}

{%- if dated.size > 0 %}
<section aria-labelledby="latest">
<h2 id="latest">Latest</h2>
<ul class="piece-list">
{%- for p in dated limit: 3 %}
{% include piece-item.html piece=p %}
{%- endfor %}
</ul>
<p><a href="{{ "/blog/" | relative_url }}">Everything by date</a></p>
</section>
{%- endif %}

{% assign tops = "" | split: "" %}
{% for entry in site.data.sections %}{% unless entry[0] contains "/" %}{% assign url = "/" | append: entry[0] | append: "/" %}{% assign found = site.pages | where: "url", url | first %}{% if found %}{% assign tops = tops | push: entry %}{% endif %}{% endunless %}{% endfor %}
{%- if tops.size > 0 %}
<section aria-labelledby="sections">
<h2 id="sections">Sections</h2>
<ul class="sections-list">
{%- for entry in tops %}
<li><a href="{{ "/" | append: entry[0] | append: "/" | relative_url }}">{{ entry[1].title }}</a>: {{ entry[1].summary }}</li>
{%- endfor %}
</ul>
</section>
{%- endif %}

<section class="about-author" aria-labelledby="who-writes">
<h2 id="who-writes">Who writes this</h2>
{%- comment -%} Figures only from linkedin.md and github.md, no employer name (DECISIONS.md §5.7, Publish Site rule 14). {%- endcomment %}
<p>I'm an engineering manager in Munich. My teams have owned the order workflows 150k+ merchants in 25+ countries run their business on, a mobile developer platform for apps with 20M+ monthly users, and the engineering of moving an acquired company onto the platform. I came up through Android engineering, and I start teams from scratch, inherit them mid-flight and rebuild them when they stall.</p>
<p>At home I build and run my own software with AI agents, and write down how it goes.</p>
<p>The full career is on <a href="https://www.linkedin.com/in/johannes-neutze">LinkedIn</a>, the code on <a href="https://github.com/neutze">GitHub</a>.</p>
</section>
</div>
