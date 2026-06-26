# Card endpoint

Using this endpoint, you can retrieve card details — from opportunity cards, contact and/or company cards — and you can also update the information on these cards.

---

## Schema of the card

The available fields and their possible values in a given system can be queried via API through the Schema endpoint.

Example:

- API endpoint: `https://r3.minicrm.hu/Api/R3/Schema/Type`
- Parameter: in place of `$Type`, use `Business`, `Person` or `Project/$CategoryId`
- HTTP method: `GET`
- Datatype: `JSON`

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/R3/Schema/Project/20"
```

```json
{
      "AutoSalesV3_Qualification": {
          "421": "Unqualified",
          "422": "Qualified",
          "423": "VIP"
      },
      "AutoSalesV3_SalesStatus": {
          "425": "Discovery",
          "426": "In progress",
          "427": "Won opportunity",
          "428": "Lost opportunity"
      },
      "Name": "Text(512)",
      "StatusUpdatedAt": "DateTime",
      "IsPrivate": "Boolean",
      "Invited": "Text(65535)",
      "Deleted": "Int",
      "UpdatedAt": "DateTime",
      "AutoSalesV3_LeadStep": {
          "2730": "Offer",
          "2731": "Follow-up offer",
          "2733": "Contracting"
      },
      "AutoSalesV3_CustomerStep": {
          "2737": "Implementation",
          "2738": "Closed succesfully",
          "2739": "Feedback"
      },
      "Enum1221": {
          "2673": "Facebook",
          "2675": "SEO",
          "2677": "Google Ads",
          "2774": "LinkedIn"
      }
}
```

In the example above, only a few values are shown, but the request returns all values of the given module.

---

## Create a new card

If you want to create a card with completely new data that does not yet exist in your system — company data, contact data and opportunity card — you need to perform 3 API requests:

- 1st request: create the company card
- 2nd request: create the contact card and assign it to the previously created company ID
- 3rd request: create the opportunity card and assign it to the previously created contact

---

### Create a new company

If the company search does not return any results and you would like to add a new company to MiniCRM, you can do it as follows.

- API endpoint: `https://r3.minicrm.hu/Api/R3/Contact/`
- HTTP method: `PUT`
- Data input: `JSON`

You must fill in the `Type` parameter with the value `Business`, as well as the `Name` parameter.

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/R3/Contact/"-d
{
     "Type": "Business",
     "Name": "MiniCRM Ltd."
}
```

```json
{
     "Id": 256
}
```

---

### Create a new contact person

If a new company is added to the system, you can also add a new contact person to it on this endpoint.

- API endpoint: `https://r3.minicrm.hu/Api/R3/Contact/`
- HTTP method: `PUT`
- Data input: `JSON`

You must fill in at least the following data:

- `Type` — with the value `Person`
- `FirstName` — at least the first name is required in MiniCRM
- `BusinessId` — optional; it should be filled in if you want to connect the contact to an existing company

Request:

```bash
$ curl -s --user "https://r3.minicrm.hu/Api/R3/Contact/" -d
{
     "Type": "Person",
     "FirstName": "John",
     "LastName": "Doe",
     "Email": "john.doe@example.com",
     "BusinessId": 256
}
```

In case of success, the system will respond with the ID of the newly created contact:

```json
{
     "Id": 256
}
```

---

## Search for cards

### Detailed data of a card

If you have the card identifier — or API URL — you can query the detailed data here. In the examples above, I received `"Id": 160` during the card search, so I will use this.

The `ProjectEmail` field stores the unique email address of the Project. If you send an email to this email address, it will be archived to that card.

The `ProjectHash` field stores the Project’s unique, hashed ID.

