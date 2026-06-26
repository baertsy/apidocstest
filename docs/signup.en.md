# Integrating Webforms through API with Advanced Webform Addon

In order to use this method, the Advanced Webform Addon must be enabled.

With this, you have the ability to create a webform within your MiniCRM system and connect it with your own custom solution, so that you can send data to your webform via API and create a card in your system.

---

## Key advantages

- **Unified request:** submit business, primary contact and opportunity card data in a single API request.
- **No duplications:** by using the endpoint, MiniCRM automatically identifies existing contacts and assigns new opportunities to them. You do not need to look up the contact by email first in a separate request.
- API endpoint: `https://r3.minicrm.hu/Api/Signup`
- HTTP method: `POST`
- Datatype: `JSON`

---

## Creating a Webform

1. Create a webform in your system with the desired fields included.
2. Get the field names from the HTML code of the form:
   1. Access the HTML code of the webform in the MiniCRM webform editor. Click `To see a detailed description, click here.` under the `Inserting a form into your website` section.
   2. Copy each field’s name.

From this:

```html
<input name="Contact[1891][FirstName]" id="Contact_Name_1331"
language="EN" type="text" />
```

Copy this:

```text
Contact[1891][FirstName]
```

3. Copy the form’s identifier, called `FormHash`.

From this:

```html
<form FormHash="61647-1ot9wrbkba0d0022zptc0m3agmw1vb"
action="https://r3.minicrm.hu/Api/Signup" method="post">
```

Copy this:

```text
61647-1ot9wrbkba0d0022zptc0m3agmw1vb
```

---

## Form submission via API

Example API request using `curl` to send a `POST` request:

```bash
$ curl -s --user $SystemId:$ApiKey -XPOST "https://r3.minicrm.hu/Api/Signup" \
    --form "FormHash=61647-1ot9wrbkba0d0022zptc0m3agmw1vb" \
    --form "Contact[6786][FirstName]=API test" \
    --form "Contact[6786][Email]=api@example.com" \
    --form "ToDo[6788][Comment]=Test"
```

---

## API response explanation

Upon form submission, the API will return a JSON response similar to this:

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

## Response fields

- `Info`: confirmation message indicating successful data processing.
- `Error`: if it is not empty, the submitted data is not saved and must be corrected according to the error message — for example, missing fields, invalid email, etc. The error message can be shown to the end user to help them correct the error.
- `ErrorFieldId`: the ID of the field that did not pass the checks. It can be used to highlight the field for the end user.
- `Data`: contains the essential data hashes and details required for further reference.
