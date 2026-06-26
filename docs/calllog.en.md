# CallLog API endpoint

This interface is designed for VOIP providers. The metadata of phone calls can be sent to MiniCRM through the CallLog API, so they will appear in the history section of the relevant customer's opportunity card.

Before sending requests to this endpoint, you must provide the IP addresses for authorization. Please use our contact details for this purpose.

---

## Create a new call log entry

- URL: `https://r3.minicrm.io/Api/CallLog`
- HTTP method: `POST`
- The sent data must be in JSON format:
  - `ApiKey`: the VOIP API key. The administrator can generate it in MiniCRM / Settings / System. This is a separate key from the “general” MiniCRM API key, which does not work with the VOIP API.
  - `UserExtension`: the internal phone number of the user who initiated / received the call. Internal user numbers can be set in MiniCRM / Profile. If the PBX does not use internal numbers, the user's full phone number can be sent in this field.
  - `Data`: arrays of history items, each consisting of:
    - `Number` — string: the phone number of the other party in the call
    - `Duration` — int: the duration of the call in seconds
    - `CallType` — int:
      - `0`: outgoing
      - `1`: incoming
      - `2`: missed
    - `Date` — string/datetime: the start date and time of the call in UTC timezone
    - `ReferenceId` — string, optional: the unique identifier of the recorded call, if the call was recorded

---

### Response

If everything went well, the API responds with the `HTTP 200 / OK` response code.

In case of errors, `HTTP 4XX` or `HTTP 5XX` codes are returned together with the error description.

The response is a JSON-encoded object containing summary statistics of the processed history records:

- `Missing`: the number of records where at least one parameter is missing.
- `NotFound`: the number of records where the other party's phone number could not be found.
- `Processed`: successful processing; the history records have been saved in MiniCRM.

---

### Request example #1

```bash
$ curl -s --user $SystemId:$ApiKey -v -X POST -H "Content-Type: application/json"
"https://r3.minicrm.hu/Api/CallLog" -d '
{
    "ApiKey":"<API_KEY>",
    "UserExtension":"001",
    "Data":[{
        "Date":"2016-03-02 16:00:12",
        "Number":"0620123456",
        "CallType": 2,
        "Duration": 132,
        "ReferenceId":"UniqueReferenceId"
       }]
}'
```

Response:

```json
{
      "Skipped": 0,
      "Processed": 1,
      "Exists": 0
}
```

---

### Request example #2

The first record contains errors, the second is from an unknown number, and the third item can be saved.

```bash
$ curl -s --user $SystemId:$ApiKey -v -XPOST "https://r3.minicrm.hu/Api/CallLog" -d '
  {
     "ApiKey": "<API_KEY>",
     "UserExtension": "001",
     "Data": [
         {
             "Date": "2016-aíYA03-02 16:00:12",
             "CallType": 2,
             "Duration": 132,
             "ReferenceId": "UniqueReferenceId"
         },
         {
             "Date": "2016-03-02 16:00:12",
             "Number": "0620123456",
             "CallType": 2,
             "Duration": 435,
             "ReferenceId": "UniqueReferenceId"
         },
         {
             "Date": "2016-03-03 12:12:12",
             "Number": "0620123456",
             "CallType": 2,
             "Duration": 251,
             "ReferenceId": "UniqueReferenceId"
         }
     ]
}'
```

Response example #2:

```json
{
    "Skipped": 1,
    "Processed": 1,
    "Exists": 1
}
```

---

## Search for phone calls

### Listening to calls in MiniCRM

If the `ReferenceId` is sent together with the calls, it is possible to listen to the calls in MiniCRM.

Without `ReferenceId`, only the call metadata appears in the card history. Playback of the recorded call, if any, is an optional feature.

There are two things to do:

1. Each recorded call must submit a unique identifier in the `ReferenceId` field.
2. Then, the URL of the recorded call must be sent to `suport@minicrm.ro`. It should be a template that MiniCRM will use to generate the URL from which it can download the recorded audio file.

URL example:

```text
https://yourdomain.com/recordings.php?Rec={%RefereceId%}
```

`HTTPS` — communication takes place over a secure channel, as recorded calls are sensitive data.

`yourdomain.com` is the server used by the VoIP provider to store the recordings.

We will download the files from the following IP addresses:

- `195.228.254.37`
- `195.228.254.43`
- `195.228.156.188`

It is recommended to set an IP restriction on the endpoint to ensure that only these IP addresses can access it.
