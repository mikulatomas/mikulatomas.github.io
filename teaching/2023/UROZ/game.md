---
layout: default
courses: UROZ
title: Herní UI pro jednoduchou FPS hru
---

## {{ page.title }}

Cílem je vytvořit jednoduché a přehledné UI pro hru demonstrovanou na videu níže.

<iframe width="560" height="315" src="https://www.youtube.com/embed/LPc5xf9ltLc" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

### Požadovaná funkcionalita
* cílem je najít a sestřelit všechny červené kostky na mapě (je jich 16)
* hráč má 5 nábojů, potom musí zbraň přebít (trvá 3s)
* čas před nalezením všech kostek se počítá a hráči zobrazuje
* hráči je zdůrazněno pokud dojde k zasažení kostky
* hráč může používat takzvaný "dash" (3x, dash se průběžně obnovuje)
* sekundárním výstřelem může hráč vystřelit speciální projektil pro teleportaci, ten zminí do tří vteřin pokud hráč nezmáčkne akci znovu, v takovém případě dojde k teleportaci na místo dopadu. Po úspěšném teleportu je tato funkce nepoužitelná po dobu 3 sekund
* po nalezení všech kostek se hráči zobrazí statistika a uloží se do žebříčku



