---
layout: default
courses: JP
title: 6. Dunder metody a dekorátory tříd (WIP)
year: 2022
---

## 6. Dunder metody a dekorátory tříd

### Dunder metody
Speciální metody sloužící k implementaci podpory pro vestavěné funkce Pythonu a jinou rozšiřující funkcionalitu (např. `__init__` jakožto konstruktor). Přehled dunder metod je dostupný [zde](https://docs.python.org/3/reference/datamodel.html#basic-customization).

#### String reprezentace
Implementací dunder metod `__repr__` a `__str__` implementujeme chování funkcí `repr()` a `str()`

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
    
    def __repr__(self):
        return f"CreditAccount({self.owner}, {self.balance})"
    
    def __str__(self):
        return self.__repr__()
    
credit_account_1 = CreditAccount("Lukas Novak")
credit_account_2 = CreditAccount("Pepa Novak", initial_credits=200)

credit_account_1
print(credit_account_1)
repr(credit_acount_1)
{% endhighlight %}

#### Převod na jiné datové typy

{% highlight python linenos %}
__bool__ # chování funkce bool()
__complex__ # chování funkce complex()
__int__ # chování funkce int()
__float__ # chování funkce float()
__hash__ # chování funkce hash()
{% endhighlight %}

Následující příklad dále implementuje dunder metody `__bool__` a `__int__`.

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
    
    def __repr__(self):
        return f"CreditAccount({self.owner}, {self.balance})"
    
    def __str__(self):
        return self.__repr__()
    
    def __bool__(self):
        return bool(self.balance)
    
    def __int__(self):
        return int(self.balance)
    
credit_account_1 = CreditAccount("Lukas Novak")
credit_account_2 = CreditAccount("Pepa Novak", initial_credits=200)

assert bool(credit_account_1) == False
assert bool(credit_account_2) == True

assert int(credit_account_1) == 0
{% endhighlight %}

#### Unární číselné operátory

{% highlight python linenos %}
__abs__ # chování funkce abs()
__neg__ # chování unárního minus
__pos__ # chování unárního plus
{% endhighlight %}

#### Porovnávání

{% highlight python linenos %}
__lt__ # chování <
__le__ # chování <=
__eq__ # chování ==
__ne__ # chování !=
__gt__ # chování >
__ge__ # chování >=
{% endhighlight %}

Následující příklad dále implementuje dunder metodu `__lt__`.

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
    
    def __repr__(self):
        return f"CreditAccount({self.owner}, {self.balance})"
    
    def __str__(self):
        return self.__repr__()
    
    def __bool__(self):
        return bool(self.balance)
    
    def __int__(self):
        return int(self.balance)
    
    def __lt__(self, other):
        if isinstance(other, CreditAccount):
            return self.balance < other.balance

        return NotImplemented

    
credit_account_1 = CreditAccount("Lukas Novak")
credit_account_2 = CreditAccount("Pepa Novak", initial_credits=200)

credit_account_1 < credit_account_2
credit_account_2 < credit_account_1
{% endhighlight %}

#### Aritmetické operátory

{% highlight python linenos %}
__add__ # chování +
__sub__ # chování -
__mul__ # chování *
__truediv__ # chování /
__floordiv__ # chování //
__mod__ # chování %
__divmod__ # chování divmod()
__pow__ # chování ** nebo pow()
__round__ # chování round()
{% endhighlight %}

Následující příklad dále implementuje dunder metodu `__add__`. V případě, že pro nějaký typ nejsou dunder metody implementovány, je nutné vracet konstantu `NotImplemented`.

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
    
    def __repr__(self):
        return f"CreditAccount({self.owner}, {self.balance})"
    
    def __str__(self):
        return self.__repr__()
    
    def __bool__(self):
        return bool(self.balance)
    
    def __int__(self):
        return int(self.balance)
    
    def __lt__(self, other):
        if isinstance(other, CreditAccount):
            return self.balance < other.balance

        return NotImplemented
    
    def __add__(self, other):
        if isinstance(other, CreditAccount):
            return self.balance + other.balance

        return NotImplemented

    
credit_account_1 = CreditAccount("Lukas Novak")
credit_account_2 = CreditAccount("Pepa Novak", initial_credits=200)

credit_account_1 + credit_account_2
{% endhighlight %}

Aritmetické operátory je možné implementovat pro "oba směry". V situaci kdy vyhodnocujeme `x + y` Python hledá implementaci `x.__add__` nebo `y.__radd__`.

{% highlight python linenos %}
__radd__
__rsub__
__rmul__
__rtruediv__
__rfloordiv__
__rmod__
__rdivmod__
__rpow__
{% endhighlight %}

#### Kombinované operátory přiřazení s aritmetickými operacemi

Je běžné, ne však nutné, aby výsledná metoda vracela `self`.

{% highlight python linenos %}
__iadd__ # chování +=
__isub__ # chování -=
__imul__ # chování *=
__itruediv__ # chování /=
__ifloordiv__ # chování //=
__imod__ # chování %=
__ipow__ # chování **=
{% endhighlight %}

Následující příklad dále implementuje dunder metodu `__iadd__`. V případě, že pro nějaký typ nejsou dunder metody implementovány, je nutné vracet konstantu `NotImplemented`.

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
    
    def __repr__(self):
        return f"CreditAccount({self.owner}, {self.balance})"
    
    def __str__(self):
        return self.__repr__()
    
    def __bool__(self):
        return bool(self.balance)
    
    def __int__(self):
        return int(self.balance)
    
    def __lt__(self, other):
        if isinstance(other, CreditAccount):
            return self.balance < other.balance

        return NotImplemented
    
    def __add__(self, other):
        if isinstance(other, CreditAccount):
            return self.balance + other.balance

        return NotImplemented
    
    def __iadd__(self, value):
        if isinstance(value, int):
            self.balance += value
            return self
        
        return NotImplemented

    
credit_account_1 = CreditAccount("Lukas Novak")
credit_account_2 = CreditAccount("Pepa Novak", initial_credits=200)

credit_account_1 += 200

credit_account_1 += credit_account_2
{% endhighlight %}

#### Bitové operátory

{% highlight python linenos %}
__invert__ # chování ~
__lshift__ # chování <<
__rshift__ # chování >>
__and__ # chování &
__or__ # chování |
__xor__ # chování ^
{% endhighlight %}

#### Emulace kolekcí

Důležité, dostaneme se k nim později.

{% highlight python linenos %}
__index__ # chování převodu na integer například při slicingu
__len__ # chování len()
__getitem__ # chování x[20]
__setitem__ # chování x[20] = 2
__delitem__ # chování del
__contains__ # chování in
{% endhighlight %}

### Použití implementovaných dunder metod
Implementovanou funkcionalitu můžeme použít rovněž v rámci třídy.

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
    
    def __repr__(self):
        return f"CreditAccount({self.owner}, {self.balance})"
    
    def __str__(self):
        return self.__repr__()
    
    def __bool__(self):
        return bool(self.balance)
    
    def __int__(self):
        return int(self.balance)
    
    def __lt__(self, other):
        if isinstance(other, CreditAccount):
            return self.balance < other.balance

        return NotImplemented
    
    def __add__(self, other):
        if isinstance(other, CreditAccount):
            return self.balance + other.balance

        return NotImplemented
    
    def __iadd__(self, value):
        if isinstance(value, int):
            self.balance += value
            return self
        
        return NotImplemented
    
    def __isub__(self, value):
        if isinstance(value, int):
            self.balance -= value
            return self
        
        return NotImplemented
    
    def transfer_to(self, other, value):
        """Transfer credit into another credit account. Negative balance is allowed.

        Args:
            other: Target of credit transfer.
            value: Amount of credit to be transfered.
        """
        self -= value
        other += value


credit_account_1 = CreditAccount("Lukas Novak")
credit_account_2 = CreditAccount("Pepa Novak", initial_credits=200)

credit_account_1.transfer_to(credit_account_2, 200)

credit_account_1
credit_account_2
{% endhighlight %}

### Vestavěné funkce
Dunder metody představují elegantní řešení pro implementaci podpory vestavěných funkcí. Většinou je tedy lepší využívat implementace těchto metod pro podporu `len()` než implementování vlastní metody `object.length()`.

Protipříkladem je pomyslná třída `Vector` ve které chceme implementovat výpočet délky vektoru. Pozor, použití `len()` na instanci třídy `Vector` není vhodné, funkci `len()` používáme v kontextu kolekcí pro zjištění počtu prvků. U třídy `Vector` požadujeme jinou sémantiku. V takovém případě je tedy vhodnější zvolit implementaci metody `Vector.length()`, rovněž jsem se setkal s implementace dunder metody `__abs__()` a následné používání funkce `abs()`.

## Dekorátory ve třídách

### @property
V jazyce Python nepoužíváme klasické (například Javovské) gettery, settery (tedy metody s názvy `get_temperature` a `set_temperature`). To plyne z vlastnosti veřejné dostupnosti všech hodnot objektu (narozdíl od jazyků jako je třeba Java). Vraťme se k jednoduchému příkladu třídy `CreditAccount`. Vlastnost `CreditAccount.balance` je dostupná pomoci tečkového operátoru.

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

credit_account = CreditAccount("Lukas Novak")

credit_account.balance
{% endhighlight %}

Co můžeme dělat pokud chceme například ověřovat, že `balance` nemůže být nastavena na zápornou hodnotu?

{% highlight python linenos %}
# špatné, ale bohužel časté řešení

class CreditAccount:
    """Account with stored credits."""

    def __init__(self, owner, initial_credits=0):
        """Creates credit account with given owner and initial credits.

        Args:
            owner: owner of the account
            initial_credits (optional): credit balance. Defaults to 0.
        """

        self.owner = owner
        self.balance = 0
        self.set_balance(initial_credits)
    
    def set_balance(self, new_balance):
        if new_balance < 0:
            raise ValueError("Balance cannot be negative number!")

        self.balance = new_balance

credit_account = CreditAccount("Lukas Novak", 0)

credit_account.set_balance(0)

# negativní hodnotu balance můžeme stále nastavit
credit_account.balance = -100
{% endhighlight %}

Pro příklady kdy chceme modifikovat chování přístupu k vlastnosti třídy je nutné použít dekorátor `@property`.

{% highlight python linenos %}
# správně

class CreditAccount:
    """Account with stored credits."""

    def __init__(self, owner, initial_credits=0):
        """Creates credit account with given owner and initial credits.

        Args:
            owner: owner of the account
            initial_credits (optional): credit balance. Defaults to 0.
        """

        self.owner = owner
        self._balance = 0
        self.balance = initial_credits
    
    @property
    def balance(self):
        return self._balance
    
    @balance.setter
    def balance(self, new_balance):
        """Sets new value of balance, new value cannot be negative."""

        if new_balance < 0:
            raise ValueError("Balance cannot be negative number!")

        self._balance = new_balance

    @balance.deleter
    def balance(self):
        self._balance = 0

credit_account = CreditAccount("Lukas Novak", 0)

credit_account.balance = 10

# volani deleteru
del credit_account.balance

credit_account.balance

# volani setteru
credit_account.balance = -100
{% endhighlight %}

### @classmethod vs @staticmethod

Nejprve se podívejme na dekorátor `@classmethod`. Tento dekorátor lze použít v situaci kdy je nutné metodám předat odkaz na celou třídu. Demonstrovat jej můžeme na příkladu metod `from_*`, tedy metod které umí vytvořit instanci třídy různými způsoby.

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
    
    @classmethod
    def from_csv(cls, input_string, separator=","):
        """Creates CreditAccount class from csv string"""
        owner, initial_credits = input_string.split(separator)

        return cls(owner, int(initial_credits))

credit_account = CreditAccount.from_csv("Lukas Novak,200")

credit_account.owner
credit_account.balance
{% endhighlight %}

Speciální dekorátor `@staticmethod` naopak nevyžaduje (a nemá) přístup ke své třídě.

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
    
    @staticmethod
    def credit_to_money(credit, exchange_rate):
        """Calculates money value of credits.

        Args:
            credit: amount of credits
            exchange_rate: how many money per one credit

        Returns: money value
        """
        return credit * exchange_rate

# dostupné z třídy
CreditAccount.credit_to_money(100, 20)

credit_account = CreditAccount("Lukas Novak", 200)

# dostupné rovněž z objektu
credit_account.credit_to_money(100, 20)
{% endhighlight %}

<!-- ### @dataclass
Pro rychlé vytvoření třídy lze použít dekorátor `@dataclass`.

{% highlight python linenos %}
from dataclasses import dataclass

@dataclass
class CreditAccount:
    """Account with stored credits."""
    owner: str
    balance: int = 0
{% endhighlight %}

Dekorátor `@dataclass` přidá automaticky konstruktor:

{% highlight python linenos %}
def __init__(self, owner: str, balance: int = 0):
    self.owner = owner
    self.balance = balance
{% endhighlight %}

Dekorátor `@dataclass` je mnohem komplexnější, celkový popis je možné nalézt [zde](https://docs.python.org/3/library/dataclasses.html). Pravděpodobně nás zaskočilo, že uvádíme datový typ u jednotlivých vlastností. Jedná se o nápovědu typování, o této možnosti v jazyku Python si řekneme v budoucnu. -->

<!-- ## Úkoly
Nevíte si rady? Přečtěte si "[Jak pracovat s Github Classroom?](/teaching/2022/JP/classroom)".

* **L01E01**: Hello world [[Náhled](https://github.com/kmi-jp/template-L01E01)], [[Příjmout úkol](https://classroom.github.com/a/BLVFlAR8)]
* **L01E02**: Integer input [[Náhled](https://github.com/kmi-jp/template-L01E02)], [[Příjmout úkol]()]
* **L01E03**: Point input [[Náhled](https://github.com/kmi-jp/template-L01E03)], [[Příjmout úkol]()]
* **L01E04PEP**: PEP8 format [[Náhled](https://github.com/kmi-jp/template-L01E04PEP)], [[Příjmout úkol]()] -->