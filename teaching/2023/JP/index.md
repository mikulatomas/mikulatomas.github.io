---
layout: default
title: KMI/PJ Jazyk Python
types: [course, winter]
course: JP
year: 2023
---

## {{ page.title }}

Cílem předmětu je seznámit studenty s programováním v jazyce Python, který patří mezi nejpopulárnější programovací jazyky současnosti. Předpokládá se pokročilejší znalost procedurálního programování (znalost jazyka Python není vyžadována) a algoritmizace. Při výuce je kladen důraz na efektivní a praktické použití jazyka Python. 

### Seznam seminářů
Obsah následujících stránek je pouze doprovodný materiál, nenahrazuje účast na semináři.
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
2. Nainstalovaný jazyk Python 3.10+ [ke stažení zde](https://www.python.org/downloads/).
2. Účet na [Github.com](https://github.com).
3. Stažený a nainstalovaný git klient [GitHub Desktop](https://desktop.github.com).
4. Stažené a nainstalované vývojářské studio [Visual Studio Code](https://code.visualstudio.com) (volitelně).
5. Osobní číslo studenta ze systému STAG (např. R180055).

### Splnění předmětu

#### Průběžné úkoly
Úkoly budou zadávány na platformě [Gihub Classroom](https://classroom.github.com/). Pro plnění úkolů je nutné vytvořit bezplatný účet na [Github.com](https://github.com) a nainstalovat klienta [GitHub Desktop](https://desktop.github.com). Demonstrace systému proběhne na prvním semináři ([Jak pracovat s Github Classroom?](/teaching/2023/JP/classroom)).

Na každém semináři bude zadáno několik úkolů. Úkoly je nutné odevzdat vždy do dalšího semináře. Pokud bude řešení nevyhovující, termín splnění se posune o další týden. Pokud ani do té doby student neodevzdá správné řešení tak úkol nebude uznán. Za celý předmět je možné nesplnit až 8 úkolů (jedinou vyjimkou je balíček `data`, ten musí být odevzdán a uznán v poslední verzi).

<img src="/assets/images/JP/schema.png" style="width:100%" srcset="/assets/images/JP/schema@2x.png 2x" />

**Kdy dostanu zpětnou vazbu?**

Před obdržením zpětné vazby je nutné splnit veškeré automatizované testy na platformě [Gihub Classroom](https://classroom.github.com/). Více informací lze přečíst [zde](/teaching/2023/JP/classroom). Veškeré úkoly, které prošly automatickým testováním by měly být opraveny během prvního týdne od zadání (zelený obdélník na obrázku). Pokud by došlo ke zdržení, deadline na finální odevzdání (červený obdélník na obrázku) bude prodloužen. V případě, že máte pocit, že na úkol bylo zapomenuto, upozorněte svého cvičícího.

**Lokální testování**

U většiny úkolů je možné provést lokální testování bez nutnosti odesílat kód do Github Classroom. Pro jeho funkčnost je nutné nainstalovat knihovny `pytest` a `pytest-console-scripts`. Detaily je možné dohledat na webu nebo v [lekci 3](https://tomasmikula.cz/teaching/2023/JP/lecture03.html).

**Co dělat pokud můj kód nesplňuje testy?**

Testy jsou navrženy tak, aby odhalily většinu funkčních problémů. Pročtěte si text k odpovídajícímu semináři a snažte se najít chybu. Textový výstup z testů nemusí být plně srozumitelný, měl by však přinést orientační informaci. Pokud i tak máte probém, obraťte se na cvičícího. Nezapomeňte, že svému kódu musíte rozumět především vy.

**Co vše je na úkolu hodnoceno?**

1. Splnění zadání.
2. Kvalita a přehlednost zdrojového kódu.
3. Dodržovaní style guide [PEP8](https://pep8.org) (bude postupně představováno na seminářích).

**Plagiátorství**

Veškeré odevzdané zdrojové kódy jsou automaticky testované na plagiátorství systémem [Moss](https://theory.stanford.edu/~aiken/moss/). Při prokázaném plagiátorství ztrácí oba studenti/studentky nárok na získání zápočtu a situaci dále řeší vedoucí katedry.

**Přehled uznaných úkolů**

* Skupina Mikula [zde](grades/2022-mikula-grades.html).
* Skupina Petržela [zde]().

### Soubor `.gitignore` pro Python 3
Při práci na úkolech bude vytvářeno velké množství souborů, které není žádoucí odesílat do repozitáře na Githubu. Základní sadu souborů můžete jednoduše ignorovat umístěním speciálního souboru `.gitignore` do hlavní složky s úkolem. Obsah ukázkového `.gitignore` souboru naleznete [zde](https://github.com/github/gitignore/blob/master/Python.gitignore).

V případě, že Vaše vývojové prostředí vytváří podpůrné složky (například složka `.idea/`) je možné tuto složku přidat do souboru `.gitignore`.

### Užitečné odkazy
* [Jak pracovat s Github Classroom?](/teaching/2023/JP/classroom)
* [Dokumentace jazyka Python3](https://docs.python.org/3/)
* [Real Python](https://realpython.com)

### Doporučená literatura
* [Learning Python](https://www.oreilly.com/library/view/learning-python-5th/9781449355722/)
* [Fluent Python](https://www.oreilly.com/library/view/fluent-python-2nd/9781492056348/)
