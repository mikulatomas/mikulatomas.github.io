---
layout: default
courses: PDS
title: 2. Práce s vlákny
year: 2022
---

## Základní práce s vlákny

#### Užitečné odkazy
* [Python `threading` — Thread-based parallelism](https://docs.python.org/3/library/threading.html)

### Objekt `Thread`
`Thread` třída reprezentuje aktivitu běžící v separátním vlákně. Jsou zde dvě možnosti jak objektu `Thread` specifikovat aktivitu: předání `callable object` do konstruktoru třídy `Thread`, nebo přepsání metody `run()`. Pro jednoduchost budeme používat první možnost.

{% highlight python linenos %}
import threading

def example_activity():
    print("Hello World")

example_thread = threading.Thread(target=example_activity)

example_thread.start()
{% endhighlight %}


Při vytváření objektu `Thread` je rovněž možné předat aktivitě argumenty.

{% highlight python linenos %}
import threading

def example_activity_with_args(number):
    print(f"Your number is {number}")

example_thread = threading.Thread(target=example_activity_with_args, args=(10,))

example_thread.start()
{% endhighlight %}

### Modifikace globální proměnné v rámci vlákna
Pokud potřebujeme modifikovat proměnnou v rámci vlákna musíme použít klíčové slovo `global`.

{% highlight python linenos %}
import threading

text = 'ahoj '

def thread_function():
    global text

    text += 'svete'

thread = threading.Thread(target=thread_function)

thread.start()
thread.join()

print(text)
{% endhighlight %}

### Problém sdíleného čítače
Mějme dvě vlákna, která postupně inkrementují sdílený čítač. Každé vlákno má čítač postupně zvýšovat o 10 (po jednom). Očekávaná výsledná hodnota je 20.

Aby bylo možné jednoduše pozorovat problém, provádějte změnu čítače následujícím způsobem:

{% highlight python linenos %}
import time

# ...
local_counter = counter
time.sleep(0.0001)
local_counter += 1
time.sleep(0.0001)
counter = local_counter
# ...
{% endhighlight %}

Správnost čítače lze testovat pomoci <code>assert</code>.
{% highlight python linenos %}
assert counter == 20
{% endhighlight %}

{% include task.html content="Vytvořte dvě vlákna, která se uspí (<a href='https://realpython.com/python-sleep/'>jak uspat Python</a>) na náhodně dlouhou dobu (v rozsahu několika sekund) a vypíší výstup na obrazovku." %}

{% include task.html content="Naprogramujte úlohu sdíleného čítače a vyzkoušejte zda opravdu nefunguje." %}

{% include task.html content="Naprogramujte úlohu sdíleného čítače, kde je možné specifikovat počet vláken inkrementující sdílený čítač (<a href='https://classroom.github.com/a/ms-b8OBg'>Příjmout úkol</a>)." %}