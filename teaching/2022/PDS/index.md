---
layout: default
title: KMI/PDS Paralelní distribuované systémy
lector: RNDr. Martin Trnečka, Ph.D.
course_page: http://trnecka.inf.upol.cz/teaching/
types: [course, winter]
course: PDS
year: 2022
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
Bude doplněno.

### Zápočet
Bude doplněno.
