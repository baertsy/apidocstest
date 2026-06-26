# Cég / kapcsolattartó végpont

Először meg kell különböztetnünk a kapcsolattartókat a cégektől. Egy céghez több kapcsolattartó is hozzárendelhető, míg egy kapcsolattartó egy időben csak egy céghez rendelhető hozzá.

Másrészt az adatlapok hozzárendelhetők kapcsolattartóhoz vagy céghez is, de kapcsolattartó nélkül. Ha egy céghez legalább egy kapcsolattartó tartozik, akkor a létrehozandó adatlapot ehhez a kapcsolattartóhoz kell hozzárendelni.

A MiniCRM-ben a cégek, kapcsolattartók és adatlapok közötti hierarchia az alábbi ábra szerint épül fel:

Tehát az adatlapok egy kapcsolattartóhoz vannak rendelve, egy kapcsolattartó pedig egy céghez van rendelve. Előfordulhatnak speciális esetek, amikor az adatlap közvetlenül egy céghez van rendelve, de ez nem a leghelyesebb működési mód, ezért integrációkban mindig arra kell törekedni, hogy az adatlapok kapcsolattartókhoz, a kapcsolattartók pedig cégekhez legyenek rendelve.

---

## Új cég / kapcsolattartó adatlap létrehozása

Új cég vagy új kapcsolattartó létrehozásához a következőkre van szükség:

- JSON kódolású kérés küldése `PUT` metódussal az adatok beállításához
- API végpont: `https://r3.minicrm.hu/Api/R3/Contact`
- HTTP metódus: `PUT`
- Adattípus: `JSON`

Új cég vagy kapcsolattartó létrehozásakor meg kell adni az adatlap típusát:

- `Business` - cégek esetén
- `Person` - kapcsolattartók esetén

Ha egy kapcsolattartót egy meglévő céghez szeretnél rendelni, akkor a `BusinessId` paramétert kell használni.

---

### Új cég létrehozása

Új cég létrehozásához a cég neve kötelező:

```bash
$ curl -XPUT --user $SystemId:$ApiKey https://r3.minicrm.hu/Api/R3/Contact -d '{
    "Name": "Company Name Here",
    "Type": "Business",
    ...
}'
```

Siker esetén a rendszer az újonnan létrehozott cég azonosítójával válaszol:

```json
{
    "Id":12345
}
```

---

### Új kapcsolattartó létrehozása

Kapcsolattartó esetén legalább a keresztnév vagy a vezetéknév megadása kötelező:

```bash
$ curl -XPUT --user $SystemId:$APIKey https://r3.minicrm.hu/Api/R3/Contact -d '
{
       "FirstName": "ContactFirstName",
       "LastName": "ContactLastName",
       "BusinessId": 1234, - OPTIONAL (please check the details above)
       "Type": "Person",
       ...
}'
```

Siker esetén a rendszer az újonnan létrehozott kapcsolattartó azonosítójával válaszol:

```json
{
    "Id":12345
}
```

---

## Cégek / kapcsolattartók keresése

### Keresés név alapján

Ez a végpont a megadott cég- vagy kapcsolattartónévre keres, és visszaadja az összes olyan céget és/vagy kapcsolattartót, amely megfelel a keresési paraméternek.

A cégeket és a kapcsolattartókat a `type` paraméter alapján lehet megkülönböztetni.

- API végpont: `https://r3.minicrm.hu/Api/R3/Contact`
- Paraméter: `Name` — cég / kapcsolattartó neve; részleges név is megadható
- HTTP metódus: `GET`
- Adattípus: `JSON`

A kérés URL-je például így néz ki:

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/R3/Contact?Name=Name"
```

A rendszer a cég / kapcsolattartó adataival válaszol. Ebben a példában cégadatok szerepelnek:

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/R3/Contact?Name=John Doe Ltd"
```

```json
{
      "Count": 1,
      "Results": {
           "37146": {
               "Id": 36721,
               "Name": "John Doe Ltd",
               "Url": "https://r3.minicrm.hu/Api/R3/Contact/36721",
               "Type": "Business",
               "Email": "contact@example.com",
               "Phone": "+40000000000"
           }
      }
}
```

