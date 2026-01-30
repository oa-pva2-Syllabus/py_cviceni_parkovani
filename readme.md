# PVA2 - Programování a vývoj aplikací
## Cvičení: Parkovací automat

**Téma:** simulace jednoduchého parkovacího automatu pro malé město  
**Co si procvičíš:** funkce, výjimky, práce s časem (datum/čas, výpočet délky parkování), validace vstupů  
**Forma:** konzolová aplikace (bez GUI)

---

## Zadání
Naprogramuj aplikaci, která umožní obsluze parkoviště nebo uživateli:
1. založit parkování (vjezd),
2. ukončit parkování (výjezd),
3. spočítat cenu podle tarifu,
4. evidovat přehled aktivních parkování,
5. zobrazit denní souhrn (kolik aut, kolik minut/hodin, kolik tržba).

Aplikace běží v menu (opakovaná nabídka voleb), dokud uživatel nezvolí „konec“.

---

## Tarif
- Prvních **15 minut zdarma**.
- Poté se účtuje po **započaté půlhodině**.
- Cena za půlhodinu je **40 Kč**.
- Musí fungovat i parkování přes půlnoc (např. 23:50 → 00:10).

---

## Evidence parkování
Každé parkování má:
- **SPZ** (text),
- **čas vjezdu** (datum + čas),
- **čas výjezdu** (datum + čas) – až po ukončení,
- **délku parkování**,
- **cenu**.

---

## Požadované funkce (návrh)
Navrhni a použij vlastní funkce. Minimálně:
- funkci pro zobrazení menu a načtení volby,
- funkci pro načtení a kontrolu SPZ,
- funkci pro načtení času (vjezd/výjezd) ze vstupu,
- funkci pro výpočet délky parkování,
- funkci pro výpočet ceny dle tarifu,
- funkci pro založení parkování,
- funkci pro ukončení parkování,
- funkci pro výpis aktivních parkování,
- funkci pro výpis denního souhrnu,
- jednoduché logování (viz níže).

*(Konkrétní názvy a parametry si zvol sám. Nezapomeň striktně dodržovat best practise)*

---

## Vstupy a chování aplikace
### 1) Založení parkování (vjezd)
Uživatel zadá:
- SPZ
- čas vjezdu (uživatel ho zadává ručně; aplikace ho musí umět načíst)

Pravidla:
- Stejná SPZ nesmí být založená dvakrát jako aktivní parkování.
- Čas vjezdu musí být platný formát.

Výstup:
- potvrzení, že parkování bylo založeno.

---

### 2) Ukončení parkování (výjezd)
Uživatel zadá:
- SPZ
- čas výjezdu

Pravidla:
- SPZ musí existovat v aktivních parkováních.
- Čas výjezdu nesmí být dřívější než čas vjezdu.
- Po ukončení se spočítá délka parkování a cena podle tarifu.

Výstup:
- shrnutí: SPZ, čas vjezdu, čas výjezdu, délka parkování, cena.

---

### 3) Výpis aktivních parkování
Zobraz:
- seznam všech aktivních SPZ,
- čas vjezdu,
- jak dlouho už auto parkuje (vypočti vůči „aktuálnímu času“, který si aplikace načte při výpisu).

Seřazení:
- od nejstaršího vjezdu po nejnovější.

---

### 4) Denní souhrn
Zobraz souhrn ukončených parkování:
- počet ukončených parkování,
- celkový součet vyparkovaných minut/hodin,
- celkovou tržbu,
- nejdelší parkování (SPZ + délka + cena),
- nejkratší zpoplatněné parkování (tj. takové, které nebylo zdarma).

Poznámka:
- Počítej pouze z ukončených parkování (ne z aktivních).

---

## Logování
Aplikace musí zapisovat do souboru (např. `parking.log`) krátké záznamy o tom, co se dělo.

Loguj:
- start a ukončení aplikace,
- každou volbu z menu,
- úspěšný vjezd a výjezd,
- každou chybu (neplatný čas, neexistující SPZ, dvojí vjezd, výjezd dřív než vjezd, neplatná volba…).

