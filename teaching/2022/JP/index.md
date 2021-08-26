---
layout: default
title: KMI/PJ Jazyk Python
types: [course, winter]
course: PAPR1
year: 2022
---

## {{ page.title }}

Cílem předmětu je seznámit studenty s programováním v jazyce Python, který patří mezi nejpopulárnější programovací jazyky současnosti. Předpokládá se pokročilejší znalost procedurálního programování (znalost jazyka Python není vyžadována) a algoritmizace. Při výuce je kladen důraz na efektivní a praktické použití jazyka Python. 

### Seznam cvičení
<ul>
{% capture type %}{{ page.course }}{% endcapture %}
{% for lecture_page in site.pages %}
{% if lecture_page.courses contains type and lecture_page.year == page.year %}
<li>
<a href="{{lecture_page.url}}">{{lecture_page.title}}</a>
</li>
{% endif %}
{% endfor %}
</ul>


### Zápočet
Splnění všech zadaných úkolů a vypracování závěrečného projektu (bude doplněno).
