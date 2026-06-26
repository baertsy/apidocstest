# To-do endpoint

Using this endpoint, you can manage your tasks — to-dos —: you can create new tasks, search, update or close existing tasks.

---

## Create a new to-do

The `ProjectId` field is mandatory in order to create a new to-do.

- API endpoint: `https://r3.minicrm.hu/Api/R3/ToDo/`
- HTTP method: `POST`
- Datatype: `JSON`

Example URL:

```bash
$ curl -s --user $SystemId:$ApiKey -XPUT "https://r3.minicrm.hu/Api/R3/ToDo/"
```

```json
{
   "UserId":"MiniCRM Helpdesk",
   "ProjectId":"1331",
   "Comment":"This To-Do was created via API.",
   "Status":"Open",
   "Deadline":"2023-02-23 15:35:40",
   "Duration": 60,
   "Reminder": 60,
   "AddressId": 4558,
   "Status":"Open"
}
```

After successful submission, you will receive an `Id` value in the response, which represents the to-do ID.

Response:

```json
{
   "Id":1111
}
```

It is important to note that when creating a new task, submitting the `ProjectId` field is mandatory, while in the case of an existing task, the `ProjectId` field can no longer be edited.

---

## Search for to-dos

- API endpoint: `https://r3.minicrm.hu/Api/R3/ToDoList/$CardId`
- Parameter: `CardId`
- HTTP method: `GET`
- Datatype: `JSON`

The card must be identified. For example, with this URL you can query all to-dos from a card:

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/R3/ToDoList/1234"
```

In the response, the result arrives in one block, where `Count` shows the number of found to-dos. Under `Results`, the to-dos can be found in separate blocks.

The listed to-dos can be filtered by status using the `Status` parameter. A to-do can be:

- `Open`
- `Closed`
- `All`

Example for a to-do list in `Open` status:

```bash
$ curl -s --user $SystemId:$ApiKey
"https://r3.minicrm.hu/Api/R3/ToDoList/1234?Status=Open"
```

Response:

```json
{
     "Count":2,
     "Results":[
        {
            "Id":"1111",
            "Status":"Open",
            "Comment":"Call the customer and ask for feedback.",
            "Deadline":"2023-03-20 23:59:00",
            "UserId":"3200",
            "Type":"0",
            "Url":"https://r3.minicrm.hu/Api/R3/ToDo/1111"
        },
        {
            "Id":"2222",
            "Status":"Open",
            "Comment":"Check if it needs further action.",
            "Deadline":"2013-01-22 23:59:00",
            "UserId":"3300",
            "Type":"1566",
            "Url":"https://r3.minicrm.hu/Api/R3/ToDo/2222"
        }
     ]
}
```

---

## Detailed data of a to-do

- API endpoint: `https://r3.minicrm.hu/Api/R3/ToDo/$ToDoId`
- Parameter: `ToDoId`
- HTTP method: `GET`
- Datatype: `JSON`

Identification is required, example URL:

```bash
$ curl -s --user $SystemId:$ApiKey "https://r3.minicrm.hu/Api/R3/ToDo/1111"
```

Querying the data of the selected task. In response, the to-do arrives in an array that contains the to-do data.

Response:

```json
{
     "Id": 3606,
     "UserId": "User A",
     "ContactId": 0,
     "ProjectId": 7888,
     "AddressId": 4558,
     "SenderUserId": 0,
     "Type": "Test type",
     "Duration": 60,
     "Reminder": 60,
     "Status": "Open",
     "Mode": "Task",
     "Deadline": "1970-01-01 01:00:00",
     "Comment": "This is a task created via API",
     "CreatedBy": "MiniCRM",
     "CreatedAt": "2024-08-19 13:54:44",
     "UpdatedBy": "MiniCRM",
     "UpdatedAt": "2024-08-19 13:56:22",
     "ClosedBy": "MiniCRM",
     "ClosedAt": "0000-00-00 00:00:00",
     "Attachments": {
         "FileName": "test contract.pdf",
         "Url": "https://r3.minicrm.hu/…"
     },
     "Members": [
         {
             "Entity": "User2",
             "EntityId": "109287"
         }
     ],
     "Notes": [
         {
             "Comment": "Test comment",
             "CreatedBy": "108637",
             "CreatedAt": "2024-08-19 13:55:00"
         }
      ]
}
```

