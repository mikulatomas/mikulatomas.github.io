---
layout: default
courses: JP
title: 4. Výjimky a dekorátory (WIP)
year: 2022
---

## 4. Výjimky a dekorátory

## Výjimky
Více informací [zde](https://docs.python.org/3/tutorial/errors.html).

Během našeho programování jsme se již setkali s několika výjimkami (exceptions). Jednu ukázkovou můžeme vyvolat následovně:

{% highlight bash linenos %}
Python 3.9.4 (default, Apr  5 2021, 01:50:46) 
[Clang 12.0.0 (clang-1200.0.32.29)] on darwin
Type "help", "copyright", "credits" or "license" for more information.

>>> 2 / 0
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ZeroDivisionError: division by zero
{% endhighlight %}

Nejdůležitější je řádek číslo 8, který obsahuje název výjimky `ZeroDivisionError` a doplňující popis `division by zero`.

### Ošetření výjimek
V předchozím příkladu jsme viděli vyvolání výjimky, teď si ukážeme jak na vyvolanou výjimku reagovat.

{% highlight python linenos %}
try:
    a = 2 / 0
except ZeroDivisionError as err:
    print(f"Following exception was raised: {err}")
finally:
    print("Finishing program")
{% endhighlight %}

Ošetření výjimek realizuje příkaz `try, except, finally`. Blok začínající příkazem `try` obsahuje kód ve kterém se výjimky odchytávají. Následují bloky `except` s typy výjimek a reakcemi na ně. Volitelný blok `finally` se provede v každém případě, ať už výjimka nastane nebo nikoli.

### Vestavěné výjimky
Jazyk Python obsahuje sadu vestavěných výjimek, které můžete ve svých programech používat. Jejich podrobnější popis naleznete [zde](https://docs.python.org/3/library/exceptions.html). Jejich krátký přehled je uveden níže.

{% highlight python linenos %}
AssertionError      # podmínka příkazu assert není splněna
IndexError          # přístup na neexistující index v kolekci
NameError           # přístup k nedefinované proměnné
TypeError           # operace s nekompatibilními datovými typy
ValueError          # operace s kompatibilními datovými typy ale s chybnou hodnotou
OverflowError       # výsledek operace je příliš velký a nelze reprezentovat v paměti
ZeroDivisionError   # druhý operant dělení nebo modulo roven nule
RuntimeError        # blíže nespecifikovaná chyba
Exception           # obecná výjimka
{% endhighlight %}

### Vyvolání výjimek
Výjimky můžeme nejen ošetřovat ale rovněž vyvolávat. V následujícím příkladu funkce vyvolá vyjimku `ValueError` pokud není její vstup validní.

{% highlight python linenos %}
def subtract_lists(list1, list2):
    """Subtract two list piecewise."""

    if len(list1) != len(list2):
        raise ValueError(f"Lists are not same length, {len(list1)} and {len(list2)} was given.")
    
    result = []

    for a, b in zip(list1, list2):
        result.append(a - b)
    
    return result

subtract_lists([1, 2], [4, 3])
{% endhighlight %}

Existují dva přístupy k práci s výjimkami *EAFP* (it’s easier to ask for forgiveness than permission) a *LBYL* (look before you leap). Rozdíl je vidět v následujícím příkladě.

{% highlight python linenos %}
# EAFP
if "key" in dict_:
    value += dict_["key"]

# LBYL
try:
    value += dict_["key"]
except KeyError:
    pass
{% endhighlight %}

V Pythonu s ohledem na jeho dynamičnost častěji upřednostnujeme *EAFP*. Je možné používat obojí, v případě *LBYL* však není dobré kontroly příliš přehánět.

## Dekorátory
### Funkce vyššího řádu
Funkce které vracejí funkce nebo přijímají funkce jako argument nazýváme *funkce vyššího řádu*.

{% highlight python linenos %}
# funkce vyššího řádu přijímající funkci jako argument
def list_operation(list, operation):
    """Applies operation on the given list"""
    return operation(list)

# běžná funkce
def list_sum_squared(list):
    """Squared sum of all members of given list"""
    return sum(list) ** 2

list_operation([1,2,3], list_sum_squared)
{% endhighlight %}

{% highlight python linenos %}
# funkce vyššího řádu vracející funkci
def parent_function():
    print("Parent function is running.")

    local_x = 10

    def child_function():
        print(local_x)
        print("Child function is running.")
    
    return child_function

# vráceni odkazu na funkci a její zavolání
parent_function()()

# k vnitřní funkci nelze přistoupit
child_function()
{% endhighlight %}

### Jednoduchý dekorátor
Co je dekorátor můžeme demonstrovat na jednoduchém příkladu. Představme si, že nami definovanou funkci chceme "obalit" další funkcionalitou (například vytisknout řetězec před a po volání).

{% highlight python linenos %}
# dekorátor
def my_decorator(func):
    def wrapper():
        print("Something before function call")
        func()
        print("Something after function call")
    
    return wrapper

# námi definovaná funkce
def my_function():
    print("Super function!")

# volání původní funkce
my_function()

my_function

# obalení funkce dekorátorem
my_function_decorated = my_decorator(my_function)

# volání modifikované funkce
my_function_decorated()

my_function_decorated
{% endhighlight %}

Všimněme si, že dekorátor splňuje definici funkce vyššího řádu. Pro jednodušší práci s dekorátory Python nabízí "syntaktický cukr" `@nazev_dekoratoru`.

{% highlight python linenos %}
# dekorátor
def do_twice(func):
    def wrapper():
        func()
        func()
    
    return wrapper

# dekorování funkce pomoci @
@do_twice
def my_function():
    print("Super function!")
{% endhighlight %}

### Dekorování funkce s argumenty
V případě, že námi dekorovaná funkce přijímá argumenty narazíme na následující problém.

{% highlight python linenos %}
# dekorátor
def do_twice(func):
    def wrapper():
        func()
        func()
    
    return wrapper

# dekorování funkce pomoci @
@do_twice
def my_function(arg):
    print(f"Super function! {arg}")

# chyba - dekorovaná funkce nepočítá s argumentem
my_function("Ahoj!")
{% endhighlight %}

Tuto situaci opravíme následovně.

{% highlight python linenos %}
# dekorátor - podporuje předání argumentů vnitřní funkci
def do_twice(func):
    def wrapper(*args, **kwargs):
        func(*args, **kwargs)
        func(*args, **kwargs)
    
    return wrapper

# dekorování funkce pomoci @
@do_twice
def my_function(arg):
    print(f"Super function! {arg}")

my_function("Ahoj!")
{% endhighlight %}

### Dekorování funkce vracející hodnotu
Podobný problém nastane pokud funkce vrací hodnotu. Její dekorovaná verze bude tuto hodnotu zahazovat.

{% highlight python linenos %}
# dekorátor
def do_twice(func):
    def wrapper(*args, **kwargs):
        func(*args, **kwargs)
        func(*args, **kwargs)
    
    return wrapper

# dekorování funkce pomoci @
@do_twice
def multiply_list(list, by):
    """Multiply items from list by given value.

    Args:
        list: list to be multiplied
        by: value by which items are multiplied

    Returns:
        multiplied list
    """
    result = []

    for item in list:
        result.append(item * by)
    
    return result

# problém - výsledek nebude vrácen
multiply_list([1, 2, 3], 5)
{% endhighlight %}

Jednoduchou modifikací dekorátoru situaci vyřešíme.

{% highlight python linenos %}
# upravený dekorátor
def do_twice(func):
    def wrapper(*args, **kwargs):
        func(*args, **kwargs)
        return func(*args, **kwargs)
    
    return wrapper
{% endhighlight %}

### Problém identity funkce
Při dekorování funkce dochází ke ztrátě identity funkce, což ovlivňuje i nedosažitelnost původního docstringu. Demonstrace z předchozího příkladu:

{% highlight python linenos %}
multiply_list
multiply_list.__name__
help(multiply_list)
{% endhighlight %}

Situaci vyřešíme použitím dekorátoru `functools.wraps`, který zachová identitu dekorované funkce.

{% highlight python linenos %}

import functools

def do_twice(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        func(*args, **kwargs)
        return func(*args, **kwargs)
    
    return wrapper

@do_twice
def multiply_list(list, by):
    """Multiply items from list by given value.

    Args:
        list: list to be multiplied
        by: value by which items are multiplied

    Returns:
        multiplied list
    """
    result = []

    for item in list:
        result.append(item * by)
    
    return result

multiply_list
multiply_list.__name__
help(multiply_list)
{% endhighlight %}

### Příklad z praxe
Základní šablona dekorátoru je následující:

{% highlight python linenos %}
import functools

def decorator(func):
    @functools.wraps(func)
    def wrapper_decorator(*args, **kwargs):
        # kód vykonaný před voláním funkce
        value = func(*args, **kwargs)
        # kód vykonaný po volání funkce
        return value
    return wrapper_decorator
{% endhighlight %}

Jedním z příkladu je takzvaný *timing* funkce (měření doby běhu).

{% highlight python linenos %}
import functools
import time

def timer(func):
    """Print the runtime of the decorated function"""
    @functools.wraps(func)
    def wrapper_timer(*args, **kwargs):
        start_time = time.perf_counter()

        value = func(*args, **kwargs)

        end_time = time.perf_counter()
        run_time = end_time - start_time

        print(f"Finished {func.__name__} in {run_time:.4f} secs")

        return value

    return wrapper_timer

@timer
def waste_some_time(num_times):
    for _ in range(num_times):
        sum([i**2 for i in range(10000)])

waste_some_time(1000) 
{% endhighlight %}

### Zanořování dekorátorů
Dekorátory je možné zanořovat, pozor, záleží na pořadí!

{% highlight python linenos %}
@do_twice
@timer
def waste_some_time(num_times):
    for _ in range(num_times):
        sum([i**2 for i in range(10000)])

waste_some_time(100) 

@timer
@do_twice
def waste_some_time(num_times):
    for _ in range(num_times):
        sum([i**2 for i in range(10000)])

waste_some_time(100) 
{% endhighlight %}

### Dekorátory s argumenty
Dekorátoru je možné předávat argumenty.

{% highlight python linenos %}
def repeat(num_times):
    def decorator_repeat(func):
        @functools.wraps(func)
        def wrapper_repeat(*args, **kwargs):
            for _ in range(num_times):
                value = func(*args, **kwargs)
            return value
        return wrapper_repeat
    return decorator_repeat

@repeat(num_times=4)
@timer
def waste_some_time(num_times):
    for _ in range(num_times):
        sum([i**2 for i in range(10000)])

waste_some_time(100) 
{% endhighlight %}


<!-- ## Úkoly
Nevíte si rady? Přečtěte si "[Jak pracovat s Github Classroom?](/teaching/2022/JP/classroom)".

* **L01E01**: Hello world [[Náhled](https://github.com/kmi-jp/template-L01E01)], [[Příjmout úkol](https://classroom.github.com/a/BLVFlAR8)]
* **L01E02**: Integer input [[Náhled](https://github.com/kmi-jp/template-L01E02)], [[Příjmout úkol]()]
* **L01E03**: Point input [[Náhled](https://github.com/kmi-jp/template-L01E03)], [[Příjmout úkol]()]
* **L01E04PEP**: PEP8 format [[Náhled](https://github.com/kmi-jp/template-L01E04PEP)], [[Příjmout úkol]()] -->


