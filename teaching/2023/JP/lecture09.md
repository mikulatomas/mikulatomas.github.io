---
layout: default
courses: JP
title: 9. Itertools a funkce vracející iterátory
year: 2023
---

## 9. Itertools a funkce vracející iterátory

## Lambda výraz
Slouží k vytváření krátkých *anonymních funkcí* (funkce bez identifikátoru - názvu). Tělo funkce je syntakticky omezeno na jeden výraz a mohou být použity všude, kde je vyžadována funkce. Sémanticky se jedná o *syntaktický cukr* pro klasickou funkci.

{% highlight python linenos %}
def apply_operation(operation, a, b):
    return operation(a, b)


def my_sum(a, b):
    return a + b


assert apply_operation(my_sum, 2, 3) == 5

assert apply_operation(lambda a, b: a + b, 2, 3) == 5
{% endhighlight %}

{% highlight python linenos %}
# anonymní funkce pro součet dvou čísel
lambda a, b: a + b

# Out[1]: <function __main__.<lambda>(a, b)>

(lambda a, b: a + b)(1, 2)

# Out[2]: 3
{% endhighlight %}

Hezkým příkladem je použití v parametru `key` u funkce `sorted`.

{% highlight python linenos %}
# seznam obsahující čísla
sorted([4, 2, 3, 1, 4])

# seznam obsahující n-tice
sorted([(1, "Lukáš Novák", 10),
        (4, "Albert Billa", 1),
        (0, "Pepa Los", 30)])
# Out[0]: [(0, 'Pepa Los', 30), (1, 'Lukáš Novák', 10), (4, 'Albert Billa', 1)]

# seřazeno dle posledního prvku
sorted([(1, "Lukáš Novák", 10), 
        (4, "Albert Billa", 1), 
        (0, "Pepa Los", 30)], key=lambda x: x[2])
# Out[1]: [(4, 'Albert Billa', 1), (1, 'Lukáš Novák', 10), (0, 'Pepa Los', 30)]
{% endhighlight %}

## Funkce `any` a `all`
Vrací `True` v případě že je některý prvek/jsou všechny prvky iterable vyhodnoceny na pravdu. Pokud je iterable prázdná, vrací `False`.

{% highlight python linenos %}
any([[], [1, 2, 3], [1]])

all([[], [1, 2, 3], [1]])
{% endhighlight %}

## Funkce `map`
Vrací iterátor (pozor nikoli sekvenci), který aplikuje funkci na každý prvek předaného iterable. Tyto výsledky jsou pomoci `yield` vráceny.

{% highlight python linenos %}
map(lambda x: x ** 2, [1, 2, 3, 4])
{% endhighlight %}

Funkce `map` může přijímat libovolný počet iterable, v takovém případě však musí aplikovaná funkce přijímat stejný počet argumentů jako je počet iterable.

{% highlight python linenos %}
import operator


# operator.mul přijímá 2 argumenty
map(operator.mul, [1, 2, 3, 4], [2, 3, 4, 5])
{% endhighlight %}

## Funkce `filter`
Vrací iterátor (pozor nikoli sekvenci) vytvořený z prvků iterable pro které se funkce vyhodnotí na pravdu. Pokud je funkce rovna `None` jsou z iterable tímto způsobem odstraněny všechny prvky vyhodnocené na nepravdu.

{% highlight python linenos %}
filter(None, [[], [1, 2, 3], [1]])

filter(lambda x: x % 3, range(20))
{% endhighlight %}

Funkce `filter(function, iterable)` je ekvivalentní generátorovému výrazu:

{% highlight python linenos %}
# pokud je function rovno None
(item for item in iterable if function(item))

# jinak
(item for item in iterable if item)
{% endhighlight %}

## Modul `itertools`
Modul implementující sadu užitečných iterátorů. Tyto iterátory preferujeme nad vlastní implementací z důvodu rychlosti a paměťové úspornosti.

### `itertools.starmap`
Vytvoří iterátor, který vyhodnocuje předanou funkci s argumenty získanými z iterable. Narozdíl od klasického `map` přijímá jeden iterable, ten však může obsahovat n-tice (v případě funkce přijímající `n` argumentů). Sémanticky podobné použítí `*` při volání funkce.

{% highlight python linenos %}
import itertools
import operator


# operator.mul přijímá 2 argumenty
itertools.starmap(operator.mul, [[1, 2], [2, 3], [3, 4], [4, 5]])
{% endhighlight %}

### `itertools.filterfalse`
Pracuje podobně jako `filter`, výsledný iterátor však produkuje prvky pro které je funkce nepravda.

{% highlight python linenos %}
import itertools


itertools.filterfalse(None, [[], [1, 2, 3], [1]])

itertools.filterfalse(lambda x: x % 3, range(20))
{% endhighlight %}

### Ostatní iterátory
Celkový výčet iterátorů dostupných v balíčku `itertools` je dostupný [zde](https://docs.python.org/3/library/itertools.html).

## List comprehension vs `map`/`filter`
Funkci `map`/`filter` je vhodné nahrazovat list comprehension, můžeme dosáhnout vyššího výkonu. Jedná se však o konstantní overhead volání funkce.

{% highlight python linenos %}
import timeit


# 10
print(timeit.timeit("list(map(lambda x: x * x, range(10)))", number=10000))
# 0.016546586997719714

print(timeit.timeit("[x * x for x in range(10)]", number=10000))
# 0.008891337001841748

# 10000
print(timeit.timeit("list(map(lambda x: x * x, range(10000)))", number=10000))
# 7.573531197998818

print(timeit.timeit("[x * x for x in range(10000)]", number=10000))
# 5.103068967000581

# 100000
print(timeit.timeit("list(map(lambda x: x * x, range(100000)))", number=10000))
# 96.5402136729972

print(timeit.timeit("[x * x for x in range(100000)]", number=10000))
# 63.06499578600051
{% endhighlight %}

## Úkoly
Nevíte si rady? Přečtěte si "[Jak pracovat s Github Classroom?](/teaching/2023/JP/classroom)".

### Skupina Mikula

* **L09E01**: Matrix rows mask [[Náhled](https://github.com/kmi-jp/template-L09E01)], [[Příjmout úkol](https://classroom.github.com/a/uRtQBH33)]
* **L09E02**: Password generator [[Náhled](https://github.com/kmi-jp/template-L09E02)], [[Příjmout úkol](https://classroom.github.com/a/DUYUYkBm)]

### Skupina Petržela

* **L09E01**: Matrix rows mask [[Náhled](https://github.com/kmi-jp/template-L09E01)], [[Příjmout úkol]()]
* **L09E02**: Password generator [[Náhled](https://github.com/kmi-jp/template-L09E02)], [[Příjmout úkol]()]