- API endpoint: `https://r3.minicrm.hu/Api/R3/Project/CardId`
- Parameter: `CardId` — opportunity card ID
- HTTP method: `GET`
- Datatype: `JSON`

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/R3/Project/160"
```

```json
{
     Id: 160,
     CategoryId: 20,
     ContactId: 155,
     UserId: "John Doe",
     Name: "Doe Industries LTD (John Doe)",
     StatusUpdatedAt: "2023-03-23 12:56:51",
     IsPrivate: 0,
     Deleted: 0,
     CreatedBy: "John Doe",
     CreatedAt: "2023-03-17 10:40:08",
     UpdatedBy: "John Doe",
     UpdatedAt: "2023-03-23 13:11:31",
     AutoSalesV3_Qualification: "Qualified",
     AutoSalesV3_IsHot: "Yes",
     AutoSalesV3_SalesStatus: "Won opportunity",
     AutoSalesV3_LeadStep: "Follow-up offer",
     AutoSalesV3_CustomerStep: "Closed successfully",
     ClosingDate: "2023-03-22 23:59:59",
     AttachedOffer: "https://r3.minicrm.io/64421/Download/S3/?t=project&inline=",
     OfferDate: "2023-03-21 23:59:59",
     SaleValue: 1500,
     WhyLost: "",
     DetailsWhyLost: "",
     BusinessId: 154,
     ProjectHash: "1bth0t9s2l2bclk3d01q0c7ci4c5ol",
     ProjectEmail: "projectemail@domain.com",
}
```

---

### A company’s cards from one module

It is often necessary to retrieve a company’s cards that are in a given module. You can find the cards using the value of the `BusinessId` field from the contact response. In the example system used here, I received `"BusinessId": 154` for myself, so I will use this.

The module identifier — `CategoryId` — can be determined using the methods mentioned at the beginning of the document.

- API endpoint: `https://r3.minicrm.hu/Api/R3/Project?MainContactId=MainContactId&CategoryId=CategoryId`
- Parameters:
  - `MainContactId` — the main contact ID of the opportunity card; please read more about this in the document
  - `CategoryId` — the ID of the product / module within which you want to search for cards
- HTTP method: `GET`
- Datatype: `JSON`

```bash
$ curl -s --user $SystemId:$ApiKey
"https://r3.minicrm.hu/Api/R3/Project?MainContactId=154&CategoryId=20"
```

```json
{
     "Count": 1,
     "Results": {
          "160": {
               "Id": 160,
               "Name": "Doe Industries LTD (John Doe)",
               "Url": "https://r3.minicrm.hu/Api/R3/Project/160",
               "ContactId": 155,
               "StatusId": 2813,
               "UserId": 115950,
               "Deleted": 0,
               "BusinessId": 154
          }
     }
}
```

If you test it with `curl`, put the URL between quotation marks, otherwise the part after `&` will be lost.

---

### Search based on card status

It is possible to combine the criteria used to narrow the search, for example, a specific customer’s cards in a specific status:

- API endpoint: `https://r3.minicrm.hu/Api/R3/Project?MainContactId=$MainContactId&StatusId=$StatusId`
- Parameters: `MainContactId`, `StatusId`
- HTTP method: `GET`
- Datatype: `JSON`

```bash
$ curl -s --user $SystemId:$ApiKey
"https://r3.minicrm.hu/Api/R3/Project?MainContactId=154&StatusId=2810"
```

Response:

```json
{
     "Count": 1,
     "Results": {
          "160": {
              "Id": 160,
              "Name": "Doe Industries LTD (John Doe)",
              "Url": "https://r3.minicrm.hu/Api/R3/Project/160",
              "ContactId": 155,
              "StatusId": 2810,
              "UserId": 115950,
              "Deleted": 0,
              "BusinessId": 154
          }
     }
}
```

---

### Search based on update time

If you want to list opportunity cards in the system based on when they were updated, you can do this by product.

If you want to list the opportunity cards that are located in a specific product and were updated at a specific time, you can use the following request.

For this, you can use the `UpdatedSince` and `UpdatedAt` parameters.

When you provide the date and time, you can increase or decrease the strictness of the specific condition.

#### General time and date format

Exact date — year, month, day — and time — hour, minute, second:

```text
UpdatedAt/Since=yyyy-mm-dd hh:mm:ss
```

Exact date — year, month, day — without time:

```text
UpdatedAt/Since=yyyy-mm-dd
```

The minimum information required for the request is: year, month and day.

---

#### 1. `UpdatedSince` parameter

- API endpoint: `https://r3.minicrm.hu/Api/R3/Project?CategoryId=$CategoryId&UpdatedSince=$UpdatedSince`
- Parameters:
  - `CategoryId` — product ID
  - `UpdatedSince` — searched date in `yyyy-mm-dd hh:mm:ss` format
- HTTP method: `GET`
- Datatype: `JSON`

With this method, you can list the cards that were updated after a specific date and time.

