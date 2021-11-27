---
layout: default
courses: PDS
title: 10. Spanning Tree Protocol
year: 2022
---

## Spanning Tree Protocol

#### Užitečné odkazy
* [Python `distsim`](https://github.com/mikulatomas/distsim)

Dnešním úkolem je implementovat zjednodušenou verzi Spanning Tree Protocol z přednášky (všechny cesty v síti mají stejnou rychlost).

Výsledný root uzel musí být uzel s nejmenším id (tedy 0). Vzorová topologie sítě pochází z tohoto článku: https://www.lewuathe.com/an-outline-of-spanning-tree-protocol.html.

Pro STP zprávy můžete použít vlastní `namedtuple`, který obalíte do `Message` z knihovny `distsim`.

{% include task.html content="Pomoci knihovny <code>distsim</code> implementujte Spanning Tree Protocol (<a href='https://classroom.github.com/a/_WxTyS1A'>Příjmout úkol</a>)." %}
