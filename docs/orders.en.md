# Order endpoint

This endpoint helps you manage the orders that you manage in MiniCRM.

The listing of orders and searching in orders are not supported via API at this point in the document. However, a separate order list endpoint appears later in the document.

---

## Create a new order

To create an order, you need to send a JSON-encoded `POST` request. The `CustomerId` or `ReferenceId` is required in the request.

- API endpoint: `https://r3.minicrm.hu/Api/Order`
- HTTP method: `POST`
- Datatype: `JSON`

The billing address will be used on orders. If there is no billing address, the contact’s default address will be used instead.

Using the `Customer` array is optional; here you can provide the buyer’s data. If there is an address, it will be used; if not, it will be set as a new default billing address.

In this case, the VAT number and the bank account number will be overwritten by the API.

```bash
$ curl -s --user $SystemId:$ApiKey -XPOST "https://r3.minicrm.hu/Api/Order/" -d
{
      "CustomerId": "261",
      "CurrencyCode": "HUF",
      "Customer":
      {
          "Name": "John Doe Ltd.",
          "Country": "36",
          "PostalCode": "0000",
          "City": "AnonymCity",
          "Address": "Anonym Street 1",
          "AccountNumber": "XXXXXXXXXXXXXXXXXXXX",
          "VatNumber": "XXXXXXXXX",
          "EUVatNumber": "XX00000000"
      },
      "Subject": "",
      "Items": [
          {
               "SKU": "",
               "Name": "Cable",
               "Description": "",
               "PriceNet": "500.00",
               "Quantity": "15.00",
               "Unit": "m",
               "VAT": "27"
         }
     ]
}
```

Authentication is mandatory. The response contains all of the order’s data and the ID of the order — `Order Id`. In the response, the system also sets the calculated fields, such as `AmountVat`, `Amount Net`. The response structure is similar to the “Retrieve an order” response structure.

---

## Search for orders

### Detailed data of an order

- API endpoint: `https://r3.minicrm.hu/Api/Order/$OrderId`
- Parameter: `OrderId`
- HTTP method: `GET`
- Datatype: `JSON`

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/Order/$OrderId"
```

```json
{
      "Id": "83",
      "ProjectId": "272",
      "CustomerId": "261",
      "Number": "",
      "InvoiceMode": "Normal",
      "Customer":
      {
          "Name": "John Doe Ltd.",
          "CountryId": "1",
          "County": "",
          "PostalCode": "0000",
          "City": "Anonym City",
          "Address": "Anonym Street 1",
          "AccountNumber": "92826253-45043722-59650728",
          "VatNumber": "17823443-1-26",
          "EUVatNumber": "HU23982273",
          "RegistrationNumber": "",
          "GroupIdentificationNumber": ""
      },
      "VendorGroupIdentificationNumber": "",
      "Subject": "",
      "PaymentMethod": "Cash",
      "Performance": "2023-09-25",
      "CurrencyCode": "HUF",
      "AmountNet": "7500",
      "AmountVat": "2025",
      "Amount": "9525",
      "Accounting": "Normal",
      "OSS": "0",
      "Items": [
          {
              "Id": "73",
              "SKU": "",
              "EAN": "",
              "Name": "Cable",
              "Description": "",
              "PriceNet": "500.000000",
              "Quantity": "15.0000",
              "Unit": "m",
              "PriceNetTotal": "7500.0000",
              "VAT": "27%",
              "VATSubtype": "",
              "PriceTotal": "9525.00"
          }
      ],
      "Project":
      {
          "Name": "To be issued (John Doe Ltd.)"
      }
}
```

---

## Order list endpoint

Using the request below, you can retrieve the list of your created orders via API. The listing returns 100 results. Pagination can be done using the `Page` parameter; see more details in the Pagination chapter.

- API endpoint: `https://r3.minicrm.hu/Api/Offer/List`
- HTTP method: `GET`
- Datatype: `JSON`

```text
https://r3.minicrm.hu/Api/Offer/List
```

Example response:

```json
{
   "Count": 52,
   "Results": {
     "4": {
       "Id": "4",
       "ProjectId": "24",
       "StornedId": "",
       "Number": "Webshop #24",
       "Type": "Order",
       "Issued": "2021-12-11",
       "Performance": "2021-12-11",
       "Prompt": "2021-12-11",
       "Paid": "",
       "CurrencyCode": "HUF",
       "AmountNet": "766.0000",
       "AmountVat": "214.0000",
       "Amount": "980.0000",
       "Status": "Successful",
       "StatusGroup": "Success",
       "DocumentUrl": ""
     }
   }
}
```

Note: In the Stock endpoint section, the order list endpoint is shown as `https://r3.minicrm.hu/Api/Order/List`, and it is also stated there that these special list endpoints only list issued documents. According to `Company Policies v2`, the order list endpoint does not list unissued orders.

---

## Update order data

Modifying order data is possible only in `Draft` status. Posting the `CustomerId` or `ReferenceId` is mandatory. Editing the `CurrencyCode` variable is not allowed; it must be submitted with a `POST` request when creating the order.

- API endpoint: `https://r3.minicrm.hu/Api/Order/$OrderId`
- Parameter: `OrderId`
- HTTP method: `POST`
- Datatype: `JSON`

```bash
$ curl -s --user $SystemId:$ApiKey -XPOST "https://r3.minicrm.hu/Api/Order/$OrderId" -d
{
      "CustomerId": "261",
      "Subject": "Call the customer when the order is ready to go"
}
```

In response, you will receive the modified order with the new `Subject` value.

---

### Add an order item

