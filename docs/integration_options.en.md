# INTEGRATION OPTIONS

MiniCRM can be integrated with any application that can support Rest API communications with the specification that in a certain integration, MiniCRM can be only the “passive” part that expects requests. MiniCRM will not initialize requests to external Rest APIs.

The system can be integrated with other applications using some different methods and for various purposes.

We support integrations via Rest API and XML synchronization. We also have some specific Rest API endpoints for invoices and for call log purposes.

In this manual, we will cover all the integration possibilities for MiniCRM.

## Rest API vs. XML synchronization

In the following table, we will compare the two main methods of integration that are available into the system.

| Rest API | XML synchronization |
| :--- | :--- |
| It is used to transfer a small amount of data. | It is used when a large amount of data needs to be transferred into the system (for example, 100+ cards). |
| There is a limitation of 60 requests per minute in place (please check the API limits chapter). | |
| The data are synced almost instantly (depending on your internet speed). | Depending on the amount of data that is synced, the transfer may not be visible right away. |
| It works as a communication between MiniCRM and a third party app, where that app can create requests for MiniCRM and the system will respond with the required data. | It works only as a one-way data transfer (from a third party app to MiniCRM). |
| Security measures can be implemented by the third party app. The system uses HTTPS protocols. | The XML file should be publicly available in order to be accessed and processed by the system¹. |
| To create a completely new card, it will need 3 different requests (one for the company card, one for the contact card and one for the opportunity card). | To create a completely new card (with company, contact, and opportunity card), one single request is needed. |

> ¹ There can be set some IP restrictions for security reasons. We can provide our servers IPs if needed.

The above table should offer you the information you need in order to be able to choose the correct way of integration with our system.

For example, if you plan to integrate a webshop with MiniCRM, using the above table you probably will find that the most appropriate solution will be to use XML synchronization since, in most common cases, you will want to synchronize a large amount of data at once.

However, if you cannot decide the proper method for what you are planning to do, please feel free to contact us:
- For Romanian customers: suport@minicrm.ro
- For Hungarian customers: help@minicrm.hu
- For customers from other countries (please write in English): help-en@minicrm.io

We will gladly help you to decide the most appropriate method based on your need.

---

## Invoice API

This endpoint enables the customer to interact with invoices in MiniCRM. You will be able to manage your invoices and perform operations like creating new invoices, searching for invoices, mark as paid or storn them.

Also, by using the endpoint, the customer can update invoice card fields. You may find detailed information by consulting the dedicated chapter.

---

## Call log endpoint (VOIP integration)

The endpoint is useful in situations where you need to use a centralized phone services like VOIP (Voice Over IP) solutions.

This endpoint will enable you to store phone calls details like phone number that was called, by whom, when and the phone call duration. Moreover, you will be able to record and listen phone calls if you will need this feature and if your VOIP solution allows you to do that².

> ² This option depends on the country legislation where your headquarter is located, and also it may depend on the EU legislation on GDPR topic. We only provide this feature, but the customer is that who is responsible to consider those legal things and who need to be sure that he inform his clients that the phone calls are recorded.

Please note that, based on your country legislation and/or European legislation, you may need to inform and get the consent from your customers about the phone calls recording policy.

After the call is finished, the details about that specific call will be recorded on the most recently updated card history section.

We will record phone call details like:
- Who called/received the phone call (MiniCRM user);
- The phone call status (answered or missed);
- The contact name;
- The phone number that was involved in the call;
- The phone call duration;
- The phone call registration (this feature depends on the VOIP solution - some of them may not include this possibility).

---

## Webhooks

There can be some situations when an external system or application needs to react to a change that happens in MiniCRM. In those cases, we recommend the webhooks.

Some examples when webhooks may be helpful:
- **Complex CRM automation** - an external system is informed about a new lead, ticket, project, invoice, offer, order etc. via webhook. That system can react to that information in various ways.
- **Sync with an external system** - when a card, project, ticket, invoice etc. is updated in MiniCRM, the external system is informed too about the change.

The changes that happen on the cards, companies, or contacts can be sent using a POST request to an API endpoint given by the customer.

The endpoint should respond with an HTTP response (200 OK) in order to consider that the data was successfully processed.

In case that we don’t receive an HTTP response 200, we will consider that an error occurred, and we will make another 6 attempts later with the following delay:
1. 10 seconds;
2. 1 minute;
3. 5 minutes;
4. 15 minutes;
5. 30 minutes;
6. 1 hour.

