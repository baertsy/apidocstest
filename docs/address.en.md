# Address endpoint

Using this endpoint, you can manage physical addresses, i.e. postal addresses.

In MiniCRM, both individual contacts and companies can have a physical address.

---

## Create a new address

In order to create a new address, the following are required:

- API endpoint: `https://r3.minicrm.hu/Api/R3/Address`
- HTTP method: `PUT`
- Datatype: `JSON`

In order to create a new address, you must specify a contact / company ID. In MiniCRM, the address must be linked to a contact or to a company. The `ContactId` parameter below can contain either a contact ID or a company ID; the parameter name does not change in the case of a company.

```bash
$ curl -s --user $SystemId:$ApiKey -XPUT https://r3.minicrm.hu/Api/R3/Address -d '{
    "ContactId": 12345,
    "Type": "Headquarter",
    "Name": "Address name",
    "CountryId": "Romania",
    "County": "John Doe",
    "City": "John Doe",
    "PostalCode": 000000,
    "Address": "John Doe, nr. 00",
    "Default": 1
    }
```

In case of success, the system will respond with the ID of the newly created address:

```json
{
    "Id":12345
}
```

---

## Search for address

### Search based on contact / company ID

- API endpoint: `https://r3.minicrm.hu/Api/R3/AddressList/ContactId`
- Parameter: `ContactId` — an existing contact or company ID
- HTTP method: `GET`
- Datatype: `JSON`

The request URL should look something like this:

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/R3/AddressList/ID"
```

The `ID` parameter can contain either a contact ID or a company ID.

The system will respond with the address data:

```bash
$ curl -s --user $SystemId:$ApiKey https://r3.minicrm.hu/Api/R3/AddressList/12
```

```json
{
      "Results": {
          "307": "NoName ( John Doe Address)",
          "310": "Office Address (John Doe Address)"
      },
      "Count": 2
}
```

The address details can also be retrieved in a structured response if needed:

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/R3/AddressList/ID?Structured=1"
```

The system will respond with the address data in structured form:

```json
{
      "Results": {
          "307": {
              "Id": 307,
              "Address": "No Name (John Doe)",
              "Url": "https://r3.minicrm.hu/Api/R3/Address/307"
          },
          "310": {
              "Id": 310,
              "Address": "Test Address (John Doe Test Street, no. 123)",
              "Url": "https://r3.minicrm.hu/Api/R3/Address/310"
          }
      },
      "Count": 2
}
```

---

### Search based on address ID

- API endpoint: `https://r3.minicrm.hu/Api/R3/Address/AddressId`
- Parameter: `AddressId` — an existing address ID
- HTTP method: `GET`
- Datatype: `JSON`

The request URL should look something like this:

```bash
$ curl https://r3.minicrm.hu/Api/R3/Address/AddressId
```

The system will respond with the address details:

```bash
curl -s --user $SystemId:$ApiKey https://r3.minicrm.hu/Api/R3/Address/307
```

```json
{
      "Id": 307,
      "ContactId": 37979,
      "Type": "",
      "Name": "",
      "CountryId": "Romania",
      "PostalCode": "",
      "City": "Test",
      "County": "Test",
      "Address": "John Doe Street, no. 1",
      "Default": 1,
      "CreatedBy": "John Doe",
      "CreatedAt": "2023-09-29 08:35:46",
      "UpdatedBy": "John Doe",
      "UpdatedAt": "2023-09-29 08:35:46"
}
```

---

## Update an address

- API endpoint: `https://r3.minicrm.hu/Api/R3/Address/AddressId`
- Parameter: `AddressId` — an existing address ID
- HTTP method: `PUT`
- Datatype: `JSON`

The request URL should look something like this:

```bash
$ curl -XPUT "https://r3.minicrm.hu/Api/R3/Address/AddressId"
```

This updates the address:

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/R3/Address/307"
```

```json
{
    "Address": "Test Street, no. 1"
}
```

In case of success, the system will respond with the ID of the updated address:

```json
{
    "Id":12345
}
```
