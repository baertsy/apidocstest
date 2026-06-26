# Integrációs Lehetőségek

A MiniCRM bármilyen olyan alkalmazással integrálható, amely támogatja a Rest API kommunikációt, azzal a kitétellel, hogy a MiniCRM minden esetben csak a „passzív” fél, amely fogadja a kéréseket[cite: 3]. A MiniCRM nem indít kéréseket külső Rest API-k felé[cite: 3].

A rendszer több különböző módszerrel integrálható külső alkalmazásokkal[cite: 3]:
* Rest API[cite: 3].
* XML szinkronizáció[cite: 3].
* Specifikus Rest API végpontok (számlákhoz és hívásnaplózáshoz)[cite: 3].
* Webhookok[cite: 3].
* Beépített integrációk[cite: 3].

---

## Rest API vs. XML szinkronizáció

Az alábbi táblázat a két fő integrációs módszert hasonlítja össze[cite: 3]:

| Rest API | XML szinkronizáció |
| :--- | :--- |
| Kisebb mennyiségű adat átvitelére használatos[cite: 3]. | Nagy mennyiségű adat (pl. 100+ adatlap) egyszerre történő átvitelére szolgál[cite: 3]. |
| Percenkénti 60 kéréses korlátozás van érvényben[cite: 3]. | Óránkénti 180 kéréses korlátozás vonatkozik rá[cite: 3]. |
| Az adatok szinte azonnal szinkronizálódnak[cite: 3]. | Az átvitt adatmennyiségtől függően előfordulhat, hogy a szinkronizáció nem látható azonnal[cite: 3]. |
| Kétirányú kommunikáció: a külső alkalmazás kéréseket küld, a MiniCRM pedig válaszol[cite: 3]. | Csak egyirányú adatátvitelre alkalmas (a külső alkalmazásból a MiniCRM felé)[cite: 3]. |
| A biztonsági intézkedéseket a harmadik féltől származó alkalmazás implementálja (HTTPS protokoll használata mellett)[cite: 3]. | Az XML fájlnak publikusan elérhetőnek kell lennie a rendszer számára a feldolgozáshoz (IP-alapú korlátozás lehetséges)[cite: 3]. |
| Egy teljesen új kártya létrehozásához 3 külön kérés szükséges (cég, kapcsolat, adatlap)[cite: 3]. | Egy teljesen új kártya (cég, kapcsolat, adatlap) létrehozásához elegendő egyetlen kérés[cite: 3]. |

---

## Számlázó API

Ez a végpont lehetővé teszi a számlák kezelését a MiniCRM-ben[cite: 3]. 
* Új számlák hozhatók létre[cite: 3].
* Számlák kereshetők[cite: 3].
* A számlák fizetettre állíthatók vagy sztornózhatók[cite: 3].
* Frissíthetők a számla adatlapjának egyedi mezői[cite: 3].

---

## Hívásnapló végpont (VOIP integráció)

Ez a végpont központosított telefonszolgáltatások (VOIP) használata esetén hasznos[cite: 3]. A hívás befejezése után a hívás részletei a legutóbb frissített adatlap előzményei között rögzülnek[cite: 3].
A rögzített adatok az alábbiak[cite: 3]:
* A hívást kezdeményező/fogadó MiniCRM felhasználó[cite: 3].
* A hívás státusza (fogadott vagy nem fogadott)[cite: 3].
* A kapcsolattartó neve[cite: 3].
* A hívásban részt vevő telefonszám[cite: 3].
* A hívás időtartama[cite: 3].
* A hívás hanganyaga (ha a VOIP szolgáltató támogatja ezt a funkciót)[cite: 3].

---

## Webhookok

A webhookok akkor hasznosak, ha egy külső rendszernek vagy alkalmazásnak azonnal reagálnia kell a MiniCRM-ben történő változásokra (pl. komplex CRM automatizációk vagy külső rendszerek szinkronizálása)[cite: 3].
* A rendszer egy POST kérést küld az ügyfél által megadott API végpontra[cite: 3].
* A végpontnak HTTP 200 (OK) vélasszal kell nyugtáznia a sikeres feldolgozást[cite: 3].
* Sikertelen küldés esetén a rendszer további 6 alkalommal próbálkozik a következő késleltetésekkel: 10 másodperc, 1 perc, 5 perc, 15 perc, 30 perc, 1 óra[cite: 3].
* A webhookok szűrhetők típus (Cég/Személy vagy Adatlap), termék (CategoryId) és specifikus mezők változásai alapján[cite: 3].
* A webhookok beállítása nem érhető el a webes felületen, ehhez az ügyfélszolgálat segítségét kell kérni[cite: 3].

---

## Beépített integrációk

A MiniCRM számos beépített kiegészítővel rendelkezik, amelyeket az Előfizetés oldalon lehet aktiválni[cite: 3]:

* **MiniSync integráció:** Kizárólag Professional és Enterprise csomagokhoz érhető el, éves előfizetés esetén[cite: 3]. Egy köztes adatbázist használ a harmadik féltől származó adatok tárolására és leképezésére[cite: 3]. Csatlakozási formái: LocalFile, HttpDownload és MySQL[cite: 3].
* **Naptár integrációk:** A feladatok szinkronizálhatók a Google Naptárral, Outlookkal és iPhone naptárral[cite: 3].
* **Gmail és Outlook:** Lehetővé teszi az e-mailek közvetlen elküldését az inboxból egy adott ügyfél MiniCRM előzményébe[cite: 3].
* **Facebook Ads űrlapok:** Facebook hirdetésekből származó leadek automatikus begyűjtése a MiniCRM-be[cite: 3].
* **Gravity Forms integráció:** WordPress alapú weboldalak űrlapjainak összekötése a MiniCRM-mel egy rövid kód (shortcode) segítségével[cite: 3].
* **Webshop integrációk:** Megrendelések szinkronizálása WooCommerce, Shoprenter és UNAS webshopokból[cite: 3].
* **Cégjelző / OpenApi:** Cégadatok (pl. adószám, cím, létszám, árbevétel) automatikus frissítése a magyar (Cégjelző) és román (OpenApi) cégjegyzékek alapján[cite: 3].
* **NAV (Bejövő számlák):** A bejövő számlák és partnerek adatainak szinkronizálása, amelyből automatikusan frissülő cash flow jelentés készíthető Google Sheets segítségével[cite: 3].
* **BestJobs / eJobs integráció:** Jelöltek adatainak begyűjtése toborzási platformokról közvetlenül a MiniCRM-be[cite: 3].
* **SmartBill integráció:** Számlák és raktárkészlet adatok összekötése[cite: 3].
