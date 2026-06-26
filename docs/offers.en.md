# Offer endpoint

On this endpoint, you have the ability to manage your offers in MiniCRM. You can create an API request to send the offer into the system in JSON-encoded format or in an XML file.

Via this endpoint, you will be able to:

- create an offer in your system
- change the status of the offer
- modify or add information to the card fields

---

## Creating and issuing offers

In the case of offers, the system can process a request with a structure similar to the response you receive when retrieving offers. The function can be used with a `POST` request. The data can be processed by the system in JSON-encoded format. In the case of offers, `CustomerId` and `ReferenceId` are mandatory information.

- API endpoint: `https://r3.minicrm.hu/Api/Offer`
- HTTP method: `POST`
- Datatype: `JSON`

For customers, the billing address will be used. If there is no such address, then the default address will be used on the offers. The use of the `Customer` block within the request is optional; here you can provide the customer's data. If the address already exists among the contact information, we will use it; if not, it will be saved as a new — default — billing address.

The VAT number and bank account number will be overwritten by the API. Offers will be created in the first, default status, which is: `Preparing offer`.

Authentication is mandatory in the request. `CustomerId` or `ReferenceId` must be provided. Editing the `CurrencyCode` variable is not allowed; it must be submitted with a `POST` request when creating the offer.

Example request:

```bash
$ curl -s --user $SystemId:$ApiKey -XPOST "http://r3.minicrm.hu/Api/Offer/" -d '
{
    "CustomerId": "20404",
    "InvoicePadCounter": "00005",
    "Number": "2024-E/00001",
    "Type": "Offer",
    "Media": "PKI",
    "Language": "RO",
    "Customer": {
        "Name": "John Doe",
        "CountryId": "40",
        "Country": "Romania",
        "PostalCode": "000000",
        "City": "Test",
        "Address": "Test Address",
        "AccountNumber": "",
        "VatNumber": "",
        "EUVatNumber": "",
        "RegistrationNumber": "",
        "GroupIdentificationNumber": ""
    },
    "Vendor": {
        "Name": "John Doe",
        "CountryId": "40",
        "PostalCode": "000000",
        "City": "Test",
        "Address": "John Doe Street, no. 1",
        "AccountNumber": "",
        "IBAN": "",
        "SWIFT": "",
        "VatNumber": "XXXXXX",
        "EUVatNumber": "ROXXXXXX",
        "GroupIdentificationNumber": "",
        "RegistrationNumber": ""
    },
    "ExtraData": "",
    "Subject": "",
    "PaymentMethod": "WiredTransfer",
    "Issued": "2024-01-01",
    "Performance": "2024-01-08",
    "Prompt": "2024-01-08",
    "Paid": "",
    "CurrencyCode": "RON",
    "ConversionRate": "",
    "AmountNet": "23.00",
    "AmountVat": "0.00",
    "Amount": "23.00",
    "VATMode": "NormalVAT",
    "Accounting": "Normal",
    "DocumentId": "6",
    "DocumentUrl":
"https://r3-test.minicrm.eu/documents/XYZ/Download/S3/?t=doc&inline=0&e=XYZ",
    "Items": [
        {
            "SKU": "",
            "EAN": "",
            "Name": "Test",
            "Description": "",
            "PriceNet": "23.0000",
            "Quantity": "1.00",
            "Unit": "piece",
            "PriceNetTotal": "23",
            "VAT": "19",
            "VATSubtype": "",
            "PriceTotal": "23.00"
        }
    ],
    "Project": {
        "String1911": "Test"
    }
}'
```

The API response contains comprehensive order data, including the unique order identifier. This response is also extended with dynamically calculated fields, such as `AmountVat` and `Amount Net`. The structure of the response is the same as the structure of the “Retrieve an order” response.

---

## Search for offers

### Detailed data of an offer

