# Számla végpont

Ezzel a végponttal a felhasználó új számlákat hozhat létre, és kezelheti a MiniCRM-ben meglévő számlákat. Emellett számlák keresésére is használható.

Ebben a fejezetben gyakran használjuk a `$InvoiceId` változót. Ez a tényleges dokumentumazonosító, és eltér az adatlap URL-jében megjelenő adatlapazonosítótól.

---

## Új számla létrehozása

Új számla létrehozásához a következőkre van szükség:

- API végpont: `https://r3.minicrm.hu/Api/Invoice`
- Paraméter: `CustomerId` — kapcsolattartó azonosítója — vagy `ReferenceId` — projekt külső azonosítója
- HTTP metódus: `POST`
- Adattípus: `JSON`

Új számla létrehozása csak a rendszeredben már meglévő kapcsolattartóval vagy céggel lehetséges.

Ezeknek két különböző azonosítójuk lehet:

1. `CustomerId` — az azonosító, amelyet a rendszer ad a cég vagy kapcsolattartó létrehozása után.
2. `ReferenceId` — külső azonosító, amelyet nem a MiniCRM hoz létre. Ezt céghez vagy kapcsolattartóhoz tudod beállítani, például kapcsolattartó XML-szinkronnal történő létrehozásakor vagy később API-n keresztül.

A kapcsolattartó létrehozásáról és a címkezelésről további információért lásd a `Company/contact endpoint` és `Address endpoint` fejezeteket.

A számlákon a számlázási cím kerül használatra, és ha nincs számlázási cím, akkor a kapcsolattartó alapértelmezett címe kerül használatra.

A vevői adatok tömbben történő használata opcionális; a vevő adatait közvetlenül a kérésben is megadhatod.

Ha van cím, azt a címet használja a rendszer; ha nincs, akkor új alapértelmezett számlázási címként kerül beállításra.

Ebben az esetben az adószámot és a bankszámlaszámot az API felülírja.

Új számla létrehozásához hitelesítés szükséges, az alábbi példának megfelelően:

```bash
$ curl -s --user $SystemId:$ApiKey -XPOST "https://r3.minicrm.hu/Api/Invoice/" -d '
{
     "Number": "2023-E/00001",
     "Type": "Invoice",
     "Media": "PKI",
     "Customer": {
          "Name": "John Doe",
          "CountryId": "40",
          "PostalCode": "000000",
          "City": "Example City",
          "Address": "Example Street 1",
          "AccountNumber": "12345678-12345678-12345678",
          "VatNumber": "RO654321"
     },
     "Vendor": {
          "Name": "Anonym Company",
          "CountryId": "40",
          "PostalCode": "000000",
          "City": "Anytown",
          "Address": "Any Street 2",
          "AccountNumber": "87654321-87654321-87654321",
          "VatNumber": "RO123456",
          "RegistrationNumber": "",
          "Miscellaneous": ""
     },
     "Subject": "",
     "PaymentMethod": "Cash",
     "Issued": "2023-11-27",
     "Performance": "2023-11-27",
     "Prompt": "2023-11-30",
     "Paid": "0000-00-00",
     "CurrencyCode": "EUR",
     "AmountNet": "2000.00",
     "AmountVat": "540.00",
     "Amount": "2540.00",
     "IsReverseCharge": "0",
     "DocumentId": "66",
     "DocumentUrl": "https://UrlOfTheDocument",
     "DocumentFileName": "Invoice-01-2023-e-00001-1.pdf",
     "Items": [
       {
           "Name": "Product_name",
           "Description": "",
           "PriceNet": "1 000,00",
           "VAT": "19%",
           "Quantity": "2",
           "Unit": "piece",
           "PriceNetTotal": "2 000,00",
           "PriceTotal": "2 380,00"
       }
  ]
}
```

Specifikációk:

- `"Media": "PKI"` az alapértelmezett számlatípust jelenti — elektronikus verzió —, míg a `Paper` a papíralapú számlatípust jelenti.

---

# Számlák keresése

## Számla részletes adatai

Ez a végpont egy megadott számlára keres, és visszaadja a számla összes részletét.

- API végpont: `https://r3.minicrm.hu/Api/Invoice`
- Paraméter: `InvoiceId` — létrehozott számla azonosítója
- HTTP metódus: `GET`
- Adattípus: `JSON`

