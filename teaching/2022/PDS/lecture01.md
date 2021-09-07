---
layout: default
courses: PDS
title: 1. Úvod a instalace jazyka Python
year: 2022
---

## Základní informace o jazyku Python
Co o Python říká oficiální web:

{% include quote.html content="Python is a programming language that lets you work quickly and integrate systems more effectively." %}

#### Užitečné odkazy
* [Dokumentace jazyka Python3](https://docs.python.org/3/)
* [The Hitchhiker’s Guide to Python!](https://docs.python-guide.org)
* [Real Python](https://realpython.com)
* [KMI/JP Jazyk Python](https://tomasmikula.cz/teaching/2022/JP/)

## Instalace jazyka Python 3
Osobní zkušenost instalace mám pouze na Mac OS a Linux. V instalaci by vám měl pomoct následující [odkaz](https://docs.python-guide.org/starting/installation/). Pokud instalujete Python na vlastní PC, instalujte verzi [Python 3.9+](https://www.python.org/downloads/) (pokud již nějakou Python 3 verzi máte, mělo by to být dostatečné).

{% include task.html content="Instalace jazyka Python 3 na Vámi zvolenou platformu." %}

### Pip vs (Ana)Conda
Jak *pip* tak *conda* jsou balíčkovače pro instalování programů/knihoven v rámci Python ekosystému. Anaconda je zaměřená více na vědecký Python (dataminig, machine learning). Na univerzitním PC najdete Condu. Pro naše potřeby bude dostatečné zprovoznení příkazu `pip3 install ...`.

### IDE
Python je možné psát v jakémkoli textovém editoru [případně používat Python interpretr](https://docs.python.org/3/tutorial/interpreter.html). Z vlastní zkušenosti doporučuji [Vistual Studio Coded](https://code.visualstudio.com/).

{% include task.html content="Zprovoznit libobolné IDE a otestujte jednoduchý <code>hello_world.py</code> skript." %}

{% highlight python linenos %}
# hello_world.py
print("Hello world!")
{% endhighlight %}

## Základy jazyka Python
Obsah seminářů 1, 2, 3, 5 [zde](https://tomasmikula.cz/teaching/2022/JP/lecture04.html).

## Python vs. parallel programming
Je Python vhodný pro paralelní/distribuované programování?

### Global Interpreter Lock
GIL je mutex (vzájemné vyloučení), který pomáhá udržovat informaci o tom, jaké příkazy Pythonu právě vykonává. Zároveň poskytuje aktuálnímu vláknu přístup k interním funkcím Python interpretru. V čem je to problém? Pouze jedno vláknu může v jeden okamžik vyhodnocovat kód v interpretru. Multi-thread scripty jsou ve výsledku pomalejší než single-thread.


{% include quote.html content="The GIL is controversial because it prevents multithreaded CPython programs from taking full advantage of multiprocessor systems in certain situations. Note that potentially blocking or long-running operations, such as I/O, image processing, and NumPy number crunching, happen outside the GIL. Therefore it is only in multithreaded programs that spend a lot of time inside the GIL, interpreting CPython bytecode, that the GIL becomes a bottleneck." %}

### Jak vyřešit GIL problém?
Oficiální knihovna `multiprocessing` pro jazyk Python je alternativa řešící problém s GIL. Místo vytváření vláken dochází k vytváření procesů. Každý proces má vlastní GIL a proto ve výsledku nedochází k problému zmíněném výše. Více informací a příklady jsou dostupné [zde](https://charlienewey.github.io/parallel-processing-python/). Vznikají však problémy s komplikovanějším sdílením dat mezi procesy.

## Jak by to tedy správně mělo být?
**Scala** [https://www.scala-lang.org](https://www.scala-lang.org) + **Apache Spark** [http://spark.apache.org](http://spark.apache.org), **Clojure** [https://clojure.org](https://clojure.org), výpočty na GPU **CUDA** [https://developer.nvidia.com/cuda-zone](https://developer.nvidia.com/cuda-zone) (existuje i API do Python).
