---
layout: default
courses: JP
title: 5. Objektově orientované programování (OOP) (WIP)
year: 2022
---

## 5. Objektově orientované programování (OOP)
Programovací paradigma, které zapouzdřuje vlastnosti a funkcionalitu do individuálních objektů. Fakticky jsme se s tímto paradigmatem potýkali celou dobou.

Objekt reprezentující řetězec `"ahoj svete"` obsahuje rovněž funkcionalitu `"ahoj svete".split()`, která vytvoří seznam obsahující jednotlivé řetězce, které odpovídají řětězcům vzniklým rozdělěním původního řětězce mezerami.

## Třídy
Vytváření uživatelsky definovaných tříd (následně pak objektů) budeme v tuto chvíli chápat hlavně jako tvorbu vlastních datových struktur s navázanou funkcionalitou.

Sahat po vytváření vlastní datové struktury bychom měli pouze v případě, že vestavěné (primitivní) datové struktury nejsou dostatečné. Zkusme realizovat účet s kredity pomoci vestavěné datové struktury `dict`.

Více informací [zde](https://docs.python.org/3/tutorial/classes.html) a [zde](https://realpython.com/python3-object-oriented-programming/).

{% highlight python linenos %}
credit_account_1 = {"owner": "Lukas Novak", "balance":100000}
credit_account_2 = {"owner": "Pepa Novak", "balance":10000}

# součet kreditů na dvou účtech
credit_account_1["balance"] +  credit_account_2["balance"]

# odečet hodnoty od kreditů
credit_account_1["balance"] - 100
{% endhighlight %}

*Třída* (klíčové slovo `Class`) tedy slouží k vytváření uživatelsky definovaných datových struktur. Určují tedy jak má výsledná datová struktura vypadat a fungovat. Na základě třídy (předpis) můžeme vytvářet jednotlivé instance třídy (objekty).

Jako příklad si můžeme představit již dobře známý seznam. Jeden konkrétní seznam je instancí (objektem) třídy seznam, která popisuje jak seznamy vypadají a jak se s nimi dá pracovat. Třídy jsou tedy obecným předpisem, objekty pak konkrétní entity vytvořené na základě tohoto předpisu.

Třídy je dobré umisťovat do modulů stejného názvu, pokud některé třídy dává smysl umístit společně do stejného modulu je to přijatelná alternativa.

V následujícím souboru `credit_account.py` (modul `credit_account`) si definujeme základní prázdnou třídu `CreditAccount`:

{% highlight python linenos %}
# definice prázdné třídy
class CreditAccount:
    pass
{% endhighlight %}

Následně nově vytvořenou třídu otestujeme:

{% highlight python linenos %}
from credit_account import CreditAccount


# dle předpisu třídy je následně možné vytvářet objekty
credit_account_1 = CreditAccount()
credit_account_2 = CreditAccount()

# ověření typu
type(credit_account_1)
{% endhighlight %}

<div class="pep">
{% highlight python linenos %}
# PEP8 - název třídy používá CapWords konvenci
# správně
class CreditAccount:
    pass

# špatně
class credit_account:
    pass
{% endhighlight %}
</div>

### Metody a vlastnosti

Vytvoření prázdné třídy neni moc praktické, pojdme definici rozšířit. Každá třída může obsahovat sadu funkcí, které jsou s třídou úzce spjaty. Těmto funkcím říkáme *metody*. Již několikrát jsme používali metodu `.split()` třídy řetězce, která umí daný řetězec rozdělit na seznam řetězců.

Všimněme si, že třída a její metody používají podobnou konvenci docstringů, budeme je tedy používat (a vyžadovat) i zde.

Nejdůležitější metodou každé třídy je *konstruktor* `.__init__()` , zatím se nemusíme trápit z jakého důvodu název obsahuje podtržítka (to si vysvětlíme další seminář). Konstruktor nastaví počáteční stav (initial state - proto název init) nově vytvořeného objektu.

Metoda `.__init__()` může obsahovat libovolný počet parametrů (podobně jako funkce), prvním parametrem však vždy musí být parametr `self`. Když je instance třídy vytvořena, je automaticky předána jako první parametr `self` metodě `.__init__()`. To je důležité pro nastavení počátečního stavu objektu (potřebujeme přístup k nově vytvořenému objektu aby jsme mohli počáteční stav nastavit).

{% highlight python linenos %}
class CreditAccount:
    """Account with stored credits."""

    # metoda __init__ je volána při vzniku objektu třídy CreditAccount, 
    # obsahuje definici vlastností objektu a kód potřebný pro vytvoření instance
    def __init__(self, owner, initial_credits=0):
        """Creates credit account with given owner and initial credits.

        Args:
            owner: owner of the account
            initial_credits (optional): credit balance. Defaults to 0.
        """

        self.owner = owner
        self.balance = initial_credits
{% endhighlight %}


{% highlight python linenos %}
from credit_account import CreditAccount


credit_account_1 = CreditAccount("Lukas Novak")
credit_account_2 = CreditAccount("Pepa Novak", initial_credits=200)

credit_account_1.owner
credit_account_1.balance

credit_account_1.owner = "Jaroslav Novak"
credit_account_1.balance += 300

assert credit_account_1.balance == 300
{% endhighlight %}

Vytvořili jsme tedy třídu `CreditAccount`, která při vytvoření nové instance nastaví dvě vlastnosti `.owner` a `.balance` na hodnoty `owner` a `initial_credits`. Každá instance třídy `CreditAccount` bude těmito vlastnosti disponovat.

Další metodou, kterou můžeme naši třídě `CreditAccount` přidat je metoda `.transfer_to(self, other, value)`.

{% highlight python linenos %}
class CreditAccount:
    """Account with stored credits."""

    def __init__(self, owner, initial_credits=0):
        """Creates credit account with given owner and initial credits.

        Args:
            owner: owner of the account
            initial_credits (optional): credit balance. Defaults to 0.
        """

        self.owner = owner
        self.balance = initial_credits

    def transfer_to(self, other, value):
        """Transfer money into another account. Negative balance is allowed.

        Args:
            other: Target of money transfer.
            value: Amount of money to be transfered.
        """

        self.balance -= value
        other.balance += value
{% endhighlight %}

{% highlight python linenos %}
from credit_account import CreditAccount


credit_account_1 = CreditAccount("Lukas Novak")
credit_account_2 = CreditAccount("Pepa Novak", initial_credits=200)

credit_account_1.transfer_to(credit_account_2, 200)

assert credit_account_1.balance == -200
assert credit_account_2.balance == 400
{% endhighlight %}

Asi nás nepřekvapí, že názvy metod podléhají doporučení PEP8.

<div class="pep">
{% highlight python linenos %}
# PEP8 - názvy metod a vlastností stejně jako u funkcí a proměnných
# správně
class TestClass:
    def reverse_order(self):
        pass

# špatně
class TestClass:
    def reverseOrder(self):
        pass
{% endhighlight %}
</div>

### Vlastnosti třídy
Zatím jsme si ukázali, že vlastnosti jsou specifické pro jednotlivé objekty, pokud zmeníme vlastnost u jednoho objektu, nezmení se u druhého. Vlastnosti definované jako vlastnosti třídy pak můžeme využívat napříč všemi instancemi dané třídy.

{% highlight python linenos %}
class CreditAccount:
    """Account with stored credits."""

    max_balance = 1000

    def __init__(self, owner, initial_credits=0):
        """Creates credit account with given owner and initial credits.

        Args:
            owner: owner of the account
            initial_credits (optional): credit balance. Defaults to 0.
        """

        self.owner = owner
        self.balance = initial_credits
{% endhighlight %}

{% highlight python linenos %}
from credit_account import CreditAccount


credit_account_1 = CreditAccount("Lukas Novak")
credit_account_2 = CreditAccount("Pepa Novak", initial_credits=200)

assert credit_account_1.max_balance == 1000
assert credit_account_2.max_balance == 1000
assert CreditAccount.max_balance == 1000

CreditAccount.max_balance = 10

assert credit_account_1.max_balance == 10
assert credit_account_2.max_balance == 10
assert CreditAccount.max_balance == 10
{% endhighlight %}

Vlastnosti třídy používejte k definovaní vlastností, které mají mít stejnou hodnoty pro všechny instance třídy. Vlastnosti instancí (objektů) používejte, pokud se mají objekt od objektu lišit.

### Přístup k vlastnostem objektu
Narozdíl od jiných programovacích jazyků, jazyk Python přistupuje k vlastnostem objektu přímo (pomoci operátoru tečky). Programátor může modifikovat a číst libovolnou vlastnost/metodu objektu.

**Není** tedy třeba vytvářet přístupové metody (takzvané gettery a settery) jako v jiných jazycích. Na příštím semináři se k této problematice ještě vrátíme.

Pozor, s tímto faktem přichází velká zodpovědnost, programátor si musí sám uvědomit, jaké zásahy do objektů jsou validní a jaké mohou vést k problémům.

Existuje však způsob, kterým lze komunikovat, že uživatel přistupuje k vlastnosti/metodě, která není zamýšlena jako veřejná (je používaná například pouze interně v rámci objektu).

<div class="pep">
{% highlight python linenos %}
# PEP8 - metody/vlastnosti, které nejsou zamýšlené jako veřejné 
# pojmenujeme s prefixem podtržítka
class TestClass:
    def __init__(self):
        self._private_data = []

    # většinou se jená o pomocné metody
    def _reverse_order(self):
        pass

    # použité v jiné metodě téže třídy
    def reverse(self):
        return self._reverse_order()
{% endhighlight %}
</div>

## Dědičnost
Koncept dědičnosti je jeden z hlavních konceptů objektově orientovaného programování. Ve zkratce si nyní ukážeme, jak může jedna třída dědit funkcionalitu od jiné.

Vraťme se k předchozímu příkladu. Představme si, že mimo `CreditAccount` můžeme mít například i `BankAccount`. Asi si umíme představit, že některá funkcionalita může být sdílená. Z toho důvodu se nabízí realizovat obecnou třídu `Account` od které pak bude dědit třída `CreditAccount`.

{% highlight python linenos %}
class Account:
    """Represents an account."""

    def __init__(self, owner, initial_balance=0):
        """Creates account with given owner and initial balance.

        Args:
            owner: owner of the account
            initial_balance (optional): initial balance. Defaults to 0.
        """

        self.owner = owner
        self.balance = initial_balance
    
    def transfer_to(self, other, value):
        """Transfer money into another account. Negative balance is allowed.

        Args:
            other: Target of money transfer.
            value: Amount of money to be transfered.
        """

        self.balance -= value
        other.balance += value
{% endhighlight %}

U definice třídy `CreditAccount` je nutné zdůraznit první řádek definice, do kulatých závorek uvádíme třídy z kterých má třída `CreditAccount` dědit.

V konstruktoru třídy `CreditAccount` si všimněme použití funkce `super()`. Na technické úrovni se jedná o poměrně složitou věc, nám bude stačit vysvětlení, které říká, že funkce `super()` umožňuje přístup k metodám z tříd od kterých dědíme.

{% highlight python linenos %}
from datetime import datetime

from account import Account


class CreditAccount(Account):
    """Represents a credit account."""

    def __init__(self, owner, initial_balance=0):
        # vyvolání konstruktoru předka
        super().__init__(owner, initial_balance=initial_balance)

        # dodané vlastnosti
        self.expiration = datetime.now() + datetime.timedelta(days=365)
    
    # dodaná funkcionalita
    def expires_soon(self):
        return datetime.now() + datetime.timedelta(days=30) >= self.expiration
{% endhighlight %}

### Mixins
Používání takzvaných *mixinů* je populární způsob využití vícenásobné dědičnosti. Již jsme zmínili, že do závorek na prvním řádku definice třídy je možné uvádět několik tříd z kterých bude třída dědit funkcionalitu.

Mějme tedy situaci, kdy definujeme třídu `Account`.

{% highlight python linenos %}
class Account:
    """Represents an account."""

    def __init__(self, owner, initial_balance=0, creation_datetime=datetime.now()):
        """Creates account with given owner and initial balance.

        Args:
            owner: owner of the account
            initial_balance (optional): initial balance. Defaults to 0.
            creation_datetime (optional): creation datetime. Defaults to datetime.now().
        """

        self.owner = owner
        self.balance = initial_balance
        self.creation_datetime = creation_datetime
    
    def transfer_to(self, other, value):
        """Transfer money into another account. Negative balance is allowed.

        Args:
            other: Target of money transfer.
            value: Amount of money to be transfered.
        """

        self.balance -= value
        other.balance += value
{% endhighlight %}

Dále definujeme `ExpirableMixin`, který dodává funkcionalitu "vypršení platnosti".

{% highlight python linenos %}
class ExpirableMixin:

    def expires_soon(self, length=timedelta(days=365)):
        """Check if it expires soon or not.

        Args:
            length (timedelta, optional): How long is not expired. Defaults to timedelta(days=365).

        Returns:
            bool: True if expires within 30 days.
        """

        return datetime.now() + datetime.timedelta(days=30) >= self.creation_datetime + length
{% endhighlight %}

Jednoduše pak můžeme vytvořit třídu `CreditAccount` se stejnou funkcionalitou.

{% highlight python linenos %}
from account import Account, ExpirableMixin


class CreditAccount(Account, ExpirableMixin):
    """Represents a credit account."""

    pass
{% endhighlight %}

Hlavní síla mixinů je v jejich modulárnosti a přehlednosti. V kontextu vícenásobné dědičnosti se v ostatních předmětech setkáte s *diamantovým problémem*.

V jazyce Python se priority v dědičnosti určují pořadím tříd uvedených v závorkách. Pokud by tedy nastal problém, že obě třídy od kterých naše třída dědí implementují metodu se stejným názvem, bude použita ta metoda jejíž třída je uvedena jako první.

## `NamedTuple`
Ne vždy je potřeba vytvářet celou třídu pro reprezentaci jednoduchých strukturovaných dat. Jestliže potřebujeme funkcionalitu `tuple` s pojmenovaním jednotlivých uložených hodnot, můžeme použít `collections.NamedTuple`.

{% highlight python linenos %}
from collections import namedtuple


Person = namedtuple("Person", ["name", "phone", "email"])

owner = Person("Lukas Novak", "723812052", "novak@gmail.com")

owner.name
owner.phone
owner.email

# funguje jako klasický tuple
assert owner.name == owner[0]

# nelze, je to tuple
owner.name = "Pepa Novak"
{% endhighlight %}

<!-- ## Úkoly
Nevíte si rady? Přečtěte si "[Jak pracovat s Github Classroom?](/teaching/2022/JP/classroom)".

* **L01E01**: Hello world [[Náhled](https://github.com/kmi-jp/template-L01E01)], [[Příjmout úkol](https://classroom.github.com/a/BLVFlAR8)]
* **L01E02**: Integer input [[Náhled](https://github.com/kmi-jp/template-L01E02)], [[Příjmout úkol]()]
* **L01E03**: Point input [[Náhled](https://github.com/kmi-jp/template-L01E03)], [[Příjmout úkol]()]
* **L01E04PEP**: PEP8 format [[Náhled](https://github.com/kmi-jp/template-L01E04PEP)], [[Příjmout úkol]()] -->