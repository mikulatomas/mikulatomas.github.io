---
layout: default
courses: JP
title: 8. Práce se souborovým systémem
alert: Pozor, při práci se soubory může dojít k nechtěnému poškození uložených dat. Ověřujte jaké zdrojové kódy na svém PC spouštíte.
year: 2023
---

## 8. Práce se souborovým systémem

## Modul `pathlib`
Modul pro manipulaci s cestami a soubory. Poskytuje objektový přístup. Dostupný od verze 3.4, dříve se používala kombinace modulů `os`, `glob` a `shutil`, která s cestami pracovala jako s běžnými řetězci. Modul `pathlib` nabízí objektový přístup.

Programátoři běžně pracovali s cestami jako s řetězci a aplikovali na ně běžné metody pro řetězce. To v praxi přináší mnohé problémy (například rozdíly mezi platformami).

Jednonuchý příklad získání aktuálního pracovního adresáře:

{% highlight python linenos %}
import pathlib


# získáme instanci třídy Path reprezentující aktuální pracovní adresář
pathlib.Path.cwd()
{% endhighlight %}

Instanci třídy `Path` je rovněž možné vytvořit z klasického řetězce reprezentující cestu v souborovém systému:

{% highlight python linenos %}
import pathlib


# Windows - využití r pro ignoraci \ jako escape znaku
pathlib.Path(r"C:\Users\user\python\file.txt")

# Unix
pathlib.Path("/home/user/python/file.txt")
{% endhighlight %}

Zde je nutné si uvědomit, že se jedná o reprezentaci libovolné (i fiktivní) cesty v souborovém systému. Soubory ani složky nemusí existovat. Důležité je, že se se s cestami pracuje jako s datovou strukturou umožňující abstrakce. Formát reprezentace je automaticky vybrát na základě operačního systému.

Jedna z abstrakcí je jednoduché získání domovského adresáře uživatele:

{% highlight python linenos %}
import pathlib


# domovský adresář
home = pathlib.Path.home()

# můžeme spojit několik složek
python_scripts = home / "python" / "scripts"

# druhý způsob spojení pomoci .joinpath
python_scripts = home.joinpath("python", "scripts")
{% endhighlight %}

V případě `pathlib.Path.home()` získáme cestu *absolutní*, obsahuje tedy kompletní cestu ke složce uživatele. Je však možné pracovat i s cestou *relativní*.

{% highlight python linenos %}
import pathlib


# relativní cesta
python_scripts = pathlib.Path("scripts", "python")
# absolutní cesta
home = pathlib.Path.cwd()

# jejich spojení
pathlib.Path.joinpath(home, python_scripts)
{% endhighlight %}

V případě relativní cesty můžeme použít speciální metodu `Path.resolve()` pro získání cesty absolutní. Dosáhneme pak stejného výsledku jako výše.

{% highlight python linenos %}
import pathlib


# relativní cesta
python_scripts = pathlib.Path("scripts", "python")

python_scripts.resolve()
{% endhighlight %}

