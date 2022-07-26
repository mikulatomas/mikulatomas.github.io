---
layout: default
courses: JP
title: 10. Testování (pytest) a type hinting
year: 2023
---
 
## 10. Testování (pytest) a type hinting
 
## Instalování balíčků
### Instalátor `pip`
Umožňuje instalování balíčku (lokálních nebo z repozitářů jako je [PyPi](https://pypi.org)). Základní popis funkcionality naleznete [zde](https://pip.pypa.io/en/stable/getting-started/).
 
Instalace balíčku `pytest` na UNIX systému (Linux, Mac OS a další):
{% highlight bash linenos %}
$ python3 -m pip install pytest
{% endhighlight %}
 
Instalace balíčku `pytest` na Windows systému:
{% highlight bash linenos %}
$ py -m pip install pytest
{% endhighlight %}
 
Pro vývoj je podstatné instalovat balíčky v interaktivním režimu. Kód potom můžeme jednoduše testovat. Tečka reprezentuje aktuální adresář (je tedy nutné být v hlavním adresáři balíčku - tam kde je umístěn soubor `setup.py`).
 
{% highlight bash linenos %}
$ python3 -m pip install -e .
{% endhighlight %}
 
Windows:
{% highlight bash linenos %}
$ py -m pip install -e .
{% endhighlight %}
 
### Soubor `setup.py`
Centrální soubor pro instalaci balíčku, obsahuje rovněž všechna důležitá metadata. V praxi může být poměrně komplexní, detailní informace je možné nalézt [zde](https://docs.python.org/3/distutils/setupscript.html).
 
Základní obsah souboru `setup.py` může vypadat následovně:
 
{% highlight python linenos %}
#!/usr/bin/env python
 
from setuptools import setup, find_packages
 
setup(name='data',
     version='1.0',
     description='Perfect library for students.',
     author='Lukas Novak',
     author_email='lukas@novak.cz',
     url='https://lukasnovak.cz',
     packages=find_packages(),
    )
{% endhighlight %}
 
Podstatná je funkce `find_packages()`, která se postará o lokalizování balíčku ve složce ve které je soubor `setup.py` umístěn. Ve většině případů stačí použít funkci `find_packages()` bez argumentů.
 
## Testování
Testování je nedílnou součástí vývoje jakéhokoli softwaru, určité typy testování jdou automatizovat. My si ukážeme takzvané *unit testy* (jednotkové testy), název je odvozen od faktu, že postupně testujeme všechny části kódu odděleně (většinou funkci po funkci). Jeden unit test testuje právě jednu věc.
 
Při vhodném návrhu testů získáme větší důvěru (nikoli jistotu), že náš kód neobsahuje zásadní funkční chyby. Z pravidla je nutné vymyslet a otestovat veškeré krajní případy a případy pro které chceme aby funkce fungovala/nefungovala.
 
Testování probíhá způsobem, kdy funkci předáme vstup a očekávaný výstup. Pokud tyto dvě věci nejsou rovny, test selže.
 
Hlavní výhodou unit testů je jejich jednoduchá znovu proveditelnost. Testy nám tímto způsobem "chrání" již implementovaný (a otestovaný) kód. V praxi je běžné nejdříve napsat testy a poté implementovat samotný kód (*test driven development* - to jste dělali po dobu celého kurzu).
 
### Balíček `pytest`
Python obsahuje nativní možnost unit testů, budeme však používat rozšířenější možnost - balíček `pytest`. Ten je tedy nutné nejprve nainstalovat.
 
### První test
Vytvořme soubor `test_inc.py` s následujícím obsahem:
 
{% highlight python linenos %}
def inc(x):
   return x + 1
 

# funkce test_inc provádějící test funkce inc
# assert již známe, pokud selže, selže i celkový test
def test_inc():
   assert inc(3) == 5
{% endhighlight %}
 
Poté můžeme pustit příkaz `pytest`:
 
{% highlight bash linenos %}
$ pytest
=========================== test session starts ============================
platform linux -- Python 3.x.y, pytest-6.x.y, py-1.x.y, pluggy-1.x.y
cachedir: $PYTHON_PREFIX/.pytest_cache
rootdir: $REGENDOC_TMPDIR
collected 1 item
 
test_sample.py F                                                     [100%]
 
================================= FAILURES =================================
_______________________________ test_answer ________________________________
 
   def test_answer():
>       assert inc(3) == 5
E       assert 4 == 5
E        +  where 4 = inc(3)
 
test_sample.py:6: AssertionError
========================= short test summary info ==========================
FAILED test_sample.py::test_answer - assert 4 == 5
============================ 1 failed in 0.12s =============================
{% endhighlight %}
 
### Organizace testů
Existují základní doporučení jak testy v rámci balíčku organizovat (nalezneme je [zde](https://docs.pytest.org/en/6.2.x/goodpractices.html#goodpractices)).
 
Jde především o následující adresářovou strukturu:
 
{% highlight plain linenos %}
setup.py
mypkg/
   __init__.py
   app.py
   view.py
tests/
   test_app.py
   test_view.py
   ...
{% endhighlight %}
 
Všimněme si prefixu `test_` u souborů obsahující testy. Druhou částí názvu je pak název modulu, který v souboru testujeme (soubor `test_app.py` může obsahovat libovolné množství testů).
 
Hlavní výhodou je, že po instalaci balíčku interaktivní metodou (`-e .`) můžeme spouštět ve složce příkaz `pytest`, který automaticky spustí všechny dostupné testy.
 
### Další příklady
Podívejme se na test `test_dot_product.py` z balíčku `algebra` (3. seminář).
{% highlight python linenos %}
import pytest
 
from algebra.vector import dot_product
 
 
def test_dot_product():
   assert dot_product([1, 2, 3], [3, 2, 1]) == 10
   assert dot_product([-1, 2, 3], [3, 2, 1]) == 4
{% endhighlight %}
 
Funkce `test_dot_product()` tedy testuje dva možné vstupy (a výstupy) pro funkci `dot_product`.
 
V případě `dot_product` může být žádoucí otestovat větší množství kombinací vstupu a výstupu. Pro tyto potřeby můžeme použít dekorátor `@pytest.mark.parametrize` - test poté parametrizuje.
 
{% highlight python linenos %}
import pytest
 
from algebra.vector import dot_product
 
 
@pytest.mark.parametrize(
   "v1, v2, result",
   [([1, 2, 3], [3, 2, 1], 10),
    ([-1, 2, 3], [3, 2, 1], 4)],
)
def test_dot_product(v1, v2, result):
   assert dot_product(v1, v2) == result
{% endhighlight %}
 
Test se poté spustí pro všechny vstupy/výstupy které uvádíme.
 
Dalším příkladem je testování třídy `Index` z balíčku `data`.
 
{% highlight python linenos %}
import pytest
 
from data.index import Index
 
 
def test_index():
   test_labels = ["key 1", "key 2", "key 3", "key 4", "key 5"]
 
   idx = Index(labels=test_labels)
   values = [0, 1, 2, 3, 4]
 
   assert idx.labels == test_labels
   assert isinstance(idx.labels, list)
   assert idx.name == ""
{% endhighlight %}
 
Funkce `test_index` testuje vytvoření instance třídy `Index`. Vzpomeňme si však, že třída `Index` obsahuje i další metody, je tedy nutné testy rozšířit.
 
{% highlight python linenos %}
import pytest
 
from data.index import Index
 
 
def test_index():
   test_labels = ["key 1", "key 2", "key 3", "key 4", "key 5"]
 
   idx = Index(labels=test_labels)
 
   assert idx.labels == test_labels
   assert isinstance(idx.labels, list)
   assert idx.name == ""
 
 
def test_get_loc():
   test_labels = ["key 1", "key 2", "key 3", "key 4", "key 5"]
 
   idx = Index(labels=test_labels)
   values = [0, 1, 2, 3, 4]
 
   assert values[idx.get_loc("key 2")] == 1
{% endhighlight %}
 
Tyto testy již *pokrývají* i metodu `Index.get_loc()` - ne však úplně, netestujeme vyvolání výjimky, o tom ale později.
 
### Fixtures
Při prvním pohledu vidíme, že v každém testu musíme znovu vytvořit instanci třídy `Index`. Balíček `pytest` však umožňuje vytvářet takzvané *fixtures*. Slouží ke zjednodušení opakujících se částí testů. Použití fixtures nám zaručí, že se data pro každý test vytvoří znovu.
 
{% highlight python linenos %}
import pytest
 
from data.index import Index
 
 
@pytest.fixtures
def labels():
   return ["key 1", "key 2", "key 3", "key 4", "key 5"]
 
 
@pytest.fixtures
def values():
   return [0, 1, 2, 3, 4]
 
 
# fixture může používat ostatní fixture
@pytest.fixtures
def index(labels):
   return Index(labels=labels)
 
 
# test používající dvě fixtures - index a labels
def test_index(index, labels):
   assert index.labels == test_labels
   assert isinstance(index.labels, list)
   assert index.name == ""
 
 
# test používající dvě fixtures - index a labels
def test_get_loc(index, values):
   assert values[index.get_loc("key 2")] == 1
{% endhighlight %}
 
### Testování vyjímek
Jak jsme zmínili, u metody `get_loc` nemáme otestovaný případ pro vyvolání výjimky. Situaci vyřešíme přidáním testu.
 
{% highlight python linenos %}
import pytest
 
from data.index import Index
 
 
@pytest.fixtures
def labels():
   return ["key 1", "key 2", "key 3", "key 4", "key 5"]
 
 
@pytest.fixtures
def values():
   return [0, 1, 2, 3, 4]
 
 
@pytest.fixtures
def index(labels):
   return Index(labels=labels)
 
 
def test_index(index, labels):
   assert index.labels == test_labels
   assert isinstance(index.labels, list)
   assert index.name == ""
 
 
def test_get_loc(index, values):
   assert values[index.get_loc("key 2")] == 1
 
 
# testování situace kdy metoda musí vyvolat vyjimku
def test_invalid_key(index):
   with pytest.raises(KeyError):
       index.get_loc("key 10")
{% endhighlight %}
 
### Testování v rámci docstringů
Elegantním způsobem pro testování jednoduchých funkcí je docstring. Knihovna `pytest` umožňuje spouštět testy ve formě příkladů v docstring. Příklady jsou potom rovněž zobrazeny v nápovědě k funkci, což zlepšuje její používání.
 
{% highlight python linenos %}
def sum_two_numbers(a, b):
   """Sums two numbers.
 
   Args:
       a: first number
       b: second number
 
   Example:
       >>> sum_two_numbers(10, 20)
       30
       >>> sum_two_numbers(-10, 20)
       10
   """
   return a + b
{% endhighlight %}
 
Poté stačí přidat argument `--doctest-modules` při spuštění `pytest`:
 
{% highlight bash linenos %}
$ pytest --doctest-modules
======================= test session starts ========================
platform darwin -- Python 3.10.0, pytest-6.2.5, py-1.10.0, pluggy-1.0.0
rootdir: /Users/tomasmikula/Downloads/vyuka
plugins: cov-3.0.0
collected 1 item                                                  
 
sum_two_numbers.py .                                         [100%]
 
======================== 1 passed in 0.03s =========================
{% endhighlight %}
 
Pozor, ne vždy stačí spoléhat na testy v podobě příkladů v docstring, komplexnější testování by mělo být provedeno v samostatných `test_*.py` souborech.
 
### Code coverage
V kontextu testování zdrojového kódu je nutné zmínit pojem *code coverage*. Jedná se o procentuální pokrytí testů, neboli jaké procento zdrojového kódu je testováno. V ideálním případě, je dobré docílit 100% pokrytí, v praxi jsou však hodnoty nad 90% dostatečné.
 
#### Jak code coverage počítat?
V kombinaci s knihovnou `pytest` lze nainstalovat knihovnu `coverage`, která code coverage vypočítá.
 
{% highlight bash linenos %}
$ py -m pip install coverage
{% endhighlight %}
 
Dále můžeme code coverage spočítat příkazem `coverage run -m pytest` (případně musíme dodat další argumenty jako `--doctest-modules`).
 
{% highlight bash linenos %}
$ coverage run -m pytest
{% endhighlight %}
 
A zobrazit výsledek:
 
{% highlight bash linenos %}
$ coverage report -m
Name                      Stmts   Miss  Cover   Missing
-------------------------------------------------------
my_program.py                20      4    80%   33-35, 39
my_other_module.py           56      6    89%   17-23
-------------------------------------------------------
TOTAL                        76     10    87%
{% endhighlight %}
 
## Type hinting
Jazyk Python je dynamicky typovaný, to nám ale nebrání implementovat takzvaný *type hinting* (nápověda datových typů). Uživatelům našeho zdrojového kódu pak umožníme jednoduše vidět s jakým datovým typem náš zdrojový kód pracuje. Tyto informace jsou využity rovněž různými editory (například Microsoft Visual Code).
 
{% highlight python linenos %}
def sum_two_numbers(a: int, b: int) -> int:
   """Sums two numbers.
 
   Args:
       a (int): first number
       b (int):  second number
 
   Returns:
       int: Summation of a and b
   """
   return a + b
{% endhighlight %}
 
Všimněme si několika věcí. Dvojtečka za názvem parametru umožňuje zápis datového typu. Pro určení typu návratové hodnoty můžeme použít `->` za závorkou na řádku definice funkce, nezapomeňte ale na `:` na konci řádku. Je nutné zdůraznit, že typing nikterak neovlivňuje funkčnost funkce, takto definované funkce můžeme předat `float` a výsledek bude vypočítán.
 
Rozšiřme tedy typování této funkce na libovolné číslo. Tahák k modulu `typing` nalezneme [zde](https://mypy.readthedocs.io/en/stable/cheat_sheet_py3.html). Abstraktní typ čísla můžeme získat z modulu `numbers`.
 
{% highlight python linenos %}
from numbers import Number
 
def sum_two_numbers(a: Number, b: Number) -> Number:
   """Sums two numbers.
 
   Args:
       a (Number): first number
       b (Number): second number
 
   Returns:
       Number: Summation of a and b
   """
   return a + b
{% endhighlight %}
 
Druhou možností by bylo použít `typing.Union` (realizuje sjednocení, funkce přijímá více datových typů).
 
{% highlight python linenos %}
import typing
 
 
def sum_two_numbers(
   a: typing.Union[int, float], b: typing.Union[int, float]
) -> typing.Union[int, float]:
   """Sums two numbers.
 
   Args:
       a (typing.Union[int, float]): first number
       b (typing.Union[int, float]): second number
 
   Returns:
       typing.Union[int, float]: Summation of a and b
   """
   return a + b
{% endhighlight %}
 
Pozor v tomto případě komunikujeme, že funkce přijímá pouze `int` a `float`, existují však další číselné typy (např `complex`).
 
### Typování sekvencí/kolekcí
Pokud funkce pracuje se sekvencemi můžeme typovat sekvenci obecnou `typing.Sequence`, obdobně potom obecnou kolekci `typing.Collection`. V případě konkrétních sekvencí/kolekce (například `list`) můžeme použít `typing.List` (`typing.Tuple`, `typing.Dict`, `typing.Set` a další).
 
Vraťme se k příkladu `dot_product()` pro dva vektory:
 
{% highlight python linenos %}
import typing
 
 
def dot_product(
   vector_1: typing.Sequence[typing.Union[int, float]],
   vector_2: typing.Sequence[typing.Union[int, float]],
) -> typing.Union[int, float]:
   """Calculates dot product of two vectors.
 
   Args:
       vector_1 (typing.Sequence): vector 1
       vector_2 (typing.Sequence): vector 2
 
   Returns:
       typing.Union[int, float]: Dot product of vector 1 and vector 2
   """
   return sum((a * b for a, b in zip(vector_1, vector_2)))
{% endhighlight %}
 
Všimněme si především toho, že v případě sekvencí/kolekcí/iterátorů můžeme udat datový typ obsahu.
 
### Typování tříd
V případě tříd metoda `__init__` nevrací žádnou hodnotu, proto u typování uvádíme `-> None`. Rovněž v případě parametru `self` typování vynecháme.
 
{% highlight python linenos %}
class MyClass:
   def __init__(self) -> None:
       ...
 
   def my_method(self, num: int, str1: str) -> str:
       return num * str1
{% endhighlight %}
 
### Volitelné argumenty a `typing.Optional`
V případě, že funkce přijímá některé volitelné argumenty, syntaxe zůstane obdobná jako u funkcí, které typování nemají. Pro případ kdy chceme definovat argument určitého typu a zároveň umožňuje hodnotu `None` použijeme `typing.Optional[typ]`, který je ekvivalentní `typing.Union[typ, None]`.
 
{% highlight python linenos %}
import typing
 
 
def submatrix(
   matrix: typing.Sequence[typing.Sequence[typing.Union[int, float]]],
   drop_rows: typing.Optional[typing.Sequence] = None,
   drop_columns: typing.Optional[typing.Sequence] = None,
) -> typing.Sequence[typing.Sequence[typing.Union[int, float]]]:
   """Creates submatrix of given matrix based on row and column indexes.
 
   Args:
       matrix (typing.Sequence[typing.Sequence[typing.Union[int, float]]]):
           input matrix
       drop_rows (typing.Optional[typing.Sequence], optional):
           rows which should be dropped. Defaults to None.
       drop_columns (typing.Optional[typing.Sequence], optional):
           columns which should be dropped. Defaults to None.
 
   Returns:
       typing.Sequence[typing.Sequence[typing.Union[int, float]]]:
           resulting matrix
   """
   new_matrix = []
 
   ...
 
   return new_matrix
{% endhighlight %}
 
### Ostatní typy
Modul `typing` obsahuje většinu typů pro pohodlné typování, jedná se například o `typing.Iterable` případně `typing.Callable` (v případě typování parametru na funkci). Doporučuji prostudovat celou nabídku.
 
## Úkoly
Nevíte si rady? Přečtěte si "[Jak pracovat s Github Classroom?](/teaching/2023/JP/classroom)".
 
### Skupina Mikula

* **L10E01**: TicTacToe [[Náhled](https://github.com/kmi-jp/template-L10E01)], [[Příjmout úkol](https://classroom.github.com/a/aPMmAXbN)]

### Skupina Petržela

* **L10E01**: TicTacToe [[Náhled](https://github.com/kmi-jp/template-L10E01)], [[Příjmout úkol]()]