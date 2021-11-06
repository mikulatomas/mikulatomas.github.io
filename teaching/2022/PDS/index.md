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
**Vyučující:** {{ page.lector }}\\
**Stránky předmětu:** [{{ page.course_page }}]({{ page.course_page }})

### Seznam cvičení
* **Cvičení dne 22.9.2021** (první cvičení) **odpadá!**
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
Praktická implementace vybraných problémů v jazyce Python 3.

### Zápočet
Vypracování tří úloh z části paralelního programování a dvou úloh z části distribuované systémy. Přehled uznaných úkolů je dostupný [zde](https://docs.google.com/spreadsheets/d/1-kvij8RxNySA3EfY_SnFAM6K2XwZd0CpWMcRg-T7k00/edit#gid=0).
