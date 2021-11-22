---
layout: default
courses: PDS
title: 9. Chord systém
year: 2022
---

## Chord systém

#### Užitečné odkazy
* [Python `distsim`](https://github.com/mikulatomas/distsim)

Dnešním úkolem je implementovat zjednodušenou verzi Chord systému z přednášky. Před spuštěním sítě nastavíme počet uzlů (například 8). Pomoci knihovny `distsim` vytvoříme instanci třídy `Network` se stejným počtem uzlů, kde každý uzel obsahuje `Link` do všech ostatních uzlů. Dále vytvoříme jeden uzel, který nebude součástí Chord systému (můžeme mu říkat klient).

Následně síť spustíme, každý uzel v chord systému vypočítá odpovídající "finger table", pomoci které bude probíhat směrování. Následně klient opakovaně kontaktujte libovolný uzel v chord systému s požadavkem doručení zprávy jinému náhodnému uzlu z Chord systému.

V Chord systému neřešíme přidávání a odebírání uzlů. Systém od začátku disponuje maximálním počtem uzlů.

{% include task.html content="Pomoci knihovny <code>distsim</code> implementujte chord systém (<a href='https://classroom.github.com/a/_8adP_ZB'>Příjmout úkol</a>)." %}
