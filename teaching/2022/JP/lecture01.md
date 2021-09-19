---
layout: default
courses: JP
title: 1. Úvod a základy jazyka Python
year: 2022
---

## 1. Úvod a základy jazyka Python

Python je **interpretovaný**, **dynamicky typovaný** jazyk využívající **garbage-collector**, podporující několik **programovacích paradigmat**.

### Proč se učit Python?
1. **Python je** navržen jako výborně čitelný jazyk.
2. **Python je** navržen s ohledem na produktivitu vývojáře.
3. **Python je** multiplatformní.
4. **Python nabízí** obrovské množství knihoven a umí "řešit skoro vše".
5. **Python je** navržen tak, aby byl jednoduše rozšiřitelný ostatními jazyky.
6. **Python je** radost používat.

{% include quote.html content="Python can do just about anything, just not very well -Michael Reeves" %}

## První program v jazyce Python
Náš první program (spíše skript) pojmenovaný `hello_world.py` obsahuje následující řádek:

{% highlight python linenos %}
print("🦄 ahoj světe 🦄")
{% endhighlight %}

### Spuštění zdrojového kódu

Spustit jej můžeme jednoduše příkazem:

{% highlight bash linenos %}
% > python3 hello_world.py
🦄 ahoj světe 🦄
{% endhighlight %}

Během provádění námi vytvořeného skriptu proběhly kroky, které byly na první pohled zatajeny.

**Překlad do byte code**

Prvním krokem je kompilace zdrojového kódu do formátu takzvaného *byte kódu*. Byte kód je uložen v souborech s příponou `.pyc` (compiled `.py` - `.py` je běžná přípona zdrojového kódu jazyka Python). Tyto soubory jsou od verze 3.2 uloženy ve složce `__pycache__` (pouze pokud je skript importován, tento fakt však zatím ignorujme).

Pokud bude stejný program (jeho obsah nebyl změněn) spuštěn znovu, překlad do byte kódu bude přeskočen.

**Python Virtual Machine (PVM)**

Přeložený program ve formě byte kódu je dále předán k vykonání Python Virtual Machine (PVM). Jedná se o program který umí přeložený byte kód vykonat.

## Ostatní implementace jazyka Python
Proces který jsme popsali je standardní postup při spuštění zdrojového kódu jazyka Python, existují však další varianty a modifikace.

### CPython
Standardní implementace, tuto verzi budeme používat. Vytvořen v ANSI C jazyku.

### PyPy ([pypy.org](https://www.pypy.org))
Alternativní implementace nahrazující CPython. Hlavní důraz je kladen na rychlost. V průměru se jedná o 4.2x rychlejší implementaci.

### Jython ([jython.org](https://www.jython.org))
Java implementace jazyka Python. Cíleno na kombinaci jazyka Python a Java.

### IronPython ([ironpython.net](https://ironpython.net))
.NET implementace jazyka Python a podpora jeho integrace do .NET ekosystému.

## Optimalizační nástroje a rozšíření jazyka Python
Implementace jazyka Python zmíněné výše pracují stejným způsobem: přeloží zdrojový kód do byte kódu a ten spustí pomoci virtuálního stroje. Existují však hybridní implementace, jako je například `Cython`, které se snaží optimalizovat tento základní model vyhodnocování.

### Cython
Hybridní jazyk který kombinuje Python s C funkcemi. Cython může být přeložen na zdrojový kód jazyka C který využívá Python/C API. Ten pak může být zkompilován jako celek. Hlavním cílem je zvýšení rychlosti.

## Interaktivní práce s jazykem Python
Již jsme si ukázali jak spustit soubor obsahující zdrojový kód v jazyce Python. Pro účely experimentování je vhodné znát a používat příkazovou řádku jazyka Python. Tu lze jednoduše spustit pomoci příkazu `python3`.

