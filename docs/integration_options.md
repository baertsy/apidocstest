# INTEGRÁCIÓS LEHETŐSÉGEK
A MiniCRM bármilyen olyan alkalmazással integrálható, amely támogatja a Rest API kommunikációt, azzal a megkötéssel, hogy egy adott integrációban a MiniCRM kizárólag a kéréseket váró „passzív” fél lehet. A MiniCRM nem indít kéréseket külső Rest API-k felé.

A rendszer többféle módszerrel és különféle célokra integrálható más alkalmazásokkal.

Támogatjuk a Rest API-n és XML szinkronizáción keresztüli integrációkat. Emellett rendelkezünk specifikus Rest API végpontokkal a számlákhoz és a hívásnaplózáshoz is.

Ebben a kézikönyvben a MiniCRM összes integrációs lehetőségét bemutatjuk.

## Rest API vs. XML szinkronizáció

Az alábbi táblázatban a rendszerben elérhető két fő integrációs módszert hasonlítjuk össze.

| Rest API | XML szinkronizáció |
| :--- | :--- |
| Kisebb mennyiségű adat átvitelére használatos. | Akkor használatos, ha nagy mennyiségű adatot kell a rendszerbe továbbítani (például 100+ adatlap). |
| Percenként 60 kéréses korlátozás van érvényben (kérjük, ellenőrizze az API limitekről szóló fejezetet). | |
| Az adatok szinte azonnal szinkronizálódnak (az internetsebességtől függően). | A szinkronizált adatmennyiségtől függően előfordulhat, hogy az átvitel nem látható azonnal. |
| Kommunikációként működik a MiniCRM és egy harmadik féltől származó alkalmazás között, ahol az alkalmazás kéréseket küld a MiniCRM-nek, a rendszer pedig a kért adatokkal válaszol. | Csak egyirányú adatátvitelként működik (a harmadik féltől származó alkalmazásból a MiniCRM felé). |
| A biztonsági intézkedéseket a harmadik fél alkalmazása valósíthatja meg. A rendszer HTTPS protokollokat használ. | Az XML fájlnak nyilvánosan elérhetőnek kell lennie ahhoz, hogy a rendszer hozzáférjen és feldolgozza azt¹. |
| Egy teljesen új kártya létrehozásához 3 különböző kérésre van szükség (egy a cégadatlaphoz, egy a kapcsolattartó adatlaphoz és egy az értékesítési/projekt adatlaphoz). | Egy teljesen új kártya létrehozásához (céggel, kapcsolattartóval és adatlappal együtt) egyetlen kérés elegendő. |

> ¹ Biztonsági okokból IP-korlátozások állíthatók be. Szükség esetén meg tudjuk adni a szervereink IP-címeit.

A fenti táblázatnak elegendő információt kell nyújtania ahhoz, hogy ki tudja választani a megfelelő integrációs módot a rendszerünkkel.

Például, ha egy webshopot tervez integrálni a MiniCRM-mel, a fenti táblázat alapján valószínűleg az XML szinkronizáció lesz a legmegfelelőbb megoldás, mivel a leggyakoribb esetekben egyszerre nagy mennyiségű adatot szeretne szinkronizálni.

Ha azonban nem tudja eldönteni, mi a megfelelő módszer a terveihez, kérjük, forduljon hozzánk bizalommal:
- Romániai ügyfeleknek: suport@minicrm.ro
- Magyarországi ügyfeleknek: help@minicrm.hu
- Más országokból érkező ügyfeleknek (kérjük, angolul írjanak): help-en@minicrm.io

Szívesen segítünk kiválasztani az Ön igényeinek leginkább megfelelő módszert.

---

## Számlázó API

Ez a végpont lehetővé teszi az ügyfél számára a MiniCRM-ben lévő számlák kezelését. Képes lesz kezelni a számláit, és olyan műveleteket végezhet, mint új számlák létrehozása, számlák keresése, fizetettként történő megjelölés vagy sztornózás.

Továbbá a végpont használatával az ügyfél frissítheti a számla adatlapjának mezőit. Részletes információkat a vonatkozó fejezetben találhat.

