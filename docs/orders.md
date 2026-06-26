# Rendelés végpont

Ez a végpont segít a MiniCRM-ben kezelt rendeléseid kezelésében.

A rendelések listázása és a rendelésekben való keresés API-n keresztül még nem támogatott a dokumentum ezen pontja szerint. Ugyanakkor a dokumentum későbbi részében külön rendeléslista végpont is szerepel.

---

## Új rendelés létrehozása

Rendelés létrehozásához JSON-kódolású `POST` kérést kell küldeni. A kérésben a `CustomerId` vagy a `ReferenceId` megadása kötelező.

- API végpont: `https://r3.minicrm.hu/Api/Order`
- HTTP metódus: `POST`
- Adattípus: `JSON`

A rendeléseken a számlázási cím kerül használatra. Ha nincs számlázási cím, akkor helyette a kapcsolattartó alapértelmezett címe kerül használatra.

A `Customer` tömb használata opcionális; itt megadhatod a vevő adatait. Ha van cím, az kerül használatra, ha nincs, akkor új alapértelmezett számlázási címként kerül beállításra.

Ebben az esetben az adószámot és a bankszámlaszámot az API felülírja.

```bash
$ curl -s --user $SystemId:$ApiKey -XPOST "https://r3.minicrm.hu/Api/Order/" -d
{
      "CustomerId": "261",
      "CurrencyCode": "HUF",
      "Customer":
      {
          "Name": "John Doe Ltd.",
          "Country": "36",
          "PostalCode": "0000",
          "City": "AnonymCity",
          "Address": "Anonym Street 1",
          "AccountNumber": "XXXXXXXXXXXXXXXXXXXX",
          "VatNumber": "XXXXXXXXX",
          "EUVatNumber": "XX00000000"
      },
      "Subject": "",
      "Items": [
          {
               "SKU": "",
               "Name": "Cable",
               "Description": "",
               "PriceNet": "500.00",
               "Quantity": "15.00",
               "Unit": "m",
               "VAT": "27"
         }
     ]
}
```

A hitelesítés kötelező. A válasz tartalmazza a rendelés összes adatát és a rendelés azonosítóját — `Order Id`. A válaszban a rendszer a számított mezőket is beállítja, például `AmountVat`, `Amount Net`. A válasz szerkezete hasonló a „Rendelés lekérése” válasz szerkezetéhez.

---

# Rendelések keresése

## Rendelés részletes adatai

- API végpont: `https://r3.minicrm.hu/Api/Order/$OrderId`
- Paraméter: `OrderId`
- HTTP metódus: `GET`
- Adattípus: `JSON`

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/Order/$OrderId"
```

```json
{
      "Id": "83",
      "ProjectId": "272",
      "CustomerId": "261",
      "Number": "",
      "InvoiceMode": "Normal",
      "Customer":
      {
          "Name": "John Doe Ltd.",
          "CountryId": "1",
          "County": "",
          "PostalCode": "0000",
          "City": "Anonym City",
          "Address": "Anonym Street 1",
          "AccountNumber": "92826253-45043722-59650728",
          "VatNumber": "17823443-1-26",
          "EUVatNumber": "HU23982273",
          "RegistrationNumber": "",
          "GroupIdentificationNumber": ""
      },
      "VendorGroupIdentificationNumber": "",
      "Subject": "",
      "PaymentMethod": "Cash",
      "Performance": "2023-09-25",
      "CurrencyCode": "HUF",
      "AmountNet": "7500",
      "AmountVat": "2025",
      "Amount": "9525",
      "Accounting": "Normal",
      "OSS": "0",
      "Items": [
          {
              "Id": "73",
              "SKU": "",
              "EAN": "",
              "Name": "Cable",
              "Description": "",
              "PriceNet": "500.000000",
              "Quantity": "15.0000",
              "Unit": "m",
              "PriceNetTotal": "7500.0000",
              "VAT": "27%",
              "VATSubtype": "",
              "PriceTotal": "9525.00"
          }
      ],
      "Project":
      {
          "Name": "To be issued (John Doe Ltd.)"
      }
}
```

---

# Rendeléslista végpont

A lenti kérés használatával API-n keresztül lekérheted a létrehozott rendeléseid listáját. A listázás 100 eredményt ad vissza. A lapozás a `Page` paraméterrel végezhető; további részletek a Lapozás fejezetben találhatók.

- API végpont: `https://r3.minicrm.hu/Api/Offer/List`
- HTTP metódus: `GET`
- Adattípus: `JSON`