Példa kapcsolattartó — személy — adataira:

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/R3/Contact?Name=John Doe"
```

```json
{
     "Count": 1,
     "Results": {
         "37147": {
             "Id": 37147,
             "Name": "John Doe",
             "Url": "https://r3.minicrm.hu/Api/R3/Contact/37147",
             "Type": "Person",
             "Email": "john.doe@example.com",
             "Phone": "",
             "BusinessId": 37146
         }
     }
}
```

---

### Keresés e-mail-cím alapján

Ez a végpont a megadott cég- vagy kapcsolattartói e-mail-címre keres, és visszaadja az összes olyan céget és/vagy kapcsolattartót, amely megfelel a keresési paraméternek.

A cégeket és a kapcsolattartókat a `type` paraméter alapján lehet megkülönböztetni.

- API végpont: `https://r3.minicrm.hu/Api/R3/Contact`
- Paraméter: `Email` — cég / kapcsolattartó e-mail-címe
- HTTP metódus: `GET`
- Adattípus: `JSON`

A kérés URL-je például így néz ki:

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/R3/Contact?Email=Email"
```

A rendszer a cég / kapcsolattartó adataival válaszol:

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/R3/Contact?Email=john.doe@test.com"
```

```json
{
      "Count": 1,
      "Results": {
          "37147": {
              "Id": 37147,
              "Name": "John Doe",
              "Url": "https://r3.minicrm.hu/Api/R3/Contact/37147",
              "Type": "Person",
              "Email": "john.doe@test.com",
              "Phone": "",
              "BusinessId": 37146
          }
      }
}
```

---

### Keresés telefonszám alapján

Ez a végpont a megadott cég- vagy kapcsolattartói telefonszámra keres, és visszaadja az összes olyan céget és/vagy kapcsolattartót, amely megfelel a keresési paraméternek.

A cégeket és a kapcsolattartókat a `type` paraméter alapján lehet megkülönböztetni.

- API végpont: `https://r3.minicrm.hu/Api/R3/Contact`
- Paraméter: `Phone` — cég / kapcsolattartó telefonszáma
- HTTP metódus: `GET`
- Adattípus: `JSON`

A kérés URL-je például így néz ki:

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/R3/Contact?Phone=Phone"
```

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/R3/Contact?Phone=+40000000000"
```

A rendszer a cég / kapcsolattartó adataival válaszol:

```json
{
      "Count": 2,
      "Results": {
          "37146": {
              "Id": 37146,
              "Name": "John Doe Inc.",
              "Url": "https://r3.minicrm.hu/Api/R3/Contact/37146",
              "Type": "Business",
              "Email": "",
              "Phone": "+40000000000"
          },
          "37147": {
              "Id": 37147,
              "Name": "John Doe",
              "Url": "https://r3.minicrm.hu/Api/R3/Contact/37147",
              "Type": "Person",
              "Email": "john.doe@example.com",
              "Phone": "+40000000000",
              "BusinessId": 37146
          }
      }
}
```

---

### Keresés frissítési dátum alapján

Ez a végpont olyan megadott cégekre / kapcsolattartókra keres, amelyek egy adott dátumon frissültek, és visszaadja az összes olyan céget és/vagy kapcsolattartót, amely megfelel a keresési paraméternek.

A cégeket és a kapcsolattartókat a `type` paraméter alapján lehet megkülönböztetni.

- API végpont: `https://r3.minicrm.hu/Api/R3/Contact`
- Paraméter: `UpdatedSince` — cég / kapcsolattartó telefonszáma. Használható formátum: `2023-03-24+10:30:00`; a magyar időzónát kell használni.
- HTTP metódus: `GET`
- Adattípus: `JSON`

A kérés URL-je például így néz ki:

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/R3/Contact?UpdatedSince=UpdatedSince"
```

A rendszer a cég / kapcsolattartó adataival válaszol:

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/R3/Contact?UpdatedSince=2023-03-24+10:30:00"
```

```json
{
      "Count": 2,
      "Results": {
            "37146": {
                "Id": 37146,
                "Name": "John Doe Inc.",
                "Url": "https://r3.minicrm.hu/Api/R3/Contact/37146",
                "Type": "Business",
                "Email": "",
                "Phone": "+40000000000"
            },
            "37147": {
                "Id": 37147,
                "Name": "John Doe",
                "Url": "https://r3.minicrm.hu/Api/R3/Contact/37147",
                "Type": "Person",
                "Email": "john.doe@example.com",
                "Phone": "+40000000000",
                "BusinessId": 37146
            }
      }
}
```

---

### Keresés megadott kulcsszó alapján

Ez a végpont a megadott kulcsszóra keres a cég / kapcsolattartó adatlap minden szöveges mezőjében. A rendszer csak azokat az eredményeket adja vissza, amelyek a teljes kulcsszóra illeszkednek, és nem számít, hogy a kulcsszót kisbetűvel, nagybetűvel vagy ékezetes betűkkel írod-e be.

