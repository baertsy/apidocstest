# Teendő végpont

Ezzel a végponttal kezelheted a feladataidat — teendőidet —: új feladatokat hozhatsz létre, kereshetsz, frissíthetsz vagy lezárhatsz meglévő feladatokat.

---

## Új teendő létrehozása

A `ProjectId` mező kötelező ahhoz, hogy új teendőt lehessen létrehozni.

- API végpont: `https://r3.minicrm.hu/Api/R3/ToDo/`
- HTTP metódus: `POST`
- Adattípus: `JSON`

Példa URL:

```bash
$ curl -s --user $SystemId:$ApiKey -XPUT "https://r3.minicrm.hu/Api/R3/ToDo/"
```

```json
{
   "UserId":"MiniCRM Helpdesk",
   "ProjectId":"1331",
   "Comment":"This To-Do was created via API.",
   "Status":"Open",
   "Deadline":"2023-02-23 15:35:40",
   "Duration": 60,
   "Reminder": 60,
   "AddressId": 4558,
   "Status":"Open"
}
```

Sikeres beküldés után válaszként egy `Id` értéket kapsz, amely a teendő azonosítóját jelenti.

Válasz:

```json
{
   "Id":1111
}
```

Fontos megjegyezni, hogy új feladat esetén a `ProjectId` mező beküldése kötelező, meglévő feladat esetén pedig a `ProjectId` mező már nem szerkeszthető.

---

## Teendők keresése

- API végpont: `https://r3.minicrm.hu/Api/R3/ToDoList/$CardId`
- Paraméter: `CardId`
- HTTP metódus: `GET`
- Adattípus: `JSON`

Az adatlapot azonosítani kell. Például ezzel az URL-lel lekérdezheted egy adatlap összes teendőjét:

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/R3/ToDoList/1234"
```

A válaszban az eredmény egy blokkban érkezik, ahol a `Count` mutatja a megtalált teendők számát. A `Results` alatt külön blokkokban találhatók a teendők.

A listázott teendők a `Status` paraméterrel szűrhetők státusz szerint. A teendő lehet:

- `Open`
- `Closed`
- `All`

Példa nyitott státuszú teendőlistára:

```bash
$ curl -s --user $SystemId:$ApiKey
"https://r3.minicrm.hu/Api/R3/ToDoList/1234?Status=Open"
```

Válasz:

```json
{
     "Count":2,
     "Results":[
        {
            "Id":"1111",
            "Status":"Open",
            "Comment":"Call the customer and ask for feedback.",
            "Deadline":"2023-03-20 23:59:00",
            "UserId":"3200",
            "Type":"0",
            "Url":"https://r3.minicrm.hu/Api/R3/ToDo/1111"
        },
        {
            "Id":"2222",
            "Status":"Open",
            "Comment":"Check if it needs further action.",
            "Deadline":"2013-01-22 23:59:00",
            "UserId":"3300",
            "Type":"1566",
            "Url":"https://r3.minicrm.hu/Api/R3/ToDo/2222"
        }
     ]
}
```

---

## Egy teendő részletes adatai

- API végpont: `https://r3.minicrm.hu/Api/R3/ToDo/$ToDoId`
- Paraméter: `ToDoId`
- HTTP metódus: `GET`
- Adattípus: `JSON`

