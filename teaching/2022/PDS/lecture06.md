---
layout: default
courses: PDS
title: 6. Bariery
year: 2022
---

## Bariery

#### Užitečné odkazy
* [Python `threading` — Bariery](https://docs.python.org/3/library/threading.html#barrier-objects)

V jazyce Python nalezneme v třídě `threading.Barrier` implementaci barier. Konstruktor přijimá jeden argument a to je počet procesů používající barieru.

{% highlight python linenos %}
b = Barrier(2, timeout=5)

def server():
    start_server()
    b.wait()
    while True:
        connection = accept_connection()
        process_server_connection(connection)

def client():
    b.wait()
    while True:
        connection = make_connection()
        process_client_connection(connection)
{% endhighlight %}

{% include task.html content="Naprogramujte úlohu hledání konce seznamu - procesy synchonizujte barierou (<a href='https://classroom.github.com/a/vicB_Xsm'>Příjmout úkol</a>)." %}
