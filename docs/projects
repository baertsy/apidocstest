# Adatlap végpont

Ezzel a végponttal le tudod kérni az adatlapok részleteit — lehetőség adatlapokból, kapcsolattartói és/vagy céges adatlapokból —, valamint frissíteni is tudod az ezeken az adatlapokon található információkat.

---

## Adatlap sémája

Egy adott rendszerben elérhető mezők és azok lehetséges értékei API-n keresztül, a Schema végponton kérdezhetők le.

Példa:

- API végpont: `https://r3.minicrm.hu/Api/R3/Schema/Type`
- Paraméter: a `$Type` helyére `Business`, `Person` vagy `Project/$CategoryId` kerül
- HTTP metódus: `GET`
- Adattípus: `JSON`

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/R3/Schema/Project/20"
```

```json
{
      "AutoSalesV3_Qualification": {
          "421": "Unqualified",
          "422": "Qualified",
          "423": "VIP"
      },
      "AutoSalesV3_SalesStatus": {
          "425": "Discovery",
          "426": "In progress",
          "427": "Won opportunity",
          "428": "Lost opportunity"
      },
      "Name": "Text(512)",
      "StatusUpdatedAt": "DateTime",
      "IsPrivate": "Boolean",
      "Invited": "Text(65535)",
      "Deleted": "Int",
      "UpdatedAt": "DateTime",
      "AutoSalesV3_LeadStep": {
          "2730": "Offer",
          "2731": "Follow-up offer",
          "2733": "Contracting"
      },
      "AutoSalesV3_CustomerStep": {
          "2737": "Implementation",
          "2738": "Closed succesfully",
          "2739": "Feedback"
      },
      "Enum1221": {
          "2673": "Facebook",
          "2675": "SEO",
          "2677": "Google Ads",
          "2774": "LinkedIn"
      }
}
```

A fenti példában csak néhány értéket tüntettünk fel, de a kérés az adott modul összes értékét visszaadja.

---

# Új adatlap létrehozása

Ha teljesen új adatokkal szeretnél adatlapot létrehozni, amelyek még nem léteznek a rendszeredben — cégadatok, kapcsolattartói adatok és lehetőségadatlap —, akkor 3 API-kérést kell végrehajtanod:

- 1. kérés: a céges adatlap létrehozása
- 2. kérés: a kapcsolattartói adatlap létrehozása és hozzárendelése az előzőleg létrehozott cégazonosítóhoz
- 3. kérés: a lehetőségadatlap létrehozása és hozzárendelése az előzőleg létrehozott kapcsolattartóhoz

---

## Új cég létrehozása

Ha a cégkeresés nem ad eredményt, és új céget szeretnél hozzáadni a MiniCRM-hez, ezt az alábbi módon teheted meg.

- API végpont: `https://r3.minicrm.hu/Api/R3/Contact/`
- HTTP metódus: `PUT`
- Adatbevitel: `JSON`

Ki kell töltened a `Type` paramétert `Business` értékkel, valamint a `Name` paramétert.

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/R3/Contact/"-d
{
     "Type": "Business",
     "Name": "MiniCRM Ltd."
}
```

```json
{
     "Id": 256
}
```

---

## Új kapcsolattartó személy létrehozása

Ha új cég kerül hozzáadásra a rendszerhez, ezen a végponton új kapcsolattartó személyt is hozzáadhatsz hozzá.

- API végpont: `https://r3.minicrm.hu/Api/R3/Contact/`
- HTTP metódus: `PUT`
- Adatbevitel: `JSON`

Legalább az alábbi adatokat kell kitölteni:

- `Type` — `Person` értékkel
- `FirstName` — legalább a keresztnév kötelező a MiniCRM-ben
- `BusinessId` — opcionális; akkor kell kitölteni, ha a kapcsolattartót egy meglévő céghez szeretnéd kapcsolni

Kérés:

```bash
$ curl -s --user "https://r3.minicrm.hu/Api/R3/Contact/" -d
{
     "Type": "Person",
     "FirstName": "John",
     "LastName": "Doe",
     "Email": "john.doe@example.com",
     "BusinessId": 256
}
```

