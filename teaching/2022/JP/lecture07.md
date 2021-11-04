---
layout: default
courses: JP
title: 7. Iterátory a comprehension
year: 2022
---

## 7. Iterátory a comprehension

## Protokol iterování (Iteration protocol)
Na předchozích semináři jsme mohli vidět použití protokolu iterování v cyklu `for`.

{% highlight python linenos %}
for x in [1, 2, 3, 4]:
    print(x)
{% endhighlight %}

Díky protokolu iterování funguje cyklus `for` (a všechny nástroje využívající postupující zleva doprava např. `while`, list comprehensions - ty uvidíme později) pro všechny objekty které jsou iterovatelné (takzvané *iterable*).

Koncept iterable je generalizací pojmu sekvence. Objekt je považován za iterovatelný pokud je fyzicky uložen jako sekvence nebo se jedná o objekt který produkuje během iterace jednu hodnotu v jeden okamžik (například během `for` loopu).

**Terminologie: iterable vs iterator**
* *iterable* - objekt podporující volání `iter()`
* *iterator* - objekt vrácený voláním `iter()` podporující volání `next()`

{% highlight python linenos %}
iterator = iter([1, 2, 3])

next(iterator)
next(iterator)
next(iterator)
# exception StopIteration
next(iterator)
{% endhighlight %}

Pro podporu funkce `iter()` je nutné implementovat dunder metodu `__iter__`. Iterátor produkovaný funkcí `iter()` musí implementovat dunder metody `__next__` a vyvolat `StopIteration` na konci produkce hodnot.

S iterable (iterovatelný objekt) jsme se již setkali mnohokrát:

{% highlight python linenos %}
point = {"x": 10, "y": 20}
point.keys() # view, neni iterator, stejne tak .items(), .values()

# iterátor lze ale jednoduše získat
iterator = iter(point)

next(iterator)
next(iterator)
# exception StopIteration
next(iterator)
{% endhighlight %}

Rovněž zde vidíme důvod proč bylo nutné použít `list()` pro zobrazení celého výsledku `range()`.

{% highlight python linenos %}
list(range(5))

iterator = iter(range(5))

next(iterator)
{% endhighlight %}

Stejně tak pro `enumerate()`.

{% highlight python linenos %}
list(enumerate(range(5)))

iterator = iter(enumerate(range(5)))

next(iterator)
{% endhighlight %}

Funkce `zip()`, `enumerate()`, `filter()` vrací iterable a rovněž iterable přijímají, lze je tedy zanořovat.

{% highlight python linenos %}
list(zip(enumerate(range(10)), range(10)))
{% endhighlight %}

## Comprehension
Comprehension spolu s cykly jsou nejčastější případy použití iteračního protokolu.

Pokud budeme chtít umocnit seznam čísel, můžeme napsat klasický `for` cyklus.

{% highlight python linenos %}
squares = []

for number in numbers:
    squares.append(number ** 2)
{% endhighlight %}

Použití *list comprehension*:

{% highlight python linenos %}
squares = [number ** 2 for number in numbers]
{% endhighlight %}

Obecně platí předpis:

{% highlight python linenos %}
new_list = [expression for member in iterable]
{% endhighlight %}

Výsledkem list comprehension je vždy nový seznam. Comprehension je tedy dobré používat pouze pokud chceme vytvořit nový seznam hodnot. Dále pak platí, že pokud je list comprehension delší než dva řádky, je vhodnější (čitelnější) použít klasický `for` cyklus.

{% highlight python linenos %}
squares_minus = [number ** 2 for number in [number - 5 for number in numbers]]
{% endhighlight %}

### Podmínky v list comprehension
#### Filtrování
Jeden ze způsobů použití podmínek v comprehension je filtrování. Nejprve se podíváme jak bychom napsali řešení klasicky:

{% highlight python linenos %}
sentence = 'the rocket came back from mars'

vowels = []

for char in sentence:
    if char in 'aeiou':
        vowels.append(char)
{% endhighlight %}

A nyní řešení pomoci list comprehension:

