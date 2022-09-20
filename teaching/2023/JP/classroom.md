## Jak pracovat s Github Classroom?

**Co je potřeba a nebude pokryto zde:**
1. Vytvoření účtu na [https://github.com/](https://github.com/).
2. Stažení a instalace Github Desktop [https://desktop.github.com](https://desktop.github.com).

V tomto návodu popíšeme splnění a odevzdání prvního úkolu **L01E01**: Hello world z prvního semináře. Github Classroom používájí git. Pro odevzdávání úkolů není jeho znalost potřebná (používají se pouze dvě základní funkce). Zájemci si mohou o základech gitu přečíst například [zde](https://git-scm.com/doc).

## 1. Přijetí úkolu
Práce na úkolu začíná jeho přijetím, každý úkol má na webu semináře odkaz "Přijmout úkol".

Ze seznamu identifikátorů studenta vyberte ten svůj (pokud nejste na seznamu, dejte mi vědět). Identifikátor naleznete v systému STAG.

<img src="/assets/images/JP/classroom/26.png" style="width:100%" />

Po výběru identifikátoru můžeme přijmout vybraný úkol.

<img src="/assets/images/JP/classroom/1.png" style="width:100%" />

Po kliknutí na "Accept this assignment" je nutné chvíli vyčkat a poté stránku obnovit.

<img src="/assets/images/JP/classroom/2.png" style="width:100%" />

Po obnovení stránky vidíme, že došlo k vytvoření repozitáře ve kterém budeme úkol programovat.

<img src="/assets/images/JP/classroom/3.png" style="width:100%" />

## 2. Klonování repozitáře
Po vytvoření repozitáře je nutné provést jeho naklonování do počítače. Na počítači poté úkol naprogramujeme a odešleme zpět na server ke kontrole. Kliknutím na odkaz z předchozího kroku se přesuneme na stránku s repozitářem.

<img src="/assets/images/JP/classroom/4.png" style="width:100%" />

Klikneme na "Code" a zvolíme "Open with Github Desktop" (ověřte zda máte hotovou instalaci).

<img src="/assets/images/JP/classroom/5.png" style="width:100%" />

V Github Desktop si zvolíme kam úkol naklonujeme (doporučuji si vytvořit jednu složku kam budete během semestru klonovat všechny úkoly).

<img src="/assets/images/JP/classroom/6.png" style="width:100%" />

To, že se klonování povedlo poznáme na následujíci obrazovce, zde rovněž provádíme hlavní operace s repozitářem.

<img src="/assets/images/JP/classroom/7.png" style="width:100%" />

## 3. Práce na úkolu
V našem oblíbeném editoru (doporučuji MS Visual Code) si otevřeme složku s naklonovaným repozitářem. Naklonované zadaní bude vždy obsahovat soubor `README.md` se zadáním úkolu a případně další soubory které budou nutné pro jeho splnění.

<img src="/assets/images/JP/classroom/8.png" style="width:100%" />

V tomto konkrétním zadání čteme, že našim úkolem je vytvořit skript `hello_world.py`, který po spuštění na standardní výstup vypíše řetězec `Hello world!`.

Vytvoříme tedy soubor `hello_world.py` s programem, který vypisuje řetězec `Hello world!`.

<img src="/assets/images/JP/classroom/9.png" style="width:100%" />

Vždy úkol nejprve otestujeme (v této fázi ručně, později automatickým testem).

<img src="/assets/images/JP/classroom/10.png" style="width:100%" />

Vše by mělo fungovat. Přejděme tedy do fáze odevzdání.

## 4. Odevzdání úkolu
V aplikaci Github Desktop otevřeme příslušný repozitář s úkolem.

<img src="/assets/images/JP/classroom/11.png" style="width:100%" />

V levém panelu vidíme provedené změny (vytvoření, smazání, modifikaci souborů). Tyto změny můžeme popsat takzvaným commitem. Jelikož máme funkční řešení, vytvoříme commit a následně jej odešleme na server. Před vytvořením commitu ověříme, zda máme v levém sloupci vybrané všechny soubory, které má commit obsahovat. Poté vyplníme název commitu a jeho popis.

<img src="/assets/images/JP/classroom/12.png" style="width:100%" />

Poté klikneme na "commit to main". Tím ale odevzdání nekončí. Commit je zatím pouze lokálně na našem počítači. Je nutné jej odeslat na server. Této akci se říká "push". V pravém horním rohu klikneme na tlačítko "Push origin" a veškeré lokální commity odešleme na Github server.

<img src="/assets/images/JP/classroom/13.png" style="width:100%" />

V záložce "history" si můžeme prohlédnout historii našeho repozitáře.

<img src="/assets/images/JP/classroom/14.png" style="width:100%" />

Vždy je rovněž dobré ověřit, že commit na server opravdu odešel. To uděláme jednoduše na webu našeho repozitáře. Klikneme na "X commits" a prohlédneme si všechny commity, které se na serveru vyskytují.

<img src="/assets/images/JP/classroom/15.png" style="width:100%" />
<img src="/assets/images/JP/classroom/16.png" style="width:100%" />

## 5. Je řešení opravdu správné?
Veškeré odeslané commity úkolů spouští automatizované testování. Výsledky těchto testů si můžete prohlédnout v repozitáři. Ze seznamu commitů výše, vybereme námi vytvořený commit a zobrazíme jeho detail.

<img src="/assets/images/JP/classroom/17.png" style="width:100%" />

V levém horním rohu, vedle názvu commitu vidíme zelenou fajfku. Po kliku na fajfku a zobrazení detailu si můžeme prohlédnout detailní výsledek.

<img src="/assets/images/JP/classroom/18.png" style="width:100%" />

Co se stane když v úkolu uděláme funkční chybu? Zkusme úkol rozbít. Před koncem termínu je možné odeslat libovolný počet commitů s úpravami. V editoru tedy upravíme program tak, aby vypisoval `Ahoj svete!`, což způsobí porušení zadání.

<img src="/assets/images/JP/classroom/19.png" style="width:100%" />

Poté vytvoříme nový commit a odešleme jej na server (opakujeme stejný proces jako dříve).

<img src="/assets/images/JP/classroom/20.png" style="width:100%" />

Commit pushneme na server a zkontrolujeme výsledek testů. Testy selhaly. V tomto momentu by jste měli obdržet email, že úkol nefunguje.

<img src="/assets/images/JP/classroom/21.png" style="width:100%" />

V detailu testu vidíme proč test selhal.

<img src="/assets/images/JP/classroom/22.png" style="width:100%" />

## 6. Testy fungují, ale?
To, že zdrojový kód prošel testy neznamená, že bude uznaný. V kódu můžete mít další sématické chyby nebo špatné formátování, které nemá přimý vliv na funkčnost programu. Zdrojový kód úkolu byl  upraven takovým způsobem, že se před názvem funkce `print` vyskytuje mezera. Na funkčnost kódu nemá tato úprava žádný vliv, jedná se však o špatné formátování Python kódu a je nutné jej opravit.

<img src="/assets/images/JP/classroom/23.png" style="width:100%" />

Každý repozitář s úkolem, který vytvoříte obsahuje v sekci "Pull requests" vlákno "Feedback". V tomto vlákně bude probíhat zpětná vazba.

<img src="/assets/images/JP/classroom/24.png" style="width:100%" />

Po obdržení zpětné vazby stačí kód upravit, vytvořit commit a pushnout změny na server. Kód znovu zkontroluji a pokud je vše v pořádku napíši komentář, že je řešení uznáno. V tento moment máte úkol uznaný. V případě dalších problému budeme kolečko zpětné vazby (code review) opakovat.