```bash
$ curl -s --user $SystemId:$ApiKey
"https://r3.minicrm.hu/Api/R3/Project?CategoryId=$Id&UpdatedSince=yyyy-mm-dd hh:mm:ss"
```

Response:

```json
{
      "Count": 1,
      "Results": {
           "XXXX": {
               "Id": XXXX,
               "Name": "Project Name 1",
               "Url": "https://r3.minicrm.hu/Api/R3/Project/XXXX",
               "ContactId": XXXX,
               "StatusId": XXXX,
               "UserId": XXXX,
               "Deleted": 0
           }
      }
}
```

---

#### 2. `UpdatedAt` parameter

With this method, you can list the cards that were updated at a specific date and time.

- API endpoint: `https:/r3.minicrm.hu/Api/R3/Project?CategoryId=$CategoryId&UpdatedAt=$UpdatedAt`
- Parameters:
  - `CategoryId` — product ID
  - `UpdatedAt` — searched date in `yyyy-mm-dd hh:mm:ss` format
- HTTP method: `GET`
- Datatype: `JSON`

Example:

```bash
$ curl -s --user $SystemId:$ApiKey
"https:/r3.minicrm.hu/Api/R3/Project?CategoryId=$Id&UpdatedAt=yyyy-mm-dd hh:mm:ss"
```

Response:

```json
{
      "Count": 1,
      "Results": {
           "XXXX": {
               "Id": XXXX,
               "Name": "Project Name 2",
               "Url": "https://r3.minicrm.hu/Api/R3/Project/XXXX",
               "ContactId": XXXX,
               "StatusId": XXXX,
               "UserId": XXXX,
               "Deleted": 0
           }
      }
}
```

---

### Search based on fields

It is also possible to search based on a specific field value, or to combine different values, as shown earlier.

- API endpoint: `https://r3.minicrm.hu/Api/R3/Project?FieldName=SomeValue`
- Parameters:
  - `FieldName`
  - `SomeValue` — searched value
- HTTP method: `GET`
- Datatype: `JSON`

```bash
$ curl -s --user $SystemId:$ApiKey
"https://r3.minicrm.hu/Api/R3/Project?Enum1221=Facebook"
```

```json
{
    "Count": 1,
    "Results": {
      "260": {
        "Id": 260,
        "Name": "John Doe",
        "Url": "https://r3.minicrm.hu/Api/R3/Project/260",
        "ContactId": 253,
        "StatusId": 2806,
        "UserId": 115950,
        "Deleted": 0,
      },
    },
}
```

---

### Card search — extra

You have the ability to search for a list of cards that have specific data saved in them.

For example, if you want to list the cards that are in the Sales product and the expected revenue is `20000 EUR`.

Endpoint:

```text
https://r3.minicrm.hu/Api/R3/Project?CategoryId=$CategoryId&$Customfield=$Value
```

Endpoint:

```text
https://r3.minicrm.hu/Api/R3/Project?CategoryId=154&RevenueEur=20000
```

---

### Search the card status history

It is possible to query a card’s status change history via API.

- API endpoint: `https://r3.minicrm.hu/Api/R3/ProjectHistory/$ProjectId/?Type=StatusHistory`
- Parameter: `ProjectId` — card ID
- HTTP method: `GET`
- Datatype: `JSON`

The current status of the card is the value of the `Status_New_Id` variable, and the old status is `Status_Old_Id`. `Status_Old_Name` and `Status_New_Name` are the status names in the system that belong to the identifiers above.

```bash
$ curl -s --user $SystemId:$ApiKey
"https://r3.minicrm.hu/Api/R3/ProjectHistory/29592?Type=StatusHistory"
```

Response:

```json
{
     "Count": "6",
     "Results": {
          "567": {
              "Id": "567",
              "ProjectId": "160",
              "Type": "StatusHistory",
              "UserId": "0",
              "ClosedAt": "2023-03-23 13:31:20",
              "Status_Old_Id": "2810",
              "Status_New_Id": "2811",
              "Status_Old_Name": "Qualified",
              "Status_New_Name": "VIP"
          },
          "353": {
              "Id": "353",
              "ProjectId": "160",
              "Type": "StatusHistory",
              "UserId": "115950",
              "ClosedAt": "2023-03-17 10:40:22",
              "Status_Old_Id": "2801",
              "Status_New_Id": "2806",
              "Status_Old_Name": "Qualify",
              "Status_New_Name": "Follow-up"
          },
          "352": {
              "Id": "352",
              "ProjectId": "160",
              "Type": "StatusHistory",
              "UserId": "115950",
              "ClosedAt": "2023-03-17 10:40:08",
              "Status_Old_Id": "",
              "Status_New_Id": "2801",
              "Status_Old_Name": "",
              "Status_New_Name": "Qualify"
          }
     }
}
```