Siker esetén a rendszer az újonnan létrehozott kapcsolattartó azonosítójával válaszol:

```json
{
     "Id": 256
}
```

---

# Adatlapok keresése

## Adatlap részletes adatai

Ha rendelkezel az adatlap azonosítójával — vagy API URL-jével —, itt le tudod kérdezni a részletes adatokat. A fenti példákban az adatlapkeresés során `"Id": 160` választ kaptam, ezért ezt fogom használni.

A `ProjectEmail` mezőben a Project egyedi e-mail-címét tároljuk. Ha erre az e-mail-címre levelet küldesz, az archiválásra kerül az adott adatlapon.

A `ProjectHash` mező a Project egyedi, hash-elt azonosítóját tárolja.

- API végpont: `https://r3.minicrm.hu/Api/R3/Project/CardId`
- Paraméter: `CardId` — lehetőségadatlap azonosítója
- HTTP metódus: `GET`
- Adattípus: `JSON`

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/R3/Project/160"
```

```json
{
     Id: 160,
     CategoryId: 20,
     ContactId: 155,
     UserId: "John Doe",
     Name: "Doe Industries LTD (John Doe)",
     StatusUpdatedAt: "2023-03-23 12:56:51",
     IsPrivate: 0,
     Deleted: 0,
     CreatedBy: "John Doe",
     CreatedAt: "2023-03-17 10:40:08",
     UpdatedBy: "John Doe",
     UpdatedAt: "2023-03-23 13:11:31",
     AutoSalesV3_Qualification: "Qualified",
     AutoSalesV3_IsHot: "Yes",
     AutoSalesV3_SalesStatus: "Won opportunity",
     AutoSalesV3_LeadStep: "Follow-up offer",
     AutoSalesV3_CustomerStep: "Closed successfully",
     ClosingDate: "2023-03-22 23:59:59",
     AttachedOffer: "https://r3.minicrm.io/64421/Download/S3/?t=project&inline=",
     OfferDate: "2023-03-21 23:59:59",
     SaleValue: 1500,
     WhyLost: "",
     DetailsWhyLost: "",
     BusinessId: 154,
     ProjectHash: "1bth0t9s2l2bclk3d01q0c7ci4c5ol",
     ProjectEmail: "projectemail@domain.com",
}
```

---

## Egy cég adatlapjai egy modulból

Gyakran szükség van egy cég olyan adatlapjaira, amelyek egy adott modulban vannak. Az adatlapokat a kapcsolattartói válasz `BusinessId` mezőjének értékével találhatod meg. A példaként használt rendszeremben saját magamra a `"BusinessId": 154` választ kaptam, ezért ezt fogom használni.

A modulazonosító — `CategoryId` — értékét a dokumentum elején említett módszerekkel lehet meghatározni.

- API végpont: `https://r3.minicrm.hu/Api/R3/Project?MainContactId=MainContactId&CategoryId=CategoryId`
- Paraméterek:
  - `MainContactId` — a lehetőségadatlap fő kapcsolattartójának azonosítója; kérjük, erről olvass többet a dokumentumban
  - `CategoryId` — annak a terméknek / modulnak az azonosítója, amelyen belül az adatlapokat keresni szeretnéd
- HTTP metódus: `GET`
- Adattípus: `JSON`

```bash
$ curl -s --user $SystemId:$ApiKey
"https://r3.minicrm.hu/Api/R3/Project?MainContactId=154&CategoryId=20"
```

```json
{
     "Count": 1,
     "Results": {
          "160": {
               "Id": 160,
               "Name": "Doe Industries LTD (John Doe)",
               "Url": "https://r3.minicrm.hu/Api/R3/Project/160",
               "ContactId": 155,
               "StatusId": 2813,
               "UserId": 115950,
               "Deleted": 0,
               "BusinessId": 154
          }
     }
}
```

Ha `curl` segítségével teszteled, tedd az URL-t idézőjelek közé, különben az `&` után következő rész elveszik.

---

## Keresés adatlap státusza alapján

Lehetőség van a keresés szűkítésére használt feltételek kombinálására, például egy adott ügyfél adott státuszban lévő adatlapjaira:

