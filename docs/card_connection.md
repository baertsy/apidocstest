# Adatlaptársítás végpont

Ezzel a végponttal a felhasználó új adatlaptársításokat hozhat létre, valamint kezelheti a MiniCRM-ben már meglévő társított adatlapokat. Emellett társítások listázására is használható.

---

## Új adatlaptársítás létrehozása

Új adtalaptársítás létrehozásához a következőkre van szükség:

- API végpont: `https://r3.minicrm.hu/Api/R3/CreateProjectConnection`
- Paraméterek:
  - `FromProjectId` — adatlap azonosítója
  - `ToProjectId` — adatlap azonosítója
- HTTP metódus: `PUT`
- Adattípus: `JSON`

Új adatlaptársítás létrehozásához hitelesítés szükséges, az alábbi példának megfelelően:

```bash
$ curl -s --user $SystemId:$ApiKey -X PUT
"https://r3.minicrm.hu/Api/R3/CreateProjectConnection/" -d
'{"FromProjectId":$FromProjectId,"ToProjectId":$ToProjectId}
```

A rendszer a társítás részleteivel válaszol:

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

## Adatlaptársítások listázása

Meglévő adatlaptársítás listázásához a következőkre van szükség:

- API végpont: `https://r3.minicrm.hu/Api/R3/ListProjectConnection`
- Paraméter: `FromProjectId` — adatlap azonosítója
- HTTP metódus: `GET`
- Adattípus: `JSON`

Meglévő adatlaptársítás listázásához hitelesítés szükséges, az alábbi példának megfelelően:

```bash
$ curl -s --user $SystemId:$ApiKey
"https://r3.minicrm.hu/Api/R3/ListProjectConnection?FromProjectId=$FromProjectId"
```

A rendszer a társítás részleteivel válaszol:

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

## Adatlaptársítás frissítése

Meglévő adatlaptársítás frissítéséhez a következőkre van szükség:

- API végpont: `https://r3.minicrm.hu/Api/R3/UpdateProjectConnection`
- Paraméterek:
  - `FromProjectId` — adatlap azonosítója
  - `ToProjectId` — adatlap azonosítója
  - `Description`
- HTTP metódus: `PUT`
- Adattípus: `JSON`

Meglévő adatlaptársítás frissítéséhez hitelesítés szükséges, az alábbi példának megfelelően:

```bash
$ curl -s --user $SystemId:$ApiKey -X PUT
"https://r3.minicrm.hu/Api/R3/UpdateProjectConnection/" -d
'{"FromProjectId":$FromProjectId,"ToProjectId":$ToProjectId,"Description":$Description}'
```

---

## Adatlaptársítás törlése

Meglévő adatlaptársítás törléséhez a következőkre van szükség:

- API végpont: `https://r3.minicrm.hu/Api/R3/DeleteProjectConnection`
- Paraméterek:
  - `FromProjectId` — adatlap azonosítója
  - `ToProjectId` — adatlap azonosítója
- HTTP metódus: `PUT`
- Adattípus: `JSON`

Meglévő adatlaptársítás törléséhez hitelesítés szükséges, az alábbi példának megfelelően:

```bash
$ curl -s --user $SystemId:$ApiKey -X PUT
"https://r3.minicrm.hu/Api/R3/DeleteProjectConnection/" -d
'{"FromProjectId":$FromProjectId,"ToProjectId":$ToProjectId}'
```