---

## Hívásnapló végpont (VOIP integráció)

A végpont olyan esetekben hasznos, amikor központosított telefonszolgáltatást, például VOIP (Voice Over IP) megoldásokat kell használnia.

Ez a végpont lehetővé teszi a telefonhívások adatainak tárolását, például: melyik telefonszámot hívták, ki, mikor, és mennyi volt a hívás időtartama. Továbbá rögzítheti és meghallgathatja a telefonhívásokat, ha szüksége van erre a funkcióra, és ha a VOIP megoldása ezt lehetővé teszi².

> ² Ez a lehetőség az Ön székhelye szerinti ország jogszabályaitól, valamint a GDPR-ra vonatkozó uniós jogszabályoktól is függ. Mi csak ezt a funkciót biztosítjuk, de az ügyfél felelőssége ezen jogi szempontok figyelembevétele, és neki kell gondoskodnia arról, hogy tájékoztassa ügyfeleit a telefonhívások rögzítéséről.

Kérjük, vegye figyelembe, hogy országa jogszabályai és/vagy az európai jogszabályok alapján előfordulhat, hogy tájékoztatnia kell ügyfeleit, és be kell szereznie a hozzájárulásukat a telefonhívások rögzítésére vonatkozó szabályzatról.

A hívás befejezése után az adott hívás adatai a legutóbb frissített adatlap előzmények (history) szekciójában lesznek rögzítve.

A következő telefonhívás adatokat rögzítjük:
- Ki hívott/fogadta a telefonhívást (MiniCRM felhasználó);
- A telefonhívás állapota (fogadott vagy nem fogadott);
- A kapcsolattartó neve;
- A hívásban részt vevő telefonszám;
- A telefonhívás időtartama;
- A telefonhívás rögzítése (ez a funkció a VOIP megoldástól függ - néhányuk nem feltétlenül tartalmazza ezt a lehetőséget).

---

## Webhookok (Webhooks)

Előfordulhatnak olyan helyzetek, amikor egy külső rendszernek vagy alkalmazásnak reagálnia kell egy, a MiniCRM-ben történt változásra. Ezekben az esetekben a webhookok használatát javasoljuk.

Néhány példa, amikor a webhookok hasznosak lehetnek:
- **Komplex CRM automatizáció** - egy külső rendszer webhookon keresztül értesül egy új érdeklődőről, jegyről, projektről, számláról, ajánlatról, megrendelésről stb. A rendszer különböző módokon tud reagálni erre az információra.
- **Szinkronizálás külső rendszerrel** - ha egy adatlap, projekt, jegy, számla stb. frissül a MiniCRM-ben, a külső rendszer is értesül a változásról.

Az adatlapokon, cégeken vagy kapcsolatokon bekövetkező változások elküldhetők egy, az ügyfél által megadott API végpontra POST kérés formájában.

A végpontnak HTTP válasszal (200 OK) kell reagálnia, hogy a rendszer az adatokat sikeresen feldolgozottnak tekintse.

Abban az esetben, ha nem kapunk HTTP 200-as választ, hibát feltételezünk, és további 6 kísérletet teszünk később, az alábbi késleltetésekkel:
1. 10 másodperc;
2. 1 perc;
3. 5 perc;
4. 15 perc;
5. 30 perc;
6. 1 óra.

A késleltetés az első kísérlettől számítódik, így az adatlap módosítása után a fogadó félnek 1 óra 51 perc és 10 másodperc áll rendelkezésére az adatok feldolgozására. Ha ezen idő alatt nem kerül feldolgozásra, nem teszünk újabb kísérletet, és a változtatásokat elavultnak tekintjük.

### Webhook szűrési feltételek (Webhook filter criteria)

A túlterhelés elkerülése érdekében számos paraméterrel szűrhető, hogy egy adott végpont mikor hívódjon meg.

> **FONTOS:** ha a meghívott program a kapott adatok alapján módosításokat végez a MiniCRM REST API-n keresztül, az ismételt értesítéseket válthat ki. Ily módon végtelen ciklus alakulhat ki. Ebben az esetben legyen nagyon óvatos a hívási kritériumok kiválasztásakor. A végpontnak csak olyan mezők változásait kellene figyelnie, amelyeket ő maga már nem fog módosítani.