A kérés URL-je például így néz ki:

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/Invoice/$InvoiceId"
```

A rendszer a számla részleteivel válaszol:

```json
{
  "Id": "12",
  "Number": "2023-E/00014",
  "Type": "Invoice",
  "Media": "PKI",
  "Customer": {
    "Name": "John Doe",
    "CountryId": "40",
    "PostalCode": "000000",
    "City": "Example City",
    "Address": "Example Street 1",
    "AccountNumber": "12345678-12345678-12345678",
    "VatNumber": "RO654321"
  },
  "Vendor": {
    "Name": "Anonym Company",
    "CountryId": "40",
    "PostalCode": "000000",
    "City": "Anytown",
    "Address": "Any Street 2",
    "AccountNumber": "87654321-87654321-87654321",
    "VatNumber": "RO123456",
    "RegistrationNumber": "",
    "Miscellaneous": ""
  },
  "Subject": "",
  "PaymentMethod": "Cash",
  "Issued": "2023-11-27",
  "Performance": "2023-11-27",
  "Prompt": "2023-11-27",
  "Paid": "0000-00-00",
  "CurrencyCode": "EUR",
  "AmountNet": "2000.00",
  "AmountVat": "540.00",
  "Amount": "2540.00",
  "IsReverseCharge": "0",
  "DocumentUrl": "https://UrlOfTheDocument",
  "DocumentFileName": "invoice-2023-e-00014-1.pdf",
  "Items": [
     {
        "Name": "Product_name",
        "Description": "",
        "PriceNet": "1 000,00",
        "VAT": "19%",
        "Quantity": "2",
        "Unit": "piece",
        "PriceNetTotal": "2 000,00",
        "PriceTotal": "2 380,00"
     }
  ]
}
```

---

# Számlalista végpont

A lenti kérés használatával API-n keresztül lekérheted a kiállított számláid listáját. Fontos: a lista nem tartalmazza azokat a számlákat, amelyek nincsenek kiállítva.

A listázás 100 eredményt ad vissza. A lapozás a `Page` paraméterrel végezhető; további részletek a Lapozás fejezetben találhatók.

- API végpont: `https://r3.minicrm.hu/Api/Invoice/List`
- HTTP metódus: `GET`
- Adattípus: `JSON`

```text
https://r3.minicrm.hu/Api/Invoice/List
```

Példa válasz:

```json
{
    "Count": 3,
    "Results": [
       "145": {
         "Id": "123",
         "ProjectId": "1234",
         "StornedId": "",
         "Number": "2024-E-EUR/00001",
         "Type": "Invoice",
         "Issued": "2024-02-19",
         "Performance": "2024-02-15",
         "Prompt": "2024-02-15",
         "Paid": "",
         "CurrencyCode": "EUR",
         "AmountNet": "1599.0000",
         "AmountVat": "432.0000",
         "Amount": "2031.0000",
         "Status": "In Preparation",
         "StatusGroup": "Open",
         "DocumentUrl":""
       },
       {...},
       {...}
    ]
}
```

Megjegyzés: a Stock végpontnál külön is szerepel, hogy az `Invoice/List`, `Offer/List` és `Order/List` végpontok dokumentumlistázásra szolgálnak. A `Company Policies v2` szerint az `Invoice/List` nem listáz ki nem állított számlákat, és a listázás 100 eredményt ad vissza, lapozással a `Page` paraméteren keresztül [2].

---

# Számla frissítése

## Számla fizetettre jelölése

Ezzel a végponttal lehetőség van a számla státuszát `Paid` értékre állítani.

- API végpont: `https://r3.minicrm.hu/Api/Invoice/$InvoiceId/Paid`
- Paraméter: `InvoiceId` — létrehozott számla azonosítója
- HTTP metódus: `POST`
- Adattípus: `JSON`

A kérés URL-je például így néz ki:

```bash
$ curl -s --user $SystemId:$ApiKey -XPOST
"https://r3.minicrm.hu/Api/Invoice/$InvoiceId/Paid"
```

Opcionálisan lehetőség van vevői adatokat tartalmazó objektum használatára is — ha szükséges a vevői adatok módosítása.

```bash
$ curl -s --user $SystemId:$ApiKey -XPOST "https://r3.minicrm.hu/Api/Invoice/16/Paid"
-d '{
      "Customer": {
          "Name": "John Doe",
          "CountryId": "40",
          "PostalCode": "000000",
          "City": "Example City",
          "Address": "Example Street",
          "AccountNumber": "12345678-12345678-12345678",
          "VatNumber": "RO654321"
      },
}'
```

Amikor egy számla fizetettre van jelölve, a rendszer válasza a számlaadatlap részleteit is tartalmazza:

```json
{
  "Id": "17",
  "Number": "2023-E/00016",
  "Type": "Invoice",
  "Media": "PKI",
  "Customer": {
    "Name": "John Doe",
    "CountryId": "40",
    "PostalCode": "000000",
    "City": "Example City",
    "Address": "Example Street 1",
    "AccountNumber": "12345678-12345678-12345678",
    "VatNumber": "RO654321"
  },
  "Vendor": {
    "Name": "Anonym Company",
    "CountryId": "40",
    "PostalCode": "000000",
    "City": "Anytown",
    "Address": "Any Street 2",
    "AccountNumber": "87654321-87654321-87654321",
    "VatNumber": "RO123456",
    "RegistrationNumber": "",
    "Miscellaneous": ""
  },
  "Subject": "Invoice no. EUR-0-00016",
  "PaymentMethod": "Cash",
  "Issued": "2023-11-27",
  "Performance": "2023-11-27",
  "Prompt": "2023-11-27",
  "Paid": "2023-11-27",
  "CurrencyCode": "EUR",
  "AmountNet": "12000.00",
  "AmountVat": "3240.00",
  "Amount": "15240.00",
  "IsReverseCharge": "0",
  "DocumentId": "68",
  "DocumentUrl": "https://UrlOfTheDocument",
  "DocumentFileName": "invoice-2023-e-00016-1.pdf",
  "Items": [
     {
        "Name": "Product_name",
        "Description": "",
        "PriceNet": "12 000,00",
        "VAT": "19%",
        "Quantity": "1",
        "Unit": "piece",
        "PriceNetTotal": "12 000,00",
        "PriceTotal": "14 280,00"
     }
  ]
}
```

---

## Számla sztornózása

Ezzel a végponttal a felhasználó sztornózhat egy adott számlát.

- API végpont: `https://r3.minicrm.hu/Api/Invoice/$InvoiceId/Storno`
- Paraméter: `InvoiceId` — létrehozott számla azonosítója
- HTTP metódus: `POST`
- Adattípus: `JSON`

A kérés URL-je például így néz ki:

```bash
$ curl -s --user $SystemId:$ApiKey -XPOST
"https://r3.minicrm.hu/Api/Invoice/$InvoiceId/Storno"
```

Ebben az esetben a rendszer automatikusan új sztornó számlát generál a megadott számlához.

A válasz a sztornózott számla részleteit tartalmazza:

```json
{
  "Id": "17",
  "Number": "2023-E/00017",
  "Type": "Invoice",
  "Media": "PKI",
  "Customer": {
    "Name": "John Doe",
    "CountryId": "40",
    "PostalCode": "000000",
    "City": "Example City",
    "Address": "Example Street 1",
    "AccountNumber": "12345678-12345678-12345678",
    "VatNumber": "RO654321"
  },
  "Vendor": {
    "Name": "Anonym Company",
    "CountryId": "40",
    "PostalCode": "000000",
    "City": "Anytown",
    "Address": "Any Street 2",
    "AccountNumber": "87654321-87654321-87654321",
    "VatNumber": "RO123456",
    "RegistrationNumber": "",
    "Miscellaneous": ""
  },
  "Subject": "Invoice no. 2023-E/00017 Storno invoice of the invoice no. 2023-E/00016",
  "PaymentMethod": "Cash",
  "Issued": "2023-11-27",
  "Performance": "2023-11-27",
  "Prompt": "2023-11-27",
  "Paid": "2023-11-27",
  "CurrencyCode": "EUR",
  "AmountNet": "12000.00",
  "AmountVat": "3240.00",
  "Amount": "15240.00",
  "IsReverseCharge": "0",
  "DocumentId": "68",
  "DocumentUrl": "https://UrlOfTheDocument",
  "DocumentFileName": "storno_invoice-2023-e-00017.pdf",
  "Items": [
     {
        "Name": "Product_name",
        "Description": "",
        "PriceNet": "12 000,00",
        "VAT": "19%",
        "Quantity": "1",
        "Unit": "piece",
        "PriceNetTotal": "12 000,00",
        "PriceTotal": "14 280,00"
     }
  ]
}
```

---

## Egyedi mezők szerkesztése — számlaadatlap mezői

Ez a végpont lehetővé teszi a felhasználó számára a számlaadatlapon található egyedi mezők kitöltését vagy módosítását.

- API végpont: `https://r3.minicrm.hu/Api/Invoice/$InvoiceId/Project`
- Paraméter: `InvoiceId` — létrehozott számla azonosítója
- HTTP metódus: `POST`
- Adattípus: `JSON`

A kérés URL-je például így néz ki:

```bash
$ curl -s --user $SystemId:$ApiKey -XPOST
"https://r3.minicrm.hu/Api/Invoice/$InvoiceId/Project"
```

A felhasználónak meg kell adnia annak a mezőnek a nevét, amelyet frissíteni szeretne:

```json
{
    "FieldName": "Test",
}
```
