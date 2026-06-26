# MiniCRM API Basics

This section covers the fundamental concepts, architecture, and core operational principles of the MiniCRM REST API.

---

## SystemID

The **SystemID** is a foundational concept in the MiniCRM API, consisting of a **5-digit unique number**. It is strictly used to authenticate API requests, is completely stable, and will never change at any point in the future.

The SystemID can be directly implemented into your cURL connection URLs:
```bash
curl [https://r3.minicrm.hu/Api](https://r3.minicrm.hu/Api)...
```

### How to find the SystemID?
The SystemID is located in your browser's address bar right after the domain name. 
*For example:* if the URL is `https://r3.minicrm.hu/51546/TodayView`, then **51546** is your SystemID.

---

## Product, Module, and Category

In MiniCRM, all data and business processes are organized into **products**. Throughout this manual and the system documentation, the terms *product*, *module*, and *category* are used interchangeably to represent the exact same thing.

* Within the API infrastructure, a product is referred to as a **Category**, and its primary identifier is the **CategoryID**.
* These match the main menu entries visible on the system sidebar (e.g., *Sales*, *Invoice*, *Helpdesk*, *Order*, etc.).
* The **CategoryID** can also be extracted from the browser's address bar when a specific module is open (the trailing number in the URL, e.g., `.../#Project-25`, where 25 is the category ID).

---

## Opportunity Card or Project (Opportunity card / project)

Opportunity cards—also called projects—are structured items listed within their respective products or modules. The overarching structure of these cards remains identical across all modules; the structural variations depend entirely on the specific fields and the data assigned to them.

In the REST API, these cards are handled under the entity name **Project**.

### Structure of an Opportunity Card
Each opportunity card consists of four core functional areas:

1. **Task Area:** Displays all active/open tasks alongside their operational parameters.
2. **History Area:** Tracks every single change occurring on that specific card (emails, SMS, closed tasks).
3. **Fields Area:** Contains the customizable field infrastructure.
4. **Contacts Area (Account Area):** This segment is broken down into Business Data, Contacts, and Projects.

### Primary and Other Contacts
An opportunity card can only have one designated primary contact at a time. If a card requires multiple contact persons, the rest are listed under the "Other contacts" section located directly beneath the primary contact.

---

## Card Structure in Special Products (Invoice, order, offer, etc.)

Cards managed within the Order, Invoice, Offer, Delivery Notes, Service Worksheet, and Certificate of Contract Completion modules possess a **unique two-tabbed structural layout**.

* **Project Tab:** Dedicated to executing standard CRM workflows, custom fields, and tasks.
* **Document Tab (e.g., Order/Invoice tab):** Handles the product-specific, accounting, or billing functions operating on a completely separate tab and API endpoint.

---

## Project Management Specifics (Kanban View)

The **Project Management** module features a distinct specialized workflow view layout known as the **Kanban view**. This graphical dashboard displays open tasks organized into progressive vertical columns:
* Ideas
* Current tasks
* Work in progress
* Review
* Pending
* Done
* Rejected

---

## Company, Contact, and Account Relationships

The database hierarchy in MiniCRM adheres to the following sequence:
**Opportunity Cards (Projects) -> Contacts -> Companies**

Cards are directly assigned to contacts, and contacts are sequentially assigned to companies.

### Company Card
Separate from the opportunity card, MiniCRM provides a comprehensive company page where all consolidated organizational records are registered.

### Contact Card
Stores personal details regarding a specific individual, including their first and last name, corporate position, direct telephone number, email address, or physical/postal address.

> 💡 **Critical Integration Requirement:** If you need to programmaticly construct an entirely new card structure from scratch via the API (containing a company, a contact, and card data), you must execute **3 separate API requests** in this exact chronological order:
> 1. Create the company entity.
> 2. Create the contact person entity, mapping the `BusinessId` parameter to the company ID.
> 3. Create the opportunity card (Project), mapping the `ContactId` parameter to the contact person ID.

---

## Database Field Names (Field name)

To extract a field's system name, you must have **Administrator privileges**:
1. Navigate to the fields configuration settings page.
2. Click on the edit icon to modify the target field.
3. Locate the immutable registered database name for that specific field (e.g., `Type`).

**When crafting integration scripts or API payloads, always ignore the prefix and dot—use only the string following the dot** (e.g., use `Type` instead of `Project.Type`).

---

## API Key Generation

### Rest API Key Generation
To communicate with the endpoints, you must generate an API Key under **Settings -> System**.

* ⚠️ **One-Time Visibility:** The newly generated API key is **only displayed once**. Save it immediately into a secure credential manager.
* **Security Protocol:** Treat this key as your master password to the API.
* **Loss Recovery:** If a key is lost, it cannot be retrieved; you must generate a replacement key, which invalidates the old one.

### Production and Test Environments

MiniCRM provides the ability to provision a completely isolated **test environment** upon request at help-en@minicrm.io. 
You cannot send outbound emails from a test environment, and password reset requests are disabled.