---

## API request to get the contents of a filter

When you create a filter in the system, you have the option to save / synchronize the results to Excel.

You have the option to synchronize the results with Google Sheets, or to create a general HTTP request to retrieve the same data.

In order to use this method, you need the paid Google Sheet sync add-on, which is available on the `Settings -> Subscription` page.

There are two ways to synchronize the data:

One option is to synchronize all data from the opportunity card and the contact card in list view using a filter created in the system.

The other option is to view and synchronize specific data from the opportunity and contact cards by switching to table view and adding the specific field.

---

### Creating the request

For this method, you do not need to use your system ID or API key.

- Method: `GET`
- Endpoint: you can get it by selecting the Google Sheet synchronization option inside the Export option.

As shown in the picture, the following data can be synchronized:

- opportunity card
- contact person / company card
- tasks / todos

Here you need to copy the sharing link and use the main endpoint part from it.

You only need to use the red part from the created function:

```text
=ImportData("https://r3-test.minicrm.eu/Api/SharedFilter?Hash=64421-0e01938fc48a2cfb5f2217fbfb00722d-EO9tXmhLWydgUmz_GTsTxQ&Type=Project")
```

Example response:

```csv
"Card: Id","Card: Name","Card: Responsible","Card: Status","Person1: First name","Person1: Email"
0qb24o3d0s0ejgpmds630tlse4wyg5,"Doe Industries LTD (John Doe)","Test User","First step",John,john.doe@doeindustries.com
```

One major advantage of this method is that you can get more and more detailed data in a single API request than if you created multiple queries to obtain the same result.

---

## All cards of a company

It is often necessary to retrieve all cards that belong to a company. You can find the cards using the value of the `BusinessId` field from the contact response.

In the example system used here, I received `"BusinessId": 154` for myself, so I will use this.

- API endpoint: `https://r3.minicrm.hu/Api/R3/Project?MainContactId=$MainContactId`
- Parameter: `MainContactId`
- HTTP method: `GET`
- Datatype: `JSON`

```bash
$ curl -s --user $SystemId:$ApiKey
"https://r3.minicrm.hu/Api/R3/Project?MainContactId=154"
```

```json
{
     "Count": 3,
     "Results": {
         "160": {
             "Id": 160,
             "Name": "Doe Industries LTD (John Doe)",
             "Url": "https://r3.minicrm.hu/Api/R3/Project/160",
             "ContactId": 155,
             "StatusId": 2807,
             "UserId": 115950,
             "Deleted": 0,
             "BusinessId": 154
         },
         "162": {
             "Id": 162,
             "Name": "Mass email sending",
             "Url": "https://r3.minicrm.hu/Api/R3/Project/162",
             "ContactId": 155,
             "StatusId": 2573,
             "UserId": 115950,
             "Deleted": 0,
             "BusinessId": 154
         }
     }
}
```

---

## Update a card

It is recommended to send only those fields that you would like to modify.

If you have the card identifier — or API URL — you can also modify data through it. In the examples above, I received `"Id": 160` during the card search, so I will use this here as well.

- API endpoint: `https://r3.minicrm.hu/Api/R3/Project/$CardId`
- Parameter: `CardId`
- HTTP method: `PUT`
- Datatype: `JSON`

```bash
$ curl -s --user $SystemId:$ApiKey -XPUT "https://r3.minicrm.hu/Api/R3/Project/160" -d
{
      "AutoSalesV3_Qualification": "VIP"
}
```

```json
{
      "Id": 256
}
```

In the case of dropdown fields, such as `StatusId` and `UserId`, you can send the value as you see it on the user interface, or you can use its unique identifier stored in the system — whichever is easier for you.

After a successful save, the response is a JSON object that contains the opportunity card identifier in the `Id` property.

If you want to add a new card, you also need to call the endpoint above using the HTTP `PUT` method and omit the `Id` field.