**Típus (Type)**
- Contact (személy és cég egyaránt)
- Project (adatlap/kártya)

**Termék (Product)**
Csak adatlapok (project) esetén értelmezhető. Értesítés kerül kiküldésre, ha egy kiválasztott modulban módosítás történt.
Ha több, de nem az összes termékről szeretne értesítést kapni, beállíthat több webhookot, minden termékhez külön-külön.

**Mezők (Fields)**
Konkrét mezők változásának szűrése. A végpont akkor hívódik meg, ha a felsorolt mezők valamelyike megváltozott. Ha nincs mező hozzáadva a végponthoz, a végpont bármely mező változása esetén meghívódik. A termék- és mezőszűrők kombinálhatók.

### Webhook példák

A Sales (Értékesítés) termék azonosítója (ID) = 3, és szeretném, ha csak az a weboldal, amely tartalmat jelenít meg az előfizetőimnek, kapna értesítést arról, ha egy előfizetés létrejött és/vagy lemondásra került.
```text
Type: Project
CategoryId: 3
Fields: ["StatusId"]
ApiUrl: https://$User:$Pass@example.com/StatusChanged/
```

A Helpdesk termék azonosítója ID = 4, és szeretném, ha az új jegyeket egy szkript automatikusan előfeldolgozná, hogy komplex követő szekvenciákat hozhassunk létre.
```text
Type: Project
CategoryId: 4
Fields: ["CreatedAt"]
ApiUrl: https://$User:$Pass@example.com/TicketAdded/
```

**Példa JSON adatcsomag (Payload):**
```json
{
   "Id": "10",
   "Type": "Project",
   "Data": {
      "CategoryId": "8",
      "ContactId": "16",
      "MainContactId": "15",
      "StatusId": "11684",
      "UserId": "2",
      "Name": "The card name goes here",
      "CreatedBy": "2",
      "CreatedAt": "2018-03-02 23:35:41",
      "UpdatedBy": "2",
      "UpdatedAt": "2018-03-02 23:55:42",
      "InternalUrl": "[https://r3.minicrm.hu/123456/#Project-8/10](https://r3.minicrm.hu/123456/#Project-8/10)"
   },
   "Changed": [
      "StatusId",
      "UserId",
      "Name",
      "UpdatedAt"
   ]
}
```

### Hogyan állítsunk be webhookokat

Jelenleg a webhookok éles-teszt (live-test) üzemmódban futnak. A belső rendszereink is élő környezetben csatlakoznak ennek használatával.
Azonban a webhook beállítások nem érhetők el a webes felületen. Kérjük, vegye fel a kapcsolatot az ügyfélszolgálatunkkal további információkért és a webhook beállításának kéréséért.

Ahogy egyre több valós tapasztalat gyűlik össze, a webhook beállítási lehetőségei és az üzenetek tartalma változhat.

---

## Beépített integrációk

A beépített integrációk többsége kiegészítő modul (add-on), és előfordulhat, hogy nincsenek aktiválva a rendszerében. Használatukhoz aktiválnia kell őket a MiniCRM rendszerének Beállítások -> Előfizetés (Subscription) oldalán.

### MiniSync integráció
A MiniSync kizárólag a Professional és Enterprise platformokon érhető el, és csak éves vagy többéves előfizetések, illetve hűségszerződések révén vehető igénybe. Fontos megjegyezni, hogy az integrációkat gyorsabban tudjuk ütemezni/elvállalni, ha azok kompatibilisek a meglévő infrastruktúránkkal.

A MiniSync egy olyan platform, amelyet az integrációs folyamatok egyszerűsítésére és felgyorsítására terveztek. Egy köztes adatbázist használ a harmadik fél cégektől származó adatok tárolására. Ezeket az adatokat ezután a MiniCRM megfelelő mezőihez rendelik (mapping).