```text
https://r3.minicrm.hu/Api/Offer/List
```

Példa válasz:

```json
{
   "Count": 52,
   "Results": {
     "4": {
       "Id": "4",
       "ProjectId": "24",
       "StornedId": "",
       "Number": "Webshop #24",
       "Type": "Order",
       "Issued": "2021-12-11",
       "Performance": "2021-12-11",
       "Prompt": "2021-12-11",
       "Paid": "",
       "CurrencyCode": "HUF",
       "AmountNet": "766.0000",
       "AmountVat": "214.0000",
       "Amount": "980.0000",
       "Status": "Successful",
       "StatusGroup": "Success",
       "DocumentUrl": ""
     }
   }
}
```

Megjegyzés: a Stock végpont részében a rendeléslista végpontként `https://r3.minicrm.hu/Api/Order/List` szerepel, és ott az is szerepel, hogy ezek a speciális lista végpontok csak kiállított dokumentumokat listáznak. A `Company Policies v2` szerint a rendeléslista végpont nem listáz ki nem állított rendeléseket [2].

---

# Rendelés adatainak frissítése

Rendelés adatainak módosítása csak `Draft` státuszban lehetséges. A `CustomerId` vagy `ReferenceId` beküldése kötelező. A `CurrencyCode` változó szerkesztése nem engedélyezett; ezt a rendelés létrehozásakor kell `POST` kéréssel beküldeni.

- API végpont: `https://r3.minicrm.hu/Api/Order/$OrderId`
- Paraméter: `OrderId`
- HTTP metódus: `POST`
- Adattípus: `JSON`

```bash
$ curl -s --user $SystemId:$ApiKey -XPOST "https://r3.minicrm.hu/Api/Order/$OrderId" -d
{
      "CustomerId": "261",
      "Subject": "Call the customer when the order is ready to go"
}
```

Válaszként a módosított rendelést kapod meg az új `Subject` értékkel.

---

## Rendelési tétel hozzáadása

- API végpont: `https://r3.minicrm.hu/Api/Order/$OrderId`
- Paraméter: `OrderId`
- HTTP metódus: `POST`
- Adattípus: `JSON`

```bash
$ curl -s --user $SystemId:$ApiKey -XPOST "https://r3.minicrm.hu/Api/Order/83" -d
{
      "CustomerId": "261",
      "Items":
          [{
          "SKU": "",
          "Name":"Linksys IP phone",
          "Description": "",
          "PriceNet": "12000.00",
          "Quantity": "3.00",
          "Unit": "piece",
          "VAT": "27"
          }]
}
```

---

## Rendelési tétel szerkesztése

Az `ItemId` megadásával felülírhatod az adott rendelési tételt. Megváltoztathatod magát a tételt vagy annak mennyiségét.

- API végpont: `https://r3.minicrm.hu/Api/Order/$OrderId`
- Paraméter: `OrderId`
- HTTP metódus: `POST`
- Adattípus: `JSON`

```bash
$ curl -s --user $SystemId:$ApiKey -XPOST "https://r3.minicrm.hu/Api/Order/83" -d
{
     "CustomerId": "261",
     "Items":
[
     {
         "Id": 74,
         "SKU": "",
         "Name": "Cisco IP phone",
         "Description": "",
         "PriceNet": "15000.00",
         "Quantity": "2.00",
         "Unit": "piece",
         "VAT": "27"
     }
]}
```

Válaszként a módosított rendelés összes adatát kapod meg, a számított mezők pedig automatikusan frissülnek, például `AmountVat`, `AmountNet`.

Rövidített válaszpélda:

```json
"Items": [
         {
             "Id": "73",
             "SKU": "",
             "EAN": "",
             "Name": "Cable",
             "Description": "",
             "PriceNet": "500.000000",
             "Quantity": "15.0000",
             "Unit": "m",
             "PriceNetTotal": "7500.0000",
             "VAT": "27%",
             "VATSubtype": "",
             "PriceTotal": "9525.00"
         },
         {
             "Id": "74",
             "SKU": "",
             "EAN": "",
             "Name": "Cisco IP phone",
             "Description": "",
             "PriceNet": "15000.000000",
             "Quantity": "2.0000",
             "Unit": "piece",
             "PriceNetTotal": "30000.0000",
             "VAT": "27%",
             "VATSubtype": "",
             "PriceTotal": "38100.00"
         }
     ],
```