{% highlight python linenos %}
vowels = [i for i in sentence if i in 'aeiou']
{% endhighlight %}

Obecný předpis je tedy:

{% highlight python linenos %}
new_list = [expression for member in iterable (if conditional)]
{% endhighlight %}

#### Úprava prvků
Dále lze podmínky použít ke změně hodnot v seznamu. Nejprve klasické řešení:

{% highlight python linenos %}
numbers = [10, 20, -5, 10]
abs_numbers = []

for number in numbers:
    if number > 0:
        abs_numbers.append(number)
    else:
        abs_numbers.append(abs(number))
{% endhighlight %}

A nyní řešení pomoci list comprehension:

{% highlight python linenos %}
numbers = [10, 20, -5, 10]

abs_numbers = [number if number > 0 else abs(number) for number in numbers]
{% endhighlight %}

Obecný předpis je tedy:

{% highlight python linenos %}
new_list = [expression (if conditional) for member in iterable]
{% endhighlight %}

### Zanořování list comprehension
List comprehension můžeme zanořovat, situaci demonstrujeme výpočtem kartézského součinu. Nejprve klasickým způsobem:

{% highlight python linenos %}
colors = ['red', 'green', 'blue']
sizes = ['S', 'M', 'L', 'XL']

tshirts = []
for color in colors:
    for size in sizes:
        tshirts.append((color, size))
{% endhighlight %}

A nyní řešení pomoci list comprehension:

{% highlight python linenos %}
tshirts_by_color = [(color, size) for color in colors for size in sizes]

# všimněme si, že na pořadí samozřejmné záleží
tshirts_by_size = [(color, size) for size in sizes for color in colors]

assert tshirts_by_color != tshirts_by_size
{% endhighlight %}

Obecně pak můžeme použít předpis:

{% highlight python linenos %}
[expression for target1 in iterable1 if condition1
            for target2 in iterable2 if condition2 ...
            for targetN in iterableN if conditionN]
{% endhighlight %}

### Set comprehension
Jak jsme viděli, v případě list comprehension je výsledkem vždy seznam. Pokud požadujeme aby výsledkem byla množina, můžeme použít *set comprehension*.

{% highlight python linenos %}
sentence = 'the rocket came back from mars'

unique_vowels = {i for i in sentence if i in 'aeiou'}
{% endhighlight %}

### Dict comprehension
Podobně pak v případě slovníku *dict comprehension*.

{% highlight python linenos %}
sentence = 'the rocket came back from mars'

word_len = {word: len(word) for word in sentence.split(' ')}
{% endhighlight %}

### Poznámka k rozsahu platnosti
V případě comprehension nejsou lokální proměnné dostupné:

{% highlight python linenos %}
[x for x in range(10)]

# nedostupná proměnná
x

# oproti tomu klasický for cyklus
output = []

for x in range(10):
    output.append(x)

# dostupná proměnná
x
{% endhighlight %}

### Poznámka k výkonu
Ve většině případů comprehensions přinesou znatelné zrychlení (často až dvojnásobné). Iterace comprehension jsou v rámci interpretu prováděny rychlostí jazyka C (narozdíl od běžného for cyklu).

Především pro případy, kdy iterujeme přes velká data, je vždy lepší použít comprehension, pozor však na čitelnost kódu.

## Generátorové funkce a výrazy
V novějších verzích jazyka Python je "prokrastinace" využívána mnohem častěji než dříve. Rovněž poskytují propracovanější nástroje pro jeho podporu. Jak se odložený výpočet projevuje? Namísto produkování celého výsledku najednou, jsou jednotlivé prvky (například prvky seznamu) produkovány v čase přístupu.

{% highlight python linenos %}
big_data = range(100000000000000)
small_data = range(1)

# 180 ns ± 14.4 ns per loop (mean ± std. dev. of 7 runs, 10000000 loops each)
%timeit zip(small_data, small_data)
# 184 ns ± 13.5 ns per loop (mean ± std. dev. of 7 runs, 10000000 loops each)
%timeit zip(big_data, big_data)