- API végpont: `https://r3.minicrm.hu/Api/R3/Project?MainContactId=$MainContactId&StatusId=$StatusId`
- Paraméterek: `MainContactId`, `StatusId`
- HTTP metódus: `GET`
- Adattípus: `JSON`

```bash
$ curl -s --user $SystemId:$ApiKey
"https://r3.minicrm.hu/Api/R3/Project?MainContactId=154&StatusId=2810"
```

Válasz:

```json
{
     "Count": 1,
     "Results": {
          "160": {
              "Id": 160,
              "Name": "Doe Industries LTD (John Doe)",
              "Url": "https://r3.minicrm.hu/Api/R3/Project/160",
              "ContactId": 155,
              "StatusId": 2810,
              "UserId": 115950,
              "Deleted": 0,
              "BusinessId": 154
          }
     }
}
```

---

## Keresés frissítési idő alapján

Ha a rendszerben a lehetőségadatlapokat az alapján szeretnéd listázni, hogy mikor frissültek, ezt termékenként teheted meg.

Ha azokat a lehetőségadatlapokat szeretnéd listázni, amelyek egy adott termékben találhatók és egy adott időpontban frissültek, a következő kérést használhatod.

Ehhez az `UpdatedSince` és `UpdatedAt` paramétereket használhatod.

Amikor megadod a dátumot és időt, növelheted vagy csökkentheted az adott feltétel szigorúságát.

### Általános idő- és dátumformátum

Pontos dátum — év, hónap, nap — és idő — óra, perc, másodperc:

```text
UpdatedAt/Since=yyyy-mm-dd hh:mm:ss
```

Pontos dátum — év, hónap, nap — idő nélkül:

```text
UpdatedAt/Since=yyyy-mm-dd
```

A kéréshez szükséges minimális információ: év, hónap és nap.

---

### 1. `UpdatedSince` paraméter

- API végpont: `https://r3.minicrm.hu/Api/R3/Project?CategoryId=$CategoryId&UpdatedSince=$UpdatedSince`
- Paraméterek:
  - `CategoryId` — termékazonosító
  - `UpdatedSince` — keresett dátum `yyyy-mm-dd hh:mm:ss` formátumban
- HTTP metódus: `GET`
- Adattípus: `JSON`

Ezzel a módszerrel azokat az adatlapokat listázhatod, amelyek egy adott dátum és idő után frissültek.

```bash
$ curl -s --user $SystemId:$ApiKey
"https://r3.minicrm.hu/Api/R3/Project?CategoryId=$Id&UpdatedSince=yyyy-mm-dd hh:mm:ss"
```

Válasz:

```json
{
      "Count": 1,
      "Results": {
           "XXXX": {
               "Id": XXXX,
               "Name": "Project Name 1",
               "Url": "https://r3.minicrm.hu/Api/R3/Project/XXXX",
               "ContactId": XXXX,
               "StatusId": XXXX,
               "UserId": XXXX,
               "Deleted": 0
           }
      }
}
```

---

### 2. `UpdatedAt` paraméter

Ezzel a módszerrel azokat az adatlapokat listázhatod, amelyek egy adott dátumon és időpontban frissültek.

- API végpont: `https:/r3.minicrm.hu/Api/R3/Project?CategoryId=$CategoryId&UpdatedAt=$UpdatedAt`
- Paraméterek:
  - `CategoryId` — termékazonosító
  - `UpdatedAt` — keresett dátum `yyyy-mm-dd hh:mm:ss` formátumban
- HTTP metódus: `GET`
- Adattípus: `JSON`

Példa:

```bash
$ curl -s --user $SystemId:$ApiKey
"https:/r3.minicrm.hu/Api/R3/Project?CategoryId=$Id&UpdatedAt=yyyy-mm-dd hh:mm:ss"
```

Válasz:

```json
{
      "Count": 1,
      "Results": {
           "XXXX": {
               "Id": XXXX,
               "Name": "Project Name 2",
               "Url": "https://r3.minicrm.hu/Api/R3/Project/XXXX",
               "ContactId": XXXX,
               "StatusId": XXXX,
               "UserId": XXXX,
               "Deleted": 0
           }
      }
}
```

---