- API endpoint: `https://r3.minicrm.hu/Api/Order/$OrderId`
- Parameter: `OrderId`
- HTTP method: `POST`
- Datatype: `JSON`

```bash
$ curl -s --user $SystemId:$ApiKey -XPOST "https://r3.minicrm.hu/Api/Order/83" -d
{
      "CustomerId": "261",
      "Items":
          [{
          "SKU": "",
          "Name":"Linksys IP phone",
          "Description": "",
          "PriceNet": "12000.00",
          "Quantity": "3.00",
          "Unit": "piece",
          "VAT": "27"
          }]
}
```

---

### Edit an order item

By specifying the `ItemId`, you can overwrite the given order item. You can change the item itself or its quantity.

- API endpoint: `https://r3.minicrm.hu/Api/Order/$OrderId`
- Parameter: `OrderId`
- HTTP method: `POST`
- Datatype: `JSON`

```bash
$ curl -s --user $SystemId:$ApiKey -XPOST "https://r3.minicrm.hu/Api/Order/83" -d
{
     "CustomerId": "261",
     "Items":
[
     {
         "Id": 74,
         "SKU": "",
         "Name": "Cisco IP phone",
         "Description": "",
         "PriceNet": "15000.00",
         "Quantity": "2.00",
         "Unit": "piece",
         "VAT": "27"
     }
]}
```

In response, you will receive all the modified order data, and the calculated fields will be updated automatically, such as `AmountVat`, `AmountNet`.

Shortened response example:

```json
"Items": [
         {
             "Id": "73",
             "SKU": "",
             "EAN": "",
             "Name": "Cable",
             "Description": "",
             "PriceNet": "500.000000",
             "Quantity": "15.0000",
             "Unit": "m",
             "PriceNetTotal": "7500.0000",
             "VAT": "27%",
             "VATSubtype": "",
             "PriceTotal": "9525.00"
         },
         {
             "Id": "74",
             "SKU": "",
             "EAN": "",
             "Name": "Cisco IP phone",
             "Description": "",
             "PriceNet": "15000.000000",
             "Quantity": "2.0000",
             "Unit": "piece",
             "PriceNetTotal": "30000.0000",
             "VAT": "27%",
             "VATSubtype": "",
             "PriceTotal": "38100.00"
         }
     ],
```

---

### Editing custom order fields

If you created custom fields on the order card, you can edit them without any status restriction.

- API endpoint: `https://r3.minicrm.hu/Api/Order/$OrderId/Project`
- Parameter: `OrderId`
- HTTP method: `POST`
- Datatype: `JSON`

```bash
$ curl -s --user $SystemId:$ApiKey -XPOST "https://r3.minicrm.hu/Api/Order/83/Project" -d
'
{
    "Text1479": "Please deliver the order between 9am and 5pm"
}'
```

---

## Editing the status of an order

### Finalizing an order

Sets the order’s status from `Draft` to `In progress`. Editing works only when the order is in `Draft` status.

- API endpoint: `https://r3.minicrm.hu/Api/Order/$OrderId/Finalize`
- Parameter: `OrderId`
- HTTP method: `POST`
- Datatype: `JSON`

```bash
$ curl -s --user $SystemId:$ApiKey -XPOST "https://r3.minicrm.hu/Api/Order/83/Finalize"
```

---

## Editing an order

Sets the order’s status from `In progress` to `Draft`. Editing works only when the order is in `Draft` status.

- API endpoint: `https://r3.minicrm.hu/Api/Order/$OrderId/Edit`
- Parameter: `OrderId`
- HTTP method: `POST`
- Datatype: `JSON`

```bash
$ curl -s --user $SystemId:$ApiKey -XPOST "https://r3.minicrm.hu/Api/Order/83/Edit"
```

---

## Completing an order

Sets the order’s status from `In progress` to `Completed`.

- API endpoint: `https://r3.minicrm.hu/Api/Order/$OrderId/Complete`
- Parameter: `OrderId`
- HTTP method: `POST`
- Datatype: `JSON`

```bash
$ curl -s --user $SystemId:$ApiKey -XPOST "https://r3.minicrm.hu/Api/Order/83/Complete"
```

---

## Setting an order to successful

Sets the order’s status from `Completed` or `Failed` to `Paid`.

- API endpoint: `https://r3.minicrm.hu/Api/Order/$OrderId/Paid`
- Parameter: `OrderId`
- HTTP method: `POST`
- Datatype: `JSON`

```bash
$ curl -s --user $SystemId:$ApiKey -XPOST "https://r3.minicrm.hu/Api/Order/83/Paid"
```

---

## Setting an order to unsuccessful

Sets the order’s status from `Completed` or `Unsuccessful` to `Failed`.

- API endpoint: `https://r3.minicrm.hu/Api/Order/$OrderId/Storno`
- Parameter: `OrderId`
- HTTP method: `POST`
- Datatype: `JSON`

```bash
$ curl -s --user $SystemId:$ApiKey -XPOST "https://r3.minicrm.hu/Api/Order/83/Storno"
```

Status changes can only be made using the method mentioned above.

For example: if you would like to set a `Paid` order to `In progress`, the API response will be `405 Method not allowed`.

---

## Delete an order item

- API endpoint: `https://r3.minicrm.hu/Api/Order/$OrderId/DeleteItem/$ItemId`
- Parameters:
  - `OrderId`
  - `ItemId`
- HTTP method: `GET`
- Datatype: `JSON`

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/Order/83/DeleteItem/74"
```

In response, you will receive all the modified order data, and the calculated fields will be updated automatically, such as `AmountVat`, `AmountNet`.
