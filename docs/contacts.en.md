# Company / contact endpoint

First, we need to differentiate contacts from companies. One company can have multiple contacts assigned to it, while one contact can be assigned to only one company at a time.

On the other hand, cards can be assigned either to a contact or to a company, but without a contact. If a company has at least one contact, the card you create should be assigned to that contact.

In MiniCRM, the hierarchy between companies, contacts and cards is structured as shown in the image below:

So, cards are assigned to a contact, and a contact is assigned to a company. There may be special cases where a card is assigned directly to a company, but this is not the most correct operating mode, therefore in integrations you should always try to assign cards to contacts, and contacts to companies.

---

## Create a new company / contact card

In order to create a new company or a new contact, the following are required:

- Send a JSON-encoded request using the `PUT` method to set the data
- API endpoint: `https://r3.minicrm.hu/Api/R3/Contact`
- HTTP method: `PUT`
- Datatype: `JSON`

When creating a new company or contact, the type of the card must be specified:

- `Business` - for companies
- `Person` - for contacts

If you want to assign a contact to an existing company, the `BusinessId` parameter must be used.

---

### Create a new company

In order to create a new company, the company name is mandatory:

```bash
$ curl -XPUT --user $SystemId:$ApiKey https://r3.minicrm.hu/Api/R3/Contact -d '{
    "Name": "Company Name Here",
    "Type": "Business",
    ...
}'
```

In case of success, the system will respond with the ID of the newly created company:

```json
{
    "Id":12345
}
```

---

### Create a new contact

For a contact, at least the first name or the last name is mandatory:

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

In case of success, the system will respond with the ID of the newly created contact:

```json
{
    "Id":12345
}
```

---

## Search for companies / contacts

### Search based on name

This endpoint searches for the specified company or contact name, and returns all companies and/or contacts that match the search parameter.

Companies and contacts can be differentiated based on the `type` parameter.

- API endpoint: `https://r3.minicrm.hu/Api/R3/Contact`
- Parameter: `Name` — company / contact name; a partial name can also be specified
- HTTP method: `GET`
- Datatype: `JSON`

The request URL should look something like this:

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/R3/Contact?Name=Name"
```

The system will respond with the company / contact data. In this example, company data is shown:

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

Example for contact — person — data:

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

### Search based on email address

This endpoint searches for the specified company or contact email address, and returns all companies and/or contacts that match the search parameter.

Companies and contacts can be differentiated based on the `type` parameter.

- API endpoint: `https://r3.minicrm.hu/Api/R3/Contact`
- Parameter: `Email` — company / contact email address
- HTTP method: `GET`
- Datatype: `JSON`

The request URL should look something like this:

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/R3/Contact?Email=Email"
```

The system will respond with the company / contact data:

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

### Search based on phone number

This endpoint searches for the specified company or contact phone number, and returns all companies and/or contacts that match the search parameter.

Companies and contacts can be differentiated based on the `type` parameter.

- API endpoint: `https://r3.minicrm.hu/Api/R3/Contact`
- Parameter: `Phone` — company / contact phone number
- HTTP method: `GET`
- Datatype: `JSON`

The request URL should look something like this:

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/R3/Contact?Phone=Phone"
```

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/R3/Contact?Phone=+40000000000"
```

The system will respond with the company / contact data:

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

### Search based on update date

This endpoint searches for specified companies / contacts that were updated on a given date, and returns all companies and/or contacts that match the search parameter.

Companies and contacts can be differentiated based on the `type` parameter.

- API endpoint: `https://r3.minicrm.hu/Api/R3/Contact`
- Parameter: `UpdatedSince` — company / contact phone number. Usable format: `2023-03-24+10:30:00`; the Hungarian timezone must be used.
- HTTP method: `GET`
- Datatype: `JSON`