## Keresés mezők alapján

Lehetőség van egy adott mezőérték alapján keresni, vagy különböző értékeket kombinálni, ahogy korábban bemutattuk.

- API végpont: `https://r3.minicrm.hu/Api/R3/Project?FieldName=SomeValue`
- Paraméterek:
  - `FieldName`
  - `SomeValue` — keresett érték
- HTTP metódus: `GET`
- Adattípus: `JSON`

```bash
$ curl -s --user $SystemId:$ApiKey
"https://r3.minicrm.hu/Api/R3/Project?Enum1221=Facebook"
```

```json
{
    "Count": 1,
    "Results": {
      "260": {
        "Id": 260,
        "Name": "John Doe",
        "Url": "https://r3.minicrm.hu/Api/R3/Project/260",
        "ContactId": 253,
        "StatusId": 2806,
        "UserId": 115950,
        "Deleted": 0,
      },
    },
}
```

---

## Adatlapkeresés — extra

Lehetőséged van olyan adatlapok listájára keresni, amelyekben meghatározott adatok vannak elmentve.

Például, ha azokat az adatlapokat szeretnéd listázni, amelyek a Sales termékben vannak, és a várható bevétel `20000 EUR`.

Végpont:

```text
https://r3.minicrm.hu/Api/R3/Project?CategoryId=$CategoryId&$Customfield=$Value
```

Végpont:

```text
https://r3.minicrm.hu/Api/R3/Project?CategoryId=154&RevenueEur=20000
```

---

## Adatlap státusztörténetének keresése

Lehetőség van egy adatlap státuszváltozási előzményeinek lekérdezésére API-n keresztül.

- API végpont: `https://r3.minicrm.hu/Api/R3/ProjectHistory/$ProjectId/?Type=StatusHistory`
- Paraméter: `ProjectId` — adatlapazonosító
- HTTP metódus: `GET`
- Adattípus: `JSON`

Az adatlap aktuális státusza a `Status_New_Id` változó értéke, a régi státusz pedig a `Status_Old_Id`. A `Status_Old_Name` és `Status_New_Name` az előbbi azonosítókhoz tartozó státusznevek a rendszerben.

```bash
$ curl -s --user $SystemId:$ApiKey
"https://r3.minicrm.hu/Api/R3/ProjectHistory/29592?Type=StatusHistory"
```

Válasz:

```json
{
     "Count": "6",
     "Results": {
          "567": {
              "Id": "567",
              "ProjectId": "160",
              "Type": "StatusHistory",
              "UserId": "0",
              "ClosedAt": "2023-03-23 13:31:20",
              "Status_Old_Id": "2810",
              "Status_New_Id": "2811",
              "Status_Old_Name": "Qualified",
              "Status_New_Name": "VIP"
          },
          "353": {
              "Id": "353",
              "ProjectId": "160",
              "Type": "StatusHistory",
              "UserId": "115950",
              "ClosedAt": "2023-03-17 10:40:22",
              "Status_Old_Id": "2801",
              "Status_New_Id": "2806",
              "Status_Old_Name": "Qualify",
              "Status_New_Name": "Follow-up"
          },
          "352": {
              "Id": "352",
              "ProjectId": "160",
              "Type": "StatusHistory",
              "UserId": "115950",
              "ClosedAt": "2023-03-17 10:40:08",
              "Status_Old_Id": "",
              "Status_New_Id": "2801",
              "Status_Old_Name": "",
              "Status_New_Name": "Qualify"
          }
     }
}
```

---

# API-kérés szűrő tartalmának lekéréséhez

Amikor szűrőt hozol létre a rendszerben, lehetőséged van az eredményeket Excelbe menteni / szinkronizálni.

Lehetőséged van az eredményeket Google Táblázatokkal szinkronizálni, vagy létrehozni egy általános HTTP-kérést ugyanazon adatok lekéréséhez.

Ennek a módszernek a használatához szükséged van a Google Sheet sync fizetős kiegészítőre, amely a `Settings -> Subscription` oldalon érhető el.

Az adatok szinkronizálásának két módja van:

Az egyik lehetőség, hogy a rendszerben létrehozott szűrővel listanézetben az összes adatot szinkronizálod a lehetőségadatlapról és a kapcsolattartói adatlapról.

A másik lehetőség, hogy a lehetőség- és kapcsolattartói adatlapok konkrét adatait látod és szinkronizálod úgy, hogy táblázatos nézetre váltasz, és hozzáadod a konkrét mezőt.

---

## A kérés létrehozása

Ehhez a módszerhez nem kell használnod a rendszerazonosítódat vagy API-kulcsodat.

- Metódus: `GET`
- Végpont: ezt úgy szerezheted meg, hogy az Export opción belül kiválasztod a Google Sheet szinkronizálás lehetőséget.

A képen látható módon az alábbi adatok szinkronizálhatók:

- lehetőségadatlap
- kapcsolattartó személy / cég adatlap
- feladatok / teendők

Itt ki kell másolnod a megosztási linket, és abból a fő végpontrészt kell használnod.

Csak a létrehozott függvény piros részét kell használnod:

```text
=ImportData("https://r3-test.minicrm.eu/Api/SharedFilter?Hash=64421-0e01938fc48a2cfb5f2217fbfb00722d-EO9tXmhLWydgUmz_GTsTxQ&Type=Project")
```

Példa válasz:

```csv
"Card: Id","Card: Name","Card: Responsible","Card: Status","Person1: First name","Person1: Email"
0qb24o3d0s0ejgpmds630tlse4wyg5,"Doe Industries LTD (John Doe)","Test User","First step",John,john.doe@doeindustries.com
```

Ennek a módszernek egy nagy előnye van: egyetlen API-kéréssel több és részletesebb adatot kaphatsz, mint ha ugyanazon eredmény eléréséhez több lekérdezést hoznál létre.

---

## Egy cég összes adatlapja

Gyakran szükség van egy céghez tartozó összes adatlapra. Az adatlapokat a kapcsolattartói válasz `BusinessId` mezőjének értékével találhatod meg.

A példaként használt rendszeremben saját magamra a `"BusinessId": 154` választ kaptam, ezért ezt fogom használni.

- API végpont: `https://r3.minicrm.hu/Api/R3/Project?MainContactId=$MainContactId`
- Paraméter: `MainContactId`
- HTTP metódus: `GET`
- Adattípus: `JSON`

```bash
$ curl -s --user $SystemId:$ApiKey
"https://r3.minicrm.hu/Api/R3/Project?MainContactId=154"
```

```json
{
     "Count": 3,
     "Results": {
         "160": {
             "Id": 160,
             "Name": "Doe Industries LTD (John Doe)",
             "Url": "https://r3.minicrm.hu/Api/R3/Project/160",
             "ContactId": 155,
             "StatusId": 2807,
             "UserId": 115950,
             "Deleted": 0,
             "BusinessId": 154
         },
         "162": {
             "Id": 162,
             "Name": "Mass email sending",
             "Url": "https://r3.minicrm.hu/Api/R3/Project/162",
             "ContactId": 155,
             "StatusId": 2573,
             "UserId": 115950,
             "Deleted": 0,
             "BusinessId": 154
         }
     }
}
```

---

# Adatlap frissítése

Ajánlott csak azokat a mezőket elküldeni, amelyeket módosítani szeretnél.

Ha rendelkezel az adatlap azonosítójával — vagy API URL-jével —, ezen keresztül is módosíthatod az adatokat. A fenti példákban az adatlapkeresés során `"Id": 160` választ kaptam, ezért itt is ezt fogom használni.

- API végpont: `https://r3.minicrm.hu/Api/R3/Project/$CardId`
- Paraméter: `CardId`
- HTTP metódus: `PUT`
- Adattípus: `JSON`

```bash
$ curl -s --user $SystemId:$ApiKey -XPUT "https://r3.minicrm.hu/Api/R3/Project/160" -d
{
      "AutoSalesV3_Qualification": "VIP"
}
```

```json
{
      "Id": 256
}
```

Legördülő mezők, például `StatusId` és `UserId` esetén elküldheted az értéket úgy, ahogy a felhasználói felületen látod, vagy használhatod a rendszerben tárolt egyedi azonosítóját is — amelyik számodra egyszerűbb.