### Přístup k jednotlivým částem cesty
Třída `Path` obsahuje vlastnosti k přístupu k užitečným částem cesty. Pro jednoduchý přehled je možné použít následující ["tahák"](https://github.com/chris1610/pbpython/blob/master/extras/Pathlib-Cheatsheet.pdf).

#### Rodičovská složka/složka ve které je soubor uložen
Pomoci vlastnosti `.parent` můžeme jednoduše získat rodičovský adresář.

{% highlight python linenos %}
import pathlib


# rodičovská složka
pathlib.Path.cwd().parent

# řetězení je možné
pathlib.Path.cwd().parent.parent.parent

# složka obsahující soubor file.py
pathlib.Path.cwd().joinpath("file.py").parent
{% endhighlight %}

#### Název souboru/složky
Pomoci vlastnosti `.name` můžeme získat název souboru nebo složky.

{% highlight python linenos %}
import pathlib


# název složky
pathlib.Path.cwd().name

# název souboru
pathlib.Path.cwd().joinpath("file.py").name
{% endhighlight %}

#### Název souboru bez přípony
Pomoci vlastnosti `.stem` můžeme získat název souboru bez jeho přípony (suffixu).

{% highlight python linenos %}
import pathlib

# název souboru bez suffixu
pathlib.Path.cwd().joinpath("file.py").stem
{% endhighlight %}

#### Přípona souboru
Pomoci vlastnosti `.suffix` můžeme získat příponu souboru (suffix).

{% highlight python linenos %}
import pathlib


# název souboru bez suffixu
pathlib.Path.cwd().joinpath("file.py").suffix

# v případě vícero přípon lze použít
pathlib.Path.cwd().joinpath("file.tar.gz").suffixes
{% endhighlight %}

#### Rozdělení cesty na jednotlivé části
Objekt třídy `Path` lze jednoduše rozdělit na jednotlivé části.

{% highlight python linenos %}
import pathlib


pathlib.Path.cwd().parts
{% endhighlight %}

### Metody na testovaní existence

#### Test zda cesta existuje
Pomoci metody `exists()` můžeme jednoduše ověřit zda zadaná cesta existuje.

{% highlight python linenos %}
import pathlib


# složka
pathlib.Path.cwd().exists()

# soubor
pathlib.Path.cwd().joinpath("file.py").exists()
{% endhighlight %}

#### Test zda se jedná soubor/složku
U objektu třídy `Path` můžeme testovat zda se jedná o složku nebo soubor. Pozor pokud soubor/složka neexistuje je vráceno `False`.

{% highlight python linenos %}
import pathlib


# složka
pathlib.Path.cwd().is_dir()

# soubor
pathlib.Path.cwd().joinpath("file.py").is_file()
{% endhighlight %}

### Selekce souborů a složek na základě vzoru
Modul `pathlib` integruje funkcionalitu modulu `glob`, který umožňuje jednoduchým způsobem selektovat soubory a složky.

{% highlight python linenos %}
import pathlib


# všechny soubory v aktuálním adresáři
pathlib.Path.cwd().glob("*")

# všechny soubory s příponou .py v aktuálním adresáři
pathlib.Path.cwd().glob("*.py")

# všechny soubory s příponou .py v aktuálním adresáři začínající písmenem a
pathlib.Path.cwd().glob("a*.py")

# všechny soubory s příponou .py v aktuálním adresáři
# začínající písmenem a, končící písmenem b
pathlib.Path.cwd().glob("a*b.py")

# všechny soubory s příponou .py v libovolném podadresáři v aktuálním adresáři
pathlib.Path.cwd().glob("*/*.py")
{% endhighlight %}

## Práce se soubory
Na chvíli odbočíme od modulu `pathlib` a podíváme se na práci se soubory (čtení a zápis).

Hlavní problematikou práce se soubory (ale i paralelní programování případně síťová komunikace) je správa prostředků. Situace kdy jeden program použije sdílený prostředek a již jej neodevzdá zpět nazýváme *memory leak*.

V kontextu souborů nám jde především o důsledné dodržování uzavírání otevřených souborů (ať už po čtení nebo zápisu). Zápis do souboru je totiž provádět přes takzvaný *buffer*. Data jsou napřed zapsána do bufferu a až po volání `.close()` jsou z bufferu zapsána na disk. V opačném případě mohou být tato data ztracena.

Dalším případem může být otevření souboru a následné vyvolání vyjimky (ukončení programu, nikoli však uzavření souboru).

Následující kód **negarantuje**, že soubor bude uzavřen v případě vyjimky.

{% highlight python linenos %}
file = open("hello.txt", "w")
file.write("Hello, World!")
file.close()
{% endhighlight %}

U funkce `open()` pouze zdůrazníme, že prvním argumentem je cesta k souboru, druhým pak mód otevření souboru:

* `w` - *write* - zápis
* `r` - *read* - čtení
* `a` - *append* - rozšiřování
* `r+` - *read plus* - čtení a zápis

Běžně jsou soubory otevřeny v textovém módu -- k souborům je přistupováno tak, že obsahují text (v určitém kódování). Všechny módy však existují ve variantě `b` (binární, např `rb`).

Situaci můžeme ošetři již známým příkazem `try ... finally`.

{% highlight python linenos %}
# bezpečné otevření souboru - pokud zde nastane chyba soubor se ani neotevře
file = open("hello.txt", "w")

try:
    file.write("Hello, World!")
finally:
    # bezpečné zavření souboru v případě chyby u zápisu
    file.close()
{% endhighlight %}

Nejjednodším způsobem je však použítí příkazu `with`, který zabezpečuje bezpečné odbavení kontextu (například otevření a zavření souboru, připojení na server a další).

{% highlight python linenos %}
with open("hello.txt", "w") as file:
    file.write("Hello, World!")
{% endhighlight %}

Příkaz `with` se postará o volání speciálních dunder metod `__enter__()` a `__exit__()`. V případě souborů se tedy jedná o otevření a zavření souboru.

Příkaz `with` lze použít i v komplikovanější podobě.

{% highlight python linenos %}
with open("input.txt") as in_file, open("output.txt", "w") as out_file:
    pass
{% endhighlight %}

### Objekt souboru
Po otevření souboru funkci `open()` získáme objekt reprezentující otevřený soubor. K dispozici máme následující metody.

{% highlight python linenos %}
with open("hello.txt", "r") as file:
    # přečtení jednoho řádku
    file.readline()

with open("hello.txt", "r") as file:
    # získání seznamu všech řádků
    file.readlines()

with open("hello.txt", "w") as file:
    # zápis řetězce
    file.write("test")

with open("hello.txt", "w") as file:
    # zápis seznamu řetězců jako jednotlivých řádků
    file.writelines(["test\n", "test2\n"])
{% endhighlight %}

Objekt souboru poskytuje iterátor proto je možné následující.

{% highlight python linenos %}
with open("hello.txt", "r") as file:
    for line in file:
        print(line)

with open("hello.txt", "r") as file:
    while file:
        print(file.readline())
{% endhighlight %}

### Použití `pathlib`
V rámci modulu `pathlib` můžeme se soubory jednoduše pracovat. K dispozici jsou čtyři vysokoůrovňové metody.

{% highlight python linenos %}
import pathlib


path = pathlib.Path.cwd() / "test.md"

# načtení textu
path.read_text()

# načtení bajtů
path.read_bytes()

# zápis textu
path.write_bytes("Test")

# zápis bajtů
path.write_bytes(b"Test")
{% endhighlight %}

Pro větši kontrolu je však možné použít příkaz `with` a metodu `.open()`.

{% highlight python linenos %}
import pathlib


path = pathlib.Path.cwd() / "test.md"

with path.open(mode="r") as file:
    # přečteme jeden řádek, můžeme použít vše jako u objektu souboru
    file.readline()
{% endhighlight %}

## Formát souboru `csv`
S formátem `csv` (comma separated value) jsme se setkali již několikrát. V jazyce Python je k dispozici modul `csv` umožňující jednoduchou práci s tímto formátem (čtení/zápis).

{% highlight python linenos %}
import pathlib
import csv


# vytvoření a zápis
path = pathlib.Path("data.csv")

data = [[str(value) for value in range(5)] for _ in range(10)]

with path.open(mode="w") as file:
    # volitelným parametrem separator nastavujeme oddělovač
    # čárka je výchozí hodnota
    csv_writer = csv.writer(file, delimiter=",")
    
    for row in data:
        csv_writer.writerow(row)

# čtení
path = pathlib.Path("data.csv")

with path.open(mode="r") as file:
    csv_reader = csv.reader(file)
    
    input_data = [row for row in csv_reader]

assert data == input_data
{% endhighlight %}

## Formát souboru `json`
Formát [JSON (JavaScript Object Notation)](https://www.json.org/json-en.html) je inspirován JavaScript syntaxí pro zápis objektů. V jazyce Python je nejblíže slovníku (případně zanořenému slovníku).

{% highlight json linenos %}
{
   "glossary":{
      "title":"example glossary",
      "GlossDiv":{
         "title":"S",
         "GlossList":{
            "GlossEntry":{
               "ID":"SGML",
               "SortAs":"SGML",
               "GlossTerm":"Standard Generalized Markup Language",
               "Acronym":"SGML",
               "Abbrev":"ISO 8879:1986",
               "GlossDef":{
                  "para":"A meta-markup language, used to create markup languages such as DocBook.",
                  "GlossSeeAlso":[
                     "GML",
                     "XML"
                  ]
               },
               "GlossSee":"markup"
            }
         }
      }
   }
}
{% endhighlight %}

Modul `json` jazyka Python můžeme použít pro jednoduché načítání a ukládání dat ve formatu JSON.

{% highlight python linenos %}
import pathlib
import json


# vytvoření a zápis
path = pathlib.Path("data.json")

data = {
    "username": "pepa",
    "name": {
        "firstname": "Josef",
        "lastname": "Novak"
        },
    "titles": ["Mgr.", "Bc."],
    "salary": "30000"
}

with path.open(mode="w") as file:
    file.write(json.dumps(data))

# čtení
path = pathlib.Path("data.json")

with path.open(mode="r") as file:
    input_data = json.loads(file.read())

assert data == input_data
{% endhighlight %}

## Úkoly
Nevíte si rady? Přečtěte si "[Jak pracovat s Github Classroom?](/teaching/2023/JP/classroom)".

*Při řešení úloh nepoužívejte pokročilejší funkcionalitu jazyka která nebyla ještě představena! Takové úkoly budou vráceny na přepracování bez ohledu na jejich funkčnost.*

### Skupina Mikula

* **L08E01**: CSV sum script [[Náhled](https://github.com/kmi-jp/template-L08E01)], [[Příjmout úkol](https://classroom.github.com/a/3d_4XLTx)]
* **L08E02**: Data 4 [[Náhled](https://github.com/kmi-jp/template-L08E02)], [[Příjmout úkol](https://classroom.github.com/a/4zapDLJx)]
* **L08E03**: Exercise points analysis [[Náhled](https://github.com/kmi-jp/template-L08E03)], [[Příjmout úkol](https://classroom.github.com/a/lI71dQfU)]

### Skupina Petržela

* **L08E01**: CSV sum script [[Náhled](https://github.com/kmi-jp/template-L08E01)], [[Příjmout úkol](https://classroom.github.com/a/pm-bqFCB)]
* **L08E02**: Data 4 [[Náhled](https://github.com/kmi-jp/template-L08E02)], [[Příjmout úkol](https://classroom.github.com/a/Q2m3SgES)]
* **L08E03**: Exercise points analysis [[Náhled](https://github.com/kmi-jp/template-L08E03)], [[Příjmout úkol](https://classroom.github.com/a/yoqFAKKu)]
