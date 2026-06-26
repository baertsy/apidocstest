# MiniCRM API Alapok (BASICS)

Ez a dokumentáció a MiniCRM REST API használatának alapvető fogalmait, architektúráját és alapvető működési elveit mutatja be.

---

## SystemID

A **SystemID** a MiniCRM API egyik alapfogalma, amely egy **5 számjegyből álló egyedi azonosító**. Ez az azonosító a kérések hitelesítésére szolgál, teljesen stabil, és a jövőben sem fog megváltozni.

A SystemID közvetlenül használható a cURL kapcsolódási URL-ekben is:
```bash
curl [https://r3.minicrm.hu/Api](https://r3.minicrm.hu/Api)...
```

### Hogyan található meg a SystemID?
A SystemID-t a böngésző címsorában találja meg, közvetlenül a domain név után. 
*Például:* ha a cím `https://r3.minicrm.hu/51546/TodayView`, akkor a **51546** lesz a SystemID.

---

## Termék, modul és kategória (Product / module / category)

A MiniCRM-ben az adatokat és a folyamatokat **termékekbe** (Products) szervezzük. A rendszerben és a dokumentációban a *termék (product)*, *modul (module)* és *kategória (category)* kifejezések egymás szinonimái, mind ugyanazt jelentik.

* Az API-ban a termékekre **Category** néven hivatkozunk, a leggyakrabban használt azonosító pedig a **CategoryID**.
* Ezek a modulok felelnek meg az oldalsáv menüpontjainak (pl. *Sales (Értékesítés)*, *Invoice (Számlázás)*, *Helpdesk*, *Order (Megrendelések)* stb.).
* A **CategoryID** szintén a böngésző címsorából olvasható ki a modul megnyitásakor (a URL végén található szám, pl. `.../#Project-25`, ahol a 25 a kategória azonosítója).

---

## Adatlap vagy projekt (Opportunity card / project)

Az adatlapok (opportunity cards) vagy projektek a termékeken (modulokon) belül vannak listázva. Az adatlapok alapvető szerkezete minden modulban megegyezik; a különbség a konkrét mezőkben és a hozzájuk rendelt adatokban van. A mezőket az adminisztrátorok testreszabhatják a webes felületen.

Az API-n keresztül az adatlapok **Project** néven érhetők el.

### Az adatlap szerkezeti felépítése
Minden adatlap négy fő területből áll:

1. **Feladatkezelő sáv (Task area):** Itt jelenik meg minden nyitott feladat a részleteivel együtt (feladat tartalma, felelős, határidő, megjegyzések).
2. **Előzmények (History area):** A rendszer itt követ minden változást, ami az adatlapon történik. Itt jelennek meg a kiküldött e-mailek, SMS-ek, WhatsApp üzenetek, valamint a lezárt feladatok is.
3. **Mezők (Fields area):** A MiniCRM egyedileg testreszabható, így itt jelennek meg a saját létrehozású egyedi mezők. Minden itt történő változás bekerül az előzmények közé is.
4. **Ügyfélblokk (Contacts area):** Ez a terület az alábbi részekre tagolódik:
    * *Cégadatok (Business data):* A vállalati adatok helye. (A MiniCRM-ben létezhet adatlap cégadatok nélkül is, ekkor csak egy kapcsolattartó látható).
    * *Kapcsolatok (Contacts):* A megjelölt céghez tartozó összes kapcsolattartó listája. Egyszerre csak egyetlen **Elsődleges kapcsolattartó (Primary contact)** létezhet, akihez az aktuális adatlap közvetlenül kapcsolódik.
    * *Projektek (Projects):* A céghez vagy kapcsolattartóhoz köthető összes többi adatlap, termékek szerint csoportosítva.

### Elsődleges és egyéb kapcsolattartók
Egy adatlagnak egyszerre legfeljebb **egy elsődleges kapcsolattartója** lehet. Ha az adatlaphoz több személy is kötődik, a többiek az elsődleges alatt, az „Egyéb kapcsolatok” (Other contacts) szekcióban jelennek meg.

---

## Speciális modulok adatlap-szerkezete (Invoice, order, offer stb.)

A Számlák (Invoice), Megrendelések (Order), Ajánlatok (Offer), Szállítólevelek (Delivery Notes), Szerviz munkalapok (Service Worksheet) és Teljesítési igazolások (Certificate of contract completion) modulokban található kártyák **speciális felépítésűek, két füllel rendelkeznek**.

* **Projekt fül (Project tab):** Itt kezelhetők az egyedi mezők, feladatok és követő szekvenciák.
* **Bizonylat fül (pl. Order/Invoice tab):** A specifikus funkciók (tételek, árak, számlázási adatok) ezen a külön fülön és külön API végponton keresztül működnek.

---

