# Invoice endpoint

Using this endpoint, the user can create new invoices and manage the existing invoices in MiniCRM. It can also be used to search invoices [1].

In this chapter, we will often use the `$InvoiceId` variable. This is the actual document ID, and it is different from the card ID displayed in the card URL [1].

---

## Create a new invoice

In order to create a new invoice, the following are required:

- API endpoint: `https://r3.minicrm.hu/Api/Invoice`
- Parameter: `CustomerId` — the contact identifier — or `ReferenceId` — the project external ID
- HTTP method: `POST`
- Datatype: `JSON`

Creating a new invoice is only possible with an existing contact or business in your system [1].

These can have two different identifiers:

1. `CustomerId` — the ID given by the system after creating a business or a contact.
2. `ReferenceId` — an external identifier that is not created by MiniCRM. You can set it for a business or contact, for example when creating the contact through XML synchronization, or later via API.

For more information about contact creation and address handling, see the `Company/contact endpoint` and `Address endpoint` chapters.

The billing address will be used on invoices, and if there is no billing address, then the contact’s default address will be used instead.

Using the customer data in an array is optional; you can also specify the buyer’s data directly in the request.

If there is an address, the system will use that address; if there is no address, it will be set as a new default billing address.

In this case, the VAT number and the bank account number will be overwritten by the API.

Authentication is required to create a new invoice, as shown in the following example:

```bash
$ curl -s --user $SystemId:$ApiKey -XPOST "https://r3.minicrm.hu/Api/Invoice/" -d '
{
     "Number": "2023-E/00001",
     "Type": "Invoice",
     "Media": "PKI",
     "Customer": {
          "Name": "John Doe",
          "CountryId": "40",
          "PostalCode": "000000",
          "City": "Example City",
          "Address": "Example Street 1",
          "AccountNumber": "12345678-12345678-12345678",
          "VatNumber": "RO654321"
     },
     "Vendor": {
          "Name": "Anonym Company",
          "CountryId": "40",
          "PostalCode": "000000",
          "City": "Anytown",
          "Address": "Any Street 2",
          "AccountNumber": "87654321-87654321-87654321",
          "VatNumber": "RO123456",
          "RegistrationNumber": "",
          "Miscellaneous": ""
     },
     "Subject": "",
     "PaymentMethod": "Cash",
     "Issued": "2023-11-27",
     "Performance": "2023-11-27",
     "Prompt": "2023-11-30",
     "Paid": "0000-00-00",
     "CurrencyCode": "EUR",
     "AmountNet": "2000.00",
     "AmountVat": "540.00",
     "Amount": "2540.00",
     "IsReverseCharge": "0",
     "DocumentId": "66",
     "DocumentUrl": "https://UrlOfTheDocument",
     "DocumentFileName": "Invoice-01-2023-e-00001-1.pdf",
     "Items": [
       {
           "Name": "Product_name",
           "Description": "",
           "PriceNet": "1 000,00",
           "VAT": "19%",
           "Quantity": "2",
           "Unit": "piece",
           "PriceNetTotal": "2 000,00",
           "PriceTotal": "2 380,00"
       }
  ]
}
```

Specifications:

- `"Media": "PKI"` represents the default invoice type — the electronic version — while `Paper` represents the paper-based invoice type.

---

## Search for invoices

### Detailed data of an invoice

This endpoint searches for a specified invoice and returns all invoice details.

- API endpoint: `https://r3.minicrm.hu/Api/Invoice`
- Parameter: `InvoiceId` — the identifier of a created invoice
- HTTP method: `GET`
- Datatype: `JSON`

