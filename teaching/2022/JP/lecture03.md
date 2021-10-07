---
layout: default
courses: JP
title: 3. Funkce a moduly
year: 2022
---

## 3. Funkce a moduly

## Funkce
Více informací [zde](https://realpython.com/defining-your-own-python-function/).

{% highlight python linenos %}
# doteď jsme se setkali s několika funkcemi, například
len([1,2,3])
max(1,5)
{% endhighlight %}

Dnes nás budou zajímat převážně ty uživatelsky definované.

{% highlight python linenos %}
# definice funkce
def subtract(a, b):
    # vrácení hodnoty z funkce
    return a - b

a = 10
subtract(2, 3)
subtract(3, 2)
{% endhighlight %}

Předpis pro definování funkce.

{% highlight bash linenos %}
def <function_name>([<parameters>]):
    <statement(s)>
{% endhighlight %}

<div class="pep">
{% highlight python linenos %}
# PEP8 - názvy funkcí podobně jako proměnné, rovněž dbáme na vhodné pojmenování parametrů
# správně
def multiply_point(point, by):
    x, y = point
    return x*by, y*by

# špatně
def multiplyPoint(a, b):
    x, y = a
    return x*b, y*b
{% endhighlight %}
</div>

Předávání argumentů (vstup funkce).

{% highlight python linenos %}
subtract(a=2, b=3)
subtract(b=3, a=2)

subtract(3)
subtract(3, b=2)

# chyba, nejprve musí být povinné parametry
subtract(b=2, 3)
# již správně
subtract(3, a=2)
{% endhighlight %}

Výchozí parametr.

{% highlight python linenos %}
def subtract(a, b=1):
    return a - b

subtract(3)
subtract(3, 2)
subtract(3, b=2)
{% endhighlight %}

Pozor na mutovatelné výchozí parametry.

{% highlight python linenos %}
def my_list(list_=[]):
    list_.append("123")
    return list_

# problém
my_list([1, 2, 3])
my_list([1, 2, 3])
my_list()
my_list()

# jak poznáme, že se jedná o stejný objekt?
my_list() is my_list()

# řešení problému
def my_list(list_=None):
    if list_ is None:
        list_ = []
    list_.append("123")
    return list_

# vše se chová jak očekáváme
my_list([1, 2, 3])
my_list([1, 2, 3])
my_list()
my_list()

my_list() is my_list()
{% endhighlight %}

Předání argumentu je v Pythonu řešeno hybridním způsobem mezi "předání hodnotou" a "předání odkazem". Funkci je předán odkaz na objekt, odkaz je ale předáván hodnotou.

{% highlight python linenos %}
name = "Lukáš Novák"

def f(name):
    name = "Petr Novák"

f(name)

print(name)
{% endhighlight %}

To však neznamená, že se obsah argumentu nikdy nemůže měnit v lokální funkci, stačí použít mutovatelné struktury.

{% highlight python linenos %}
names = ["Lukáš Novák", "Karel Novák"]

def change_name(names):
    names[0] = "Petr Novák"

change_name(names)

print(names)
{% endhighlight %}

### Příkaz `return` podrobněji

Příkaz `return` okamžitě ukončí veškeré vykonávání funkce (včetně cyklů) a vrátí uvedenou hodnotu.

{% highlight python linenos %}
def member(target, iterable):
    for idx, item in enumerate(iterable):
        if item == target:
            return idx

    return False

member(4, [1, 2, 3, 4, 5, 6, 7])
member(10, [1, 2, 3, 4, 5, 6, 7])
{% endhighlight %}

Pokud `return` není uveden, vrací se automaticky hodnota `None`.

{% highlight python linenos %}
def nothing():
    pass

nothing() is None

def nothing():
    return

nothing() is None
{% endhighlight %}

Příkaz `return` můžeme v kombinaci se sekvencí `tuple` použít na vrácení několika hodnot.

{% highlight python linenos %}
def multiply_point(point, by):
    x, y = point
    # konstruktor tuple
    return x*by, y*by

multiply_point((10, 5), 2)

# tuple unpacking
new_x, new_y = multiply_point((10, 5), 2)
{% endhighlight %}

Tuple unpacking je užitečný pro předání seznamu/tuple argumentů.

{% highlight python linenos %}
def multiply_point(x, y, by):
    return x*by, y*by

point = (10, 5)

# chyba
multiply_point(point, 2)

# správně, dojde k unpackingu pomocí operátoru *
multiply_point(*point, 2)

# příklad z praxe, transformace matice
zip(*[[1, 2, 3], [4, 5, 6]])

# slovník lze použít pro pojmenované argumenty
point = {"x": 10, "y": 5}

# unpacking pomoci operatoru **
multiply_point(**point, by=2)
{% endhighlight %}

### Volitelný počet argumentů

Volitelný počet argumentu pomocí tuple.

{% highlight python linenos %}
def average(*args):
    return sum(args) / len(args)

average(1, 2, 3, 4)
{% endhighlight %}

Volitelný počet argumentů pomocí slovníku.

{% highlight python linenos %}
def print_name(**kwargs):
    for key, value in kwargs.items():
        print(f"{key}: {value}")

print_name(title="Ing.", firstname="Lukáš", lastname="Novák")
print_name("Novák", title="Ing.", firstname="Lukáš")

def print_name(**kwargs):
    for key, value in kwargs.items():
        print(f"{key}: {value}")

person = {"title": "Ing.", "firname": "Lukáš", "lastname": "Novák", "job": "programmer"}

print_name(**person)
{% endhighlight %}

Kombinace předchozích možností.

{% highlight python linenos %}
def func(a, b, *args, **kwargs):
    print(a)
    print(b)
    print(args)
    print(kwargs)

func(1, 2, "ahoj", "svete", x=11, y=22)
{% endhighlight %}

### Docstrings
Každá definice funkce může (a měla by) obsahovat takzvaný docstring. Jedná se o řetězec s krátkou dokumentací funkce.

{% highlight python linenos %}
def subtract(a, b):
    """Subtract b from a."""
    return a - b

def subtract(a, b):
    """Subtract b from a.

    Args:
        a: Number to subtract from.
        b: Number being subtracted.

    Returns:
        Result of subtraction.
    """
    return a - b

subtract.__doc__
help(subtract)
{% endhighlight %}

Od této chvíle **bude vyžadována dokumentace ve formě docstringů u všech funkcí, které se v domácích úkolech vyskytnou**.

## Příkaz `assert`

Příkaz `assert` je vhodným nástrojem defenzivního programování, umožňuje nám za běhu programu zaručit, že jsou uvedené výrazy platné.

{% highlight python linenos %}
a = 10
assert a == 10 
{% endhighlight %}

Je to jednoduchý nástroj jak ověřovat základní funkčnost funkcí (k pokročilejšímu testování později). Je však nutné zmínit, že `assert` do finálního kódu nepatří.

## Moduly a balíčky
Doteď jsme náš kód organizovali do jednotlivých skriptů (samostatně fungující soubor). S příchodem funkcí je vhodné dělit definice funkcí na různé soubory a z těchto souborů potom uživatelsky definované funkce importovat do dalších skriptů. Tímto je možné sdílet funkcionalitu mezi více projektů a vytvářet knihovny.

Modul je tedy soubor obsahující Python definice a příkazy. Název modulu je název souboru s příponou `.py`. V rámci modulu můžeme název modulu získat pomoci speciální proměné `__name__`.

Ukázka importování vestavěného modulu `math` pomoci příkazu `import`.

{% highlight python linenos %}
import math

math.sqrt(6)

math.__name__
{% endhighlight %}

Vlastní modul pak můžeme vytvořit jednoduše. Vytvořme soubor `list_operations.py` s následujícím obsahem:

{% highlight python linenos %}
def subtract_lists(list1, list2):
    """Subtract two list piecewise."""

    if len(list1) != len(list2):
        raise ValueError(f"Lists are not same length, {len(list1)} and {len(list2)} was given.")
    
    result = []

    for a, b in zip(list1, list2):
        result.append(a - b)
    
    return result

def sum_lists(list1, list2):
    """Sum two list piecewise."""

    if len(list1) != len(list2):
        raise ValueError(f"Lists are not same length, {len(list1)} and {len(list2)} was given.")
    
    result = []

    for a, b in zip(list1, list2):
        result.append(a + b)
    
    return result
{% endhighlight %}

V jiném skriptu umístěném v téže složce (nebo v interpretu spuštěném z téže složky - vhodné pro rychlé testování) můžeme modul `list_operations` importovat.

{% highlight python linenos %}
import list_operations

list_operations.subtract_lists([1, 2], [4, 3])
{% endhighlight %}

Modul nemusí obsahovat pouze definice ale i příkazy, tyto příkazy jsou zamýšleny jako inicializace modulu a jsou provedeny pouze jednou při načtení modulu.

Každý modul má pak odpovídající **jmenný rozsah**, proto můžeme používat dvě funkce z rozdílných modulů se stejným názvem.

{% highlight python linenos %}
import math
import my_math

math.sqrt
my_math.sqrt
{% endhighlight %}

<div class="pep">
{% highlight python linenos %}
# PEP8 - jednotlivé importy musí být na samostatném řádku
# správně
import os
import sys

# špatně
import sys, os

# PEP8 - pořadí importů
import sys # systémová knihovna
import numpy # externí knihovna
import my_math # lokální modul
{% endhighlight %}
</div>

Existuje varianta `import` příkazu, který přímo importuje jména z jmenného prostoru modulu do aktuálního jmenného prostoru.

{% highlight python linenos %}
from list_operations import subtract_lists, sum_lists

subtract_lists([1, 2], [4, 3])
{% endhighlight %}

<div class="pep">
{% highlight python linenos %}
# PEP8 - jednotlivé importy musí být na samostatném řádku
# toto je výjimka z pravidla
from list_operations import subtract_lists, sum_lists
{% endhighlight %}
</div>

Variantu, která importuje kompletní jmenný prostor modulu do toho aktuálního, je lepší nepoužívat.

{% highlight python linenos %}
from list_operations import *
{% endhighlight %}

Přejmenování modulu při importu je však užitečné.

{% highlight python linenos %}
import list_operations as loperations

loperations.subtract_lists([1, 2], [4, 3])
{% endhighlight %}

Rovněž je možné přejmenovat importované funkce.

{% highlight python linenos %}
from list_operations import subtract_lists as lsubtract
{% endhighlight %}

Pokud do modulu umístíme následující podmínku, obsah bude vykonán pouze při přímem spuštění zdrojového kódu, nikoli při jeho importování.

{% highlight python linenos %}
if __name__ == "__main__":
    print("Only when script is launched.")
{% endhighlight %}

### Odkud se moduly importují?
Při importu modulu `my_math` hledá interpret nejdříve modulu vestavěné. Pokud nebyl žádný vestavěný modul nalezen, hledá soubor s názvem `my_math.py` v seznamu adresářích uložném v proměnné `sys.path`. Seznam obsahuje adresář odkud byl skript spuštěn, `PYTHONPATH` s cestami k instalovaným modulů a další.

## Balíčky
Balíčky jsou způsob jak strukturovat jmenný prostor Python modulu. Jmenný prostor je potom přístupný skrze tečkovou notaci.

{% highlight python linenos %}
# z modulu A importujeme submodul B
import A.B
{% endhighlight %}

Příklad struktury balíčku:

{% highlight text linenos %}
sound/                          Top-level package
      __init__.py               Initialize the sound package
      formats/                  Subpackage for file format conversions
              __init__.py
              wavread.py
              wavwrite.py
              aiffread.py
              aiffwrite.py
              auread.py
              auwrite.py
              ...
      effects/                  Subpackage for sound effects
              __init__.py
              echo.py
              surround.py
              reverse.py
              ...
      filters/                  Subpackage for filters
              __init__.py
              equalizer.py
              vocoder.py
              karaoke.py
              ...
{% endhighlight %}

Balíček `sound` má podřazené balíčky `formats`, `effects` a další.

Jakmile balíček importujeme, Python automaticky dohledá veškeré podsložky balíčků.

Speciální soubor `__init__.py` je potřebný k tomu, aby Python považoval složku za balíček. Pro základní funkcionalitu stačí, aby se jednalo o prázdný soubor, může zde však být provedena inicializace balíčku.

Z takové struktury může uživatel importovat následujícím způsobem:

{% highlight python linenos %}
import sound.effects.echo

# aby mohl být modul použít, musí být napsáno jeho plné jméno
sound.effects.echo.echofilter(input, output, delay=0.7, atten=4)
{% endhighlight %}

Alternativně však můžeme použít následující:

{% highlight python linenos %}
from sound.effects import echo

echo.echofilter(input, output, delay=0.7, atten=4)
{% endhighlight %}

Posledním způsobem je potom importování samostatné funkce `echofilter`.

{% highlight python linenos %}
from sound.effects.echo import echofilter

echofilter(input, output, delay=0.7, atten=4)
{% endhighlight %}

### Reference v rámci balíčku
Často je potřeba provádět relativní import v rámci balíčku. Relativní import je možné provést následovně:

{% highlight python linenos %}
# v rámci složky
from . import echo
# nadřazená složka
from .. import formats
from ..filters import equalizer
{% endhighlight %}

### Instalace externích balíčku
V budoucnu rozebereme detailněji, aktuálně pro nás bude relevantní, že balíčky v drtivé většině instalujeme z [Python Package Index (PyPI)](https://pypi.org) pomocí nástroje `pip`.

Detailnější popis nalezneme v [dokumentaci](https://packaging.python.org/tutorials/installing-packages/#installing-from-pypi).

Od dnešního semináře budou testy na splnění úkolu realizované externí knihovnou `pytest`, kterou je tedy možné instalovat příkazem:

{% highlight bash linenos %}
pip install pytest
{% endhighlight %}

Posléze ve složce s repozitářem úkolu L03E01 stačí spustit příkaz (kde soubor `tests.py` obsahuje testy):

{% highlight bash linenos %}
pytest tests.py
{% endhighlight %}

A v případě úkolu obsahující balíček (například L03E02) je nutné balíček nejprve lokálně nainstalovat a poté testy spustit:

{% highlight bash linenos %}
pip install -e .
pytest
{% endhighlight %}

Zatím není nutné předchozím krokům rozumět, o publikování a instalovaní balíčků si více řekneme na konci tohoto kurzu.

<!-- ## Úkoly
Nevíte si rady? Přečtěte si "[Jak pracovat s Github Classroom?](/teaching/2022/JP/classroom)".

* **L03E01**: Read points [[Náhled](https://github.com/kmi-jp/template-L03E01)], [[Příjmout úkol](https://classroom.github.com/a/FpicPoW7)]
* **L03E02**: Matrix multiplication [[Náhled](https://github.com/kmi-jp/template-L03E02)], [[Příjmout úkol](https://classroom.github.com/a/0CtAuBs6)] -->