The delay is calculated from the first attempt, therefore, after the card was modified, the receiving party has 1 hour 51 minutes and 10 seconds to process the data. If it is not processed over this time, we will not make another attempt, and we will consider the changes obsolete.

### Webhook filter criteria

In order to avoid overload, it can be filtered by many parameters that when is a specific endpoint called.

> **IMPORTANT:** if the program that is called makes modifications via MiniCRM REST API based on the data received, it can receive repeated notification. This way an infinite loop can occur. In this case, be really careful when selecting the call criteria. The endpoint should only pay attention to the changes of those fields that won’t be changed by it anymore.

**Type**
- Contact (person and company as well)
- Project (card)

**Product**
It can only be interpreted in case of cards (project). Notification is sent if a modification was made in a selected module.
If you would like to receive notifications about more than one product, but not about all of them, you can set more webhooks, one for every product separately.

**Fields**
Filter the change of specific fields. The endpoint is called if one of the listed fields has been changed. If no field is added to the endpoint, the endpoint is called if any of the fields has changed. Products and field filters can be combined.

### Webhook examples

The Sales product having the ID = 3, and I would like that only the webpage that displays content to my subscribers would be notified if a subscription was made and/or canceled.
```text
Type: Project
CategoryId: 3
Fields: ["StatusId"]
ApiUrl: https://$User:$Pass@example.com/StatusChanged/
```

The Helpdesk product with ID = 4, and I would like that new tickets would be preprocessed automatically by a script, so we could create complex follow-up sequences.
```text
Type: Project
CategoryId: 4
Fields: ["CreatedAt"]
ApiUrl: https://$User:$Pass@example.com/TicketAdded/
```

**Example JSON Payload:**
```json
{
   "Id": "10",
   "Type": "Project",
   "Data": {
      "CategoryId": "8",
      "ContactId": "16",
      "MainContactId": "15",
      "StatusId": "11684",
      "UserId": "2",
      "Name": "The card name goes here",
      "CreatedBy": "2",
      "CreatedAt": "2018-03-02 23:35:41",
      "UpdatedBy": "2",
      "UpdatedAt": "2018-03-02 23:55:42",
      "InternalUrl": "[https://r3.minicrm.hu/123456/#Project-8/10](https://r3.minicrm.hu/123456/#Project-8/10)"
   },
   "Changed": [
      "StatusId",
      "UserId",
      "Name",
      "UpdatedAt"
   ]
}
```

### How to set webhooks

Currently, webhooks are running in a live-test mode. Our internal systems are also connected in a live environment by using this.
However, webhook settings are not available on the web interface. Contact our helpdesk to request more information and the setting of the webhook.

As more and more real-life experiences are collected, the webhook settings options and the content of the messages could be changed.

---

## Built-in integrations

Most of these built-in integrations are add-ons, and they may not be activated for your system. In order to use them, you will need to activate them on the Subscription page from settings directly from your MiniCRM system.

### MiniSync Integration
MiniSync is exclusively available for the Professional and Enterprise platforms, offered only through annual or multi-year subscriptions or loyalty agreements. It's important to note that we can schedule/undertake integrations more quickly if they are compatible with our existing infrastructure.

MiniSync is a platform designed to simplify and expedite the integration processes. It utilizes an intermediary database to store data from third-party companies. These data are then mapped to the corresponding fields within MiniCRM.

**Benefits of MiniSync**
You don't need to navigate the technical complexities of MiniCRM when setting up SyncFeed. You can submit data in the format/structure received from a third party without adhering strictly to MiniCRM specifications. There's no need to fully comprehend our 100+ pages of API documentation, MiniCRM's data structure, or deal with identification/duplication, API limitations, etc. If you have a "data feed" in your own structure, it's sufficient for us. We take care of the mapping logic and submit it to your MiniCRM system.

**When to Choose MiniSync**
It's essential to understand that integration is fundamentally a collaborative process involving your participation and that of the third party, preferably a technical person from the software you're using. While our tool can assist you in fetching data from another software, processing and retrieving data from the other software are not solely our responsibility. This requires cooperation and support from the third party.

**Use cases for MiniSync**
- Continuous data synchronization between two systems (e.g., MiniCRM and an ERP).
- Limited resources: Ideal for clients lacking internal software design or development capacity and lacking detailed technical knowledge about MiniCRM.

**Your tasks in the integration process:**
- Imagine and document your needs: Note down your requirements – data fields, workflow ideas, and how you want things to synchronize.
- Consult with the third party about integration. Discuss technical details, fees, and ensure that the data connection will always remain active and supported in the future.