---

## Egyedi rendelési mezők szerkesztése

Ha egyedi mezőket hoztál létre a rendelés adatlapján, ezeket státuszkorlátozás nélkül szerkesztheted.

- API végpont: `https://r3.minicrm.hu/Api/Order/$OrderId/Project`
- Paraméter: `OrderId`
- HTTP metódus: `POST`
- Adattípus: `JSON`

```bash
$ curl -s --user $SystemId:$ApiKey -XPOST "https://r3.minicrm.hu/Api/Order/83/Project" -d
'
{
    "Text1479": "Please deliver the order between 9am and 5pm"
}'
```

---

# Rendelés státuszának szerkesztése

## Rendelés véglegesítése

A rendelés státuszát `Draft` értékről `In progress` értékre állítja. A szerkesztés csak akkor működik, ha a rendelés `Draft` státuszban van.

- API végpont: `https://r3.minicrm.hu/Api/Order/$OrderId/Finalize`
- Paraméter: `OrderId`
- HTTP metódus: `POST`
- Adattípus: `JSON`

```bash
$ curl -s --user $SystemId:$ApiKey -XPOST "https://r3.minicrm.hu/Api/Order/83/Finalize"
```

---

## Rendelés szerkesztése

A rendelés státuszát `In progress` értékről `Draft` értékre állítja. A szerkesztés csak akkor működik, ha a rendelés `Draft` státuszban van.

- API végpont: `https://r3.minicrm.hu/Api/Order/$OrderId/Edit`
- Paraméter: `OrderId`
- HTTP metódus: `POST`
- Adattípus: `JSON`

```bash
$ curl -s --user $SystemId:$ApiKey -XPOST "https://r3.minicrm.hu/Api/Order/83/Edit"
```

---

## Rendelés teljesítése

A rendelés státuszát `In progress` értékről `Completed` értékre állítja.

- API végpont: `https://r3.minicrm.hu/Api/Order/$OrderId/Complete`
- Paraméter: `OrderId`
- HTTP metódus: `POST`
- Adattípus: `JSON`

```bash
$ curl -s --user $SystemId:$ApiKey -XPOST "https://r3.minicrm.hu/Api/Order/83/Complete"
```

---

## Rendelés sikeresre állítása

A rendelés státuszát `Completed` vagy `Failed` értékről `Paid` értékre állítja.

- API végpont: `https://r3.minicrm.hu/Api/Order/$OrderId/Paid`
- Paraméter: `OrderId`
- HTTP metódus: `POST`
- Adattípus: `JSON`

```bash
$ curl -s --user $SystemId:$ApiKey -XPOST "https://r3.minicrm.hu/Api/Order/83/Paid"
```

---

## Rendelés sikertelenre állítása

A rendelés státuszát `Completed` vagy `Unsuccessful` értékről `Failed` értékre állítja.

- API végpont: `https://r3.minicrm.hu/Api/Order/$OrderId/Storno`
- Paraméter: `OrderId`
- HTTP metódus: `POST`
- Adattípus: `JSON`

```bash
$ curl -s --user $SystemId:$ApiKey -XPOST "https://r3.minicrm.hu/Api/Order/83/Storno"
```

Státuszmódosításokat csak a fent említett módszerrel lehet végrehajtani.

Például: ha egy `Paid` rendelést `In progress` státuszra szeretnél állítani, az API válasza `405 Method not allowed` lesz.

---

## Rendelési tétel törlése

- API végpont: `https://r3.minicrm.hu/Api/Order/$OrderId/DeleteItem/$ItemId`
- Paraméterek:
  - `OrderId`
  - `ItemId`
- HTTP metódus: `GET`
- Adattípus: `JSON`

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/Order/83/DeleteItem/74"
```

Válaszként a módosított rendelés összes adatát kapod meg, a számított mezők pedig automatikusan frissülnek, például `AmountVat`, `AmountNet`.
