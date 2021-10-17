---
layout: default
courses: PDS
title: 4. Semafory
year: 2022
---

## Semafory

#### Užitečné odkazy
* [Python `threading` — Semaphore](https://docs.python.org/3/library/threading.html#semaphore-objects)

V jazyce python nalezneme semafory implementované v tříde `threading.Semaphore`.

{% highlight python linenos %}
from threading import Semaphore

# výchozí hodnota na 1
s = Semaphore()

# pro jinou hodnotu předáme číslo
s = Semaphore(5)

# podporuje dvě hlavní metody

# acquire, sníží hodnotu semaforu o jedna, je blokující
s.acquire()
# release, zvýší hodnotu semaforu o jedna, možné použít parametr n pro zvýšení o n
s.release()
{% endhighlight %}

{% include task.html content="Naprogramujte úlohu producent a konzument s omezeným bufferem - synchronizace pomoci semaforů (<a href='https://classroom.github.com/a/qVrUzJvp'>Příjmout úkol</a>)." %}

{% include task.html content="Naprogramujte úlohu čtenáři a písaři ve variantě upřednostnění čtenářů - synchronizace pomoci semaforů (<a href='https://classroom.github.com/a/jhALpG3a'>Příjmout úkol</a>)." %}