Každý záznam má obsahovat:
- datum a čas,
- typ záznamu (např. INFO / WARNING / ERROR),
- stručný popis a případně SPZ.

Log se má **přidávat** (nesmí se při spuštění mazat/přepisovat).

---

## Ošetření chyb (výjimky)
Aplikace musí bezpečně zvládnout:
- neplatný formát času,
- prázdný vstup tam, kde být nesmí,
- neexistující SPZ při výjezdu,
- pokus o dvojí vjezd stejné SPZ,
- výjezd dřív než vjezd,
- neplatnou volbu v menu.

Po chybě:
- program nesmí spadnout,
- zobraz srozumitelnou hlášku,
- zapiš chybu do logu,
- vrať se do menu.

---

## Povinné formáty vstupu času
Zvol si **jeden** přesný formát a ten dodržuj v celé aplikaci.  
Musí obsahovat datum i čas (např. „den.měsíc.rok hodina:minuta“ nebo „rok-měsíc-den hodina:minuta“).  
Uživatel musí být při zadávání jasně informován, jaký formát má použít.

---

## Testovací scénáře (musíš ověřit)
1. 10 minut → 0 Kč.
2. 16 minut → po odečtení zdarma účtuj započatou půlhodinu.
3. Parkování přes půlnoc.
4. Dvojí vjezd stejné SPZ.
5. Výjezd neexistující SPZ.
6. Výjezd dřív než vjezd.
7. Neplatný čas (špatný formát nebo neexistující datum).
8. Neplatná volba v menu (program pokračuje, zapíše do logu).

---

## Volitelné rozšíření (pro rychlejší)
- Uložení ukončených parkování do souboru a načtení při startu.
- Nastavitelný tarif při startu.
- Vyhledání historie podle SPZ.
- Export denního souhrnu do textového souboru.
- Menu volba „zobraz posledních N řádků logu“.

---

## Odevzdávání
Práci odevzdáváš **průběžně přes Git**. Cílem je, aby z historie repozitáře byl jasně vidět postup (co přibylo, co se opravilo, kdy ses posunul).

**Pravidla odevzdávání:**
- Po dokončení **každé funkce / logického kroku** proveď commit (tj. ne jeden commit na konci).
- **Minimální očekávání:** alespoň 8–12 commitů (menu, validace vstupů, práce s časem, výpočet ceny, založení výpůjčky, ukončení výpůjčky, výpis aktivních, denní souhrn, logování, opravy chyb).
- Každý commit musí mít **smysluplnou zprávu** (např. `Add time parsing`, `Implement price calculation`, `Handle invalid SPZ`), ne `update` / `final` apod.
- Commity dělej tak, aby projekt byl po každém commitu v rozumném stavu (ideálně spustitelný).

**Zákazy:**
- Je zakázáno odevzdat projekt jako „hotové všechno najednou“ v jednom nebo pár commitech.
- Je zakázáno nahrávat zdrojáky přes webové rozhraní GitHubu („Upload files“, editace souborů v prohlížeči). Odevzdání musí probíhat přes Git z počítače (commit + push).
- Pokud bude historie commitů nepoužitelná (např. 1–2 commity na konci, nebo upload přes GitHub), bude to hodnoceno jako **nesplnění požadavků na odevzdání**.

**Doporučené členění commitů (příklad):**
1. Skeleton projektu + hlavní smyčka menu  
2. Validace SPZ  
3. Načítání a parsování času (datetime)  
4. Výpočet délky parkování  
5. Výpočet ceny dle tarifu  
6. Zahájení parkování  
7. Ukončení parkování + výstupní shrnutí  
8. Výpis aktivních parkování (včetně „kolik běží“)  
9. Denní souhrn  
10. Logování + ošetření chyb + úklid/refaktor

---

## Hodnocení
- Funkce jsou smysluplné, program není „jeden velký blok“.
- Chyby jsou ošetřené, program nespadne.
- Výpočty času a ceny jsou správné i přes půlnoc.
- Výstup je přehledný a log pomáhá dohledat průběh i chyby.