- API endpoint: `https://r3.minicrm.hu/Api/Offer/OfferId`
- Parameter: `OfferId`
- HTTP method: `GET`
- Datatype: `JSON`

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/Offer/932"
```

```json
{
    "Id": "932",
    "InvoicePadCounter": "00008",
    "Number": "AJ2021-E/00008",
    "Type": "Offer",
    "InvoiceMode": "Normal",
    "Media": "PKI",
    "Language": "HU",
    "Customer": {
      "Name": "John Doe",
      "CountryId": "36",
      "County": "",
      "PostalCode": "XXXX",
      "City": "Budapest",
      "Address": "XXX",
      "AccountNumber": "",
      "VatNumber": "",
      "EUVatNumber": "",
      "RegistrationNumber": "",
      "GroupIdentificationNumber": ""
    },
    "Vendor": {
      "Name": "John Doe INC",
      "CountryId": "36",
      "PostalCode": "XXXX",
      "City": "XXX",
      "Address": "XXX",
      "AccountNumber": "XXXXXXXXXXXXXXXXXXXXX",
      "IBAN": "XXXXXXXXXXXXXXXXXXXXX",
      "SWIFT": "XXXXXXXX",
      "VatNumber": "XXXXXXXXX",
      "EUVatNumber": "XXXXXXXX",
      "GroupIdentificationNumber": "",
      "RegistrationNumber": ""
    },
    "ExtraData": "{\"VendorMiscellaneous\":\"XXXXXXXX\"}",
    "Subject": "",
    "PaymentMethod": "WiredTransfer",
    "Issued": "2021-10-15",
    "Performance": "2021-10-15",
    "Prompt": "2021-10-15",
    "Paid": "",
    "CurrencyCode": "HUF",
    "ConversionRate": "",
    "AmountNet": "23",
    "AmountVat": "0",
    "Amount": "23",
    "VATMode": "VATExempt",
    "Accounting": "Normal",
    "DocumentId": "819",
    "DocumentUrl":
"https://r3.minicrm.hu/41274/Download/S3/?t=doc&inline=0&e=XXXXXXXXXX",
    "Items": [
       {
          "Id": "1306",
          "SKU": "",
          "EAN": "",
          "Name": "testr",
          "Description": "",
          "PriceNet": "23.0000",
          "Quantity": "1.00",
          "Unit": "darab",
          "PriceNetTotal": "23",
          "PriceTotal": "23"
       }
    ],
    "Project": {
       "Enum1907": "",
       "Float1910": "",
       "Int1909": "",
       "Int1912": "",
       "String1911": "TEST",
       "String1913": "",
       "String1914": "",
       "String1915": "",
       "StatusId": "3426"
    }
}
```

---

## Offer list endpoint

Using the request below, you can retrieve the list of your created offers via API. The listing returns 100 results. Pagination can be done using the `Page` parameter; see more details in the Pagination chapter.

- API endpoint: `https://r3.minicrm.hu/Api/Offer/List`
- HTTP method: `GET`
- Datatype: `JSON`

```text
https://r3.minicrm.hu/Api/Offer/List
```

Example response:

```json
{
    "Count": 12,
    "Results": {
      "9": {
        "Id": "9",
        "ProjectId": "264",
        "StornedId": "",
        "Number": "AJ2022-E/00001",
        "Type": "Offer",
        "Issued": "2022-04-14",
        "Performance": "2022-02-03",
        "Prompt": "2022-02-03",
        "Paid": "",
        "CurrencyCode": "EUR",
        "AmountNet": "678.0000",
        "AmountVat": "183.0000",
        "Amount": "862.0000",
        "Status": "Accepted",
        "StatusGroup": "Success",
        "DocumentUrl": ""
      }
    }
}
```

Note: The Stock endpoint states that `Offer/List` is a special endpoint for listing documents. According to `Company Policies v2`, the offer list also lists offers that have not been issued.

---

## Update offer data

Complete modification of an offer is restricted to instances in `Draft` status, and such edits must be initiated exclusively from within your system. In contrast, when the offer is created in the system, you can still flexibly modify the items, the opportunity card fields, and issue modified offers without having to create an entirely new offer from the beginning.

It is important to note that certain operations, including the following, cannot be performed using API requests:

- changing the owner of the opportunity card — offer
- deleting an item
- modifying an item
- adding a new item

Structure:

```json
{
"FieldName": "Value"
}
```

Example request:

```bash
$ curl -s --user $SystemId:$ApiKey -POST "https://r3.minicrm.hu/Api/Offer/10/Project"
-d '
{
"StatusId": "Paid",
"String1616": "Test"
}'
```
