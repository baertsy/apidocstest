# Ajánlat végpont

Ezen a végponton lehetőséged van a MiniCRM-ben lévő ajánlataid kezelésére. API-kérést hozhatsz létre ajánlat beküldésére a rendszerbe JSON-kódolt formátumban vagy XML-fájlban.

Ezen a végponton keresztül az alábbiakra van lehetőség:

- ajánlat létrehozása a rendszeredben
- az ajánlat státuszának módosítása
- információ módosítása vagy hozzáadása az adatlapmezőkhöz

---

# Ajánlatok létrehozása és kiállítása

A rendszer az ajánlatok esetében hasonló szerkezetű kérést tud feldolgozni, mint amilyen választ ajánlatok lekérésekor kapsz. A funkció `POST` kéréssel használható. Az adatokat a rendszer JSON-kódolt formátumban tudja feldolgozni. Ajánlatok esetén a `CustomerId` és a `ReferenceId` kötelezően megadandó információ.

- API végpont: `https://r3.minicrm.hu/Api/Offer`
- HTTP metódus: `POST`
- Adattípus: `JSON`

Az ügyfelek esetében a számlázási cím kerül használatra. Ha nincs ilyen cím, akkor az alapértelmezett cím kerül használatra az ajánlatokon. A `Customer` blokk használata a kérésen belül opcionális; itt megadhatod az ügyfél adatait. Ha a cím már létezik a kapcsolattartói információk között, azt használjuk, ha nem, akkor új — alapértelmezett — számlázási címként kerül mentésre.

Az adószámot és a bankszámlaszámot az API felülírja. Az ajánlatok az első, alapértelmezett státuszban jönnek létre, amely: `Preparing offer`.

A kérésben a hitelesítés kötelező. A `CustomerId` vagy a `ReferenceId` megadása kötelező. A `CurrencyCode` változó szerkesztése nem engedélyezett; ezt az ajánlat létrehozásakor kell `POST` kéréssel beküldeni.

Példakérés:

```bash
$ curl -s --user $SystemId:$ApiKey -XPOST "http://r3.minicrm.hu/Api/Offer/" -d '
{
    "CustomerId": "20404",
    "InvoicePadCounter": "00005",
    "Number": "2024-E/00001",
    "Type": "Offer",
    "Media": "PKI",
    "Language": "RO",
    "Customer": {
        "Name": "John Doe",
        "CountryId": "40",
        "Country": "Romania",
        "PostalCode": "000000",
        "City": "Test",
        "Address": "Test Address",
        "AccountNumber": "",
        "VatNumber": "",
        "EUVatNumber": "",
        "RegistrationNumber": "",
        "GroupIdentificationNumber": ""
    },
    "Vendor": {
        "Name": "John Doe",
        "CountryId": "40",
        "PostalCode": "000000",
        "City": "Test",
        "Address": "John Doe Street, no. 1",
        "AccountNumber": "",
        "IBAN": "",
        "SWIFT": "",
        "VatNumber": "XXXXXX",
        "EUVatNumber": "ROXXXXXX",
        "GroupIdentificationNumber": "",
        "RegistrationNumber": ""
    },
    "ExtraData": "",
    "Subject": "",
    "PaymentMethod": "WiredTransfer",
    "Issued": "2024-01-01",
    "Performance": "2024-01-08",
    "Prompt": "2024-01-08",
    "Paid": "",
    "CurrencyCode": "RON",
    "ConversionRate": "",
    "AmountNet": "23.00",
    "AmountVat": "0.00",
    "Amount": "23.00",
    "VATMode": "NormalVAT",
    "Accounting": "Normal",
    "DocumentId": "6",
    "DocumentUrl":
"https://r3-test.minicrm.eu/documents/XYZ/Download/S3/?t=doc&inline=0&e=XYZ",
    "Items": [
        {
            "SKU": "",
            "EAN": "",
            "Name": "Test",
            "Description": "",
            "PriceNet": "23.0000",
            "Quantity": "1.00",
            "Unit": "piece",
            "PriceNetTotal": "23",
            "VAT": "19",
            "VATSubtype": "",
            "PriceTotal": "23.00"
        }
    ],
    "Project": {
        "String1911": "Test"
    }
}'
```

Az API-válasz átfogó rendelési adatokat tartalmaz, beleértve az egyedi rendelésazonosítót is. Ez a válasz dinamikusan számított mezőkkel is kiegészül, például `AmountVat` és `Amount Net`. A válasz szerkezete megegyezik a „Rendelés lekérése” válasz szerkezetével.

---

# Ajánlatok keresése

## Ajánlat részletes adatai

