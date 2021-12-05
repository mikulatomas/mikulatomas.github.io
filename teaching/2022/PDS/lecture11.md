---
layout: default
courses: PDS
title: 11. Chain replication
year: 2022
---

## Chain replication

#### Užitečné odkazy
* [Python `distsim`](https://github.com/mikulatomas/distsim)

Naprogramujte zjednodušenou verzi Chain replication (v základní verzi nerealizujeme výpadky a dynamické přidávání uzlů). Na vstupu určíme velikost řetězu, o spuštění sítě se náhodně vybere `head` a `tail`, zbytek uzlu se přiřadí do řetězu. 

Mimo uzly v řetězu síť obsahuje uzly `client` a `master`. Uzel `client` periodicky zasílá uzlu `master` požadavky `get` a `set` pro čtení/uložené klíče. Uzel `master` tyto požadavky přepošle odpovídajícímu uzlu (`head` nebo `tail`). V rámci řetězu dále zasílejte zprávu u provedení změny (`ack`, `tail` bude první uzel který změnu klíče provede a informuje o tom všechny uzly řetězu).

{% include task.html content="Pomoci knihovny <code>distsim</code> implementujte Chain replication (<a href='https://classroom.github.com/a/6bJsPjzL'>Příjmout úkol</a>)." %}
