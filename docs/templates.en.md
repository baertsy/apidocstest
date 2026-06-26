# Templates endpoint

The purpose of this endpoint is to provide the possibility to list all templates from a specific product. A detailed view of a specific template is also available.

---

## Get the template list for a specific product

- API endpoint: `https://r3.minicrm.hu/Api/R3/TemplateList/$CategoryId`
- Parameter: `CategoryId` — product ID, as described earlier in the document
- HTTP method: `GET`
- Datatype: `JSON`

The request URL should look something like this:

```bash
$ curl -s --user $SystemId:$ApiKey
"https://r3.minicrm.hu/Api/R3/TemplateList/$CategoryId"
```

The system will respond with the company / contact details:

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

## Search for templates

### Template details

This endpoint displays the details of a specific template.

- API endpoint: `https://r3.minicrm.hu/Api/R3/Template/$TemplateId`
- Parameter: `TemplateId`
- HTTP method: `GET`
- Datatype: `JSON`

The request URL should look something like this:

```bash
$ curl -s --user $SystemId:$ApiKey
"https://r3.minicrm.hu/Api/R3/Template/$TemplateId"
```

Example:

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/R3/Template/2583"
```

The system will respond with the company / contact details:

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