Sikeres mentés után a válasz egy JSON objektum, amely az `Id` tulajdonságban tartalmazza a lehetőségadatlap azonosítóját.

Ha új adatlapot szeretnél hozzáadni, a fenti végpontot szintén HTTP `PUT` metódussal kell meghívnod, és az `Id` mezőt ki kell hagynod.

A beküldött adatok között a kötelező mezőket ki kell tölteni. Az, hogy melyik mező kötelező, a rendszertől, modultól és státusztól függ. Ezt a MiniCRM felhasználói felületén ellenőrizheted, ha manuálisan adsz hozzá új adatlapot a kiválasztott modul kiválasztott státuszához.

---

## Lehetőségadatlap tulajdonosa / felelőse

Amikor olyan JSON-nal hozol létre vagy módosítasz lehetőségadatlapot, amely tartalmazza a felelős felhasználó nevét, a működés a következő:

Lehetőséged van a felhasználó karakterpontosan megadott nevét vagy azonosítóját használni, aki az adott lehetőségadatlap felelőse / tulajdonosa lesz a létrehozáskor vagy frissítéskor.

Példa JSON:

Felhasználónév verzió:

```json
{
    "Id": $ProjectId,
    "CategoryId": $CategoryId,
    "ContactId": $ContactId,
    "StatusId": "Processing",
    "UserId": "Specific User Name",
    "Name": "Test Opportunity card",
....
}
```

Felhasználóazonosító verzió:

```json
{
    "Id": $ProjectId,
    "CategoryId": $CategoryId,
    "ContactId": $ContactId,
    "StatusId": "Processing",
    "UserId": 108637",
    "Name": "Test Opportunity card",
....
}
```

Ha a felhasználó mező értéke elírást tartalmaz, és nincs aktív felhasználó a rendszeredben ezzel a névvel vagy azonosítóval, a kérés sikeresen beküldésre kerül, és a következő választ kapod: `Ok`, `200` kóddal. Ebben az esetben a rendszer véletlenszerűen rendel hozzá egy felhasználót az adott lehetőségadatlaphoz.

Olyan felhasználók esetén, akiknek nincs hozzáférésük az adott termékhez — például Sales —, de a beküldött tömb érvényes felhasználónevet vagy azonosítót tartalmaz, a rendszer automatikusan olyan felhasználóra állítja a felelőst, akinek van hozzáférése az adott termékhez.

Ezzel az API-logikával kevesebb hiba keletkezik a kéréseknél, és hatékonyabb az adatfeldolgozás, mivel a hiányzó lehetőségadatlapok száma minimális lesz, ahelyett, hogy a rendszer hibaüzenetet adna vissza, és elírások miatt nem hozna létre adatlapot.

---

## Lehetőségadatlap áthelyezése ugyanahhoz a céghez kapcsolt másik kapcsolattartóra

- Végpont: `https://r3.minicrm.hu/Api/R3/Project/$ProjectId`
- Metódus: `PUT`
- Példa JSON

Eredeti:

```json
{
"ContactId": 6505
}
```

Módosított:

```json
{
"ContactId": 6623
}
```

---

## Az adatlap fő kapcsolattartójának módosítása, ha az másik céghez kapcsolódik

Ehhez a módosításhoz be kell küldened annak az adatlapnak az új fő kapcsolattartóját, amelyet át szeretnél helyezni, valamint annak a cégnek az azonosítóját is, amelyhez a kapcsolattartó kapcsolódik.

- Végpont: `https://r3.minicrm.hu/Api/R3/Project/$ProjectId`
- Metódus: `PUT`

Példa JSON:

Eredeti:

```json
{
"ContactId": 6505,
"BusinessId": 6504
}
```

Módosított:

```json
{
"ContactId": 1622,
"BusinessId": 1235
}
```

Korlátozás:

A rendszer jelenlegi verziójában nem lehet a kapcsolattartó személyt olyan másik kapcsolattartó személyre módosítani, aki nincs cég entitáshoz kapcsolva.

---

# Fájl feltöltése adatlapra

