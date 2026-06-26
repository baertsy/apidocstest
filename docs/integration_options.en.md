# INTEGRATION OPTIONS

MiniCRM can be integrated with any application that can support Rest API communications with the specification that in a certain integration, MiniCRM can be only the “passive” part that expects requests[cite: 3]. MiniCRM will not initialize requests to external Rest APIs[cite: 3].

The system can be integrated with other applications using different methods[cite: 3]:
* Rest API[cite: 3].
* XML synchronization[cite: 3].
* Specific Rest API endpoints for invoices and for call log purposes[cite: 3].
* Webhooks[cite: 3].
* Built-in integrations[cite: 3].

---

## Rest API vs. XML synchronization

In the following table, we compare the two main methods of integration that are available in the system[cite: 3]:

| Rest API | XML synchronization |
| :--- | :--- |
| It is used to transfer a small amount of data[cite: 3]. | It is used when a large amount of data needs to be transferred into the system (for example, 100+ cards)[cite: 3]. |
| There is a limitation of 60 requests per minute in place[cite: 3]. | There is a limit of 180 requests per hour for XML synchronization requests[cite: 3]. |
| The data are synced almost instantly (depending on your internet speed)[cite: 3]. | Depending on the amount of data that is synced, the transfer may not be visible right away[cite: 3]. |
| It works as a communication between MiniCRM and a third party app, where that app can create requests for MiniCRM and the system will respond with the required data[cite: 3]. | It works only as a one-way data transfer (from a third party app to MiniCRM)[cite: 3]. |
| Security measures can be implemented by the third party app. The system uses HTTPS protocols[cite: 3]. | The XML file should be publicly available in order to be accessed and processed by the system (IP restrictions can be set)[cite: 3]. |
| To create a completely new card, it will need 3 different requests (one for the company card, one for the contact card and one for the opportunity card)[cite: 3]. | To create a completely new card (with company, contact, and opportunity card), one single request is needed[cite: 3]. |

---

## Invoice API

This endpoint enables the customer to interact with invoices in MiniCRM[cite: 3]. 
* You can manage your invoices and perform operations like creating new invoices[cite: 3].
* Search for invoices[cite: 3].
* Mark invoices as paid or storn them[cite: 3].
* The customer can update invoice card fields[cite: 3].

---

## Call log endpoint (VOIP integration)

The endpoint is useful in situations where you need to use centralized phone services like VOIP (Voice Over IP) solutions[cite: 3]. After the call is finished, the details about that specific call will be recorded on the most recently updated card history section[cite: 3].
We will record phone call details like[cite: 3]:
* Who called/received the phone call (MiniCRM user)[cite: 3].
* The phone call status (answered or missed)[cite: 3].
* The contact name[cite: 3].
* The phone number that was involved in the call[cite: 3].
* The phone call duration[cite: 3].
* The phone call registration (this feature depends on the VOIP solution)[cite: 3].

---

## Webhooks

Webhooks are recommended when an external system or application needs to react to a change that happens in MiniCRM (e.g., complex CRM automation or syncing with an external system)[cite: 3].
* Changes are sent using a POST request to an API endpoint given by the customer[cite: 3].
* The endpoint should respond with an HTTP response (200 OK)[cite: 3].
* In case of an error, the system will make another 6 attempts with the following delays: 10 seconds, 1 minute, 5 minutes, 15 minutes, 30 minutes, 1 hour[cite: 3].
* Webhooks can be filtered by Type (Contact or Project), Product (CategoryId), and specific Fields[cite: 3].
* Webhook settings are not available on the web interface; you must contact the helpdesk to request the setting[cite: 3].

---

## Built-in integrations

Most of these built-in integrations are add-ons, and they can be activated on the Subscription page[cite: 3]:

* **MiniSync Integration:** Exclusively available for the Professional and Enterprise platforms through annual or multi-year subscriptions[cite: 3]. It utilizes an intermediary database to store and map data from third-party companies[cite: 3]. Connection forms include LocalFile Connector, HttpDownload Connector, and MySQL Connector[cite: 3].
* **Integration with calendars:** Synchronize tasks with Google calendar, Outlook, or iPhone calendar[cite: 3].
* **Gmail & Outlook:** Enables you to directly send emails from your inbox to MiniCRM history for a specific customer[cite: 3].
* **Facebook ads forms:** Collect leads data from Facebook paid ads form directly into MiniCRM[cite: 3].
* **Gravity forms Integration:** Connect customized WordPress webforms to MiniCRM using a plugin and short code[cite: 3].
* **Webshops (WooCommerce, Shoprenter, UNAS):** Synchronize your customers and orders directly into MiniCRM[cite: 3].
* **OpenApi & Cégjelző:** Scans and synchronizes company records with up-to-date details (e.g., VAT number, address, revenue) from the Romanian (OpenApi) and Hungarian (Cégjelző) databases[cite: 3].
* **NAV (Bejövő számlák):** Sync in all the invoices which were issued for you and create a Cash flow report automatically in Google Sheets[cite: 3].
* **BestJobs / eJobs integration:** Gather information about candidates from these platforms directly into your MiniCRM system[cite: 3].
* **SmartBill integration:** Generate invoices automatically based on offers/orders and use the stock item's module[cite: 3].