The request URL should look something like this:

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/Invoice/$InvoiceId"
```

The system will respond with the invoice details:

```json
{
  "Id": "12",
  "Number": "2023-E/00014",
  "Type": "Invoice",
  "Media": "PKI",
  "Customer": {
    "Name": "John Doe",
    "CountryId": "40",
    "PostalCode": "000000",
    "City": "Example City",
    "Address": "Example Street 1",
    "AccountNumber": "12345678-12345678-12345678",
    "VatNumber": "RO654321"
  },
  "Vendor": {
    "Name": "Anonym Company",
    "CountryId": "40",
    "PostalCode": "000000",
    "City": "Anytown",
    "Address": "Any Street 2",
    "AccountNumber": "87654321-87654321-87654321",
    "VatNumber": "RO123456",
    "RegistrationNumber": "",
    "Miscellaneous": ""
  },
  "Subject": "",
  "PaymentMethod": "Cash",
  "Issued": "2023-11-27",
  "Performance": "2023-11-27",
  "Prompt": "2023-11-27",
  "Paid": "0000-00-00",
  "CurrencyCode": "EUR",
  "AmountNet": "2000.00",
  "AmountVat": "540.00",
  "Amount": "2540.00",
  "IsReverseCharge": "0",
  "DocumentUrl": "https://UrlOfTheDocument",
  "DocumentFileName": "invoice-2023-e-00014-1.pdf",
  "Items": [
     {
        "Name": "Product_name",
        "Description": "",
        "PriceNet": "1 000,00",
        "VAT": "19%",
        "Quantity": "2",
        "Unit": "piece",
        "PriceNetTotal": "2 000,00",
        "PriceTotal": "2 380,00"
     }
  ]
}
```

---

## Invoice list endpoint

Using the request below, you can retrieve the list of your issued invoices via API. Important: the list does not contain invoices that have not been issued [1].

The listing returns 100 results. Pagination can be done using the `Page` parameter; see more details in the Pagination chapter [1].

- API endpoint: `https://r3.minicrm.hu/Api/Invoice/List`
- HTTP method: `GET`
- Datatype: `JSON`

```text
https://r3.minicrm.hu/Api/Invoice/List
```

Example response:

```json
{
    "Count": 3,
    "Results": [
       "145": {
         "Id": "123",
         "ProjectId": "1234",
         "StornedId": "",
         "Number": "2024-E-EUR/00001",
         "Type": "Invoice",
         "Issued": "2024-02-19",
         "Performance": "2024-02-15",
         "Prompt": "2024-02-15",
         "Paid": "",
         "CurrencyCode": "EUR",
         "AmountNet": "1599.0000",
         "AmountVat": "432.0000",
         "Amount": "2031.0000",
         "Status": "In Preparation",
         "StatusGroup": "Open",
         "DocumentUrl":""
       },
       {...},
       {...}
    ]
}
```

Note: The Stock endpoint also separately states that the `Invoice/List`, `Offer/List` and `Order/List` endpoints are used for listing documents. According to the API documentation, `Invoice/List` does not list invoices that have not been issued, and the listing returns 100 results, with pagination available through the `Page` parameter [1].

---

## Update an invoice

### Mark an invoice as paid

Using this endpoint, it is possible to set the invoice status to `Paid`.

- API endpoint: `https://r3.minicrm.hu/Api/Invoice/$InvoiceId/Paid`
- Parameter: `InvoiceId` — the identifier of a created invoice
- HTTP method: `POST`
- Datatype: `JSON`

The request URL should look something like this:

```bash
$ curl -s --user $SystemId:$ApiKey -XPOST
"https://r3.minicrm.hu/Api/Invoice/$InvoiceId/Paid"
```

Optionally, it is also possible to use an object containing customer data — if the customer data needs to be changed.

```bash
$ curl -s --user $SystemId:$ApiKey -XPOST "https://r3.minicrm.hu/Api/Invoice/16/Paid"
-d '{
      "Customer": {
          "Name": "John Doe",
          "CountryId": "40",
          "PostalCode": "000000",
          "City": "Example City",
          "Address": "Example Street",
          "AccountNumber": "12345678-12345678-12345678",
          "VatNumber": "RO654321"
      },
}'
```

When an invoice is marked as paid, the system response also includes the details of the invoice card:

```json
{
  "Id": "17",
  "Number": "2023-E/00016",
  "Type": "Invoice",
  "Media": "PKI",
  "Customer": {
    "Name": "John Doe",
    "CountryId": "40",
    "PostalCode": "000000",
    "City": "Example City",
    "Address": "Example Street 1",
    "AccountNumber": "12345678-12345678-12345678",
    "VatNumber": "RO654321"
  },
  "Vendor": {
    "Name": "Anonym Company",
    "CountryId": "40",
    "PostalCode": "000000",
    "City": "Anytown",
    "Address": "Any Street 2",
    "AccountNumber": "87654321-87654321-87654321",
    "VatNumber": "RO123456",
    "RegistrationNumber": "",
    "Miscellaneous": ""
  },
  "Subject": "Invoice no. EUR-0-00016",
  "PaymentMethod": "Cash",
  "Issued": "2023-11-27",
  "Performance": "2023-11-27",
  "Prompt": "2023-11-27",
  "Paid": "2023-11-27",
  "CurrencyCode": "EUR",
  "AmountNet": "12000.00",
  "AmountVat": "3240.00",
  "Amount": "15240.00",
  "IsReverseCharge": "0",
  "DocumentId": "68",
  "DocumentUrl": "https://UrlOfTheDocument",
  "DocumentFileName": "invoice-2023-e-00016-1.pdf",
  "Items": [
     {
        "Name": "Product_name",
        "Description": "",
        "PriceNet": "12 000,00",
        "VAT": "19%",
        "Quantity": "1",
        "Unit": "piece",
        "PriceNetTotal": "12 000,00",
        "PriceTotal": "14 280,00"
     }
  ]
}
```

---

### Storno an invoice

Using this endpoint, the user can storno a specific invoice.

- API endpoint: `https://r3.minicrm.hu/Api/Invoice/$InvoiceId/Storno`
- Parameter: `InvoiceId` — the identifier of a created invoice
- HTTP method: `POST`
- Datatype: `JSON`

The request URL should look something like this:

```bash
$ curl -s --user $SystemId:$ApiKey -XPOST
"https://r3.minicrm.hu/Api/Invoice/$InvoiceId/Storno"
```

In this case, the system automatically generates a new storno invoice for the specified invoice.

The response contains the details of the storned invoice:

```json
{
  "Id": "17",
  "Number": "2023-E/00017",
  "Type": "Invoice",
  "Media": "PKI",
  "Customer": {
    "Name": "John Doe",
    "CountryId": "40",
    "PostalCode": "000000",
    "City": "Example City",
    "Address": "Example Street 1",
    "AccountNumber": "12345678-12345678-12345678",
    "VatNumber": "RO654321"
  },
  "Vendor": {
    "Name": "Anonym Company",
    "CountryId": "40",
    "PostalCode": "000000",
    "City": "Anytown",
    "Address": "Any Street 2",
    "AccountNumber": "87654321-87654321-87654321",
    "VatNumber": "RO123456",
    "RegistrationNumber": "",
    "Miscellaneous": ""
  },
  "Subject": "Invoice no. 2023-E/00017 Storno invoice of the invoice no. 2023-E/00016",
  "PaymentMethod": "Cash",
  "Issued": "2023-11-27",
  "Performance": "2023-11-27",
  "Prompt": "2023-11-27",
  "Paid": "2023-11-27",
  "CurrencyCode": "EUR",
  "AmountNet": "12000.00",
  "AmountVat": "3240.00",
  "Amount": "15240.00",
  "IsReverseCharge": "0",
  "DocumentId": "68",
  "DocumentUrl": "https://UrlOfTheDocument",
  "DocumentFileName": "storno_invoice-2023-e-00017.pdf",
  "Items": [
     {
        "Name": "Product_name",
        "Description": "",
        "PriceNet": "12 000,00",
        "VAT": "19%",
        "Quantity": "1",
        "Unit": "piece",
        "PriceNetTotal": "12 000,00",
        "PriceTotal": "14 280,00"
     }
  ]
}
```

---

### Editing custom fields — invoice card fields

This endpoint allows the user to fill in or modify the custom fields located on the invoice card.

- API endpoint: `https://r3.minicrm.hu/Api/Invoice/$InvoiceId/Project`
- Parameter: `InvoiceId` — the identifier of a created invoice
- HTTP method: `POST`
- Datatype: `JSON`

The request URL should look something like this:

```bash
$ curl -s --user $SystemId:$ApiKey -XPOST
"https://r3.minicrm.hu/Api/Invoice/$InvoiceId/Project"
```

The user must specify the name of the field they want to update:

```json
{
    "FieldName": "Test",
}
```