big_zip = zip(big_data, big_data)
# jednotlivé hodnoty jsou generovány postupně
# zde se vypočítá první hodnota
next(big_zip)
# zde se vypočítá druhá hodnota
next(big_zip)

# nelze, tato hodnota neexistuje (generatory obecne nejsou indexovatelné)
big_zip[100000]
{% endhighlight %}

### Generátorové funkce
Narozdíl od normálních funkcí, které skončí výpočet a vrátí hodnotu (pomoci příkazu `return`), generátorové funkce automaticky uspávají a probouzejí jejich vykonávání (pomoci příkazu `yield`).

Generátorové funkce jsou úzce spojené s iteračním protokolem. Aby bylo možné iterační protokol používat, jsou funkce obsahující `yield` kompilovány jako generátory. Nejedná se tedy o klasické funkce. Jejich architektura zaručuje, aby vracely objekt podporující iterační protokol. Později, při volání, vrací generátorový objekt, který podporuje iterační rozhraní (automaticky vytvoří metodu `__next__`).

V rámci generátorové funkce můžeme použít rovněž příkaz `return`, ten pak slouží na předčasné ukončení výpočtu (interně pomoci `StopIteration`).

{% highlight python linenos %}
# klasická funkce
def calculate_squares(n):
    for i in range(n):
        return i ** 2

# generátorová funkce
def generate_squares(n):
    for i in range(n):
        yield i ** 2

# 683 ns ± 52 ns per loop (mean ± std. dev. of 7 runs, 1000000 loops each)
%timeit calculate_squares(20000)

# 244 ns ± 6.12 ns per loop (mean ± std. dev. of 7 runs, 1000000 loops each)
%timeit generate_squares(20000)
{% endhighlight %}

Generátorové funkce je nejvhodnější používat tehdy, když nevíme, zda budeme potřebovat celý výsledek nebo pouze jeho část.

{% highlight python linenos %}
# v rámci for loopu
for square in generate_squares(20000000):
    print(square)
    if square > 100:
        break

# jako generátor
generator = generate_squares(20000000)

next(generator)
next(generator)
next(generator)
{% endhighlight %}

Generátory jsou *single-iteration* objekty. To znamená, že je přes ně možné iterovat pouze jedenkrát. Pokud se chceme k výsledkům vracet, je nutné je ukládat.

{% highlight python linenos %}
generator = generate_squares(20)

for square in generator:
    print(square)

# zde se již nic nevypíše, generátor je prázdný
for square in generator:
    print(square)
{% endhighlight %}

Pokud výsledky uložíme do seznamu, můžeme je procházet vícekrát.

{% highlight python linenos %}
generator = list(generate_squares(20))

for square in generator:
    print(square)

# zde se vypise
for square in generator:
    print(square)
{% endhighlight %}

### Generátorové výrazy
Obdoba list comprehension v kontextu iterátorů. Generátory můžeme vytvářet jednodušeji než definováním funkce.

{% highlight python linenos %}
# klasicky list comprehension (výpočet proběhne okamžitě)
[number ** 2 for number in range(10)]
# vrací - [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]

# generátorový výraz
(number ** 2 for number in range(10))
# vrací - <generator object <genexpr> at 0x106b73ac0>

generator = (number ** 2 for number in range(10))

# zde se vypočítá první hodnota
next(generator)
# zde se vypočítá druhá hodnota
next(generator)
{% endhighlight %}

### Příkaz `yield from`
Ve verzi 3.3 byl přidán příkaz `yield from`na delegování generátoru. Pokud máme nějaký generátor a chceme z něj vytvořit další generátor je možné napsat funkci:

{% highlight python linenos %}
def my_gen(gen):
    yield from gen
{% endhighlight %}

Příkaz `yield from` je rovněž elegantním způsobem na vytvoření generátoru (iterátoru) z uložené sekvence.

{% highlight python linenos %}
def g(x):
    yield from range(x, 0, -1)
    yield from range(x)

list(g(5))
# vysledek: [5, 4, 3, 2, 1, 0, 1, 2, 3, 4]
{% endhighlight %}

