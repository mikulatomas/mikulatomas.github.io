---
layout: default
courses: JP
title: 11. Ostatní moduly standardní knihovny
year: 2022
---
## 11. Ostatní moduly standardní knihovny
## Práce s časem - modul `datetime`
Modul pro práci s časem a datumem. Příklady vycházejí z podrobnějšího článku [zde](https://realpython.com/python-datetime/). Následující příklad ukazuje jak vytvořit instance ručně.
 
{% highlight python linenos %}
from datetime import date, time, datetime
 
date(year=2020, month=1, day=31)
 
time(hour=13, minute=14, second=31)
 
datetime(year=2020, month=1, day=31, hour=13, minute=14, second=31)
{% endhighlight %}
 
Můžeme však použít i užitečné metody pro jejich vytvoření.
 
{% highlight python linenos %}
from datetime import date, time, datetime
 
date.today()
datetime.now()
{% endhighlight %}
 
Datum můžeme vytvořit rovněž pomoci ISO 8601 formátu.
 
{% highlight python linenos %}
from datetime import date
 
date.fromisoformat("2020-01-31")
{% endhighlight %}
 
V případě, že nemáme údaje v žádném ISO formátu můžeme použít formátovací jazyk na parsování údajů z řetězce. Jeho celý popis naleznete [zde](https://docs.python.org/3/library/datetime.html#strftime-and-strptime-behavior).
 
{% highlight python linenos %}
from datetime import datetime
 
date_string = "01-31-2020 14:45:37"
format_string = "%m-%d-%Y %H:%M:%S"
 
datetime.strptime(date_string, format_string)
{% endhighlight %}
 
### Rozdíl času
V případě, že máme dvě instance třídy `datetime`, můžeme provádět jejich rozdíl. Výsledkem je instance `datetime.timedelta`.
 
{% highlight python linenos %}
from datetime import datetime
 
# čas do Nového roku
datetime(year=2022, month=1, day=1) - datetime.now()
{% endhighlight %}
 
Případně můžeme vytvořit `datetime.timedelta` ručně a provádět aritmetické operace.
 
{% highlight python linenos %}
from datetime import datetime, timedelta
 
# vrátí datetime posunutý o jeden den dopředu
datetime.now() + timedelta(days=1)
 
# vrátí datetime posunutý o jeden den dozadu
datetime.now() - timedelta(days=1)
# ekvivalentní
datetime.now() + timedelta(days=-1)
{% endhighlight %}
 
### Měření doby běhu
V praxi často chceme změřit dobu běhu našeho programu/funkce. V modulu `time` nalezneme `perf_counter` což je nejpřesnější způsob měření času. Více informací naleznete [zde](https://realpython.com/python-timer/).
 
{% highlight python linenos %}
from time import perf_counter
 
start = perf_counter()
 
for _ in range(10000):
  2 ** 10
 
perf_counter() - start
{% endhighlight %}
 
## Práce s "náhodou" - modul `random`
Na začátku je nutné upozornit na fakt, že většina náhodně generovaných dat nejsou náhodná ve vědeckém slova smyslu, jedná se o *pseudonáhodná* data.
 
Náhodná čísla je možné generovat pomoci *true random number generator (TRNG)*, tím je například opakovaný hod hrací kostkou. Za předpokladu že nemáme "cinklou" kostku nedokážeme odhadnout jaké číslo na hrací kostce padne.
 
Pseudonáhodná čísla je narozdíl od náhodných čísel možné generovat za pomoci softwaru. Proces generování začne náhodným číslem (*seed*) a následně použitím algoritmu, který na základě seedu vygeneruje pseudonáhodné číslo.
 
Dále je nutné upozornit, že pseudonáhodné generátory z modulu `random` není vhodné používat pro bezpečnostní aplikace.
 
{% highlight python linenos %}
import random
 
# náhodné číslo z rozsahu [0.0, 1.0]
random.random()
# 0.6974581966212106
random.random()
# 0.3773080227020298
{% endhighlight %}
 
Generovaná čísla vypadají náhodně, problém však nastane v následující demonstraci.
 
{% highlight python linenos %}
import random
 
random.seed(123)
# náhodné číslo z rozsahu [0.0, 1.0]
random.random()
# 0.052363598850944326
 
random.seed(123)
random.random()
# 0.052363598850944326
{% endhighlight %}
 
Nastavením stejného seedu jsme schopni zopakovat stejné "náhodné" číslo! Celý popis problematiky a mnoho dalšího naleznete [zde](https://realpython.com/python-random/).
 
Dále se podívejme jak generovat náhodný `int` z rozsahu.
 
{% highlight python linenos %}
import random
 
# náhodný integer z intervalu [0, 10]
random.randint(0, 10)
# náhodný integer z intervalu [0, 10)
random.randrange(0, 10)
{% endhighlight %}
 
Rovněž je možné generovat náhodné `float` z rozsahu.
 
{% highlight python linenos %}
import random
 
# náhodný float z intervalu [0, 10]
random.uniform(0, 10)
{% endhighlight %}
 
## Použití `random` u sekvencí
Výběr náhodného prvku.
 
{% highlight python linenos %}
import random
 
values = [2, 5, 1, 6, 7]
 
# náhodná hodnota ze seznamu values
random.choice(values)
 
# náhodné 3 hodnoty (s opakováním) ze seznamu values
random.choice(values, k=3)
 
# náhodný podseznam délky 3 (bez opakování) ze seznamu values
random.sample(items, 3)
{% endhighlight %}
 
Náhodné zamíchání sekvence.
 
{% highlight python linenos %}
import random
 
values = [2, 5, 1, 6, 7]
 
# náhodné zamíchání, pozor je inplace
random.shuffle(values)
 
values
{% endhighlight %}
 
## Pokročilejší matematika - modul `math`
Modul `math` obsahuje množství užitečných high-level funkcí pro matematické operace. Obsáhlejší článek je možné najít [zde](https://realpython.com/python-math-module/).
 
### Konstanty
Modul `math` obsahuje několik matematicky významných konstant.
 
 
{% highlight python linenos %}
import math
 
# Pi
math.pi
# Tau - 2*pi
math.tau
# Eulerovo číslo
math.e
# Nekonečno
math.inf
# Not a Number
math.nan
{% endhighlight %}
 
### Užitečné aritmetické funkce
 
{% highlight python linenos %}
import math
 
# faktoriál 7!
math.factorial(7)
 
# nejbližší celé číslo, které je větší nebo rovno zadanému číslu
math.ceil(4.2)
# 5
math.ceil(-4.2)
# -4
 
# nejbližší celé číslo, které je menší nebo rovno zadanému číslu
math.floor(4.2)
# 4
math.floor(-4.2)
# -5
 
# odebrání desetíné části čísla
math.trunc(4.2)
# 4
math.trunc(-4.2)
# -4
assert math.trunc(12.32) == math.floor(12.32)
assert math.trunc(-43.24) == math.ceil(-43.24)
{% endhighlight %}
 
Dále můžeme zjistit zda jsou dvě čísla blízko (se zadanou přesností).
 
{% highlight python linenos %}
import math
 
# abs(a-b) <= max(rel_tol * max(abs(a), abs(b)), abs_tol).
math.isclose(6, 7, rel_tol=0.2)
 
math.isclose(6, 7, rel_tol=0.1)
{% endhighlight %}
 
### Výpočet mocniny
 
{% highlight python linenos %}
import math
 
# na první pohled zbytečné ale..
math.pow(2, 3) == 2 ** 3
 
import timeit
 
timeit.timeit("math.pow(10, 300)", setup="import math")
# 0.1181572710047476
 
timeit.timeit("10 ** 300")
# 0.8508502449840307
{% endhighlight %}
 
### Druhá odmocnina
 
{% highlight python linenos %}
import math
 
math.sqrt(9)
{% endhighlight %}
 
### Výpočet logaritmu
 
{% highlight python linenos %}
import math
 
# přirozený, tedy ln
math.log(4)
 
# o základu 2
math.log(3, 2)
 
# existují zkrácené verze
math.log2(3)
math.log10(3)
{% endhighlight %}
 
### Největší společný dělitel
 
{% highlight python linenos %}
import math
 
math.gcd(15, 25)
{% endhighlight %}
 
### Suma čísel v iterable (přesněji)
Doteď jsme používali `sum()` pro součet čísel v iterable, existuje však přesnější verze.
 
{% highlight python linenos %}
import math
 
sum([.1, .1, .1, .1, .1, .1, .1, .1, .1, .1])
# 0.9999999999999999
 
math.fsum([.1, .1, .1, .1, .1, .1, .1, .1, .1, .1])
# 1.0
{% endhighlight %}
 
### Trigonometrické funkce
 
{% highlight python linenos %}
import math
 
math.sin()
math.cos()
math.tan()
 
# Inverzni funkce
math.asin()
math.acos()
math.atan()
 
# výpočet přepony - hypotenuse
math.hypot()
{% endhighlight %}
 
## Základní statistika - modul `statistics`
Modul `statistics` obsahuje základní funkce pro statistiku. Více informací o modulu naleznete [zde](https://docs.python.org/3/library/statistics.html).
 
{% highlight python linenos %}
import statistics
 
# aritmetický průměr
statistics.mean()
# aritmetický průměr - fast, floating point
statistics.fmean()
# medián
statistics.meadian()
# kvantily
statistics.quantiles()
 
# rozptyl (variance)
statistics.variance()
# směrodatná odchylka
statistics.stdev()
# covariance
statistics.covariance()
{% endhighlight %}
 
## Práce se zlomky - modul `fractions`
Pro práci se zlomky (pro zachování přesnosti) můžeme použít modul `fractions`, více informací nalezneme [zde](https://docs.python.org/3/library/fractions.html).
{% highlight python linenos %}
import fractions
 
fractions.Fraction(16, -10)
# Fraction(-8, 5)
fractions.Fraction(123)
# Fraction(123, 1)
fractions.Fraction()
# Fraction(0, 1)
fractions.Fraction('3/7')
# Fraction(3, 7)
fractions.Fraction(' -3/7 ')
# Fraction(-3, 7)
fractions.Fraction('-.125')
# Fraction(-1, 8)
fractions.Fraction('7e-6')
# Fraction(7, 1000000)
fractions.Fraction(2.25)
# Fraction(9, 4)
fractions.Fraction(1.1)
# Fraction(2476979795053773, 2251799813685248)
 
fractions.Fraction(16, -10).numerator
# -8
fractions.Fraction(16, -10).denominator
# 5
{% endhighlight %}
 
## Práce s URL - balíček `urllib`
Python obsahuje balíček `urllib`, existuje však lepší alternativa, balíček `requests` (Requests: HTTP for Humans). Ukážeme si jeho základní použití.
 
{% highlight python linenos %}
import requests
 
r = requests.get("https://portal.upol.cz")
r
# <Response [200]>
 
r.status_code
# 200
 
r.text
# '\r\n<!DOCTYPE html>\r\n<html  lang="cs">\r\n<head>\r\n...
 
r.encoding
# 'utf-8'
 
r.history
# []
 
r.url
# 'https://portal.upol.cz/'
 
r.headers
# {
#    'Cache-Control': 'no-cache,no-store',
#    'Pragma': 'no-cache',
#    'Transfer-Encoding': 'chunked',
#    'Content-Type': 'text/html; charset=utf-8',
#    'Expires': '-1',
#    'Server': 'Microsoft-IIS/8.5',
#    'Strict-Transport-Security': 'max-age=2592000',
#    'Set-Cookie': '.AspNetCore.Session=CfDJ8MpsEsD%2FF1VIgp%2Fgwefz4M4D015SL03szWA6MVPQtW9qZKTKLS8%2FoOKftWDZ7qCmBfokp53BXMAUgsxqgyMhkveV2mfa6DadtTFnak1vbXkPugxW%2Fma%2BgdXsP6JjwZdzgc2IC9G9uDZ6pDTrNqusDL0hF4bRy8xiw93luh8%2FXiEz; path=/; samesite=lax; httponly',
#    'X-Powered-By': 'ASP.NET',
#    'Date': 'Wed, 01 Dec 2021 12:08:35 GMT'
 
r.headers['Content-Type']
# 'text/html; charset=utf-8'
}
{% endhighlight %}