Azonosítás szükséges, példa URL:

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/R3/ToDo/1111"
```

A kiválasztott feladat adatainak lekérdezése. Válaszként a teendő egy olyan tömbben érkezik, amely tartalmazza a teendő adatait.

Válasz:

```json
{
     "Id": 3606,
     "UserId": "User A",
     "ContactId": 0,
     "ProjectId": 7888,
     "AddressId": 4558,
     "SenderUserId": 0,
     "Type": "Test type",
     "Duration": 60,
     "Reminder": 60,
     "Status": "Open",
     "Mode": "Task",
     "Deadline": "1970-01-01 01:00:00",
     "Comment": "This is a task created via API",
     "CreatedBy": "MiniCRM",
     "CreatedAt": "2024-08-19 13:54:44",
     "UpdatedBy": "MiniCRM",
     "UpdatedAt": "2024-08-19 13:56:22",
     "ClosedBy": "MiniCRM",
     "ClosedAt": "0000-00-00 00:00:00",
     "Attachments": {
         "FileName": "test contract.pdf",
         "Url": "https://r3.minicrm.hu/…"
     },
     "Members": [
         {
             "Entity": "User2",
             "EntityId": "109287"
         }
     ],
     "Notes": [
         {
             "Comment": "Test comment",
             "CreatedBy": "108637",
             "CreatedAt": "2024-08-19 13:55:00"
         }
      ]
}
```

---

## Teendő frissítése

Meglévő teendő módosítása vagy új létrehozása. Célszerű csak a módosított adatokat újraküldeni, hogy a program hatékonyabban fusson, és elkerülhető legyen, hogy időközben már módosított adatokat korábban letöltött értékekre állíts vissza.

```bash
$ curl -s --user $SystemId:$ApiKey -XPUT "https://r3.minicrm.hu/Api/R3/ToDo/1111" -d '
{
  "Comment":"New text sent in via API",
  "Deadline":"2023-03-25 13:30:00"
}'
```

Csak a nyitott feladatokat lehet módosítani. Lezárt feladatok nem módosíthatók.

A szolgáltatás URL-je megegyezik a letöltés URL-jével. A letöltés `GET` kéréssel indítható, az adatmódosítás pedig `PUT` kéréssel. Új feladat hozzáadása az azonosító elhagyásával lehetséges — például az URL végén szereplő `1111` elhagyásával.

Válasz:

```json
{
    "Id":1111
}
```

---

### Módosítható mezők

- `ContactId`: akkor releváns, ha az ügyfélszolgálati termékben lévő feladat válasz e-mail típusú feladat. Ebben az esetben a `ContactId` a címzett azonosítója.
- `AddressId`: a feladatban megadott cím rendszerazonosítója. Ez az Address végponton keresztül kérdezhető le — lásd a cím végpont fejezetet.
- `Members`: a feladatba bevont érintettek típusa és azonosítója. A Contact végponton keresztül kérdezhető le. Egy feladatban több tag is lehet.
- `Comment`: a megjegyzés szövege.
- `Duration`: megadhatod a feladat elvégzésének tervezett időtartamát. Ez az adat percben értendő.
- `Reminder`: ha a felhasználód kapcsolódik a Google Calendar / Outlook Calendar rendszerhez, értesítéseket kaphatsz. Beállíthatod, hogy az esedékesség előtt hány perccel szeretnél emlékeztetőt kapni.
- `Status`: ez a mező jelzi, hogy a feladat nyitott vagy lezárt. Egy aktív feladat egyszerűen lezárható úgy, hogy ennek a mezőnek az értékét `Closed` értékre módosítod.
- `Attachments`: ha egy elérhető URL-t illesztesz be, amely egy fájl letöltési linkjére mutat, a fájl automatikusan feltöltődik a feladathoz. Ha több mellékletet szeretnél feltölteni, vesszővel `,` választhatod el őket, szóköz nélkül.

---

### Csak olvasható mezők

- `CreatedBy`: létrehozó — `UserId`. A `Schema/Project` végponton keresztül kérdezhető le — lásd az adott fejezetet.
- `CreatedAt`: a feladat létrehozásának idejét adja meg.
- `UpdateAt`: megadja, mikor módosították utoljára a feladatot.
- `UpdatedBy`: melyik felhasználó végezte a legutóbbi módosítást a feladaton.
- `ClosedAt`: mikor lett lezárva a feladat.
- `ClosedBy`: melyik felhasználó zárta le a feladatot.
- `Mode`: értéke lehet `Task` vagy `Email`.
- `Notes`: a feladathoz írt megjegyzések mind ebben a tömbben lesznek listázva.

---

### Általános

- `Attachments`: itt megkapod a legutóbb feltöltött fájl pontos nevét és formátumát, valamint annak letöltési linkjét.

Ha egy nyitott feladatot `POST` metódussal módosítasz, és a `ClosedAt` mezőt adott dátummal küldöd be, miközben a tömb nem tartalmaz `Status` mezőt, a rendszer lezárja ezt a feladatot.

---

### Egyedi lezárási dátum és idő

Lehetőséged van lezárni a feladatot, és egyedi lezárási dátumot hozzáadni. Ha így zársz le egy feladatot, az időrendi sorrendben fog megjelenni az adatlap előzményeiben.

Ez jó megoldás, ha hozzá szeretnéd adni vagy frissíteni szeretnéd a lehetőségadatlap előzményeit, amit más módszerekkel nem lehet megtenni.

Fontos megjegyezni, hogy először létre kell hoznod egy feladatot, hogy kapj egy azonosítót, majd ezt módosíthatod az alábbi példa alapján.

Ez csak akkor működik, ha a feladat normál `Closed` vagy `Failed` státuszban van. Nem fog működni, ha a feladatot a rendszerben `Successful` státuszra zárod.

A `Closed` vagy `Successful` státuszok nem jelennek meg abban a JSON tömbben, amelyet válaszként kapsz; ezek csak a rendszerben, a lehetőségadatlap előzményeiben lesznek láthatók.

- API végpont: `https://r3.minicrm.hu/Api/R3/ToDo/$ToDoId`
- Paraméter: `ToDoId`
- HTTP metódus: `Post`
- Adattípus: `JSON`

Példa kérés:

```json
{
  "Status":"Closed",
  "ClosedAt": "2009-01-09"
}
```

Válasz:

```json
{
   "Id":1111
}
```


## Fájlfeltöltés

- API végpont: `https://r3.minicrm.hu/Api/R3/ToDo/$ToDoId`
- Paraméter: `ToDoId`
- HTTP metódus: `PUT`
- Adattípus: `JSON`

### Egy fájl feltöltése

```json
{
    "Attachments": "https://www.testsite1.com/files/test.pdf"
}
```

### Több fájl feltöltése

```json
{
     "Attachments": "https://www.testsite1.com/files/test.pdf,https://www.testsite2.com/documents/test_2.pdf"
}
```

Lehetőséged van egy vagy több fájlt egyszerre feltölteni egy feladathoz. Ha új fájlt töltesz fel, az hozzáadódik a már feltöltött mellékletekhez, nem írja felül azokat.

Nem tölthetsz fel olyan fájlt vagy fájlokat, amelyek együttes mérete nagyobb mint 24 MB.

---
