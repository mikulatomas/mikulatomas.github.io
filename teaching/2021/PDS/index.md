---
layout: default
title: KMI/PDS Paralelní distribuované systémy
lector: Mgr. Jan Tříska, Ph.D.
types: [course, winter]
course: PDS
year: 2021
---

## {{ page.title }}
**Vyučující:** {{ page.lector }}

**Stránky předmětu:** [{{ page.course_page }}]({{ page.course_page }})

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

### Co bude obsahem cvičení PDS
Implementace paralelních a distribuovaných algoritmů.

### Zápočet
Splnění a odevzdání závěrečné implementace distribuovaného algoritmu. Plagiátorství není tolerováno.