**A MiniSync előnyei**
A SyncFeed beállításakor nem kell eligazodnia a MiniCRM technikai összetettségében. Az adatokat abban a formátumban/struktúrában küldheti be, ahogyan azokat a harmadik féltől kapta, anélkül, hogy szigorúan be kellene tartania a MiniCRM specifikációit. Nincs szükség a 100+ oldalas API dokumentációnk, a MiniCRM adatstruktúrájának teljes megértésére, vagy az azonosítással/duplikációval, API korlátozásokkal stb. való foglalkozásra. Ha van egy "adatfolyama" (data feed) a saját struktúrájában, az nekünk elegendő. Mi gondoskodunk a leképezési logikáról, és beküldjük a MiniCRM rendszerébe.

**Mikor érdemes a MiniSync-et választani?**
Lényeges megérteni, hogy az integráció alapvetően egy együttműködési folyamat, amely magában foglalja az Ön és a harmadik fél (lehetőleg az Ön által használt szoftver egy technikai szakembere) részvételét. Bár eszközünk segíthet az adatok lekérésében egy másik szoftverből, a másik szoftverből származó adatok feldolgozása és visszanyerése nem kizárólag a mi felelősségünk. Ehhez a harmadik fél együttműködésére és támogatására van szükség.

**MiniSync használati esetek**
- Folyamatos adatszinkronizáció két rendszer (pl. MiniCRM és egy ERP) között.
- Korlátozott erőforrások: Ideális olyan ügyfelek számára, akik nem rendelkeznek belső szoftvertervezési vagy fejlesztési kapacitással, és nincs részletes technikai tudásuk a MiniCRM-ről.

**Az Ön feladatai az integrációs folyamatban:**
- Képzelje el és dokumentálja igényeit: Jegyezze fel a követelményeit – adatmezők, munkafolyamat ötletek, és hogy hogyan szeretné a dolgok szinkronizálását.
- Egyeztessen a harmadik féllel az integrációról. Beszéljék meg a technikai részleteket, a díjakat, és biztosítsák, hogy az adatkapcsolat mindig aktív és támogatott maradjon a jövőben.

**A harmadik fél felelősségei:**
Bár különböző csatlakozókat (connector) kínálunk az adatok kinyerésére a harmadik féltől, kulcsfontosságú, hogy ők biztosítsák a folyamatos hozzáférést a naprakész információkhoz.
Ezek a csatlakozási formák a következők:
- **LocalFile Connector** - egyedi végpontot biztosít a harmadik fél számára a friss adatok feltöltéséhez.
- **HttpDownload Connector** - egy végpontot igényel a harmadik féltől, ahonnan letölthetjük a friss adatokat.
- **MySQL Connector** - hozzáférést igényel az adott rendszer adatbázisához a szükséges adatok kinyeréséhez. Ez egy időigényesebb csatlakozási forma, mivel meg kell értenünk az adatbázis struktúráját.

### Naptár integrációk

**Google Naptár**
Az integráció lehetővé teszi, hogy szinkronizálja a MiniCRM feladatait a Google Naptár fiókjával, és megjelenítse azokat a naptárban. Csak be kell állítani az integrációt, és a feladatok automatikusan szinkronizálódnak.
Részletek:
- Románul: https://www.minicrm.ro/help/sincronizarea-cu-calendarul/
- Magyarul: https://www.minicrm.hu/help/google-naptar-szinkronizalas/

**Egyéb naptárak**
A MiniCRM képes feladatok szinkronizálására néhány más naptárral is (például Outlook és/vagy iPhone naptár). Az elérhető naptárak listája idővel változhat.

### Gmail
Ez a kiegészítő lehetővé teszi, hogy közvetlenül a beérkező levelek (inbox) mappájából küldjön e-maileket egy adott ügyfél MiniCRM előzményébe (ehhez először meg kell nyitnia egy e-mailt az ügyféltől). Továbbá új kártyákat, feladatokat is hozzáadhat, vagy megnyithatja a MiniCRM-et közvetlenül a Gmail felületéről.
- Románul: https://www.minicrm.ro/help/integrare-cu-gmail/
- Magyarul: https://www.minicrm.hu/help/minicrm-integracioja-a-gmail-webes-feluletevel/