Az adatlap azonosítójával vagy API URL-jével fájlt tölthetsz fel egy adatlapra. Ez meg fog jelenni az előzményekben, és egy kattintással letölthető lesz. Ehhez a fájlt egy publikus szerveren kell tárolnod — HTTP-hitelesítés használható —, ahol a MiniCRM eléri és le tudja tölteni.

Az adatlapodon el kell helyezned egy `File` oszlopot is.

- API végpont: `https://r3.minicrm.hu/Api/R3/Project/$CardId`
- Paraméter: `CardId`
- HTTP metódus: `PUT`
- Adattípus: `JSON`

```bash
$ curl -s --user $SystemId:$ApiKey -XPUT "https://r3.minicrm.hu/Api/R3/Project/160"-d
{
      "$FileColumnName": "$FileUrl"
}
```

```json
{
      "Id": 160
}
```

Sikeres mentés után a válasz az `Id` adatnév és az adatlap azonosítóértéke lesz.

---

## Több fájl feltöltése adatlapra

Fájlfeltöltés esetén a rendszerből és API-val is kétféleképpen lehet eljárni.

A lehetőségadatlapokon hozzá kell adnod egy fájlmezőt a terméked meglévő mezőihez. Itt lehetőséged van módosítani a fájlmezők működési beállításait.

Először használhatod úgy a mezőt, hogy csak 1 fájlt tartson, és amikor újabb fájl kerül feltöltésre, a rendszer felülírja a régit.

Másodszor használhatod úgy a fájlmezőket, hogy egyszerre több fájlt is tartsanak. Ha ezt szeretnéd, jelöld be a mezőbeállításoknál található jelölőnégyzetet.

---

## Több fájl feltöltése

- API végpont: `https://r3.minicrm.hu/Api/R3/Project/$CardId`
- Paraméter: `CardId`
- HTTP metódus: `PUT`
- Adattípus: `JSON`

```bash
$ curl -s --user $SystemId:$ApiKey -XPUT "https://r3.minicrm.hu/Api/R3/Project/160"-d
{
      "$FileFieldName": "$FileUrl1,$FileUrl2"
}
```

Példakérés:

```bash
$ curl -s --user $SystemId:$ApiKey -XPUT "https://r3.minicrm.hu/Api/R3/Project/160"-d
{
"FileField":"https://file-examples.com/wp-content/storage/2017/02/file_example_XML_24kb.xml,https://file-examples.com/wp-content/storage/2017/02/zip_2MB.zip"
}
```

Példaválasz:

```json
{
    "Id": 6356
}
```

---

# Adatlap törlése

Lehetőséged van a lehetőségadatlapokat API-n keresztül a Kukába helyezni az alábbi módszerrel:

- Végpont: `https://r3.minicrm.hu/Api/R3/Project/$ProjectId`
- Metódus: `PUT`

Példa JSON:

```json
{
      "Deleted": 1
}
```

Fontos: Ezzel a módszerrel nem tudsz véglegesen adatot törölni a rendszeredből. Ehhez be kell jelentkezned a rendszerbe, és az alábbi műveletek egyikét kell elvégezned:

- nyisd meg a Kukát az egyik termékedben
- nyisd meg a törölni kívánt lehetőségadatlapot
- a jobb felső sarokban kattints a piros `Delete permanently` gombra; itt az alábbi módszerekkel törölhetsz véglegesen adatot:
  - törölheted csak az adott lehetőségadatlapot, semmi mást
  - törölheted a kapcsolattartót és az összes hozzá kapcsolt adatlapot

Megjegyzés: tételalapú termékekben, például Offer esetén, nem lehet véglegesen törölni adatlapadatot.

---

## Lekérdezés létrehozása törölt adatlapokhoz

Lehetőséged van lekérni azoknak az adatlapoknak a listáját, amelyeket korábban töröltek az adott termékben.

Nem tudsz egyetlen kéréssel minden törölt adatlapot lekérni az egész rendszeredből; ehhez több kérésre van szükség.

- Végpont: `https://r3.minicrm.hu/Api/R3/Project?CategoryId=$CategoryId&Deleted=1`
- Metódus: `GET`

Példa válasz JSON:

```json
{
     "Count": 2,
     "Results": {
         "8185": {
              "Id": 8185,
              "Name": "Test Contact Project 1",
              "Url": "https://r3.minicrm.hu/Api/R3/Project/8185",
              "ContactId": 6495,
              "StatusId": 5227,
              "UserId": 108637,
              "Deleted": 1
         },
         "8186": {
              "Id": 8186,
              "Name": "Test Contact 2 Project 2",
              "Url": "https://r3.minicrm.hu/Api/R3/Project/8186",
              "ContactId": 6496,
              "StatusId": 5227,
              "UserId": 108637,
              "Deleted": 1
         }
     }
}
```

Itt a `Count` jelzi az adott termékben lévő törölt adatlapok számát, amely termék azonosítóját a végpontban használtad.

Az első lekérdezés válaszában kapott azonosítóval le tudod kérni az adott lehetőségadatlap részleteit és a teendőlistát a Project végponton.

Törölt adatlap státusztörténetét nem tudod ellenőrizni; ezt csak aktív adatlapokkal lehet megtenni.

---

## Törölt adatlapok módosítása

Lehetőségadatlapok esetén akkor is van lehetőséged módosítani az adatlap státuszát, mezőit, vagy létrehozni / módosítani rajta teendőt, ha az adatlap a Kukában van.

A teendőkkel kapcsolatban fontos információ, hogy ha törölt adatlapon hozol létre teendőt, az nem fog megjelenni az adott felhasználó Today nézetében.

Ha adatlapadatot szeretnél módosítani:

- Végpont: `https://r3.minicrm.hu/Api/R3/Project/$ProjectId`
- Metódus: `PUT`

Ha teendőt szeretnél létrehozni vagy módosítani az adatlapon:

- Végpont: `https://r3.minicrm.hu/Api/R3/Todo/`

VAGY

- Végpont: `https://r3.minicrm.hu/Api/R3/Todo/$ToDoId`
- Metódus: `PUT`

---

## Adatlap visszaállítása

Ehhez az általános Project végpontot használhatod. Egy adott adatlap visszaállításához csak a `Deleted` sor értékét kell módosítanod.

- Végpont: `https://r3.minicrm.hu/Api/R3/Project/$ProjectId`
- Metódus: `PUT`

Példa JSON:

```json
{
     "Deleted": 0
}
```

---

# Kapcsolattartó törlése

Egy kapcsolattartó akkor törölhető, ha nem elsődleges kapcsolattartó egyetlen adatlapon sem.

- API végpont: `https://r3.minicrm.hu/Api/R3/PurgePerson/$ContactId`
- Paraméter: `ContactId`
- HTTP metódus: `GET`
- Adattípus: `JSON`

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/R3/PurgePerson/257"
```

Válasz:

```json
{
     "Message": "Successfully deleted"
}
```

Ha egy kapcsolattartó elsődleges kapcsolattartó egy vagy több adatlapon, akkor új elsődleges kapcsolattartót kell beállítanod az adott adatlapon úgy, hogy új `ContactId` értéket küldesz az adott céghez.

---

# Hibakezelés

Ha a beküldendő adat nem megfelelő, a szerver `HTTP 400 Bad Request` választ ad vissza.

Amikor Terminalban próbálsz ki API-hívásokat, alapértelmezés szerint nem látható, hogy a MiniCRM milyen kódot adott vissza — `200` vagy valami más. A `-w` paraméter használatával bármely parancshoz hozzáfűzhető a visszatérési kód.

Hiba esetén a válasz nem JSON-kódolású, hanem egyszerű szöveges hibaüzenet.

Ha az alábbihoz hasonló eset történik, a lehetséges értékeket a fent említett Schema végponton vagy a MiniCRM felhasználói felületén tudod meghatározni.

```bash
$ curl -s --user $SystemId:$ApiKey -w "\nHTTP CODE: %{http_code}\n" -XPUT
"https://r3.minicrm.hu/Api/R3/Project/2056" -d
{
     "AutoSalesV3_Qualification": "Such value cannot be found in the CRM"
}
```

```text
Project (PUT): Invalid value 'Such value cannot be found in CRM' for the Lead qualification field. HTTP CODE: 400
```
