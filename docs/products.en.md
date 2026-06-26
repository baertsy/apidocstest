# Stock endpoint

Using this endpoint, you can interact with the stock of products and/or services used by products such as Invoice, Offer and Orders.

There are special endpoints for these products:

- Invoice endpoint: `https://r3.minicrm.hu/Api/Invoice/List`
- Offer endpoint: `https://r3.minicrm.hu/Api/Offer/List`
- Order endpoint: `https://r3.minicrm.hu/Api/Order/List`

These endpoints list only issued documents.

Pagination limits also apply.

---

## Additional pagination details for stock-related products

For example, in the invoice product — but this also applies to orders and offers — the invoices that were most recently modified appear first, not the cards related to the invoice:

```json
{
    "Count": 3,
    "Results": [
      "145": {
         "Id": "145",
         "ProjectId": "3529",
         "StornedId": "",
         "Number": "AJ2024-E/00001",
         "Type": "Offer",
         "Issued": "2024-02-19",
         "Performance": "2024-02-15",
         "Prompt": "2024-02-15",
         "Paid": "",
         "CurrencyCode": "HUF",
         "AmountNet": "1599.0000",
         "AmountVat": "432.0000",
         "Amount": "2031.0000",
         "Status": "In Preparation",
         "StatusGroup": "Open",
         "DocumentUrl":
"https://r3.minicrm.hu/xxxxx/Download/S3/?t=doc&inline=0&e=2h99asbfjn1zvi9e7yjw0jas6ewk27"
       },
       {...},
       {...}
    ]
}
```

---

## List filtering parameters

### Page — optional

```text
https://r3.minicrm.hu/Api/Invoice/List?Page=1
```

### UpdatedSince — optional

The date of the last modification of the invoice. Invoices modified more recently than this date will be included in the list.

```text
https://r3.minicrm.hu/Api/Invoice/List?UpdatedSince=2024-02-12 10:00
```

### StatusGroup — optional

The current status group of the card related to the invoice.

Possible values:

- `Lead`
- `Open`
- `Success`
- `Failed`

```text
https://r3.minicrm.hu/Api/Invoice/List?StatusGroup=Success
```

Filters can be combined to get a more refined search for your documents:

```text
https://r3.minicrm.hu/Api/Invoice/List?StatusGroup=Success&UpdatedSince=2024-02-12 10:00&Page=2
```

---

## Adding a new product

- API endpoint: `https://r3.minicrm.hu/Api/R3/Product/`
- HTTP method: `PUT`
- Datatype: `JSON`

```json
{
     "Name": "Apple Pie",
     "SKU": "APPLE_PIE",
     "EAN": "123456789",
     "FolderName": "Basic Services",
     "VAT": "19%",
     "PriceNet": 10,
     "CurrencyCode": "RON",
     "UnitName": "Unit_Piece",
     "Quantity": 5,
     "Warehouse_Name": "Warehouse Name",
     "Description": "Delicious apple pie.",
     "Deleted": 0
}
```

`SKU`: If it is not empty and already exists in the system, the system will update the existing data instead of creating a new product.

`FolderName`: If it is not specified or does not exist, it will be placed in the first folder recorded in the system.

Custom-created fields can also be sent.

The response contains the generated MiniCRM product ID:

```json
{ "Id": 1 }
```

---

## Updating a product

- API endpoint: `https://r3.minicrm.hu/Api/R3/Product/$Id`
- Parameter: `Id` — replace it with the MiniCRM product ID that you received in the response when creating a new product.
- HTTP method: `PUT`
- Datatype: `JSON`

The expected JSON is the same as when creating a new product. If it is not specified, the default currency will be used for `CurrencyCode` — as stored in the database, for example `HUF` in Hungary, `RON` in Romania, etc. — and for VAT, the highest VAT rate of the given country will be used — theoretically the default everywhere — which is `27%` in Hungary, `19%` in Romania, etc.

```json
{
    "Name": "Apple Pie (updated)",
    "SKU": "APPLE_PIE",
    "EAN": "123456789",
    "FolderName": "Basic Services",
    "VAT": "19%",
    "PriceNet": 10,
    "CurrencyCode": "RON",
    "UnitName": "Unit_Piece",
    "Quantity": 5,
    "Warehouse_Name": "Warehouse Name",
    "Description": "Delicious apple pie.",
    "Deleted": 0
}
```

If the update was successful, the system will respond with the product ID:

```json
{ "Id": 1 }
```

---

## Deleting a product

In order to delete a product, you need to update it using the `Deleted` parameter and set its value to `1`.

- API endpoint: `https://r3.minicrm.hu/Api/R3/Product/$Id`
- Parameter: `Id` — replace it with the MiniCRM product ID that you received in the response when creating a new product.
- HTTP method: `PUT`
- Datatype: `JSON`

The expected JSON is the same as when creating a new product. If it is not specified, the default currency will be used for `CurrencyCode` — as stored in the database, for example `HUF` in Hungary, `RON` in Romania, etc. — and for VAT, the highest VAT rate of the given country will be used — theoretically the default everywhere — which is `27%` in Hungary, `19%` in Romania, etc.

```json
{
    "Deleted": 1
}
```

If the deletion was successful, the system will respond with the product ID:

```json
{ "Id": 1 }
```