A cégeket és a kapcsolattartókat a `type` paraméter alapján lehet megkülönböztetni.

- API végpont: `https://r3.minicrm.hu/Api/R3/Contact`
- Paraméter: `Query`
- HTTP metódus: `GET`
- Adattípus: `JSON`

A kérés URL-je például így néz ki:

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/R3/Contact?Query=Query"
```

Kérjük, ellenőrizd az alábbi kimenet piros színű szövegét.

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/R3/Contact?Query=mc"
```

A rendszer a cég / kapcsolattartó adataival válaszol:

```json
{
      "Count": 2,
      "Results": {
          "36590": {
              "Id": 36590,
              "Name": "Test contact",
              "Url": "https://r3.minicrm.hu/Api/R3/Contact/36590",
              "Type": "Person",
              "Email": "john.doe+mc@example.com",
              "Phone": "+40000000000",
              "BusinessId": 275
          },
          "37147": {
              "Id": 37147,
              "Name": "John (MC) Doe (MC)",
              "Url": "https://r3.minicrm.hu/Api/R3/Contact/37147",
              "Type": "Person",
              "Email": "john.doe@example.com",
              "Phone": "+40000000000",
              "BusinessId": 37146
          }
      }
}
```

---

### Keresés megadott mező alapján

Ez a végpont a cég / kapcsolattartó adatlapon lévő megadott mezőben keres megadott értékre. Megadott mező alatt itt azokat a mezőket értjük, amelyeket a felhasználók adtak hozzá és állítottak be az adatlapokon.

A cégeket és a kapcsolattartókat a `type` paraméter alapján lehet megkülönböztetni.

Újra elolvashatod azt a fejezetet, ahol elmagyaráztuk, hogyan lehet lekérni az egyedi mezők neveit.

- API végpont: `https://r3.minicrm.hu/Api/R3/Contact`
- Paraméter: `FieldName`
- HTTP metódus: `GET`
- Adattípus: `JSON`

A kérés URL-je például így néz ki:

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/R3/Contact?FieldName=FieldName"
```

Ebben a példában feltételezzük, hogy van egy `TestCardContact` nevű mező a MiniCRM rendszeremben, a kapcsolattartó adatlapon, amely véletlenszerű szöveget tartalmaz, és meg szeretném keresni az összes olyan kapcsolattartót, amelynél ebben a mezőben szerepel egy adott szöveg.

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/R3/Contact?TestCardContact=test"
```

A rendszer a cég / kapcsolattartó adataival válaszol:

```json
{
      "Count": 3,
      "Results": {
          "44": {
              "Id": 44,
              "Name": "Test John Doe",
              "Url": "https://r3.minicrm.hu/Api/R3/Contact/44",
              "Type": "Person",
              "Email": "john.doe@example.com",
              "Phone": "+40000000001",
              "BusinessId": 275
          },
          "37542": {
              "Id": 37542,
              "Name": "Test Jane Smith",
              "Url": "https://r3.minicrm.hu/Api/R3/Contact/37542",
              "Type": "Person",
              "Email": "",
              "Phone": ""
          },
          "37147": {
              "Id": 37147,
              "Name": "Test Mike Johnson",
              "Url": "https://r3.minicrm.hu/Api/R3/Contact/37147",
              "Type": "Person",
              "Email": "mike.johnson@example.com",
              "Phone": "+40000000002",
              "BusinessId": 37146
          }
      }
}
```

---

## Egy cég összes kapcsolattartójának lekérése

Ez a végpont összeállítja egy megadott céghez társított összes kapcsolattartó listáját.

A rendszer az eredmények között a cég adatait is visszaadja.

- API végpont: `https://r3.minicrm.hu/Api/R3/Contact`
- Paraméter: `MainContactId`
- HTTP metódus: `GET`
- Adattípus: `JSON`

A kérés URL-je például így néz ki:

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/R3/Contact?MainContactId=MainContactId"
```

Példa:

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/R3/Contact?MainContactId=37146"
```

A rendszer a cég / kapcsolattartó adataival válaszol:

```json
{
      "Count": 3,
      "Results": {
          "44": {
              "Id": 44,
              "Name": "John Doe",
              "Url": "https://r3.minicrm.hu/Api/R3/Contact/44",
              "Type": "Person",
              "Email": "john.doe@example.com",
              "Phone": "+40000000001",
              "BusinessId": 275
          },
          "37147": {
              "Id": 37147,
              "Name": "Mike Johnson",
              "Url": "https://r3.minicrm.hu/Api/R3/Contact/37147",
              "Type": "Person",
              "Email": "mike.johnson@example.com",
              "Phone": "+40000000002",
              "BusinessId": 37146
          }
      }
}
```