## Sekvence vs Iterátory
Nyní se podíváme na vytvoření vlastní sekvence pomoci implementování dunder metod `__len__` a `__getitem__`.

{% highlight python linenos %}
class Sentence:
    """Represents one sentence."""

    def __init__(self, text):
        self.text = text
        self.words = text.split(' ')
    
    def __repr__(self):
        return f"Sentence({self.text})"
    
    def __len__(self):
        return len(self.words)
    
    def __getitem__(self, index):
        return self.words[index]
    
sentence = Sentence("Ahoj svete jak se mas")

# magie
for word in sentence:
    print(word)

sentence[2]

len(sentence)
{% endhighlight %}

Nyní se podíváme na stejnou věc pohledem iterátoru. Zde je důležité rozlišit *iterable* a *iterator*. Třída `Sentence` je iterable, třída `SentenceIterator` je pak iterátor, který vrací metoda `Sentence.__iter__`.

Pro vytvoření iterátoru je tedy nutné implementovat dunder metody `__iter__` a `__next__`.

{% highlight python linenos %}
class Sentence:
    """Represents one sentence."""
    def __init__(self, text):
        self.text = text
        self.words = text.split(' ')
    
    def __repr__(self):
        return f"Sentence({self.text})"
    
    def __iter__(self):
        return SentenceIterator(self.words)

class SentenceIterator():
    def __init__(self, words):
        self.words = words
        self.index = 0
    
    def __next__(self):
        try:
            word = self.words[self.index]
        except IndexError:
            raise StopIteration
        
        self.index += 1
        return word
    
    def __iter__(self):
        return self
    
sentence = Sentence("Ahoj svete jak se mas")

# magie
for word in sentence:
    print(word)

len(sentence)
{% endhighlight %}

Další možností je situaci implementovat jako generátor. Tady pouze upozorním, že nevyužíváme opravdového postupného výpočtu, pro demonstraci je však tento příklad dostačující.

{% highlight python linenos %}
class Sentence:
    """Represents one sentence."""
    def __init__(self, text):
        self.text = text
        self.words = text.split(' ')
    
    def __repr__(self):
        return f"Sentence({self.text})"
    
    def __iter__(self):
        yield from self.words
        
sentence = Sentence("Ahoj svete jak se mas")

# magie
for word in sentence:
    print(word)

iterator = iter(sentence)

next(iterator)
next(iterator)
next(iterator)
next(iterator)
next(iterator)
# StopIteration
next(iterator)
{% endhighlight %}

Jak využít toho, aby byl výpočet vyhodnocován postupně? Zkusme navrhnout generátor opravdový.

{% highlight python linenos %}
def split_generator(text, sep=[" "]):
    """Creates generator for splitted text by given separator"""
    word = []

    for char in text:
        if char in sep:
            if word:
                yield "".join(word)
                word = []
        else:
            word.append(char)

    if word:
        yield "".join(word)

class Sentence:
    """Represents one sentence."""
    def __init__(self, text):
        self.text = text
    
    def __repr__(self):
        return f"Sentence({self.text})"
    
    def __iter__(self):
        yield from split_generator(self.text):
        
        # zde je rovněž možné použít generator expression
        # return (word for word in split_generator(self.text))

        # nejlepším řešením je však vrátit samotný generátor
        # return split_generator(self.text)

sentence = Sentence("Ahoj svete jak se mas")

# magie
for word in sentence:
    print(word)
    if word == "svete":
        break 

iterator = iter(sentence)

next(iterator)
next(iterator)
next(iterator)
next(iterator)
next(iterator)
# StopIteration
next(iterator)
{% endhighlight %}


## Úkoly
Nevíte si rady? Přečtěte si "[Jak pracovat s Github Classroom?](/teaching/2022/JP/classroom)".

* **L07E01**: Generator function [[Náhled](https://github.com/kmi-jp/template-L07E01)], [[Příjmout úkol](https://classroom.github.com/a/H24-_heG)]
* **L07E02**: Data 3 [[Náhled](https://github.com/kmi-jp/template-L07E02)], [[Příjmout úkol](https://classroom.github.com/a/r97TpK58)]