The request URL should look something like this:

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/R3/Contact?UpdatedSince=UpdatedSince"
```

The system will respond with the company / contact data:

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

### Search based on a specific keyword

This endpoint searches for the specified keyword in every text field of the company / contact card. The system returns only those results that match the entire keyword, and it does not matter whether you type the keyword in lowercase, uppercase, or with accented letters.

Companies and contacts can be differentiated based on the `type` parameter.

- API endpoint: `https://r3.minicrm.hu/Api/R3/Contact`
- Parameter: `Query`
- HTTP method: `GET`
- Datatype: `JSON`

The request URL should look something like this:

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/R3/Contact?Query=Query"
```

Please check the red-colored text in the output below.

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/R3/Contact?Query=mc"
```

The system will respond with the company / contact data:

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

### Search based on a specific field

This endpoint searches for a specified value in the specified field on the company / contact card. By specific field, we mean here the fields that were added and configured by users on the cards.

Companies and contacts can be differentiated based on the `type` parameter.

You can reread the chapter where we explained how to retrieve the names of custom fields.

- API endpoint: `https://r3.minicrm.hu/Api/R3/Contact`
- Parameter: `FieldName`
- HTTP method: `GET`
- Datatype: `JSON`

The request URL should look something like this:

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/R3/Contact?FieldName=FieldName"
```

In this example, we assume that there is a field named `TestCardContact` in my MiniCRM system, on the contact card, which contains random text, and I want to search for all contacts where this field contains a specific text.

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/R3/Contact?TestCardContact=test"
```

The system will respond with the company / contact data:

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

## Retrieve all contacts from a company

This endpoint compiles the list of all contacts associated with a specified company.

The system also returns the company data among the results.

- API endpoint: `https://r3.minicrm.hu/Api/R3/Contact`
- Parameter: `MainContactId`
- HTTP method: `GET`
- Datatype: `JSON`

The request URL should look something like this:

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/R3/Contact?MainContactId=MainContactId"
```

Example:

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/R3/Contact?MainContactId=37146"
```

The system will respond with the company / contact data:

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

## Detailed data of a contact / company

This endpoint returns the detailed data of a contact or company based on the ID of that contact or company.

- API endpoint: `https://r3.minicrm.hu/Api/R3/Contact`
- Parameter: `Id`
- HTTP method: `GET`
- Datatype: `JSON`

The request URL should look something like this:

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/R3/Contact/$Id"
```

Example:

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/R3/Contact/37146"
```

System response in case of a company:

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

System response in case of a person:

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

## Update company / contact data

This endpoint allows you to update the data of a specified company or contact.

- API endpoint: `https://r3.minicrm.hu/Api/R3/Contact/Id`
- Parameter: `Id`
- HTTP method: `PUT`
- Datatype: `JSON`

The request URL should look something like this in case of updating a company:

```bash
$ curl -s --user $SystemId:$ApiKey -XPUT "https://r3.minicrm.hu/Api/R3/Contact/37147" -d '{
      "Email": "suport@example.com"
}'
```

The request URL should look something like this in case of updating a person:

```bash
$ curl -s --user $SystemId:$ApiKey -XPUT https://r3.minicrm.hu/Api/R3/Contact/37147 -d '{
   "FirstName":"John",
   "LastName":"Doe"
}'
```

In case of a successful update, the system will respond with the ID of that company / contact:

```json
{
      "Id": 37146
}
```

---

## Delete a contact

A contact can only be deleted if it is not set as the primary contact on any card.

- API endpoint: `https://r3.minicrm.hu/Api/R3/PurgePerson/ContactId`
- Parameter: `ContactId`
- HTTP method: `PUT`
- Datatype: `JSON`

```bash
$ curl -s --user $SystemId:$ApiKey -XPUT "https://r3.minicrm.hu/Api/R3/PurgePerson/37148"
```

In case of success, the system will respond with a success message:

```json
{
      "Message": "Successfully deleted"
}
```

In case of error, the system will respond with a helpful message:

```json
{
      "Message": "The person is marked as the main contact on one or more cards."
}
```

Companies cannot be deleted using this method.