Among the data sent in, the required fields must be filled in. Which field is required depends on the system, module and status. You can check this on the MiniCRM user interface by manually adding a new card to the selected status of the selected module.

---

## Card owner / responsible user

When you create or modify a card with a JSON that contains the responsible user’s name, it works as follows:

You have the ability to use either the character-accurate name or the ID of the user who will be the responsible user / owner of the given opportunity card when creating or updating it.

Example JSON:

User name version:

```json
{
    "Id": $ProjectId,
    "CategoryId": $CategoryId,
    "ContactId": $ContactId,
    "StatusId": "Processing",
    "UserId": "Specific User Name",
    "Name": "Test Opportunity card",
....
}
```

User ID version:

```json
{
    "Id": $ProjectId,
    "CategoryId": $CategoryId,
    "ContactId": $ContactId,
    "StatusId": "Processing",
    "UserId": 108637",
    "Name": "Test Opportunity card",
....
}
```

If the value of the user field contains a typo and there is no active user in your system with that name or ID, the request will still be sent successfully, and you will receive the following response: `Ok`, with code `200`. In this case, the system will randomly assign a user to the given opportunity card.

In the case of users who do not have access to the given product — for example Sales — but the submitted array contains a valid user name or ID, the system will automatically set the responsible user to someone who has access to that product.

Using this API logic results in fewer request errors and more efficient data processing, since the number of missing cards will be minimal, instead of the system returning an error message and not creating a card because of typos.

---

## Moving a card to another contact connected to the same company

- Endpoint: `https://r3.minicrm.hu/Api/R3/Project/$ProjectId`
- Method: `PUT`
- Example JSON

Original:

```json
{
"ContactId": 6505
}
```

Changed:

```json
{
"ContactId": 6623
}
```

---

## Changing the main contact of the card when it is connected to another company

For this change, you need to send in the new main contact of the card you want to move, and also the ID of the company to which the contact is connected.

- Endpoint: `https://r3.minicrm.hu/Api/R3/Project/$ProjectId`
- Method: `PUT`

Example JSON:

Original:

```json
{
"ContactId": 6505,
"BusinessId": 6504
}
```

Changed:

```json
{
"ContactId": 1622,
"BusinessId": 1235
}
```

Limitation:

In the current version of the system, you cannot change the contact person to another contact person who is not connected to a business entity.

---

## File upload to a card

With the card ID or API URL, you can upload a file to a card. It will appear in the history and can be downloaded with a single click. To do this, you need to store the file on a public server — HTTP authentication can be used — where MiniCRM can access and download it.

You also need to place a `File` column on your card.

- API endpoint: `https://r3.minicrm.hu/Api/R3/Project/$CardId`
- Parameter: `CardId`
- HTTP method: `PUT`
- Datatype: `JSON`

```bash
$ curl -s --user $SystemId:$ApiKey -XPUT "https://r3.minicrm.hu/Api/R3/Project/160"-d
{
      "$FileColumnName": "$FileUrl"
}
```

```json
{
      "Id": 160
}
```

After successful saving, the response will contain the `Id` data name and the card ID value.

---

### Multiple file upload to a card

When uploading files, there are two ways to do it from the system and via API as well.

On opportunity cards, you need to add a file field to the existing fields of your product. There, you have the ability to modify the settings of how file fields work.

First, you can use the field to hold only 1 file, and when another file is uploaded, the system overwrites the old one.

Second, you can use file fields to hold multiple files at once. If you want this, check the checkbox in the field settings.

- API endpoint: `https://r3.minicrm.hu/Api/R3/Project/$CardId`
- Parameter: `CardId`
- HTTP method: `PUT`
- Datatype: `JSON`

```bash
$ curl -s --user $SystemId:$ApiKey -XPUT "https://r3.minicrm.hu/Api/R3/Project/160"-d
{
      "$FileFieldName": "$FileUrl1,$FileUrl2"
}
```

Example request:

```bash
$ curl -s --user $SystemId:$ApiKey -XPUT "https://r3.minicrm.hu/Api/R3/Project/160"-d
{
"FileField":"https://file-examples.com/wp-content/storage/2017/02/file_example_XML_24kb.xml,https://file-examples.com/wp-content/storage/2017/02/zip_2MB.zip"
}
```

Example response:

```json
{
    "Id": 6356
}
```

---

## Deleting a card

