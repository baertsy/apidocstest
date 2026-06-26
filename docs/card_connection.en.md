# Card connection endpoint

Using this endpoint, the user can create new card connections and manage the existing connected cards in MiniCRM. It can also be used to list connections.

---

## Create a new card connection

In order to create a new card connection, the following are required:

- API endpoint: `https://r3.minicrm.hu/Api/R3/CreateProjectConnection`
- Parameters:
  - `FromProjectId` — the card identifier
  - `ToProjectId` — the card identifier
- HTTP method: `PUT`
- Datatype: `JSON`

Authentication is required to create a new card connection, as shown in the following example:

```bash
$ curl -s --user $SystemId:$ApiKey -X PUT
"https://r3.minicrm.hu/Api/R3/CreateProjectConnection/" -d
'{"FromProjectId":$FromProjectId,"ToProjectId":$ToProjectId}
```

The system will respond with the connection details:

```json
{
     "ProjectId": 22222,
     "ProjectName": ProjectName,
     "Description": ,
     "CategoryId": 1,
     "StatusId": 33333,
     "MainContactId": 1
}
```

---

## List card connections

In order to list an existing card connection, the following are required:

- API endpoint: `https://r3.minicrm.hu/Api/R3/ListProjectConnection`
- Parameter: `FromProjectId` — the card identifier
- HTTP method: `GET`
- Datatype: `JSON`

Authentication is required to list an existing card connection, as shown in the following example:

```bash
$ curl -s --user $SystemId:$ApiKey
"https://r3.minicrm.hu/Api/R3/ListProjectConnection?FromProjectId=$FromProjectId"
```

The system will respond with the connection details:

```json
{
     "TotalCount": 1,
     "Page": 0,
     "ItemPerPage": 100,
     "Connections": {
               "0": {
                       "ProjectId": 22222,
                       "ProjectName": ProjectName,
                       "Description": ,
                       "CategoryId": 1,
                       "StatusId": 33333,
                       "MainContactId": 1
                   }
          }
}
```

---

## Update a card connection

In order to update an existing card connection, the following are required:

- API endpoint: `https://r3.minicrm.hu/Api/R3/UpdateProjectConnection`
- Parameters:
  - `FromProjectId` — the card identifier
  - `ToProjectId` — the card identifier
  - `Description`
- HTTP method: `PUT`
- Datatype: `JSON`

Authentication is required to update an existing card connection, as shown in the following example:

```bash
$ curl -s --user $SystemId:$ApiKey -X PUT
"https://r3.minicrm.hu/Api/R3/UpdateProjectConnection/" -d
'{"FromProjectId":$FromProjectId,"ToProjectId":$ToProjectId,"Description":$Description}'
```

---

## Delete a card connection

In order to delete an existing card connection, the following are required:

- API endpoint: `https://r3.minicrm.hu/Api/R3/DeleteProjectConnection`
- Parameters:
  - `FromProjectId` — the card identifier
  - `ToProjectId` — the card identifier
- HTTP method: `PUT`
- Datatype: `JSON`

Authentication is required to delete an existing card connection, as shown in the following example:

```bash
$ curl -s --user $SystemId:$ApiKey -X PUT
"https://r3.minicrm.hu/Api/R3/DeleteProjectConnection/" -d
'{"FromProjectId":$FromProjectId,"ToProjectId":$ToProjectId}'
```
