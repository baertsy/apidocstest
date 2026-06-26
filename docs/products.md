# Készlet végpont

Ezzel a végponttal a termékek és/vagy szolgáltatások készleteivel tudsz interakcióba lépni, amelyeket olyan termékek használnak, mint az Invoice, Offer és Orders.

Ezekhez a termékekhez speciális végpontok tartoznak:

- Invoice endpoint: `https://r3.minicrm.hu/Api/Invoice/List`
- Offer endpoint: `https://r3.minicrm.hu/Api/Offer/List`
- Order endpoint: `https://r3.minicrm.hu/Api/Order/List`

Ezek a végpontok csak kiállított dokumentumokat listáznak.

A lapozási korlátok szintén érvényesek.

---

## További lapozási részletek készlethez kapcsolódó termékekhez

Például az invoice termékben — de ez a rendelésekre és ajánlatokra is vonatkozik — azok a számlák jelennek meg először, amelyeket legutóbb módosítottak, nem pedig a számlához kapcsolódó adatlapok:

```json
{
    "Count": 3,
    "Results": [
      "145": {
         "Id": "145",
         "ProjectId": "3529",
         "StornedId": "",
         "Number": "AJ2024-E/00001",
         "Type": "Offer",
         "Issued": "2024-02-19",
         "Performance": "2024-02-15",
         "Prompt": "2024-02-15",
         "Paid": "",
         "CurrencyCode": "HUF",
         "AmountNet": "1599.0000",
         "AmountVat": "432.0000",
         "Amount": "2031.0000",
         "Status": "In Preparation",
         "StatusGroup": "Open",
         "DocumentUrl":
"https://r3.minicrm.hu/xxxxx/Download/S3/?t=doc&inline=0&e=2h99asbfjn1zvi9e7yjw0jas6ewk27"
       },
       {...},
       {...}
    ]
}
```

---

## Lista szűrési paraméterei

### Page — opcionális

```text
https://r3.minicrm.hu/Api/Invoice/List?Page=1
```

### UpdatedSince — opcionális

A számla utolsó módosításának dátuma. Az ennél a dátumnál frissebben módosított számlák kerülnek bele a listába.

```text
https://r3.minicrm.hu/Api/Invoice/List?UpdatedSince=2024-02-12 10:00
```

### StatusGroup — opcionális

A számlához kapcsolódó adatlap aktuális státuszcsoportja.

Lehetséges értékek:

- `Lead`
- `Open`
- `Success`
- `Failed`

```text
https://r3.minicrm.hu/Api/Invoice/List?StatusGroup=Success
```

A szűrők kombinálhatók, hogy pontosabb keresést kapj a dokumentumaidra:

```text
https://r3.minicrm.hu/Api/Invoice/List?StatusGroup=Success&UpdatedSince=2024-02-12 10:00&Page=2
```

---

## Új termék hozzáadása

- API végpont: `https://r3.minicrm.hu/Api/R3/Product/`
- HTTP metódus: `PUT`
- Adattípus: `JSON`

```json
{
     "Name": "Apple Pie",
     "SKU": "APPLE_PIE",
     "EAN": "123456789",
     "FolderName": "Basic Services",
     "VAT": "19%",
     "PriceNet": 10,
     "CurrencyCode": "RON",
     "UnitName": "Unit_Piece",
     "Quantity": 5,
     "Warehouse_Name": "Warehouse Name",
     "Description": "Delicious apple pie.",
     "Deleted": 0
}
```

`SKU`: Ha nem üres, és már létezik a rendszerben, akkor a rendszer az új termék létrehozása helyett a meglévő adatokat frissíti.

`FolderName`: Ha nincs megadva, vagy nem létezik, akkor a rendszerben elsőként rögzített mappába kerül.

Egyedileg létrehozott mezők is beküldhetők.

A válasz tartalmazza a generált MiniCRM termékazonosítót:

```json
{ "Id": 1 }
```

---

## Termék frissítése

- API végpont: `https://r3.minicrm.hu/Api/R3/Product/$Id`
- Paraméter: `Id` — cseréld ki arra a MiniCRM termékazonosítóra, amelyet új termék létrehozásakor kaptál válaszként.
- HTTP metódus: `PUT`
- Adattípus: `JSON`

Az elvárt JSON ugyanaz, mint új termék létrehozásakor. Ha nincs megadva, akkor a `CurrencyCode` esetén az alapértelmezett pénznem kerül használatra — ahogy az adatbázisban tárolva van, például Magyarországon `HUF`, Romániában `RON` stb. —, az ÁFA esetén pedig az adott ország legmagasabb ÁFA-kulcsa — elméletileg mindenhol az alapértelmezett —, amely Magyarországon `27%`, Romániában `19%` stb.

```json
{
    "Name": "Apple Pie (updated)",
    "SKU": "APPLE_PIE",
    "EAN": "123456789",
    "FolderName": "Basic Services",
    "VAT": "19%",
    "PriceNet": 10,
    "CurrencyCode": "RON",
    "UnitName": "Unit_Piece",
    "Quantity": 5,
    "Warehouse_Name": "Warehouse Name",
    "Description": "Delicious apple pie.",
    "Deleted": 0
}
```

Ha a frissítés sikeres volt, a rendszer a termékazonosítóval válaszol:

```json
{ "Id": 1 }
```

---

## Termék törlése

Ahhoz, hogy törölni tudj egy terméket, frissítened kell azt a `Deleted` paraméter használatával, és értékként `1`-et kell megadnod.

- API végpont: `https://r3.minicrm.hu/Api/R3/Product/$Id`
- Paraméter: `Id` — cseréld ki arra a MiniCRM termékazonosítóra, amelyet új termék létrehozásakor kaptál válaszként.
- HTTP metódus: `PUT`
- Adattípus: `JSON`

Az elvárt JSON ugyanaz, mint új termék létrehozásakor. Ha nincs megadva, akkor a `CurrencyCode` esetén az alapértelmezett pénznem kerül használatra — ahogy az adatbázisban tárolva van, például Magyarországon `HUF`, Romániában `RON` stb. —, az ÁFA esetén pedig az adott ország legmagasabb ÁFA-kulcsa — elméletileg mindenhol az alapértelmezett —, amely Magyarországon `27%`, Romániában `19%` stb.

```json
{
    "Deleted": 1
}
```

Ha a törlés sikeres volt, a rendszer a termékazonosítóval válaszol:

```json
{ "Id": 1 }
```
