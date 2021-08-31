---
layout: default
courses: JP
title: 2. Sekvence a cykly (WIP)
year: 2022
---

## 2. Sekvence a cykly

## List (seznam)
Více informací [zde](https://docs.python.org/3/tutorial/introduction.html#lists) nebo [zde](https://docs.python.org/3/tutorial/datastructures.html#more-on-lists).

{% highlight python linenos %}
numbers = [10, 20, 5, 10]
point = ["Point 1", 10, 20]
{% endhighlight %}

<div class="pep">
{% highlight python linenos %}
# PEP8 - v seznamech se na poslední pozici čárka nepíše
# správně
numbers = [10, 20, 5, 10]
# špatně
numbers = [10, 20, 5, 10,]
{% endhighlight %}
</div>

Práce s prvky seznamu.

{% highlight python linenos %}
# délka seznamu
len(numbers)
# počet výskytu
numbers.count(10)

# přístup na index
numbers[0]
numbers[1]
numbers[-1]
# slicing
numbers[0:3]
numbers[-2:]

# chyba - IndexError
numbers[1000]

# zjištění indexu
numbers.index(20)
{% endhighlight %}

Modifikace seznamu.

{% highlight python linenos %}
# modifikace prvku na indexu
numbers[0] += 20

# přidání prvku
numbers.append(99)

# odebrání prvku
numbers.remove(10)

# prodloužení seznamu
numbers.extend([1, 2, 3])

# smazání prvku na indexu
del numbers[0]

# následující kód se chová jako remove
del numbers[numbers.index(10)]

numbers = [10, 20, 5]
# odebrání posledního prvku a jeho vrácení
numbers.pop()
# odebrání prvku na indexu 1 a jeho vrácení
numbers.pop(1)
{% endhighlight %}

Užitečné funkce (nejen) pro seznamy.

{% highlight python linenos %}

# maximální prvek
max(numbers)
# minimální prvek
min(numbers)
# vytvoření seřazeného seznamu
sorted(numbers)
# součet prvků seznamu
sum(numbers)

# vytvoření seznamu z řetězce, proč funguje se dozvíme později
list("123456")
# chuba - int is not an iterable
list(123456)
{% endhighlight %}

Odbočka k řetězcům.

{% highlight python linenos %}
# seznam slov (oddělených mezerou)
"ahoj svete jak se mas".split()
# seznam slov (oddělených čárkou)
"jakub,milan,petr,tomas".split(',')
# vytvoření řetězce ze seznamu řetězců
" ".join(["ahoj", "svete", "jak", "se", "mas"])
# co je výsledek?
" ".join("ahoj")
{% endhighlight %}

## Tuple
Více informací [zde](https://docs.python.org/3/tutorial/datastructures.html#tuples-and-sequences).

{% highlight python linenos %}
point = (10, 20)
point = 10, 20

len(point)

# nelze! tuple není mutovatelný
point[0] = 20

# tuple unpacking
x, y = point
{% endhighlight %}

## Dictionary (slovník)
Více informací [zde](https://docs.python.org/3/tutorial/datastructures.html#dictionaries).

{% highlight python linenos %}
point = {"x": 10, "y": 20}
point = dict([("x", 10), ("y", 20)])
{% endhighlight %}

<div class="pep">
{% highlight python linenos %}
# PEP8 - mezery
# správně
point["z"] = 40
# špatně
point ["z"] = 40
{% endhighlight %}
</div>

{% highlight python linenos %}
# test zda ma slovník klíč "z"
"z" in point

# odstranění položky s klíčem "z"
del point["z"]
{% endhighlight %}

Přístup k hodnotě přes klíč.

{% highlight python linenos %}
# chyba - KeyError
point["z"]

# None
point.get("z")
{% endhighlight %}

Vnořené slovníky a jiné.

{% highlight python linenos %}
# seřazení seznam klíčů
sorted(point)
# délka slovníku (přesně řečeno klíčů)
len(point)

# zanořování slovníků
points = {"point 2": {"x": 10, "y": 20}, "point 1": {"x": 5, "y": 2}}

# přístup k vnořenému slovníku
points["point 1"]["x"]
points["point 1"]["y"]
{% endhighlight %}

## Set (množina)
Více informací [zde](https://docs.python.org/3/tutorial/datastructures.html#sets).

{% highlight python linenos %}
numbers = {10, 20, 5, 10}

# vytvoření množiny ze seznamu
numbers = [10, 20, 5, 10]
numbers = set(numbers)
{% endhighlight %}

{% highlight python linenos %}
# počet prvků v množině
len(numbers)

# testování zda má nebo nemá množina prvek
10 in numbers
100 not in numbers
{% endhighlight %}

Modifikace prvků.

{% highlight python linenos %}
# chyba - AttributeError, Set nemá metodu append
numbers.append(10)

# přidání prvku probíhá skrze add
numbers.add(20)
# opakované přidání nemá efekt
numbers.add(20)

# odebrání prvku
numbers.remove(20)
# chyba - KeyError, prvek v množině již není
numbers.remove(20)
{% endhighlight %}

Vytvoření prázdné množiny.

{% highlight python linenos %}
# pozor, vytvoří slovník!
empty = {}
type(empty)

# toto je správný způsob
empty = set()
type(empty)
{% endhighlight %}

Používání `set()`.

{% highlight python linenos %}
# množina písmen z řetězce
letters = set("ahoj svete")

# nelze! seznam je mutovatelný, tedy není hashovatelný, proto nemůže být prvkem množiny
lists = set([[1, 2], [3, 4], [1, 2]])

# tuple již může
tuples = set([(1, 2), (3, 4), (1, 2)])

# nelze! slovník jako celek je mutovatelný
dicts = set([{"a": 1, "b": 2}])

# pozor zde pracujeme pouze s klíči slovníku, ty hashovatelné být musí
keys = set({"a": 1, "b": 2})

# řetězce nejsou mutovatelné, proto jsou hashovatelné a proto mohou být v množině
strings = set(["ahoj", "svete"])
{% endhighlight %}

Používání `frozenset()`.

{% highlight python linenos %}
# nelze, protože množina je mutovatelná
set([set([1, 2]), set([3, 4])])

# lze, protože frozenset není mutovatelný
set([frozenset([1, 2]), frozenset([3, 4])])

# nelze - frozenset není mutovatelný
frozenset([1, 2]).add(5)
{% endhighlight %}

## Cyklus for (průchod sekvencí)
Více informací [zde](https://docs.python.org/3/tutorial/datastructures.html#looping-techniques).

{% highlight python linenos %}
for char in "ahoj svete":
    print(char)

for number in [10, 20, 5, 10]:
    print(number)

for number in (10, 20, 5, 10):
    print(number)

for point in ((10, 20), (5, 2), (20, 30)):
    print(point)

# použití tuple unpackingu, velice populární a čisté řešení!
for x, y in ((10, 20), (5, 2), (20, 30)):
    print(x, y)

# zanořený unpacking
cities = [
    ('Tokyo','JP',36.933, (35.689722, 139.691667)),
    ('Delhi NCR', 'IN', 21.935, (28.613889, 77.208889)),
    ('Mexico City', 'MX', 20.142, (19.433333, -99.133333))]

for name, short_name, population, (latitude, longitude) in cities:
    print(name, short_name, population, latitude, longitude)
{% endhighlight %}

<div class="pep">
{% highlight python linenos %}
# PEP8 - pojmenování nepoužité hodnoty
for x, _ in ((10, 20), (5, 2), (20, 30)):
    print(x)
{% endhighlight %}
</div>

Iterace přes množinu.

{% highlight python linenos %}
# pozor, prvky nejsou seřazené
for number in {10, 20, 5, 10}:
    print(number)

# množinu je nutné seřadit
for number in sorted({10, 20, 5, 10}):
    print(number)
{% endhighlight %}

Iterace přes slovník.

{% highlight python linenos %}
points = {"point 2": {"x": 10, "y": 20}, "point 1": {"x": 5, "y": 2}}

# přes klíče a hodnoty
for label, point in points.items():
    print(label, point)

# pouze přes klíče
for label in points.keys():
    print(label)

# pouze přes hodnoty
for point in points.values():
    print(point)
{% endhighlight %}

Použití `enumerate()`.

{% highlight python linenos %}
# získání indexu
for idx, number in enumerate([10, 20, 5, 10]):
    print(idx, number)

# jak enumerate funguje?
list(enumerate([10, 20, 5, 10]))
{% endhighlight %}

Použití `zip()`.

{% highlight python linenos %}
names = ["Lukáš Novák", "Petr Novák"]
salaries = [30000, 20000]

for name, salary in zip(names, salaries):
    print(name, salary)

# jak zip funguje?
list(zip(names, salaries, ["Olomouc", "Přerov"]))

# ruzne délky nevadí
list(zip(names, salaries, ["Olomouc", "Přerov", "Krnov"]))

# použití zip pro vytvoření slovníku
dict(zip(names, salaries))
{% endhighlight %}

Použití `reversed()`.

{% highlight python linenos %}
# iterace přes otočený seznam
for number in reversed([10, 20, 5, 10]):
    print(number)
{% endhighlight %}

Použití `range()`.

{% highlight python linenos %}
for number in range(10):
    print(number)

for number in range(5, 10):
    print(number)

# lichá čísla
for odd_number in range(0, 10, 2):
    print(odd_number)

# jak range funguje?
list(range(0, 10, 2))

for number in range(10, 0, -1):
    print(number)

for number in reversed(range(10)):
    print(number)
{% endhighlight %}

## Cyklus while
Více informací [zde](https://docs.python.org/3/tutorial/introduction.html#first-steps-towards-programming).

{% highlight python linenos %}
numbers = [10, 20, 5, 10]

while numbers:
    print(numbers.pop())
{% endhighlight %}

{% highlight python linenos %}
while True:
    user_input = input("Input:")

    if user_input == 'stop':
        # příkaz break přeruší nadřazený cyklus, rovněž existuje continue
        break
    else:
        print(user_input)
{% endhighlight %}

## Příkaz if v kontextu sekvencí
Více informací [zde](https://docs.python.org/3/tutorial/controlflow.html#if-statements).

{% highlight python linenos %}
numbers = []

# co se zde děje?
if numbers:
    print(numbers)

# toto, není to však třeba explicitně psát
if bool(numbers):
    print(numbers)
{% endhighlight %}

<div class="pep">
{% highlight python linenos %}
# PEP8 - využívejte faktu, že prázdné sekvence jsou vyhodnoceny jako False 
# správně
if not numbers:
    pass 

if numbers:
    pass 

# špatně
if not len(numbers):
    pass 

if len(numbers):
    pass 

# PEP8 - pořadí psaní podmínek
numbers = [1, 20, 5, 1]

# správně
if 10 not in numbers:
    print("There is no 10!")

# špatně
if not 10 in numbers:
    print("There is no 10!")
{% endhighlight %}
</div>


<!-- ## Úkoly
Nevíte si rady? Přečtěte si "[Jak pracovat s Github Classroom?](/teaching/2022/JP/classroom)".

* **L01E01**: Hello world [[Náhled](https://github.com/kmi-jp/template-L01E01)], [[Příjmout úkol](https://classroom.github.com/a/BLVFlAR8)]
* **L01E02**: Integer input [[Náhled](https://github.com/kmi-jp/template-L01E02)], [[Příjmout úkol]()]
* **L01E03**: Point input [[Náhled](https://github.com/kmi-jp/template-L01E03)], [[Příjmout úkol]()]
* **L01E04PEP**: PEP8 format [[Náhled](https://github.com/kmi-jp/template-L01E04PEP)], [[Příjmout úkol]()] -->