---

## Kapcsolattartó / cég részletes adatai

Ez a végpont a kapcsolattartó vagy cég azonosítója alapján visszaadja az adott kapcsolattartó vagy cég részletes adatait.

- API végpont: `https://r3.minicrm.hu/Api/R3/Contact`
- Paraméter: `Id`
- HTTP metódus: `GET`
- Adattípus: `JSON`

A kérés URL-je például így néz ki:

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/R3/Contact/$Id"
```

Példa:

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/R3/Contact/37146"
```

Rendszerválasz cég esetén:

```json
{
      "Id": 37146,
      "Type": "Business",
      "Name": "ABC Corporation",
      "Email": "",
      "Phone": "+40 0000 0000",
      "Description": "",
      "Deleted": 0,
      "CreatedBy": "John Doe",
      "CreatedAt": "2022-08-25 12:18:49",
      "UpdatedBy": "John Doe",
      "UpdatedAt": "2023-03-30 09:29:55",
      "Url": "https://www.example.com",
      "BankAccount": "",
      "Swift": "",
      "RegistrationNumber": "",
      "VatNumber": "",
      "Industry": "",
      "Region": "",
      "Employees": 0,
      "YearlyRevenue": 0,
      "EUVatNumber": "",
      "FoundingYear": 0,
      "Capital": 0,
      "MainActivity": "",
      "BisnodeTrafficLight": "",
      "NonGovernmentalOrganization": "No",
      "Tags": []
}
```

Rendszerválasz személy esetén:

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/R3/Contact/37147"
```

```json
{
     "Id": 37147,
     "Type": "Person",
     "FirstName": "John",
     "LastName": "Doe",
     "Email": "john.doe@example.com",
     "Phone": "+40 0000 0000",
     "Description": "",
     "Deleted": 0,
     "CreatedBy": "John Doe",
     "CreatedAt": "2022-08-25 12:18:49",
     "UpdatedBy": "John Doe",
     "UpdatedAt": "2023-04-05 09:56:10",
     "Url": "",
     "BankAccount": "",
     "Swift": "",
     "VatNumber": "",
     "Position": "",
     "Industry": "",
     "Region": "",
     "YearlyRevenue": 0,
     "DataManagement_Consent": "",
     "TestCardContact": "Another test sample text",
     "BusinessId": 37146,
     "Tags": []
}
```

---

## Cégek / kapcsolattartók adatainak frissítése

Ez a végpont lehetővé teszi egy megadott cég vagy kapcsolattartó adatainak frissítését.

- API végpont: `https://r3.minicrm.hu/Api/R3/Contact/Id`
- Paraméter: `Id`
- HTTP metódus: `PUT`
- Adattípus: `JSON`

A kérés URL-je például így néz ki cég frissítése esetén:

```bash
$ curl -s --user $SystemId:$ApiKey -XPUT "https://r3.minicrm.hu/Api/R3/Contact/37147" -d '{
      "Email": "suport@example.com"
}'
```

A kérés URL-je például így néz ki személy frissítése esetén:

```bash
$ curl -s --user $SystemId:$ApiKey -XPUT https://r3.minicrm.hu/Api/R3/Contact/37147 -d '{
   "FirstName":"John",
   "LastName":"Doe"
}'
```

Sikeres frissítés esetén a rendszer az adott cég / kapcsolattartó azonosítójával válaszol:

```json
{
      "Id": 37146
}
```

---

## Kapcsolattartó törlése

Egy kapcsolattartó csak akkor törölhető, ha nincs elsődleges kapcsolattartóként beállítva egyetlen adatlapon sem.

- API végpont: `https://r3.minicrm.hu/Api/R3/PurgePerson/ContactId`
- Paraméter: `ContactId`
- HTTP metódus: `PUT`
- Adattípus: `JSON`

```bash
$ curl -s --user $SystemId:$ApiKey -XPUT "https://r3.minicrm.hu/Api/R3/PurgePerson/37148"
```

Siker esetén a rendszer sikeres üzenettel válaszol:

```json
{
      "Message": "Successfully deleted"
}
```

Hiba esetén a rendszer hasznos üzenettel válaszol:

```json
{
      "Message": "The person is marked as the main contact on one or more cards."
}
```

Cégek ezzel a módszerrel nem törölhetők.