**Responsibilities of the Third Party:**
While we offer various connectors for data retrieval from the third party, it's crucial that they provide continuous access to up-to-date information.
These connection forms include:
- **LocalFile Connector** - provides a unique endpoint for the third party to upload fresh data.
- **HttpDownload Connector** - requires an endpoint from the third party from which we can download fresh data.
- **MySQL Connector** - requires access to the database of the respective system to extract the necessary data. This is a more time-consuming connection form, as we need to understand the database structure.

### Integration with calendars

**Google calendar**
The integration allows you to synchronize the tasks from MiniCRM with your Google calendar account and display them into the calendar. The only thing to do is to set the integration, then the tasks will be automatically synchronized with it.
Details:
- Romanian: https://www.minicrm.ro/help/sincronizarea-cu-calendarul/
- Hungarian: https://www.minicrm.hu/help/google-naptar-szinkronizalas/

**Other calendars**
MiniCRM can synchronize tasks in some other calendars (like Outlook and/or iPhone calendar). The list of available calendars may change over time.

### Gmail
This addon enables you to directly send emails from your inbox to MiniCRM history for a specific customer (you have to open an email from the customer first). Also, you may add new cards, tasks or to open MiniCRM directly from Gmail interface.
- Romanian: https://www.minicrm.ro/help/integrare-cu-gmail/
- Hungarian: https://www.minicrm.hu/help/minicrm-integracioja-a-gmail-webes-feluletevel/

### Outlook
This integration is very similar with Gmail one.
- Romanian: https://www.minicrm.ro/help/integrare-outlook/
- Hungarian: https://www.minicrm.hu/help/minicrm-kontaktok-szinkronizalasa-outlook-nevjegyekkel/

### Facebook ads forms
Using this built-in integration, you will be able to collect leads data from Facebook paid ads form directly into MiniCRM.
- Romanian: https://www.minicrm.ro/help/integrarea-cu-facebook-lead-ads/
- Hungarian: https://www.minicrm.hu/help/facebook-lead-ads-connector/

### Gravity forms Integration
If you want to gather all your leads through your WordPress based website into your MiniCRM system with custom webform design, this integration is the best way up to date way. Gravity forms give you the ability to create styled webforms and connect it to your system.
- Romanian: https://www.minicrm.ro/help/integrare-gravity-forms/
- Hungarian: https://www.minicrm.hu/help/gravity-forms-integracio/

### WooCommerce
This integration may be used in case you have an WordPress and WooCommerce webshop in order to synchronize your orders directly into MiniCRM.
- Romanian: https://www.minicrm.ro/help/integrare-woocommerce/
- Hungarian: https://www.minicrm.hu/help/woocommerce-integracio/

### Shoprenter
With this integration, you can synchronize all your orders and customers from your Shoprenter webshop into two different products in your system: Webshop and Orders.
- Hungarian: https://www.minicrm.hu/help/shoprenter-integracio/

### UNAS
This webshop integration is available in multiple countries and works similarly to other Webshop integrations.
- Romanian: https://www.minicrm.ro/help/sincronizareunaswebshop/
- Hungarian: https://www.minicrm.hu/help/unas-webshop-integracio/

### OpenApi
OpenApi is similar to ListaFirme from Romania and this service allows you to have updated data about the companies. The data came from https://openapi.ro/ database. This will sync details like Tax identification number, Company name, Address, etc.
- Romanian: https://www.minicrm.ro/help/openapi/

### Cégjelző
This is our boxed solution on how the MiniCRM system can keep your database, and the partner companies' details, up to date regarding companies data. This solution works via API and gets data from the https://cegjelzo.com/ database.
- Hungarian: https://www.minicrm.hu/help/ceginfo-szolgaltatas-integracio-cegjelzo-creditinfo/

### NAV (Bejövő számlák)
With this custom integration, we provide you the opportunity to sync in all the invoices which were issued for you and also the partners who issued them.
- Hungarian: https://www.minicrm.hu/help/nav-bejovo-szamlak-integracio/

### BestJobs / eJobs integration
These integrations may help you gather information about the candidates from that platforms directly into your MiniCRM system.
- BestJobs: https://www.minicrm.ro/help/integrare-bestjobs/
- eJobs: https://www.minicrm.ro/help/integrare-ejobs/

### SmartBill integration
You may use your MiniCRM and SmartBill accounts together creating offers or orders, for example, from MiniCRM and then generate automatically the invoice based on those documents in SmartBill.
- Romanian: https://www.minicrm.ro/help/integrare-smart-bill/
