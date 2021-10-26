---
layout: default
courses: PDS
title: 5. Monitor
year: 2022
---

## Monitory

#### Užitečné odkazy
* [Python `threading` — Condition](https://docs.python.org/3/library/threading.html#condition-objects)

V jazyce Python nenalezneme přímý ekvivalent monitorů. Nejblíže se funkcionalitou blíží třída `threading.Condition`. Pomoci `Condition` můžeme monitor naprogramovat, mimo zámek totiž podporuje i metody `.wait()` a `.notify()`.

U třídy `Condition` lze využít kontext manažerů, kteří se postarají o odemčení a zamčení vnitřního zámku.

{% highlight python linenos %}
# Consume one item
with cv:
    while not an_item_is_available():
        cv.wait()
    get_an_available_item()

# Produce one item
with cv:
    make_an_item_available()
    cv.notify()
{% endhighlight %}

{% include task.html content="Naprogramujte úlohu producent a konzument - synchronizace pomoci monitoru (<a href='https://classroom.github.com/a/79WjCccu'>Příjmout úkol</a>)." %}
