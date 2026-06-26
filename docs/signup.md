# Webformok integrálása API-n keresztül Advanced Webform Addonnal

Ahhoz, hogy ezt a módszert használni tudd, az Advanced webform addonnak be kell kapcsolva lennie.

Ezzel lehetőséged van webformot létrehozni a MiniCRM rendszereden belül, és összekapcsolni azt a saját egyedi megoldásoddal, hogy API-n keresztül küldj adatokat a webformodba, és adatlapot hozz létre a rendszeredben.

---

## Fő előnyök

- **Egységes kérés:** cég-, elsődleges kapcsolattartó- és lehetőségadatlap-adatok beküldése egyetlen API-kérésben.
- **Nincs duplikáció:** a végpont használatával a MiniCRM automatikusan azonosítja a meglévő kapcsolattartókat, és az új lehetőségeket hozzájuk rendeli. Nem kell külön kérésben e-mail alapján kapcsolattartót keresned.
- API végpont: `https://r3.minicrm.hu/Api/Signup`
- HTTP metódus: `POST`
- Adattípus: `JSON`

---

# Webform létrehozása

1. Hozz létre egy webformot a rendszeredben a kívánt mezőkkel.
2. Szerezd meg a mezőneveket az űrlap HTML-kódjából:
   1. Nyisd meg a webform HTML-kódját a MiniCRM webform szerkesztőjében. Kattints erre: `To see a detailed description, click here.` az `Inserting a form into your website` rész alatt.
   2. Másold ki minden mező nevét.

Ebből:

```html
<input name="Contact[1891][FirstName]" id="Contact_Name_1331"
language="EN" type="text" />
```

Ezt másold ki:

```text
Contact[1891][FirstName]
```

3. Másold ki az űrlap azonosítóját, amelynek neve `FormHash`.

Ebből:

```html
<form FormHash="61647-1ot9wrbkba0d0022zptc0m3agmw1vb"
action="https://r3.minicrm.hu/Api/Signup" method="post">
```

Ezt másold ki:

```text
61647-1ot9wrbkba0d0022zptc0m3agmw1vb
```

---

# Űrlap beküldése API-n keresztül

Példa API-kérés `curl` használatával `POST` kérés küldésére:

```bash
$ curl -s --user $SystemId:$ApiKey -XPOST "https://r3.minicrm.hu/Api/Signup" \
    --form "FormHash=61647-1ot9wrbkba0d0022zptc0m3agmw1vb" \
    --form "Contact[6786][FirstName]=API test" \
    --form "Contact[6786][Email]=api@example.com" \
    --form "ToDo[6788][Comment]=Test"
```

---

# API-válasz magyarázata

Űrlapbeküldés után az API ehhez hasonló JSON-választ ad vissza:

```json
{
  "Url": "",
  "Info": "The provided data has been successfully processed.",
  "Warning": "",
  "Error": "",
  "Message": "",
  "ErrorFieldId": "",
  "Data": {
       "ProjectHash": "<PROJECT_HASH>",
       "ContactHash": "<CONTACT_HASH>",
       "Email": "api@example.com",
       "Name": "John Doe"
    }
}
```

```json
{
  "Url": "",
  "Info": "The provided data has been successfully processed.",
  "Warning": "",
  "Error": "",
  "Message": "",
  "ErrorFieldId": "",
  "Data": {
       "ProjectHash": "<PROJECT_HASH>",
       "ContactHash": "<CONTACT_HASH>",
       "Email": "api@example.com",
       "Name": "API test"
    }
}
```

---

## Válaszmezők

- `Info`: megerősítő üzenet, amely jelzi a sikeres adatfeldolgozást.
- `Error`: ha nem üres, akkor a beküldött adat nincs mentve, és az hibaüzenet szerint javítandó — például hiányzó mezők, érvénytelen e-mail stb. A hibaüzenet megjeleníthető a végfelhasználónak, hogy segítsen kijavítani a hibát.
- `ErrorFieldId`: annak a mezőnek az azonosítója, amely nem ment át az ellenőrzéseken. Használható a mező kiemelésére a végfelhasználó számára.
- `Data`: a további hivatkozáshoz szükséges alapvető adathasheket és részleteket tartalmazza.