## Projektmenedzsment sajátosságok (Kanban nézet)

A **Project Management** nevű termék egy speciális feladat-nézettel rendelkezik, amelyet **Kanban nézetként** is ismerünk. Ez a felület a nyitott feladatokat meghatározott oszlopokban jeleníti meg a munkafolyamat állapota szerint:
* Ötletek (Ideas)
* Aktuális feladatok (Current tasks)
* Folyamatban lévő munkák (Work in progress)
* Ellenőrzés (Review)
* Függőben lévő (Pending)
* Kész (Done)
* Elutasított (Rejected)

---

## Cég, kapcsolat és entitások viszonya

Az adatlap jobb oldalsávjában látható az ügyfél és a kapcsolattartók, akikhez az adatlap tartozik. Az ügyfél lehet egy cég, egy magánszemély, vagy egy cég a hozzá tartozó kapcsolattartókkal.

A MiniCRM-ben a logikai hierarchia a következőképpen épül fel:
**Adatlapok (Projects) -> Kapcsolattartók (Contacts) -> Cégek (Companies)**

Az adatlapok a kapcsolattartókhoz vannak rendelve, a kapcsolattartók pedig a cégekhez. 

### Cég adatlap (Company card)
Egy dedikált oldal, ahol a specifikus cég összes adata regisztrálva van. Tartalmazza a céghez köthető összes korábbi adatlap összesített előzményeit (Account history), a kapcsolattartók listáját, valamint a cég alapadatait (weboldal, leírás, e-mail, telefon, adószám, EU adószám, cégjegyzékszám stb.). A cég adatlapon is létrehozhatók egyedi mezők.

### Kapcsolattartó adatlap (Contact card)
Ezen a kártyán regisztrálhatók a személyes adatok: vezetéknév, keresztnév, beosztás, telefonszám, e-mail cím vagy levelezési cím, szükség esetén egyedi mezőkkel kiegészítve.

> 💡 **Fontos integrációs szabály:** Ha az API-n keresztül egy teljesen új struktúrát szeretne létrehozni (új céggel, új kapcsolattartóval és új adatlappal), ehhez **3 különálló API hívásra** van szükség ebben a kötelező sorrendben:
> 1. Cég létrehozása (Create company).
> 2. Kapcsolattartó létrehozása (Create contact), és annak hozzárendelése az 1. lépésben kapott cégazonosítóhoz (`BusinessId`).
> 3. Az adatlap (Opportunity card / Project) létrehozása, és annak hozzárendelése a 2. lépésben kapott kapcsolat-azonosítóhoz (`ContactId`).

---

## Mezőnevek az adatbázisban (Field name)

A MiniCRM lehetőséget biztosít egyedi mezők létrehozására. Az integrációk során az ezekre való hivatkozáshoz ismerni kell a mezők adatbázisban regisztrált pontos nevét.

Ehhez **Adminisztrátor** joggal kell rendelkeznie a MiniCRM rendszerben:
1. Navigáljon a mezők konfigurációs oldalára.
2. Kattintson a szerkesztés ikonra a mező módosításához.
3. Itt megtalálja a mező regisztrált nevét (pl. `Type`).

A rendszerben a mezők felépítése a kártya típusától függően `Contact.MezoNeve` vagy `Project.MezoNeve` formátumban látható. **Az integrációk és API hívások során a pont előtti részt figyelmen kívül kell hagyni, csak a pont utáni nevet kell használni** (pl. a `Project.Type`-ból csak a `Type`-ot használjuk).

---

## API Kulcs Generálása (API key generation)

### REST API kulcs generálása
Az API használatához egy API Kulcsra (API Key) van szükség, amelyet kizárólag egy **Adminisztrátor** generálhat a **Beállítások -> Rendszer** oldalon.

* ⚠️ **Egyszeri megjelenítés:** A generált kulcs biztonsági okokból **csak egyetlen egyszer jelenik meg** a generálás pillanatában, később nem hozzáférhető. Azonnal mentse el egy biztonságos jelszókezelőbe.
* **Védelem:** A kulcs az Ön jelszava az adatokhoz, senkinek ne adja át (még a MiniCRM támogatásnak sem).
* **Elvesztés kezelése:** Ha a kulcs elveszik, nincs lehetőség a megtekintésére; új kulcsot kell generálni. Új kulcs generálása esetén az összes meglévő aktív külső integrációt frissíteni kell az új kulccsal.

### Éles és Tesztkörnyezetek (Production and test environments)

A MiniCRM-ben lehetőség van az élő (production) rendszertől teljesen különálló, független **tesztkörnyezet** igénylésére a help@minicrm.hu címen. 

A tesztkörnyezetből nem lehet e-maileket küldeni, nem lehet jelszót visszaállítani, és GDPR okokból az adatok alapértelmezetten titkosítva vannak.
