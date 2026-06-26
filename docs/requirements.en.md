# REQUIREMENTS AND TECHNICAL INFORMATION

## Requirements
To use our API, you must authenticate. The authentication process requires two elements:
1. **SystemID** (Please check the Basics chapter).
2. **API key** (Please check the Basics chapter).

> **Important:** It is highly recommended to run the examples presented in this manual before starting your integration to better understand how our implementation works.

---

## Pagination
Endpoints that filter/search in the system return results using a unified framework.

In an API search, 100 results are shown on one page. You can page by using the `Page` parameter. Paging starts from 0, so the second page can be accessed using `Page=1`.

**Example:**
```bash
curl -s --user $SystemId:$ApiKey "[https://r3.minicrm.hu/Api/R3/Project?CategoryId=5&Page=1](https://r3.minicrm.hu/Api/R3/Project?CategoryId=5&Page=1)"
```

- **Count:** The number of results is found in the `Count` key.
- **Results:** Data is listed in separate arrays. Only fundamental data is shown.
- **Url:** Use the address in the `Url` field to access all available information about an item via API.

---

## cUrl usage
If you are on Windows 10 (version 1803 or later), curl is installed by default. For installation help, see [https://everything.curl.dev/](https://everything.curl.dev/).

MiniCRM REST API delivers JSON responses without formatting. You can use `jq` (https://jqlang.github.io/jq/) to understand responses easier:
```bash
curl -s --user $SystemId:$ApiKey "[https://r3.minicrm.hu/Api/R3/Category](https://r3.minicrm.hu/Api/R3/Category)" | jq
```

> **Reminder:** MiniCRM API uses Unicode code page with UTF-8 representation. Every parameter and response is UTF-8 encoded.

---

## Postman and HTTPie
* **Postman:** Set Authentication "Type" to Basic. Username is your `SystemID`, Password is your `API key`. Use the API endpoints in the URL field without the full base path.
* **HTTPie:** An online Rest API tool, similar to Postman, which can be used without installation directly in your browser.

---

## Error messages and error handling
MiniCRM API uses standard HTTP response error codes:

* **200 (OK):** The request was successful.
* **400 (Bad Request):** Parameters not recognized or missing. (Note: Our API is **case-sensitive**).
* **401 (Unauthorized):** Incorrect SystemID/API key, or your subscription plan does not include REST API access.
* **404 (Not Found):** The URL is not found (often a typo).
* **405 (Method not allowed):** Wrong method used (e.g., POST instead of PUT).
* **429 (Too many requests):** You exceeded API limits (60 requests/minute).
* **500 (Internal Server Error):** Internal processing error (e.g., trying to set a non-existing value for a field).

---

## API limits
1. **60 requests / minute:** For all standard API endpoints (URLs starting with https://r3.minicrm.hu/Api/R3).
2. **180 requests / hour:** For XML synchronization requests.
3. **No limit:** For the invoice endpoint (https://r3.minicrm.hu/Api/Invoice).

---

## Other specific limitations
* **Item based products:** You cannot add, modify, or delete items for the "Offer" product.
* **To-do endpoint:** You can only request data for opened/closed tasks (tasks cannot be reopened via API) and cannot create recurring tasks.
* **Templates:** You cannot create new templates via API.
* **Contacts:** You cannot move contacts between companies via API.
* **Addresses/Companies:** Cannot be deleted via API.
