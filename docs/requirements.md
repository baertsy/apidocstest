# KÖVETELMÉNYEK ÉS TECHNIKAI INFORMÁCIÓK

## Követelmények
Az API használatához hitelesítés szükséges. Az autentikációs folyamathoz az alábbi két elemre van szükség:
1. **SystemID:** (Részletek a Basics fejezetben).
2. **API kulcs:** (Részletek a Basics fejezetben).

> **Fontos:** Erősen ajánlott az integráció megkezdése előtt futtatni a kézikönyvben bemutatott példákat, mivel ez segít jobban megérteni a megvalósításunk működését.

---

## Oldalazás (Pagination)
Azok a végpontok, amelyek lehetővé teszik a rendszerben történő szűrést/keresést, egységes keretrendszer szerint adják vissza az eredményeket.

Egy API keresés esetén egy oldalon 100 eredmény jelenik meg. A `Page` paraméter használatával lehet lapozni. Az API esetén a számozás 0-tól indul, így a második oldal a `Page=1` paraméterrel érhető el.

**Példa:**
```bash
curl -s --user $SystemId:$ApiKey "[https://r3.minicrm.hu/Api/R3/Project?CategoryId=5&Page=1](https://r3.minicrm.hu/Api/R3/Project?CategoryId=5&Page=1)"
```

- **Count:** A válaszban az eredmények egy tömbben érkeznek, a darabszám a `Count` kulcs alatt található.
- **Results:** Az eredmények külön tömbökben találhatók. Csak az alapvető adatok szerepelnek itt.
- **Url:** Használja az `Url` mezőben kapott címet az API-n keresztül elérhető összes információ lekéréséhez.

---

## cUrl használat
Windows 10 1803-as (2018. április) vagy újabb verzióján a curl alapértelmezetten telepítve van. Ha segítségre van szüksége a telepítéshez, látogassa meg a [https://everything.curl.dev/](https://everything.curl.dev/) oldalt.

A MiniCRM REST API formázatlan JSON választ küld. A válaszok könnyebb megértéséhez használhatja a `jq` (https://jqlang.github.io/jq/) eszközt:
```bash
curl -s --user $SystemId:$ApiKey "[https://r3.minicrm.hu/Api/R3/Category](https://r3.minicrm.hu/Api/R3/Category)" | jq
```

> **Emlékeztető:** A MiniCRM API Unicode kódlapot és UTF-8 kódolást használ minden bejövő paraméternél és válasznál.

---

## Postman és HTTPie
* **Postman:** Az autentikációs típust állítsa "Basic"-re, ahol a felhasználónév a `SystemID`, a jelszó pedig az `API kulcs`. Az API végpontokat a teljes URL helyett a bázis URL-től kezdve használja (pl. `https://r3.minicrm.hu/Api/R3/Contact?Name=Name`).
* **HTTPie:** Egy online Rest API eszköz, amely hasonlóan működik a Postmanhez, de telepítés nélkül, közvetlenül a böngészőből használható.

---

## Hibakódok és hibakezelés
A MiniCRM API szabványos HTTP hibakódokat használ:

* **200 (OK):** A kérés sikeres volt.
* **400 (Bad Request):** Fel nem ismert paraméterek vagy hiányzó kötelező paraméterek. (Megjegyzés: Az API **kis- és nagybetűérzékeny**!)
* **401 (Unauthorized):** Helytelen SystemID vagy API kulcs, esetleg a csomagja nem tartalmazza a REST API elérést.
* **404 (Not Found):** A megadott URL nem található (elírás).
* **405 (Method not allowed):** Rossz HTTP metódus (pl. POST helyett PUT).
* **429 (Too many requests):** Túllépte az API limiteket (60 kérés/perc).
* **500 (Internal Server Error):** Belső feldolgozási hiba (pl. érvénytelen érték beállítása egy mezőhöz).

---

## API Limitek
1. **60 kérés / perc:** Minden standard API végpontra (https://r3.minicrm.hu/Api/R3).
2. **180 kérés / óra:** XML szinkronizációs kérésekre.
3. **Nincs limit:** A számla végpontra (https://r3.minicrm.hu/Api/Invoice).

---

## Speciális korlátozások
* **Ajánlat (Offer):** Nem adható hozzá, módosítható vagy törölhető ajánlati tétel.
* **Feladatok (To-do):** Csak megnyitott vagy lezárt feladatokhoz kérhető le adat (újra nem nyithatók API-n keresztül), és nem hozhatók létre ismétlődő feladatok.
* **Sablonok:** Nem hozható létre új sablon API-n keresztül.
* **Kapcsolatok:** Nem helyezhetők át cégek között API-n keresztül.
* **Címek és Cégek:** Nem törölhetők API-n keresztül.
