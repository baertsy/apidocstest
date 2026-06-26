# CallLog API végpont

Ez az interfész VOIP szolgáltatók számára készült. A telefonhívások metaadatai elküldhetők a MiniCRM-be a CallLog API-n keresztül, így azok megjelennek az adott ügyfél releváns lehetőségadatlapjának előzmény szekciójában.

Mielőtt kéréseket küldenél erre a végpontra, meg kell adnod az IP-címeket engedélyezéshez. Ehhez kérjük, használd a kapcsolattartási adatainkat.

---

## Új hívásnapló-bejegyzés létrehozása

- URL: `https://r3.minicrm.io/Api/CallLog`
- HTTP metódus: `POST`
- A küldött adatoknak JSON formátumban kell lenniük:
  - `ApiKey`: a VOIP API-kulcs. Az adminisztrátor a MiniCRM / Settings / System menüpontban tudja generálni. Ez egy külön kulcs az „általános” MiniCRM API-kulcstól, amely nem működik a VOIP API-val.
  - `UserExtension`: annak a felhasználónak a belső telefonszáma, aki a hívást kezdeményezte / fogadta. A belső felhasználói számok a MiniCRM / Profile alatt állíthatók be. Ha a PBX nem használ belső számokat, akkor a felhasználó teljes telefonszáma küldhető ebben a mezőben.
  - `Data`: előzménytételek tömbjei, amelyek mindegyike az alábbiakból áll:
    - `Number` — string: a hívás másik résztvevőjének telefonszáma
    - `Duration` — int: a hívás időtartama másodpercben
    - `CallType` — int:
      - `0`: kimenő
      - `1`: bejövő
      - `2`: elveszett
    - `Date` — string/datetime: a hívás kezdési dátuma és ideje UTC időzónában
    - `ReferenceId` — string, opcionális: a rögzített hívás egyedi azonosítója, ha a hívás rögzítésre került

---

### Válasz

Ha minden rendben ment, az API `HTTP 200 / OK` válaszkóddal válaszol.

Hiba esetén `HTTP 4XX` vagy `HTTP 5XX` kódok érkeznek a hiba leírásával együtt.

A válasz egy JSON-kódolt objektum, amely összesítő statisztikákat tartalmaz a feldolgozott előzményrekordokról:

- `Missing`: azoknak a rekordoknak a száma, amelyeknél legalább egy paraméter hiányzik.
- `NotFound`: azoknak a rekordoknak a száma, amelyeknél a másik fél telefonszáma nem található.
- `Processed`: sikeres feldolgozás, az előzményrekordok mentésre kerültek a MiniCRM-ben.

---

### Kérés példa #1

```bash
$ curl -s --user $SystemId:$ApiKey -v -X POST -H "Content-Type: application/json"
"https://r3.minicrm.hu/Api/CallLog" -d '
{
    "ApiKey":"<API_KEY>",
    "UserExtension":"001",
    "Data":[{
        "Date":"2016-03-02 16:00:12",
        "Number":"0620123456",
        "CallType": 2,
        "Duration": 132,
        "ReferenceId":"UniqueReferenceId"
       }]
}'
```

Válasz:

```json
{
      "Skipped": 0,
      "Processed": 1,
      "Exists": 0
}
```

---

### Kérés példa #2

Az első rekord hibákat tartalmaz, a második ismeretlen számról érkezik, a harmadik elem pedig menthető.

```bash
$ curl -s --user $SystemId:$ApiKey -v -XPOST "https://r3.minicrm.hu/Api/CallLog" -d '
  {
     "ApiKey": "<API_KEY>",
     "UserExtension": "001",
     "Data": [
         {
             "Date": "2016-aíYA03-02 16:00:12",
             "CallType": 2,
             "Duration": 132,
             "ReferenceId": "UniqueReferenceId"
         },
         {
             "Date": "2016-03-02 16:00:12",
             "Number": "0620123456",
             "CallType": 2,
             "Duration": 435,
             "ReferenceId": "UniqueReferenceId"
         },
         {
             "Date": "2016-03-03 12:12:12",
             "Number": "0620123456",
             "CallType": 2,
             "Duration": 251,
             "ReferenceId": "UniqueReferenceId"
         }
     ]
}'
```

Válasz példa #2:

```json
{
    "Skipped": 1,
    "Processed": 1,
    "Exists": 1
}
```

---

## Telefonhívások keresése

### Hívások meghallgatása MiniCRM-ben

Ha a hívásokkal együtt a `ReferenceId` is beküldésre kerül, akkor lehetőség van a hívások meghallgatására a MiniCRM-ben.

`ReferenceId` nélkül csak a hívás metaadatai jelennek meg az adatlap előzményeiben. Opcionális funkció a rögzített hívás lejátszása, ha van ilyen.

Két teendő van:

1. Minden rögzített hívásnak egyedi azonosítót kell beküldenie a `ReferenceId` mezőben.
2. Ezután el kell küldeni a rögzített hívás URL-jét a `suport@minicrm.ro` címre. Ennek egy sablonnak kell lennie, amelyet a MiniCRM arra használ, hogy legenerálja azt az URL-t, ahonnan a rögzített hangfájlt le tudja tölteni.

URL példa:

```text
https://yourdomain.com/recordings.php?Rec={%RefereceId%}
```

`HTTPS` — a kommunikáció biztonságos csatornán történik, a rögzített hívások érzékeny adatok.

`yourdomain.com` az a szerver, amelyet a VoIP szolgáltató a felvételek tárolására használ.

A fájlokat az alábbi IP-címekről fogjuk letölteni:

- `195.228.254.37`
- `195.228.254.43`
- `195.228.156.188`

Ajánlott IP-korlátozást beállítani a végponton, hogy biztosított legyen: csak ezek az IP-címek férhetnek hozzá.
