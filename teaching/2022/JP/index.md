---
layout: default
title: KMI/PJ Jazyk Python
types: [course, winter]
course: JP
alert: Jedná se o první semestr výuky předmětu, podmínky zápočtu se mohou měnit.
year: 2022
---

## {{ page.title }}

Cílem předmětu je seznámit studenty s programováním v jazyce Python, který patří mezi nejpopulárnější programovací jazyky současnosti. Předpokládá se pokročilejší znalost procedurálního programování (znalost jazyka Python není vyžadována) a algoritmizace. Při výuce je kladen důraz na efektivní a praktické použití jazyka Python. 

### Seznam seminářů
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

### Co je potřeba na první seminář?
1. Vlastní počítač (volitelně, na učebně budou připravené počítače, není jich však dostatek pro všechny studenty).
2. Nainstalovaný jazyk Python 3.9+ [ke stažení zde](https://www.python.org/downloads/).
2. Účet na [Github.com](https://github.com).
3. Stažený a nainstalovaný git klient [GitHub Desktop](https://desktop.github.com).
4. Stažené a nainstalované vývojářské studio [Visual Studio Code](https://code.visualstudio.com) (volitelně).
5. Osobní číslo studenta ze systému STAG (např. R180055).

### Splnění předmětu

#### Průběžné úkoly
Úkoly budou zadávány na platformě [Gihub Classroom](https://classroom.github.com/). Pro plnění úkolů je nutné vytvořit bezplatný účet na [Github.com](https://github.com) a nainstalovat klienta [GitHub Desktop](https://desktop.github.com). Demonstrace systému proběhne na prvním semináři.

Na každém semináři bude zadáno několik úkolů. Úkoly je nutné odevzdat vždy do dalšího semináře. Pokud bude řešení nevyhovující, termín splnění se posune o další týden. Pokud ani do té doby student neodevzdá správné řešení tak úkol nebude uznán. Za celý předmět je možné nesplnit až 6 úkolů.

<img src="/assets/images/JP/schema.png" srcset="/assets/images/JP/schema@2x.png 2x" />

#### Závěrečný projekt
Na závěr předmětu je nutné vypracovat závěrečný projekt, detailní informace budou dodány během semestru.

### Užitečné odkazy
* [Dokumentace jazyka Python3](https://docs.python.org/3/)
* [Real Python](https://realpython.com)

### Doporučená literatura
* [Learning Python](https://www.oreilly.com/library/view/learning-python-5th/9781449355722/)
* [Fluent Python](https://www.oreilly.com/library/view/fluent-python-2nd/9781492056348/)
