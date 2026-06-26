# Sablonok végpont

Ennek a végpontnak a célja, hogy lehetőséget biztosítson egy adott termék összes sablonjának listázására. Egy adott sablon részletes nézete is elérhető.

---

## Sablonlista lekérése egy adott termékhez

- API végpont: `https://r3.minicrm.hu/Api/R3/TemplateList/$CategoryId`
- Paraméter: `CategoryId` — termékazonosító, a dokumentumban korábban leírtak szerint
- HTTP metódus: `GET`
- Adattípus: `JSON`

A kérés URL-je például így néz ki:

```bash
$ curl -s --user $SystemId:$ApiKey
"https://r3.minicrm.hu/Api/R3/TemplateList/$CategoryId"
```

A rendszer a cég / kapcsolattartó adataival válaszol:

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/R3/TemplateList/111"
```

```json
{
      "Count": 5,
      "Results": [
          {
              "Id": 2647,
              "Type": "Email",
              "Name": "Template with customer name",
              "Url": "https://r3.minicrm.hu/Api/R3/Template/2647"
          },
          {
              "Id": 2646,
              "Type": "Email",
              "Name": "Custom template",
              "Url": "https://r3.minicrm.hu/Api/R3/Template/2646"
          },
          {
              "Id": 2583,
              "Type": "Todo",
              "Name": "Create a custom task",
              "Url": "https://r3.minicrm.hu/Api/R3/Template/2583"
          },
          {
              "Id": 2589,
              "Type": "Sms",
              "Name": "Test",
              "Url": "https://r3.minicrm.hu/Api/R3/Template/2589"
          },
          {
              "Id": 2591,
              "Type": "File",
              "Name": "Test_file",
              "Url": "https://r3.minicrm.hu/Api/R3/Template/2591"
          }
       ]
}
```

---

# Sablonok keresése

## Sablon részletes adatai

Ez a végpont egy adott sablon részleteit jeleníti meg.

- API végpont: `https://r3.minicrm.hu/Api/R3/Template/$TemplateId`
- Paraméter: `TemplateId`
- HTTP metódus: `GET`
- Adattípus: `JSON`

A kérés URL-je például így néz ki:

```bash
$ curl -s --user $SystemId:$ApiKey
"https://r3.minicrm.hu/Api/R3/Template/$TemplateId"
```

Példa:

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/R3/Template/2583"
```

A rendszer a cég / kapcsolattartó adataival válaszol:

```json
{
      "Id": 2583,
      "CategoryId": 111,
      "Type": "To do",
      "Name": "Task template example",
      "Subject": "",
      "Content": "<!DOCTYPE html><html><head><title></title></head><body data-gramm=\"false\" data-lt-tmp-id=\"lt-122144\" spellcheck=\"false\">\n\nThis is a test</body></html>",
      "FolderId": 7621,
      "Folders": [
          7621
      ]
}
```