### Outlook
Ez az integráció nagyon hasonló a Gmail-eshez.
- Románul: https://www.minicrm.ro/help/integrare-outlook/
- Magyarul: https://www.minicrm.hu/help/minicrm-kontaktok-szinkronizalasa-outlook-nevjegyekkel/

### Facebook ads űrlapok
Ezzel a beépített integrációval képes lesz begyűjteni a Facebook fizetett hirdetéseiből származó lead-ek adatait közvetlenül a MiniCRM-be.
- Románul: https://www.minicrm.ro/help/integrarea-cu-facebook-lead-ads/
- Magyarul: https://www.minicrm.hu/help/facebook-lead-ads-connector/

### Gravity forms integráció
Ha WordPress alapú weboldalán keresztül szeretné begyűjteni az összes érdeklődőjét a MiniCRM rendszerébe, egyedi webes űrlap dizájnnal, ez az integráció jelenleg a legjobb módszer. A Gravity forms lehetővé teszi stílusos webes űrlapok létrehozását és azok összekötését a rendszerrel.
- Románul: https://www.minicrm.ro/help/integrare-gravity-forms/
- Magyarul: https://www.minicrm.hu/help/gravity-forms-integracio/

### WooCommerce
Ezt az integrációt abban az esetben használhatja, ha WordPress és WooCommerce webshoppal rendelkezik, hogy a rendeléseit közvetlenül a MiniCRM-be szinkronizálja.
- Románul: https://www.minicrm.ro/help/integrare-woocommerce/
- Magyarul: https://www.minicrm.hu/help/woocommerce-integracio/

### Shoprenter
Ezzel az integrációval az összes rendelését és ügyfelét szinkronizálhatja Shoprenter webshopjából a rendszerében lévő két különböző termékbe: Webshopba és Rendelésekbe (Orders).
- Magyarul: https://www.minicrm.hu/help/shoprenter-integracio/

### UNAS
Ez a webshop integráció több országban is elérhető, és a többi Webshop integrációhoz hasonlóan működik.
- Románul: https://www.minicrm.ro/help/sincronizareunaswebshop/
- Magyarul: https://www.minicrm.hu/help/unas-webshop-integracio/

### OpenApi
Az OpenApi a romániai ListaFirme-hez hasonló szolgáltatás, amely lehetővé teszi a cégek naprakész adatainak lekérését. Az adatok a https://openapi.ro/ adatbázisból származnak. Ez olyan adatokat szinkronizál, mint az Adószám, Cégnév, Cím stb.
- Románul: https://www.minicrm.ro/help/openapi/

### Cégjelző
Ez a mi beépített megoldásunk arra vonatkozóan, hogyan tarthatja naprakészen a MiniCRM rendszer az Ön adatbázisát és a partnercégek adatait a céginformációkat illetően. Ez a megoldás API-n keresztül működik, és a https://cegjelzo.com/ adatbázisból nyeri az adatokat.
- Magyarul: https://www.minicrm.hu/help/ceginfo-szolgaltatas-integracio-cegjelzo-creditinfo/

### NAV (Bejövő számlák)
Ezzel az egyedi integrációval lehetőséget biztosítunk arra, hogy szinkronizálja az összes Önnek kiállított számlát, valamint a partnereket, akik kiállították azokat.
- Magyarul: https://www.minicrm.hu/help/nav-bejovo-szamlak-integracio/

### BestJobs / eJobs integráció
Ezek az integrációk segíthetnek abban, hogy a jelöltekről szóló információkat ezekről a platformokról közvetlenül a MiniCRM rendszerébe gyűjtse be.
- BestJobs: https://www.minicrm.ro/help/integrare-bestjobs/
- eJobs: https://www.minicrm.ro/help/integrare-ejobs/

### SmartBill integráció
Használhatja a MiniCRM és a SmartBill fiókját együttesen; például létrehozhat ajánlatokat vagy rendeléseket a MiniCRM-ből, majd ezen dokumentumok alapján automatikusan generálhatja a számlát a SmartBillben.
- Románul: https://www.minicrm.ro/help/integrare-smart-bill/