---

## Update a to-do

Modify an existing to-do or create a new one. It is advisable to resend only the modified data, so that the program can run more efficiently, and to avoid restoring data that may have already been modified in the meantime to previously downloaded values.

```bash
$ curl -s --user $SystemId:$ApiKey -XPUT "https://r3.minicrm.hu/Api/R3/ToDo/1111" -d '
{
  "Comment":"New text sent in via API",
  "Deadline":"2023-03-25 13:30:00"
}'
```

Only open tasks can be modified. Closed tasks cannot be modified.

The service URL is the same as the download URL. A download can be initiated with a `GET` request, and data modification can be initiated with a `PUT` request. Adding a new task is possible by omitting the identifier — for example, by omitting `1111` at the end of the URL.

Response:

```json
{
    "Id":1111
}
```

---

### Modifiable fields

- `ContactId`: relevant if the task in the customer service product is a reply email type task. In this case, `ContactId` is the ID of the recipient.
- `AddressId`: the system ID of the address specified in the task. This can be queried through the Address endpoint — see the address endpoint chapter.
- `Members`: the type and ID of the stakeholders involved in the task. This can be queried through the Contact endpoint. A task can have multiple members.
- `Comment`: the text of the comment.
- `Duration`: you can specify the planned duration of completing the task. This value is measured in minutes.
- `Reminder`: if your user is connected to Google Calendar / Outlook Calendar, you can receive notifications. You can set how many minutes before the due date you want to receive a reminder.
- `Status`: this field indicates whether the task is open or closed. An active task can simply be closed by changing this field’s value to `Closed`.
- `Attachments`: if you insert an accessible URL that points to a file download link, the file will automatically be uploaded to the task. If you want to upload multiple attachments, you can separate them with a comma `,`, without spaces.

---

### Read-only fields

- `CreatedBy`: created by — `UserId`. It can be queried through the `Schema/Project` endpoint — see the relevant chapter.
- `CreatedAt`: gives the creation time of the task.
- `UpdateAt`: gives the time when the task was last modified.
- `UpdatedBy`: the user who made the latest modification to the task.
- `ClosedAt`: when the task was closed.
- `ClosedBy`: the user who closed the task.
- `Mode`: can be `Task` or `Email`.
- `Notes`: all comments written for the task will be listed in this array.

---

### General

- `Attachments`: here you receive the exact name and format of the latest uploaded file, as well as its download link.

If you modify an open task with the `POST` method and send the `ClosedAt` field with a given date while the array does not contain a `Status` field, the system will close this task.

---

### Custom close date and time

You have the ability to close the task and add a custom close date. If you close a task this way, it will appear in chronological order in the card history.

This is a good solution if you want to add to or update the history of the opportunity card, which cannot be done with other methods.

It is important to note that first you have to create a task in order to get an ID, and then you can modify it based on the example below.

This will only work if the task is in normal `Closed` or `Failed` status. It will not work if you close the task to `Successful` in the system.

The `Closed` or `Successful` statuses will not appear in the JSON array you receive as a response; they will only be visible in the system, in the opportunity card history.

- API endpoint: `https://r3.minicrm.hu/Api/R3/ToDo/$ToDoId`
- Parameter: `ToDoId`
- HTTP method: `Post`
- Datatype: `JSON`

Example request:

```json
{
  "Status":"Closed",
  "ClosedAt": "2009-01-09"
}
```

Response:

```json
{
   "Id":1111
}
```

## File upload

- API endpoint: `https://r3.minicrm.hu/Api/R3/ToDo/$ToDoId`
- Parameter: `ToDoId`
- HTTP method: `PUT`
- Datatype: `JSON`

### Single file upload

```json
{
    "Attachments": "https://www.testsite1.com/files/test.pdf"
}
```

### Multiple file upload

```json
{
     "Attachments": "https://www.testsite1.com/files/test.pdf,https://www.testsite2.com/documents/test_2.pdf"
}
```

You have the ability to upload one or more files at once to a task. If you upload a new file, it will be added to the already uploaded attachments; it will not overwrite them.

You cannot upload a file or files whose combined size is larger than 24 MB.

---