{% highlight bash linenos %}
% > python3
Python 3.9.4 (default, Apr  5 2021, 01:50:46)
[Clang 12.0.0 (clang-1200.0.32.29)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>>
{% endhighlight %}

Vylepšená verze `ipython` je dostupná [zde](https://ipython.org/install.html).

{% highlight bash linenos %}
% > ipython
Python 3.9.4 (default, Apr  5 2021, 01:50:46)
Type 'copyright', 'credits' or 'license' for more information
IPython 7.25.0 -- An enhanced Interactive Python. Type '?' for help.

In [1]:
{% endhighlight %}

Do interaktivního interpretu můžeme psát zdrojový kód, ten se vždy vyhodnotí a vrátí výsledek.

{% highlight bash linenos %}
>>> print("🦄 ahoj světe 🦄")
🦄 ahoj světe 🦄
>>>
{% endhighlight %}

## Základní datové typy

{% highlight python linenos %}
42
3.14
complex(-1, 0)

"ahoj svete"
'ahoj svete'
'ahoj "svete"'
"🦄 ahoj světe 🦄"

"""Ahoj svete,
toto je delsi text,
ma nekolik radku.
"""

True
False
None
{% endhighlight %}

<div class="pep">
{% highlight python linenos %}
# PEP8 - je možné používat jak uvozovky tak apostrofy, konzistence je však klíčová
"ahoj svete"
'ahoj svete'
{% endhighlight %}
</div>

## Číselné operátory
Více informací [zde](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex).

{% highlight python linenos %}
7 + 4
-7 + 4
-(7 + 4)
7 / 4
7 // 4
7 % 4
2 ** 3
abs(-42)
{% endhighlight %}

<div class="pep">
{% highlight python linenos %}
# PEP8 - mezery okolo závorek
# správěn
(7 + 4)
# špatně
( 7 + 4 )
{% endhighlight %}
</div>

## Pravdivostní operátory
Více informací [zde](https://docs.python.org/3/library/stdtypes.html#boolean-operations-and-or-not).

{% highlight python linenos %}
True and False
True or False
not True and False
not (True and False)
{% endhighlight %}

## Porovnání
Více informací [zde](https://docs.python.org/3/library/stdtypes.html#comparisons).

{% highlight python linenos %}
5 == 5
5 < 2
5 <= 5
5 >= 5
5 > 3
5 != 2
{% endhighlight %}

## Operace se sekvencemi (řetězci)
Více informací [zde](https://docs.python.org/3/library/stdtypes.html#common-sequence-operations).

{% highlight python linenos %}
# získání prvního prvku (znaku)
"ahoj svete"[0]
# ověření zda sekvence obsahuje prvek
"a" in "ahoj svete"
"ahoj" in "ahoj svete"
"x" not in "ahoj svete"

# spojování sekvencí
"ahoj" + " " + "svete"
"🦄 " * 5

# délka sekvence
len("ahoj svete")

# slicing
"ahoj svete"[1:3]
"ahoj svete"[0:2]
"ahoj svete"[:2]
{% endhighlight %}

<div class="pep">
{% highlight python linenos %}
# PEP8 - mezery a slicing
# správně
"ahoj svete"[0:2]
# špatně
"ahoj svete"[0 : 2]
{% endhighlight %}
</div>

### Užitečné metody pro práci s řetězci
Více informací [zde](https://docs.python.org/3/library/stdtypes.html#string-methods).

{% highlight python linenos %}
# zjištění indexu prvku v řetězci
"ahoj svete".index("e")

# počet výskytů prvku v řetězci
"ahoj svete".count("ahoj")

# nový řetězec psaný malými písmeny
"Ahoj Svete".lower()

# nový řetězec psaný velkými písmeny
"Ahoj Svete".upper()

# nový řetězec psaný jako titulek
"ahoj svete jak se mas?".title()

# nový řetězec s prohozenou velikostí písmen
"Ahoj Svete".swapcase()

# nový řetězec s velkým prvním písmenem
"každá věta začíná velkým písmenem!".capitalize()

# nový řetězec s odstranením mezer na začátku a konci
"     ahoj svete    ".strip()

# nový řetězec s odstranením vykčníků na začátku a konci
"!!!!!!!!ahoj svete!!!!!!!".strip("!")

# nový řetězec s odstranením vykčníků na začátku
"!!!!!!!!ahoj svete!!!!!!!".lstrip("!")

# nový řetězec s odstranením vykčníků na konci
"!!!!!!!!ahoj svete!!!!!!!".rstrip("!")

# nový řetězec ve kterém byl nahrazen podřetězec
"ahoj svete".replace("ahoj", "cau")
{% endhighlight %}

## Formátování řetězců
Více informací [zde](https://docs.python.org/3/library/string.html#string-formatting). Případně dobrý článek na [RealPython](https://realpython.com/python-string-formatting/)

### `.format()`
{% highlight python linenos %}
surname = "Novák"
"Lukáš {}".format(surname)
"{firstname} {surname}".format(firstname="Lukáš", surname="Novák")

# hexa
"{:x}".format(10)
"{:e}".format(10000000)

# thousands separator
"{:,}".format(1000000)
"{:.2}".format(1.2312414)
{% endhighlight %}

### `f` string (Python 3.6+)
Více informací [zde](https://docs.python.org/3/tutorial/inputoutput.html#formatted-string-literals).
{% highlight python linenos %}
surname = "Novák"
f"Lukáš {surname}"

f"calculation: {10 + 2 * 20}"

# hexa
f"{10:x}"
f"{10000000:e}"

# thousands separator
f"{1000000:,}"
f"{1.2312414:.2}"
{% endhighlight %}

### debug string (Python 3.8+)
{% highlight python linenos %}
surname = "Novák"
f"{surname =}"
{% endhighlight %}

## Transformace datových typů
{% highlight python linenos %}
bool(10)
bool(None)
str(10)
str(None)
int(7 / 4)
float(10)
int("123")
float("1.24")
int(float(1))

# nelze
int("ahoj svete")
{% endhighlight %}

<div class="pep">
{% highlight python linenos %}
# PEP8 - mezery
# správně
bool(10)
# špatně
bool (10)
{% endhighlight %}
</div>

## Přířazení
{% highlight python linenos %}
x = 10
y = 20

x, y = 10, 20

# prohození bez tmp
x, y = y, x

x += 200
x -= 10
x /= 2
x //= 2

# není reprezentováno jako zlomek, nutné použít fraction
x /= 3

# mutovatelnost řetězců
label = "Point 1"

# nelze
label[-1] = "2"
{% endhighlight %}

<div class="pep">
{% highlight python linenos %}
# PEP8 - mezery
# správně
x = 10
y = 20
# špatně
x    = 10
y    = 10

# PEP8 - pojmenování proměnných
# správně (pojmenování souřadnic bodu)
x = 10
y = 20
# špatně
a = 10
g = 20

# PEP8 - pojmenování víceslovných proměnných
# správně
odd_number
# špatně
oddNumber

# PEP8 - pojmenování konstant
CENTER_X, CENTER_Y = 0, 0
{% endhighlight %}
</div>

## Standardní výstup

{% highlight python linenos %}
print("Ahoj svete")
{% endhighlight %}

## Větvení programu
Více informací [zde](https://docs.python.org/3/tutorial/controlflow.html#if-statements).

{% highlight python linenos %}
# získaní vstupu od uživatele a jeho převedení na integer
user_integer = int(input("Please enter an integer: "))

division_remainder = user_integer % 2

# příkaz větvení if pro větvení programu, pozor na dělení kódu do bloků pomocí mezer/tabulatoru, časté chyby
if not division_remainder:
    print(x, "is divisible by 2")
{% endhighlight %}

<div class="pep">
{% highlight python linenos %}
user_integer = int(input("Please enter an integer: "))

# PEP8 - pořadí podmínek (dle četnosti výskytu)
# správně
if user_integer > 0:
    print(user_integer, "is larger than 0")
elif user_integer < 0:
    print(user_integer, "is smaller than 0")
else:
    print(user_integer, "is 0")

# špatně
if user_integer == 0:
    print(user_integer, "is 0")
elif user_integer < 0:
    print(user_integer, "is smaller than 0")
else:
    print(user_integer, "is larger than 0")
{% endhighlight %}
</div>

<div class="pep">
{% highlight python linenos %}
# PEP8 - Neporovnávat boolean hodnoty s True nebo False pomocí ==
boolean_value = False

# správně
if boolean_value:
    print("It is true!")

# špatně
if boolean_value == True:
    print("It is true!")
{% endhighlight %}
</div>

## Úkoly
Nevíte si rady? Přečtěte si "[Jak pracovat s Github Classroom?](/teaching/2022/JP/classroom)".

* **L01E01**: Hello world [[Náhled](https://github.com/kmi-jp/template-L01E01)], [[Příjmout úkol](https://classroom.github.com/a/BLVFlAR8)]
* **L01E02**: Integer input [[Náhled](https://github.com/kmi-jp/template-L01E02)], [[Příjmout úkol](https://classroom.github.com/a/rwxzX3zO)]
* **L01E03**: Point input [[Náhled](https://github.com/kmi-jp/template-L01E03)], [[Příjmout úkol](https://classroom.github.com/a/heMpd6hw)]
* **L01E04PEP**: PEP8 format [[Náhled](https://github.com/kmi-jp/template-L01E04PEP)], [[Příjmout úkol](https://classroom.github.com/a/z4iC8M2S)]