- API végpont: `https://r3.minicrm.hu/Api/Offer/OfferId`
- Paraméter: `OfferId`
- HTTP metódus: `GET`
- Adattípus: `JSON`

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/Offer/932"
```

```json
{
    "Id": "932",
    "InvoicePadCounter": "00008",
    "Number": "AJ2021-E/00008",
    "Type": "Offer",
    "InvoiceMode": "Normal",
    "Media": "PKI",
    "Language": "HU",
    "Customer": {
      "Name": "John Doe",
      "CountryId": "36",
      "County": "",
      "PostalCode": "XXXX",
      "City": "Budapest",
      "Address": "XXX",
      "AccountNumber": "",
      "VatNumber": "",
      "EUVatNumber": "",
      "RegistrationNumber": "",
      "GroupIdentificationNumber": ""
    },
    "Vendor": {
      "Name": "John Doe INC",
      "CountryId": "36",
      "PostalCode": "XXXX",
      "City": "XXX",
      "Address": "XXX",
      "AccountNumber": "XXXXXXXXXXXXXXXXXXXXX",
      "IBAN": "XXXXXXXXXXXXXXXXXXXXX",
      "SWIFT": "XXXXXXXX",
      "VatNumber": "XXXXXXXXX",
      "EUVatNumber": "XXXXXXXX",
      "GroupIdentificationNumber": "",
      "RegistrationNumber": ""
    },
    "ExtraData": "{\"VendorMiscellaneous\":\"XXXXXXXX\"}",
    "Subject": "",
    "PaymentMethod": "WiredTransfer",
    "Issued": "2021-10-15",
    "Performance": "2021-10-15",
    "Prompt": "2021-10-15",
    "Paid": "",
    "CurrencyCode": "HUF",
    "ConversionRate": "",
    "AmountNet": "23",
    "AmountVat": "0",
    "Amount": "23",
    "VATMode": "VATExempt",
    "Accounting": "Normal",
    "DocumentId": "819",
    "DocumentUrl":
"https://r3.minicrm.hu/41274/Download/S3/?t=doc&inline=0&e=XXXXXXXXXX",
    "Items": [
       {
          "Id": "1306",
          "SKU": "",
          "EAN": "",
          "Name": "testr",
          "Description": "",
          "PriceNet": "23.0000",
          "Quantity": "1.00",
          "Unit": "darab",
          "PriceNetTotal": "23",
          "PriceTotal": "23"
       }
    ],
    "Project": {
       "Enum1907": "",
       "Float1910": "",
       "Int1909": "",
       "Int1912": "",
       "String1911": "TEST",
       "String1913": "",
       "String1914": "",
       "String1915": "",
       "StatusId": "3426"
    }
}
```

---

# Ajánlatlista végpont

A lenti kérés használatával API-n keresztül lekérheted a létrehozott ajánlataid listáját. A listázás 100 eredményt ad vissza. A lapozás a `Page` paraméterrel végezhető; további részletek a Lapozás fejezetben találhatók.

- API végpont: `https://r3.minicrm.hu/Api/Offer/List`
- HTTP metódus: `GET`
- Adattípus: `JSON`

```text
https://r3.minicrm.hu/Api/Offer/List
```

Példa válasz:

```json
{
    "Count": 12,
    "Results": {
      "9": {
        "Id": "9",
        "ProjectId": "264",
        "StornedId": "",
        "Number": "AJ2022-E/00001",
        "Type": "Offer",
        "Issued": "2022-04-14",
        "Performance": "2022-02-03",
        "Prompt": "2022-02-03",
        "Paid": "",
        "CurrencyCode": "EUR",
        "AmountNet": "678.0000",
        "AmountVat": "183.0000",
        "Amount": "862.0000",
        "Status": "Accepted",
        "StatusGroup": "Success",
        "DocumentUrl": ""
      }
    }
}
```

Megjegyzés: a Stock végpontnál az szerepel, hogy az `Offer/List` speciális végpont dokumentumokat listáz. A `Company Policies v2` szerint az ajánlatlista a ki nem állított ajánlatokat is listázza [2].

---

# Ajánlat adatainak frissítése

Az ajánlat teljes módosítása csak `Draft` státuszú példányoknál korlátozott, és az ilyen szerkesztéseket kizárólag a rendszereden belül kell kezdeményezni. Ezzel szemben, amikor az ajánlat a rendszerben jön létre, továbbra is rugalmasan módosíthatod a tételeket, a lehetőségadatlap mezőit, és kiállíthatsz módosított ajánlatokat anélkül, hogy teljesen új ajánlatot kellene létrehoznod az elejétől.

Fontos megjegyezni, hogy bizonyos műveletek, többek között az alábbiak, nem hajthatók végre API-kérésekkel:

- a lehetőségadatlap — ajánlat — tulajdonosának módosítása
- tétel törlése
- tétel módosítása
- új tétel hozzáadása

Szerkezet:

```json
{
"FieldName": "Value"
}
```

Példakérés:

```bash
$ curl -s --user $SystemId:$ApiKey -POST "https://r3.minicrm.hu/Api/Offer/10/Project"
-d '
{
"StatusId": "Paid",
"String1616": "Test"
}'
```
