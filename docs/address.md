# Cím végpont

Ezzel a végponttal fizikai címeket, azaz postai címeket lehet kezelni.

A MiniCRM-ben kapcsolattartóknak és cégeknek is lehet fizikai címe.

---

## Új cím létrehozása

Új cím létrehozásához a következőkre van szükség:

- API végpont: `https://r3.minicrm.hu/Api/R3/Address`
- HTTP metódus: `PUT`
- Adattípus: `JSON`

Új cím létrehozásához meg kell adni egy kapcsolattartó / cég azonosítót. A MiniCRM-ben a címet kapcsolattartóhoz vagy céghez kell kapcsolni. Az alábbi `ContactId` paraméter tartalmazhat kapcsolattartó- vagy cégazonosítót; cég esetén sem változik a paraméter neve.

```bash
$ curl -s --user $SystemId:$ApiKey -XPUT https://r3.minicrm.hu/Api/R3/Address -d '{
    "ContactId": 12345,
    "Type": "Headquarter",
    "Name": "Address name",
    "CountryId": "Romania",
    "County": "John Doe",
    "City": "John Doe",
    "PostalCode": 000000,
    "Address": "John Doe, nr. 00",
    "Default": 1
    }
```

Siker esetén a rendszer az újonnan létrehozott cím azonosítójával válaszol:

```json
{
    "Id":12345
}
```

---

## Cím keresése

### Keresés kapcsolattartó / cég azonosító alapján

- API végpont: `https://r3.minicrm.hu/Api/R3/AddressList/ContactId`
- Paraméter: `ContactId` — meglévő kapcsolattartó- vagy cégazonosító
- HTTP metódus: `GET`
- Adattípus: `JSON`

A kérés URL-je például így néz ki:

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/R3/AddressList/ID"
```

Az `ID` paraméter tartalmazhat kapcsolattartó-azonosítót vagy cégazonosítót.

A rendszer a címadatokkal válaszol:

```bash
$ curl -s --user $SystemId:$ApiKey https://r3.minicrm.hu/Api/R3/AddressList/12
```

```json
{
      "Results": {
          "307": "NoName ( John Doe Address)",
          "310": "Office Address (John Doe Address)"
      },
      "Count": 2
}
```

A címek részletei szükség esetén strukturált válaszban is lekérhetők:

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/R3/AddressList/ID?Structured=1"
```

A rendszer strukturált formában válaszol a címadatokkal:

```json
{
      "Results": {
          "307": {
              "Id": 307,
              "Address": "No Name (John Doe)",
              "Url": "https://r3.minicrm.hu/Api/R3/Address/307"
          },
          "310": {
              "Id": 310,
              "Address": "Test Address (John Doe Test Street, no. 123)",
              "Url": "https://r3.minicrm.hu/Api/R3/Address/310"
          }
      },
      "Count": 2
}
```

---

### Keresés cím azonosító alapján

- API végpont: `https://r3.minicrm.hu/Api/R3/Address/AddressId`
- Paraméter: `AddressId` — meglévő cím azonosítója
- HTTP metódus: `GET`
- Adattípus: `JSON`

A kérés URL-je például így néz ki:

```bash
$ curl https://r3.minicrm.hu/Api/R3/Address/AddressId
```

A rendszer a cím részleteivel válaszol:

```bash
curl -s --user $SystemId:$ApiKey https://r3.minicrm.hu/Api/R3/Address/307
```

```json
{
      "Id": 307,
      "ContactId": 37979,
      "Type": "",
      "Name": "",
      "CountryId": "Romania",
      "PostalCode": "",
      "City": "Test",
      "County": "Test",
      "Address": "John Doe Street, no. 1",
      "Default": 1,
      "CreatedBy": "John Doe",
      "CreatedAt": "2023-09-29 08:35:46",
      "UpdatedBy": "John Doe",
      "UpdatedAt": "2023-09-29 08:35:46"
}
```

---

## Cím frissítése

- API végpont: `https://r3.minicrm.hu/Api/R3/Address/AddressId`
- Paraméter: `AddressId` — meglévő cím azonosítója
- HTTP metódus: `PUT`
- Adattípus: `JSON`

A kérés URL-je például így néz ki:

```bash
$ curl -XPUT "https://r3.minicrm.hu/Api/R3/Address/AddressId"
```

Ez frissíti a címet:

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/R3/Address/307"
```

```json
{
    "Address": "Test Street, no. 1"
}
```

Siker esetén a rendszer a frissített cím azonosítójával válaszol:

```json
{
    "Id":12345
}
```