You have the ability to put opportunity cards into the Trash via API using the following method:

- Endpoint: `https://r3.minicrm.hu/Api/R3/Project/$ProjectId`
- Method: `PUT`

Example JSON:

```json
{
      "Deleted": 1
}
```

Important: With this method, you cannot permanently delete data from your system. To do that, you need to log into the system and perform one of the following actions:

- open the Trash in one of your products
- open the opportunity card you want to delete
- click the red `Delete permanently` button in the upper right corner; here you can permanently delete data using the following methods:
  - you can delete only that specific opportunity card and nothing else
  - you can delete the contact and all cards connected to it

Side note: In item-based products, such as Offer, you cannot permanently delete card data.

---

### Creating a query for deleted cards

You have the ability to get the list of cards that were previously deleted in the given product.

You cannot create a single request to retrieve all deleted cards from your whole system; you need more than one request for that.

- Endpoint: `https://r3.minicrm.hu/Api/R3/Project?CategoryId=$CategoryId&Deleted=1`
- Method: `GET`

Example JSON response:

```json
{
     "Count": 2,
     "Results": {
         "8185": {
              "Id": 8185,
              "Name": "Test Contact Project 1",
              "Url": "https://r3.minicrm.hu/Api/R3/Project/8185",
              "ContactId": 6495,
              "StatusId": 5227,
              "UserId": 108637,
              "Deleted": 1
         },
         "8186": {
              "Id": 8186,
              "Name": "Test Contact 2 Project 2",
              "Url": "https://r3.minicrm.hu/Api/R3/Project/8186",
              "ContactId": 6496,
              "StatusId": 5227,
              "UserId": 108637,
              "Deleted": 1
         }
     }
}
```

Here, `Count` indicates the number of deleted cards in the specific product whose ID was used in the endpoint.

Using the ID received in the response to the first query, you can retrieve the details of the specific opportunity card and the todo list on the Project endpoint.

You cannot check the status history of a deleted card; you can only do this with active cards.

---

### Modifying deleted cards

In the case of opportunity cards, even if they are in the Trash, you have the ability to change the card’s status, fields, or even create / modify a todo on it.

Regarding todos, it is important to know that if you create a todo on a deleted card, it will not appear in the Today view of that specific user.

If you want to modify card data:

- Endpoint: `https://r3.minicrm.hu/Api/R3/Project/$ProjectId`
- Method: `PUT`

If you want to create or modify todos on the card:

- Endpoint: `https://r3.minicrm.hu/Api/R3/Todo/`

OR

- Endpoint: `https://r3.minicrm.hu/Api/R3/Todo/$ToDoId`
- Method: `PUT`

---

### Restoring a card

For this, you can use the general Project endpoint. To restore a specific card, you only need to change the value of the `Deleted` row.

- Endpoint: `https://r3.minicrm.hu/Api/R3/Project/$ProjectId`
- Method: `PUT`

Example JSON:

```json
{
     "Deleted": 0
}
```

---

## Delete a contact

A contact can be deleted when it is not the primary contact of any card.

- API endpoint: `https://r3.minicrm.hu/Api/R3/PurgePerson/$ContactId`
- Parameter: `ContactId`
- HTTP method: `GET`
- Datatype: `JSON`

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/R3/PurgePerson/257"
```

Response:

```json
{
     "Message": "Successfully deleted"
}
```

If a contact is the primary contact on one or more cards, you need to set a new primary contact on that card by sending a new `ContactId` to that company.

---

## Error handling

If the data to be sent in is not correct, the server will return an `HTTP 400 Bad Request` response.

When you are trying API calls in Terminal, by default it is not visible what code MiniCRM returned — `200` or something else. By using the `-w` parameter, you can append the return code to any command.

In case of an error, the response is not JSON-encoded, but a plain text error message.

If a case similar to the one below occurs, you can determine the possible values through the Schema endpoint mentioned above or on the MiniCRM user interface.

```bash
$ curl -s --user $SystemId:$ApiKey -w "\nHTTP CODE: %{http_code}\n" -XPUT
"https://r3.minicrm.hu/Api/R3/Project/2056" -d
{
     "AutoSalesV3_Qualification": "Such value cannot be found in the CRM"
}
```

```text
Project (PUT): Invalid value 'Such value cannot be found in CRM' for the Lead qualification field. HTTP CODE: 400
```
