# 1. Executive Summary

This Airtable base appears to be a comprehensive **Aviation Field Operations and FBO (Fixed-Base Operator) Management System**. It is designed to manage and reconcile aircraft fueling operations, track fuel storage and truck inventory, handle service orders, and manage aviation parts inventory.

**Major operational workflows:**

- **Fuel Management & Reconciliation:** Tracking fuel from delivery to storage tanks, transferring to fuel trucks, dispensing to aircraft, and calculating variances/losses.

- **Aircraft Service & Dispatch:** Handling customer aircraft intake, service requests (Fuel, Oil, Lavatory, Maintenance), and dispatching employees/trucks.

- **Inventory & Parts Management:** Tracking aviation parts master lists, serialized inventory items, movements, and vendors.

- **Maintenance & Work Orders:** Issuing parts to jobs and tracking aircraft maintenance.


**Key entities involved:**

- **Entities:** Customers, Aircraft, Employees, Vendors.

- **Assets:** Fuel Storage Tanks, Fuel Trucks, Fuel Meters, Inventory Items, Parts.

- **Transactions:** Fuel Deliveries, Nightly Truck Fills, Aircraft Fueling Transactions, Service Orders, Fuel Reconciliation.


# 2. Table Inventory

| Table Name | Purpose | Record Count (if available) |
| ---------- | ------- | --------------------------- |
| Fuel Types | Manages records for fuel types. | N/A (Schema API does not provide counts) |
| Fuel Storage Tanks | Tracks static fuel storage tanks, capacity, and current levels. | N/A (Schema API does not provide counts) |
| Fuel Deliveries | Manages records for fuel deliveries. | N/A (Schema API does not provide counts) |
| Nightly Truck Fill | Manages mobile fuel trucks, capacities, and active fuel levels. | N/A (Schema API does not provide counts) |
| Fuel Trucks | Manages mobile fuel trucks, capacities, and active fuel levels. | N/A (Schema API does not provide counts) |
| Fuel Meters | Manages records for fuel meters. | N/A (Schema API does not provide counts) |
| Aircraft Fueling Transactions | Logs individual fueling operations for specific aircraft. | N/A (Schema API does not provide counts) |
| Fuel Meter Lines | Manages records for fuel meter lines. | N/A (Schema API does not provide counts) |
| Service Orders | Tracks customer requests for service (fuel, lav, maintenance) and job status. | N/A (Schema API does not provide counts) |
| Fuel Reconciliation | Manages records for fuel reconciliation. | N/A (Schema API does not provide counts) |
| Customers | Stores customer CRM data, contact info, and linked aircraft. | N/A (Schema API does not provide counts) |
| Aircraft | Maintains a registry of aircraft by tail number, linking to customers and service orders. | N/A (Schema API does not provide counts) |
| Services | Manages records for services. | N/A (Schema API does not provide counts) |
| Employees | Manages records for employees. | N/A (Schema API does not provide counts) |
| Parts Master | Master catalog of aviation parts and components. | N/A (Schema API does not provide counts) |
| Inventory Items | Tracks physical stock of parts, serial numbers, and locations. | N/A (Schema API does not provide counts) |
| Vendors | Manages records for vendors. | N/A (Schema API does not provide counts) |
| Vendor Transactions | Logs individual fueling operations for specific aircraft. | N/A (Schema API does not provide counts) |
| Jobs/Work Orders | Manages records for jobs/work orders. | N/A (Schema API does not provide counts) |
| Parts Issued to Job | Master catalog of aviation parts and components. | N/A (Schema API does not provide counts) |
| Shipping PDF Data | Manages records for shipping pdf data. | N/A (Schema API does not provide counts) |
| Maintenance Work Plans | Manages records for maintenance work plans. | N/A (Schema API does not provide counts) |
| Company Info2 | Manages records for company info2. | N/A (Schema API does not provide counts) |
| Shipments | Manages records for shipments. | N/A (Schema API does not provide counts) |
| Shipment Line Items | Manages records for shipment line items. | N/A (Schema API does not provide counts) |
| Work Plan Tasks | Manages records for work plan tasks. | N/A (Schema API does not provide counts) |
| Planned Parts | Master catalog of aviation parts and components. | N/A (Schema API does not provide counts) |
| Labor Entries | Manages records for labor entries. | N/A (Schema API does not provide counts) |
| Inventory Movements | Tracks physical stock of parts, serial numbers, and locations. | N/A (Schema API does not provide counts) |
| Truck Fill Allocations (Tank-Truck) | Manages mobile fuel trucks, capacities, and active fuel levels. | N/A (Schema API does not provide counts) |
| Delivery Allocations | Manages records for delivery allocations. | N/A (Schema API does not provide counts) |
| Tank Level Checks | Manages records for tank level checks. | N/A (Schema API does not provide counts) |


# 3. Detailed Table Documentation

## Table: Fuel Types

| Field Name | Field Type | Required? | Description |
| ---------- | ---------- | --------- | ----------- |
| Fuel Types | Singlelinetext | No |  |
| Billing Precision  | Formula | No | Returns 0 if Fuel Types is Jet-A, otherwise returns 1. |
| Fuel Storage Tanks | Linked Record (Fuel Storage Tanks) | No |  |
| Fuel Trucks | Linked Record (Fuel Trucks) | No |  |
| Fuel Meters | Linked Record (Fuel Meters) | No |  |

**Special Fields:**
- **Primary Field:** Fuel Types
- **Formula Field:** Billing Precision  (`IF({fld4Q1lOyyouhnpRg}="Jet-A", 0, 1)`)
- **Linked Record:** Fuel Storage Tanks -> Fuel Storage Tanks
- **Linked Record:** Fuel Trucks -> Fuel Trucks
- **Linked Record:** Fuel Meters -> Fuel Meters


## Table: Fuel Storage Tanks

| Field Name | Field Type | Required? | Description |
| ---------- | ---------- | --------- | ----------- |
| Tank Name | Singlelinetext | No |  |
| Fuel Type | Linked Record (Fuel Types) | No |  |
| Fuel Types (from Fuel Type) | Lookup | No |  |
| Capacity | Number | No |  |
| Starting Gallons | Number | No |  |
| Fuel Deliveries | Linked Record (Fuel Deliveries) | No |  |
| Fuel Transfers Out | Linked Record (Nightly Truck Fill) | No |  |
| Gallons In | Lookup | No |  |
| Current Gallons | Formula | No |  |
| % Full | Formula | No |  |
| Gallons Out (Allocated) | Rollup | No |  |
| Gallons In (Rollup) | Rollup | No |  |
| Low Fuel Warning | Formula | No |  |
| Truck Fill Allocations (Tank-Truck) | Linked Record (Truck Fill Allocations (Tank-Truck)) | No |  |
| Delivery Allocations | Linked Record (Delivery Allocations) | No |  |
| Tank Level Checks | Linked Record (Tank Level Checks) | No |  |

**Special Fields:**
- **Primary Field:** Tank Name
- **Linked Record:** Fuel Type -> Fuel Types
- **Lookup Field:** Fuel Types (from Fuel Type)
- **Linked Record:** Fuel Deliveries -> Fuel Deliveries
- **Linked Record:** Fuel Transfers Out -> Nightly Truck Fill
- **Lookup Field:** Gallons In
- **Formula Field:** Current Gallons (`{fld6I4hKCkmgrqNCb} + IF({fld0LtiEJ1jPuEvas}, {fld0LtiEJ1jPuEvas}, 0) - IF({fldJ4sSFBy9eMHqKu}, {fldJ4sSFBy9eMHqKu}, 0)`)
- **Formula Field:** % Full (`{fldPA938nT6hwSlJQ} / {fldZRgr8HA9R4alBm}`)
- **Rollup Field:** Gallons Out (Allocated)
- **Rollup Field:** Gallons In (Rollup)
- **Formula Field:** Low Fuel Warning (`IF(   {fldPA938nT6hwSlJQ} / {fldZRgr8HA9R4alBm} <= 0.2,   "⚠ LOW FUEL",   "" )`)
- **Linked Record:** Truck Fill Allocations (Tank-Truck) -> Truck Fill Allocations (Tank-Truck)
- **Linked Record:** Delivery Allocations -> Delivery Allocations
- **Linked Record:** Tank Level Checks -> Tank Level Checks


## Table: Fuel Deliveries

| Field Name | Field Type | Required? | Description |
| ---------- | ---------- | --------- | ----------- |
| Fuel Delivery | Formula | No |  |
| Fuel Storage Tanks | Linked Record (Fuel Storage Tanks) | No |  |
| Gallons Delivered | Number | No |  |
| Date | Datetime | No |  |
| BOL # | Number | No |  |
| BOL  | Multipleattachments | No |  |
| Fuel Reconciliation | Linked Record (Fuel Reconciliation) | No |  |
| Month Key | Formula | No |  |
| Delivery Allocations | Linked Record (Delivery Allocations) | No |  |

**Special Fields:**
- **Primary Field:** Fuel Delivery
- **Formula Field:** Fuel Delivery (`CONCATENATE({fldQUbhUfev6nuRz0})`)
- **Linked Record:** Fuel Storage Tanks -> Fuel Storage Tanks
- **Linked Record:** Fuel Reconciliation -> Fuel Reconciliation
- **Formula Field:** Month Key (`DATETIME_FORMAT({fldzOMdc84E0cl7FZ}, "YYYY-MM")`)
- **Linked Record:** Delivery Allocations -> Delivery Allocations


## Table: Nightly Truck Fill

| Field Name | Field Type | Required? | Description |
| ---------- | ---------- | --------- | ----------- |
| Name | Formula | No |  |
| Date | Datetime | No |  |
| Fuel Storage Tanks | Linked Record (Fuel Storage Tanks) | No |  |
| Fuel Trucks | Linked Record (Fuel Trucks) | No |  |
| Meter Start | Number | No |  |
| Meter End | Number | No |  |
| Gallon Out  | Formula | No | Calculates the difference between Meter End and Meter Start to show usage. |
| Selected Tank Count | Formula | No |  |
| Whole Gallons Out Per Tank | Formula | No |  |
| Override Approved | Checkbox | No |  |
| Override Reason | Multilinetext | No |  |
| Variance Flag | Formula | No |  |
| Fuel Reconciliation | Linked Record (Fuel Reconciliation) | No |  |
| Month Key | Formula | No |  |
| Truck Fill Allocations (Tank-Truck) | Linked Record (Truck Fill Allocations (Tank-Truck)) | No |  |

**Special Fields:**
- **Primary Field:** Name
- **Formula Field:** Name (`CONCATENATE({fldYFSWEJroDGJMNw})`)
- **Linked Record:** Fuel Storage Tanks -> Fuel Storage Tanks
- **Linked Record:** Fuel Trucks -> Fuel Trucks
- **Formula Field:** Gallon Out  (`{fldxu7RUJsGSBKoxH} - {fldrpvUYkbGgpJ1xB}`)
- **Formula Field:** Selected Tank Count (`COUNTA({fldk3FGLkWf4wPVA5})`)
- **Formula Field:** Whole Gallons Out Per Tank (`IF(   {fldDoaHZqeyvFxabp}=0,   BLANK(),   ROUND({fld5iGGNZOgBfklrY}/{fldDoaHZqeyvFxabp},0) )`)
- **Formula Field:** Variance Flag (`IF(   ABS({fld5iGGNZOgBfklrY} - ({fldxu7RUJsGSBKoxH} - {fldrpvUYkbGgpJ1xB})) > 5,   "REVIEW REQUIRED",   "" )`)
- **Linked Record:** Fuel Reconciliation -> Fuel Reconciliation
- **Formula Field:** Month Key (`DATETIME_FORMAT({fldYFSWEJroDGJMNw}, "YYYY-MM")`)
- **Linked Record:** Truck Fill Allocations (Tank-Truck) -> Truck Fill Allocations (Tank-Truck)


## Table: Fuel Trucks

| Field Name | Field Type | Required? | Description |
| ---------- | ---------- | --------- | ----------- |
| Truck Name | Singlelinetext | No |  |
| Fuel Type | Linked Record (Fuel Types) | No |  |
| Fuel Types (from Fuel Type) | Lookup | No |  |
| Capacity | Number | No |  |
| Starting Gallons | Number | No |  |
| Fuel Transfers In | Linked Record (Nightly Truck Fill) | No |  |
| Gallons Loaded | Singleselect | No |  |
| Aircraft Fueling | Linked Record (Aircraft Fueling Transactions) | No |  |
| Gallons Dispensed | Rollup | No |  |
| Fuel Meters | Linked Record (Fuel Meters) | No |  |
| Current Gallons | Formula | No |  |
| Gallons Loaded (Rollup) | Rollup | No |  |
| Gallons Dispensed (Rollup) | Rollup | No |  |
| Low Truck Fuel  | Formula | No |  |

**Special Fields:**
- **Primary Field:** Truck Name
- **Linked Record:** Fuel Type -> Fuel Types
- **Lookup Field:** Fuel Types (from Fuel Type)
- **Linked Record:** Fuel Transfers In -> Nightly Truck Fill
- **Linked Record:** Aircraft Fueling -> Aircraft Fueling Transactions
- **Rollup Field:** Gallons Dispensed
- **Linked Record:** Fuel Meters -> Fuel Meters
- **Formula Field:** Current Gallons (`{fldV1A4IVVSYFQRDB} + IF({fldL9PKPrMA0GDWcB}, {fldL9PKPrMA0GDWcB}, 0) - IF({fldEJY3uSvNakznPB}, {fldEJY3uSvNakznPB}, 0)`)
- **Rollup Field:** Gallons Loaded (Rollup)
- **Rollup Field:** Gallons Dispensed (Rollup)
- **Formula Field:** Low Truck Fuel  (`IF(   {fldYA8JR4OYwuU2Cp} < 200,   "⚠ REFILL REQUIRED",   "" )`)


## Table: Fuel Meters

| Field Name | Field Type | Required? | Description |
| ---------- | ---------- | --------- | ----------- |
| Meter Name | Singlelinetext | No |  |
| Meter Type | Singleselect | No |  |
| Fuel Type | Linked Record (Fuel Types) | No |  |
| Fuel Types (from Fuel Type) | Lookup | No |  |
| Truck | Linked Record (Fuel Trucks) | No |  |
| Truck Name (from Truck) | Lookup | No |  |
| Meter SN: | Singlelinetext | No |  |
| Aircraft Fueling Transactions | Singlelinetext | No |  |
| Fuel Meter Lines | Linked Record (Fuel Meter Lines) | No |  |
| Meter Position | Singleselect | No |  |
| Active | Checkbox | No |  |
| Last Posted Reading | Number | No |  |
| Last Posted Date | Date | No |  |

**Special Fields:**
- **Primary Field:** Meter Name
- **Linked Record:** Fuel Type -> Fuel Types
- **Lookup Field:** Fuel Types (from Fuel Type)
- **Linked Record:** Truck -> Fuel Trucks
- **Lookup Field:** Truck Name (from Truck)
- **Linked Record:** Fuel Meter Lines -> Fuel Meter Lines


## Table: Aircraft Fueling Transactions

| Field Name | Field Type | Required? | Description |
| ---------- | ---------- | --------- | ----------- |
| Fuel Transaction # | Formula | No |  |
| Aircraft | Lookup | No |  |
| Service Orders | Linked Record (Service Orders) | No |  |
| Customer | Lookup | No |  |
| Employee | Linked Record (Employees) | No |  |
| Fuel Trucks | Linked Record (Fuel Trucks) | No |  |
| Meter Start | Number | No |  |
| Meter End | Number | No |  |
| Gallons Pumped | Formula | No |  |
| Billed Gallons | Formula | No |  |
| Fuel Reconciliation | Linked Record (Fuel Reconciliation) | No |  |
| Fueling Date | Datetime | No |  |
| Month Key | Formula | No |  |
| Transaction Time | Datetime | No |  |
| Fuel Reconciliation 2 | Linked Record (Fuel Reconciliation) | No |  |
| Fuel Types | Rollup | No |  |
| Truck | Singlelinetext | No |  |
| Aircraft (from Customer) | Lookup | No |  |
| Aircraft link | Linked Record (Aircraft) | No |  |
| Customers link | Linked Record (Customers) | No |  |
| Status | Singleselect | No |  |
| Posted Gallons | Formula | No |  |
| Fuel Meter Lines | Linked Record (Fuel Meter Lines) | No |  |
| Total Posted Gallons | Rollup | No |  |
| Fueling Summary Notes (AI) | Aitext | No |  |

**Special Fields:**
- **Primary Field:** Fuel Transaction #
- **Formula Field:** Fuel Transaction # (`CONCATENATE({fldNj2N6Op4PYOPMu}, {fld7myMoCSBKjRzBQ})`)
- **Lookup Field:** Aircraft
- **Linked Record:** Service Orders -> Service Orders
- **Lookup Field:** Customer
- **Linked Record:** Employee -> Employees
- **Linked Record:** Fuel Trucks -> Fuel Trucks
- **Formula Field:** Gallons Pumped (`({fldudKxh5Z5n03dVM}-{fldQwAOgmXcozSJdo})`)
- **Formula Field:** Billed Gallons (`IF(   {fld8oqNGiIgeweOQw}="Jet-A",   ROUND({flduqG6RfiiH6JbIe},0),   ROUND({flduqG6RfiiH6JbIe},1) )`)
- **Linked Record:** Fuel Reconciliation -> Fuel Reconciliation
- **Formula Field:** Month Key (`DATETIME_FORMAT({fldyaRW2qEhfmbKAo}, "YYYY-MM")`)
- **Linked Record:** Fuel Reconciliation 2 -> Fuel Reconciliation
- **Rollup Field:** Fuel Types
- **Lookup Field:** Aircraft (from Customer)
- **Linked Record:** Aircraft link -> Aircraft
- **Linked Record:** Customers link -> Customers
- **Formula Field:** Posted Gallons (`IF({fldskpklqZ6cNj3cn}="Posted", {flduqG6RfiiH6JbIe}, 0)`)
- **Linked Record:** Fuel Meter Lines -> Fuel Meter Lines
- **Rollup Field:** Total Posted Gallons


## Table: Fuel Meter Lines

| Field Name | Field Type | Required? | Description |
| ---------- | ---------- | --------- | ----------- |
| Line ID | Autonumber | No |  |
| Fueling Transaction | Linked Record (Aircraft Fueling Transactions) | No |  |
| Meter | Linked Record (Fuel Meters) | No |  |
| Truck (from meter) | Lookup | No |  |
| Meter Start | Number | No |  |
| Meter End | Number | No |  |
| Gallons | Formula | No |  |
| Status  | Singleselect | No |  |
| Posted Gallons | Formula | No |  |
| Notes | Multilinetext | No |  |

**Special Fields:**
- **Primary Field:** Line ID
- **Linked Record:** Fueling Transaction -> Aircraft Fueling Transactions
- **Linked Record:** Meter -> Fuel Meters
- **Lookup Field:** Truck (from meter)
- **Formula Field:** Gallons (`{fld5M0VZvmBqRka0O}-{fldj1W9B8NF7z39Na}`)
- **Formula Field:** Posted Gallons (`IF({fldlVDn7sNEuk4FQz}= "Posted", {fldVWZSlOPtIWrsZN}, 0)`)


## Table: Service Orders

| Field Name | Field Type | Required? | Description |
| ---------- | ---------- | --------- | ----------- |
| Service Order # | Autonumber | No |  |
| Check-in Time | Datetime | No |  |
| Service Order Created | Date | No |  |
| Customers | Linked Record (Customers) | No |  |
| Aircraft | Linked Record (Aircraft) | No |  |
| Requested Services | Multipleselects | No |  |
| Status | Singleselect | No |  |
| Requested Gallons | Number | No |  |
| Special Instructions | Multilinetext | No |  |
| Departure Time | Datetime | No |  |
| CSR | Singleselect | No |  |
| Ramp Spot | Singleselect | No |  |
| ETA | Datetime | No |  |
| Fuel Transactions | Linked Record (Aircraft Fueling Transactions) | No |  |
| Visit Type | Singleselect | No |  |
| Fuel Requested | Checkbox | No |  |
| Requested Fuel Type | Lookup | No |  |
| Opened Time | Datetime | No |  |
| Completed Time | Date | No |  |
| Assigned Truck | Singlelinetext | No |  |
| Aircraft Fueling Transactions | Singlelinetext | No |  |
| Maintenance/Work Orders | Linked Record (Employees) | No |  |
| Customers copy | Linked Record (Company Info2) | No |  |
| Jobs/Work Orders | Linked Record (Jobs/Work Orders) | No |  |

**Special Fields:**
- **Primary Field:** Service Order #
- **Linked Record:** Customers -> Customers
- **Linked Record:** Aircraft -> Aircraft
- **Linked Record:** Fuel Transactions -> Aircraft Fueling Transactions
- **Lookup Field:** Requested Fuel Type
- **Linked Record:** Maintenance/Work Orders -> Employees
- **Linked Record:** Customers copy -> Company Info2
- **Linked Record:** Jobs/Work Orders -> Jobs/Work Orders


## Table: Fuel Reconciliation

| Field Name | Field Type | Required? | Description |
| ---------- | ---------- | --------- | ----------- |
| Name | Singlelinetext | No |  |
| Month | Date | No |  |
| Fuel Type | Singleselect | No |  |
| Deliveries | Linked Record (Fuel Deliveries) | No |  |
| Truck Fills | Linked Record (Nightly Truck Fill) | No |  |
| Fuelings | Linked Record (Aircraft Fueling Transactions) | No |  |
| Fueling (Recon) | Linked Record (Aircraft Fueling Transactions) | No |  |
| Total Delivered (Rollup) | Rollup | No |  |
| Total Pumped to Trucks (Rollup) | Rollup | No |  |
| Total Dispensed to Aircraft (Rollup) | Rollup | No |  |
| Calculated Loss | Formula | No |  |
| Loss % | Formula | No |  |
| Month Key | Formula | No |  |

**Special Fields:**
- **Primary Field:** Name
- **Linked Record:** Deliveries -> Fuel Deliveries
- **Linked Record:** Truck Fills -> Nightly Truck Fill
- **Linked Record:** Fuelings -> Aircraft Fueling Transactions
- **Linked Record:** Fueling (Recon) -> Aircraft Fueling Transactions
- **Rollup Field:** Total Delivered (Rollup)
- **Rollup Field:** Total Pumped to Trucks (Rollup)
- **Rollup Field:** Total Dispensed to Aircraft (Rollup)
- **Formula Field:** Calculated Loss (`IF(   {fldms9l0iOoBJXusA},   {fldms9l0iOoBJXusA} - {fldAtApy9sKsVEZVo},   BLANK() )`)
- **Formula Field:** Loss % (`IF(   {fldms9l0iOoBJXusA},   {fldBzLaOBmd7cbkRo} / {fldms9l0iOoBJXusA},   BLANK() )`)
- **Formula Field:** Month Key (`DATETIME_FORMAT({fldgnP7cjV8MRwtKU}, "YYYY-MM")`)


## Table: Customers

| Field Name | Field Type | Required? | Description |
| ---------- | ---------- | --------- | ----------- |
| Customer Name | Singlelinetext | No |  |
| Phone | Phonenumber | No |  |
| Email | Email | No |  |
| Company | Singlelinetext | No |  |
| Notes | Singlelinetext | No |  |
| Aircraft | Linked Record (Aircraft) | No |  |
| Service Orders | Linked Record (Service Orders) | No |  |
| Aircraft Fueling Transactions | Singlelinetext | No |  |
| Street Address | Singlelinetext | No |  |
| Jobs/Work Orders | Linked Record (Jobs/Work Orders) | No |  |
| Aircraft Fueling Transactions 2 | Linked Record (Aircraft Fueling Transactions) | No |  |

**Special Fields:**
- **Primary Field:** Customer Name
- **Linked Record:** Aircraft -> Aircraft
- **Linked Record:** Service Orders -> Service Orders
- **Linked Record:** Jobs/Work Orders -> Jobs/Work Orders
- **Linked Record:** Aircraft Fueling Transactions 2 -> Aircraft Fueling Transactions


## Table: Aircraft

| Field Name | Field Type | Required? | Description |
| ---------- | ---------- | --------- | ----------- |
| Tail Number | Singlelinetext | No |  |
| Customers | Linked Record (Customers) | No |  |
| Type | Singlelinetext | No |  |
| Fuel Type | Singleselect | No |  |
| Service Orders | Linked Record (Service Orders) | No |  |
| Fuelings | Singlelinetext | No |  |
| Customers copy | Linked Record (Company Info2) | No |  |
| Jobs/Work Orders | Linked Record (Jobs/Work Orders) | No |  |
| Aircraft Fueling Transactions | Linked Record (Aircraft Fueling Transactions) | No |  |

**Special Fields:**
- **Primary Field:** Tail Number
- **Linked Record:** Customers -> Customers
- **Linked Record:** Service Orders -> Service Orders
- **Linked Record:** Customers copy -> Company Info2
- **Linked Record:** Jobs/Work Orders -> Jobs/Work Orders
- **Linked Record:** Aircraft Fueling Transactions -> Aircraft Fueling Transactions


## Table: Services

| Field Name | Field Type | Required? | Description |
| ---------- | ---------- | --------- | ----------- |
| Name | Singlelinetext | No |  |

**Special Fields:**
- **Primary Field:** Name


## Table: Employees

| Field Name | Field Type | Required? | Description |
| ---------- | ---------- | --------- | ----------- |
| Name | Singlelinetext | No |  |
| Notes | Multilinetext | No |  |
| Assignee | Singlecollaborator | No |  |
| Status | Singleselect | No |  |
| Attachments | Multipleattachments | No |  |
| Attachment Summary | Aitext | No | An AI generated summary of the Attachments field. Upload files to Attachments to generate a summary. |
| Aircraft Fueling Transactions | Linked Record (Aircraft Fueling Transactions) | No |  |
| Service Orders | Linked Record (Service Orders) | No |  |
| Parts Issued to Job | Linked Record (Parts Issued to Job) | No |  |

**Special Fields:**
- **Primary Field:** Name
- **Linked Record:** Aircraft Fueling Transactions -> Aircraft Fueling Transactions
- **Linked Record:** Service Orders -> Service Orders
- **Linked Record:** Parts Issued to Job -> Parts Issued to Job


## Table: Parts Master

| Field Name | Field Type | Required? | Description |
| ---------- | ---------- | --------- | ----------- |
| Part Number | Singlelinetext | No |  |
| Discription | Singlelinetext | No |  |
| ATA Chapter | Singlelinetext | No |  |
| Manufacturer | Singlelinetext | No |  |
| Condition | Singleselect | No |  |
| Serialized | Checkbox | No |  |
| Unit of Measure | Singleselect | No |  |
| Min Stock | Number | No |  |
| Notes | Multilinetext | No |  |
| Active | Checkbox | No |  |
| Inventory | Linked Record (Inventory Items) | No |  |
| Qty | Number | No |  |
| Location | Singlelinetext | No |  |
| Shelf / Bin | Linked Record (Inventory Items) | No |  |
| Shelf / Bin (from Inventory 2) | Lookup | No |  |
| Shipping PDF Data | Linked Record (Shipping PDF Data) | No |  |
| Inventory 2 | Linked Record (Inventory Items) | No |  |
| Planned Parts | Linked Record (Planned Parts) | No |  |

**Special Fields:**
- **Primary Field:** Part Number
- **Linked Record:** Inventory -> Inventory Items
- **Linked Record:** Shelf / Bin -> Inventory Items
- **Lookup Field:** Shelf / Bin (from Inventory 2)
- **Linked Record:** Shipping PDF Data -> Shipping PDF Data
- **Linked Record:** Inventory 2 -> Inventory Items
- **Linked Record:** Planned Parts -> Planned Parts


## Table: Inventory Items

| Field Name | Field Type | Required? | Description |
| ---------- | ---------- | --------- | ----------- |
| Inventory ID | Autonumber | No |  |
| Part | Linked Record (Parts Master) | No |  |
| Serial Number | Singlelinetext | No |  |
| Quantity On Hand | Number | No |  |
| Contition | Singleselect | No |  |
| Location | Singleselect | No |  |
| Shelf / Bin | Singlelinetext | No |  |
| Status | Singleselect | No |  |
| Last Movement Date | Lastmodifiedtime | No |  |
| Linked Job | Singlelinetext | No |  |
| Linked Vendor Transaction | Singlelinetext | No |  |
| Vendor Transactions | Linked Record (Vendor Transactions) | No |  |
| Parts Issued to Job | Linked Record (Parts Issued to Job) | No |  |
| Parts Master | Linked Record (Parts Master) | No |  |
| Shipment Line Items | Linked Record (Shipment Line Items) | No |  |
| Description | Linked Record (Parts Master) | No |  |
| Discription (from Description) | Lookup | No |  |
| Planned Parts | Linked Record (Planned Parts) | No |  |
| Owner | Singleselect | No |  |
| Photos | Multipleattachments | No |  |
| Inventory Movements | Linked Record (Inventory Movements) | No |  |

**Special Fields:**
- **Primary Field:** Inventory ID
- **Linked Record:** Part -> Parts Master
- **Linked Record:** Vendor Transactions -> Vendor Transactions
- **Linked Record:** Parts Issued to Job -> Parts Issued to Job
- **Linked Record:** Parts Master -> Parts Master
- **Linked Record:** Shipment Line Items -> Shipment Line Items
- **Linked Record:** Description -> Parts Master
- **Lookup Field:** Discription (from Description)
- **Linked Record:** Planned Parts -> Planned Parts
- **Linked Record:** Inventory Movements -> Inventory Movements


## Table: Vendors

| Field Name | Field Type | Required? | Description |
| ---------- | ---------- | --------- | ----------- |
| Vendor Name | Singlelinetext | No |  |
| Vendor Type | Singleselect | No |  |
| FAA Repair Station # | Singlelinetext | No |  |
| Contact Name | Singlelinetext | No |  |
| Phone Number | Phonenumber | No |  |
| Email | Email | No |  |
| Turn Time (Days) | Singlelinetext | No |  |
| Notes | Multilinetext | No |  |
| Vendor Transactions | Linked Record (Vendor Transactions) | No |  |
| Shipping PDF Data | Linked Record (Shipping PDF Data) | No |  |
| Shipments | Linked Record (Shipments) | No |  |

**Special Fields:**
- **Primary Field:** Vendor Name
- **Linked Record:** Vendor Transactions -> Vendor Transactions
- **Linked Record:** Shipping PDF Data -> Shipping PDF Data
- **Linked Record:** Shipments -> Shipments


## Table: Vendor Transactions

| Field Name | Field Type | Required? | Description |
| ---------- | ---------- | --------- | ----------- |
| Transaction ID | Autonumber | No |  |
| Inventory Item | Linked Record (Inventory Items) | No |  |
| Vendor | Linked Record (Vendors) | No |  |
| Transaction Type | Singleselect | No |  |
| Date Out | Date | No |  |
| Expected Return Date | Formula | No |  |
| Turn Time | Lookup | No |  |
| Actual Return Date | Date | No |  |
| Status | Singleselect | No |  |
| 8139n  Recieved | Checkbox | No |  |
| 8130 | Multipleattachments | No |  |
| Cost | Currency | No |  |
| Notes | Multilinetext | No |  |
| Shipping Form (PDF) | Multipleattachments | No |  |
| Tracking Number | Singlelinetext | No |  |
| Carrier | Singleselect | No |  |
| Ship Date | Date | No |  |
| Received by Vendor | Checkbox | No |  |
| Return Shipping (PDF) | Multipleattachments | No |  |
| Inbound Tracking # | Singlelinetext | No |  |
| Receiving Inspection Complete | Checkbox | No |  |
| Shipments | Linked Record (Shipments) | No |  |
| Date In | Date | No |  |
| Inventory Movements | Linked Record (Inventory Movements) | No |  |

**Special Fields:**
- **Primary Field:** Transaction ID
- **Linked Record:** Inventory Item -> Inventory Items
- **Linked Record:** Vendor -> Vendors
- **Formula Field:** Expected Return Date (`DATEADD({fldsPhsJnqH8J80vg}, VALUE({fldHHkSFjLNFbAVWr}),'days')`)
- **Lookup Field:** Turn Time
- **Linked Record:** Shipments -> Shipments
- **Linked Record:** Inventory Movements -> Inventory Movements


## Table: Jobs/Work Orders

| Field Name | Field Type | Required? | Description |
| ---------- | ---------- | --------- | ----------- |
| Job Number | Singlelinetext | No |  |
| Aircraft N-Number | Linked Record (Aircraft) | No |  |
| Aircraft Type | Singlelinetext | No |  |
| Customer | Linked Record (Customers) | No |  |
| Job Status | Singleselect | No |  |
| Date Opened | Date | No |  |
| Date Closesd | Date | No |  |
| Assigned Employee | Singlelinetext | No |  |
| Notes | Multilinetext | No |  |
| Parts Issued to Job | Linked Record (Parts Issued to Job) | No |  |
| Shipping PDF Data | Linked Record (Shipping PDF Data) | No |  |
| Maintenance Work Plans | Linked Record (Maintenance Work Plans) | No |  |
| Inventory Movements | Linked Record (Inventory Movements) | No |  |
| Job Type | Singleselect | No |  |
| Service Order | Linked Record (Service Orders) | No |  |
| Work Plan Tasks | Linked Record (Work Plan Tasks) | No |  |

**Special Fields:**
- **Primary Field:** Job Number
- **Linked Record:** Aircraft N-Number -> Aircraft
- **Linked Record:** Customer -> Customers
- **Linked Record:** Parts Issued to Job -> Parts Issued to Job
- **Linked Record:** Shipping PDF Data -> Shipping PDF Data
- **Linked Record:** Maintenance Work Plans -> Maintenance Work Plans
- **Linked Record:** Inventory Movements -> Inventory Movements
- **Linked Record:** Service Order -> Service Orders
- **Linked Record:** Work Plan Tasks -> Work Plan Tasks


## Table: Parts Issued to Job

| Field Name | Field Type | Required? | Description |
| ---------- | ---------- | --------- | ----------- |
| Issue ID | Autonumber | No |  |
| Inventory Item | Linked Record (Inventory Items) | No |  |
| Job | Linked Record (Jobs/Work Orders) | No |  |
| Job Number (from Job) | Lookup | No |  |
| Qty Issued | Number | No |  |
| Date Issued | Date | No |  |
| Issued By | Linked Record (Employees) | No |  |
| Name (from Issued By) | Lookup | No |  |
| Returned | Checkbox | No |  |
| Qty Returned | Number | No |  |

**Special Fields:**
- **Primary Field:** Issue ID
- **Linked Record:** Inventory Item -> Inventory Items
- **Linked Record:** Job -> Jobs/Work Orders
- **Lookup Field:** Job Number (from Job)
- **Linked Record:** Issued By -> Employees
- **Lookup Field:** Name (from Issued By)


## Table: Shipping PDF Data

| Field Name | Field Type | Required? | Description |
| ---------- | ---------- | --------- | ----------- |
| Shipping ID | Autonumber | No |  |
| Company Info | Linked Record (Company Info2) | No |  |
| Customer Name (from Company Info) | Lookup | No |  |
| Vendor Info | Linked Record (Vendors) | No |  |
| Vendor Name (from Vendor Info) | Lookup | No |  |
| Part Number | Linked Record (Parts Master) | No |  |
| Part Number (from Part Number) | Lookup | No |  |
| Discription (from Part Number) | Lookup | No |  |
| Serial Number | Number | No |  |
| Qty | Number | No |  |
| Condition | Singleselect | No |  |
| Transaction Type | Singleselect | No |  |
| Job Number | Linked Record (Jobs/Work Orders) | No |  |
| Date Shipped | Date | No |  |
| RMA / Vendor Reference | Singlelinetext | No |  |
| Notes / Special Instructions | Multilinetext | No |  |

**Special Fields:**
- **Primary Field:** Shipping ID
- **Linked Record:** Company Info -> Company Info2
- **Lookup Field:** Customer Name (from Company Info)
- **Linked Record:** Vendor Info -> Vendors
- **Lookup Field:** Vendor Name (from Vendor Info)
- **Linked Record:** Part Number -> Parts Master
- **Lookup Field:** Part Number (from Part Number)
- **Lookup Field:** Discription (from Part Number)
- **Linked Record:** Job Number -> Jobs/Work Orders


## Table: Maintenance Work Plans

| Field Name | Field Type | Required? | Description |
| ---------- | ---------- | --------- | ----------- |
| Work Plan ID | Autonumber | No |  |
| Job | Linked Record (Jobs/Work Orders) | No |  |
| Plan Type | Singleselect | No |  |
| Aircraft N-Number | Lookup | No |  |
| Aircraft Type | Lookup | No |  |
| Status | Singleselect | No |  |
| Prepared By | Singlelinetext | No |  |
| Approved By | Singlelinetext | No |  |
| Date Prepared | Date | No |  |
| Date Approved | Date | No |  |
| Notes | Multilinetext | No |  |
| Work Plan Tasks | Linked Record (Work Plan Tasks) | No |  |

**Special Fields:**
- **Primary Field:** Work Plan ID
- **Linked Record:** Job -> Jobs/Work Orders
- **Lookup Field:** Aircraft N-Number
- **Lookup Field:** Aircraft Type
- **Linked Record:** Work Plan Tasks -> Work Plan Tasks


## Table: Company Info2

| Field Name | Field Type | Required? | Description |
| ---------- | ---------- | --------- | ----------- |
| Company Info | Singlelinetext | No |  |
| Phone | Phonenumber | No |  |
| Email | Email | No |  |
| Company | Singlelinetext | No |  |
| Notes | Singlelinetext | No |  |
| Aircraft | Linked Record (Aircraft) | No |  |
| Service Orders | Linked Record (Service Orders) | No |  |
| Aircraft Fueling Transactions | Singlelinetext | No |  |
| Street Address | Singlelinetext | No |  |
| Shipping PDF Data | Linked Record (Shipping PDF Data) | No |  |

**Special Fields:**
- **Primary Field:** Company Info
- **Linked Record:** Aircraft -> Aircraft
- **Linked Record:** Service Orders -> Service Orders
- **Linked Record:** Shipping PDF Data -> Shipping PDF Data


## Table: Shipments

| Field Name | Field Type | Required? | Description |
| ---------- | ---------- | --------- | ----------- |
| Shipment Number | Autonumber | No |  |
| Type | Singleselect | No |  |
| Vendor / Custormer | Linked Record (Vendors) | No |  |
| Ship From | Multilinetext | No |  |
| Ship To | Multilinetext | No |  |
| Ship Date | Date | No |  |
| Carrier | Singleselect | No |  |
| Tracking # | Number | No |  |
| Related Vendor Transaction | Linked Record (Vendor Transactions) | No |  |
| Shipment Status | Singleselect | No |  |
| PFD Documents | Multipleattachments | No |  |
| Shipment Line Items | Linked Record (Shipment Line Items) | No |  |

**Special Fields:**
- **Primary Field:** Shipment Number
- **Linked Record:** Vendor / Custormer -> Vendors
- **Linked Record:** Related Vendor Transaction -> Vendor Transactions
- **Linked Record:** Shipment Line Items -> Shipment Line Items


## Table: Shipment Line Items

| Field Name | Field Type | Required? | Description |
| ---------- | ---------- | --------- | ----------- |
| Line Items | Singlelinetext | No |  |
| Shipment | Linked Record (Shipments) | No |  |
| Inventory Item | Linked Record (Inventory Items) | No |  |
| Part Number | Lookup | No |  |
| Serial Number | Lookup | No |  |
| Qty | Number | No |  |
| Condition | Singlelinetext | No |  |
| Notes / Scope | Multipleselects | No |  |
| Description | Lookup | No |  |

**Special Fields:**
- **Primary Field:** Line Items
- **Linked Record:** Shipment -> Shipments
- **Linked Record:** Inventory Item -> Inventory Items
- **Lookup Field:** Part Number
- **Lookup Field:** Serial Number
- **Lookup Field:** Description


## Table: Work Plan Tasks

| Field Name | Field Type | Required? | Description |
| ---------- | ---------- | --------- | ----------- |
| Task ID | Autonumber | No |  |
| Work Plan | Linked Record (Maintenance Work Plans) | No |  |
| Task Type | Singleselect | No |  |
| Reference | Singlelinetext | No |  |
| CMM | Singlelinetext | No |  |
| AMM | Singlelinetext | No |  |
| AD # | Singlelinetext | No |  |
| SB # | Singlelinetext | No |  |
| Task Description | Multilinetext | No |  |
| ATA Chapter | Singlelinetext | No |  |
| Labor Estimate (hrs) | Number | No |  |
| Status | Singleselect | No |  |
| Assigned Tech | Singlelinetext | No |  |
| Completion Date | Date | No |  |
| Signoff Required | Checkbox | No |  |
| Inspector Signoff | Singlelinetext | No |  |
| Notes | Multilinetext | No |  |
| Planned Parts | Linked Record (Planned Parts) | No |  |
| Labor Entries | Linked Record (Labor Entries) | No |  |
| Jobs | Linked Record (Jobs/Work Orders) | No |  |

**Special Fields:**
- **Primary Field:** Task ID
- **Linked Record:** Work Plan -> Maintenance Work Plans
- **Linked Record:** Planned Parts -> Planned Parts
- **Linked Record:** Labor Entries -> Labor Entries
- **Linked Record:** Jobs -> Jobs/Work Orders


## Table: Planned Parts

| Field Name | Field Type | Required? | Description |
| ---------- | ---------- | --------- | ----------- |
| Planned Part ID | Autonumber | No |  |
| Work Plan Task | Linked Record (Work Plan Tasks) | No |  |
| Part | Linked Record (Parts Master) | No |  |
| Qty Required | Number | No |  |
| Qty Available (lookup from Inventory) | Number | No |  |
| Shortage? | Formula | No |  |
| Reserved Inventory | Linked Record (Inventory Items) | No |  |
| Issued? | Checkbox | No |  |
| Shortage Status (AI Categorization) | Singleselect | No |  |

**Special Fields:**
- **Primary Field:** Planned Part ID
- **Linked Record:** Work Plan Task -> Work Plan Tasks
- **Linked Record:** Part -> Parts Master
- **Formula Field:** Shortage? (`IF({fld5QbX9eNyQJrWNZ} > {fldYUzK91Pp6TrOKX}, TRUE(), FALSE())`)
- **Linked Record:** Reserved Inventory -> Inventory Items


## Table: Labor Entries

| Field Name | Field Type | Required? | Description |
| ---------- | ---------- | --------- | ----------- |
| Labor ID | Autonumber | No |  |
| Task | Linked Record (Work Plan Tasks) | No |  |
| Tech | Singlelinetext | No |  |
| Date | Date | No |  |
| Hours | Number | No |  |
| Description | Multilinetext | No |  |
| Work Plan Task Status (AI Summarization) | Singleselect | No |  |
| Requires Invoice? (AI Decision) | Singleselect | No |  |

**Special Fields:**
- **Primary Field:** Labor ID
- **Linked Record:** Task -> Work Plan Tasks


## Table: Inventory Movements

| Field Name | Field Type | Required? | Description |
| ---------- | ---------- | --------- | ----------- |
| Move ID | Autonumber | No |  |
| Inventory Item | Linked Record (Inventory Items) | No |  |
| Move Type | Singleselect | No |  |
| Qty Change | Number | No |  |
| From Location | Singleselect | No |  |
| To Location | Singleselect | No |  |
| Related Vendor Transaction | Linked Record (Vendor Transactions) | No |  |
| Related Job | Linked Record (Jobs/Work Orders) | No |  |
| Moved By | Singleselect | No |  |
| Move Date | Date | No |  |
| Notes | Multilinetext | No |  |
| Shipping PDF | Multipleattachments | No |  |

**Special Fields:**
- **Primary Field:** Move ID
- **Linked Record:** Inventory Item -> Inventory Items
- **Linked Record:** Related Vendor Transaction -> Vendor Transactions
- **Linked Record:** Related Job -> Jobs/Work Orders


## Table: Truck Fill Allocations (Tank-Truck)

| Field Name | Field Type | Required? | Description |
| ---------- | ---------- | --------- | ----------- |
| Allocation ID | Autonumber | No |  |
| Nightly Truck Fill | Linked Record (Nightly Truck Fill) | No |  |
| From Tank | Linked Record (Fuel Storage Tanks) | No |  |
| Allocated Gallons | Number | No |  |
| Auto Split? | Checkbox | No |  |
| Notes | Aitext | No | An AI generated summary of the Attachments field. Upload files to Attachments to generate a summary. |

**Special Fields:**
- **Primary Field:** Allocation ID
- **Linked Record:** Nightly Truck Fill -> Nightly Truck Fill
- **Linked Record:** From Tank -> Fuel Storage Tanks


## Table: Delivery Allocations

| Field Name | Field Type | Required? | Description |
| ---------- | ---------- | --------- | ----------- |
| Delivery Allocation ID | Autonumber | No |  |
| Fuel Delivery | Linked Record (Fuel Deliveries) | No |  |
| To Tank | Linked Record (Fuel Storage Tanks) | No |  |
| Allocated Gallons | Number | No |  |
| Auto Split? | Checkbox | No |  |
| Notes | Multilinetext | No |  |

**Special Fields:**
- **Primary Field:** Delivery Allocation ID
- **Linked Record:** Fuel Delivery -> Fuel Deliveries
- **Linked Record:** To Tank -> Fuel Storage Tanks


## Table: Tank Level Checks

| Field Name | Field Type | Required? | Description |
| ---------- | ---------- | --------- | ----------- |
| Check ID | Autonumber | No |  |
| Tank | Linked Record (Fuel Storage Tanks) | No |  |
| Check Date/Time | Datetime | No |  |
| Measured Gallons | Number | No |  |
| System Gallons | Lookup | No |  |
| Variance | Formula | No |  |
| Adjustment Approved | Checkbox | No |  |
| Adjustment Reason | Singleselect | No |  |
| Notes | Multilinetext | No |  |

**Special Fields:**
- **Primary Field:** Check ID
- **Linked Record:** Tank -> Fuel Storage Tanks
- **Lookup Field:** System Gallons
- **Formula Field:** Variance (`{fldCcrjYXzJmkpfX0} - {fld32ErkexYA1kQ9V}`)


# 4. Relationship Diagram

```text
Customers
│   └── Aircraft
│   │   └── Aircraft Fueling Transactions
│   │   │   └── Employees
│   │   │   │   └── Parts Issued to Job
│   │   │   │   │   └── Employees (recursive/already shown)
│   │   │   │   │   └── Inventory Items
│   │   │   │   │   │   └── Parts Master
│   │   │   │   │   │   │   └── Shipping PDF Data
│   │   │   │   │   │   │   │   └── Vendors
│   │   │   │   │   │   │   │   │   └── Shipments
│   │   │   │   │   │   │   │   │   │   └── Shipment Line Items
│   │   │   │   │   │   │   │   │   │   │   └── Shipments (recursive/already shown)
│   │   │   │   │   │   │   │   │   │       └── Inventory Items (recursive/already shown)
│   │   │   │   │   │   │   │   │   │   └── Vendors (recursive/already shown)
│   │   │   │   │   │   │   │   │       └── Vendor Transactions
│   │   │   │   │   │   │   │   │       │   └── Shipments (recursive/already shown)
│   │   │   │   │   │   │   │   │       │   └── Vendors (recursive/already shown)
│   │   │   │   │   │   │   │   │       │   └── Inventory Items (recursive/already shown)
│   │   │   │   │   │   │   │   │           └── Inventory Movements
│   │   │   │   │   │   │   │   │           │   └── Inventory Items (recursive/already shown)
│   │   │   │   │   │   │   │   │           │   └── Jobs/Work Orders
│   │   │   │   │   │   │   │   │           │   │   └── Maintenance Work Plans
│   │   │   │   │   │   │   │   │           │   │   │   └── Jobs/Work Orders (recursive/already shown)
│   │   │   │   │   │   │   │   │           │   │       └── Work Plan Tasks
│   │   │   │   │   │   │   │   │           │   │       │   └── Maintenance Work Plans (recursive/already shown)
│   │   │   │   │   │   │   │   │           │   │       │   └── Jobs/Work Orders (recursive/already shown)
│   │   │   │   │   │   │   │   │           │   │       │   └── Planned Parts
│   │   │   │   │   │   │   │   │           │   │       │   │   └── Inventory Items (recursive/already shown)
│   │   │   │   │   │   │   │   │           │   │       │   │   └── Parts Master (recursive/already shown)
│   │   │   │   │   │   │   │   │           │   │       │       └── Work Plan Tasks (recursive/already shown)
│   │   │   │   │   │   │   │   │           │   │           └── Labor Entries
│   │   │   │   │   │   │   │   │           │   │               └── Work Plan Tasks (recursive/already shown)
│   │   │   │   │   │   │   │   │           │   │   └── Inventory Movements (recursive/already shown)
│   │   │   │   │   │   │   │   │           │   │   └── Parts Issued to Job (recursive/already shown)
│   │   │   │   │   │   │   │   │           │   │   └── Customers
│   │   │   │   │   │   │   │   │           │   │   │   └── Aircraft (recursive/already shown)
│   │   │   │   │   │   │   │   │           │   │   │   └── Service Orders
│   │   │   │   │   │   │   │   │           │   │   │   │   └── Employees (recursive/already shown)
│   │   │   │   │   │   │   │   │           │   │   │   │   └── Aircraft Fueling Transactions (recursive/already shown)
│   │   │   │   │   │   │   │   │           │   │   │   │   └── Customers (recursive/already shown)
│   │   │   │   │   │   │   │   │           │   │   │   │   └── Jobs/Work Orders (recursive/already shown)
│   │   │   │   │   │   │   │   │           │   │   │   │   └── Company Info2
│   │   │   │   │   │   │   │   │           │   │   │   │   │   └── Aircraft (recursive/already shown)
│   │   │   │   │   │   │   │   │           │   │   │   │   │   └── Shipping PDF Data (recursive/already shown)
│   │   │   │   │   │   │   │   │           │   │   │   │       └── Service Orders (recursive/already shown)
│   │   │   │   │   │   │   │   │           │   │   │       └── Aircraft (recursive/already shown)
│   │   │   │   │   │   │   │   │           │   │   │   └── Aircraft Fueling Transactions (recursive/already shown)
│   │   │   │   │   │   │   │   │           │   │       └── Jobs/Work Orders (recursive/already shown)
│   │   │   │   │   │   │   │   │           │   │   └── Work Plan Tasks (recursive/already shown)
│   │   │   │   │   │   │   │   │           │   │   └── Aircraft (recursive/already shown)
│   │   │   │   │   │   │   │   │           │   │   └── Shipping PDF Data (recursive/already shown)
│   │   │   │   │   │   │   │   │           │       └── Service Orders (recursive/already shown)
│   │   │   │   │   │   │   │   │               └── Vendor Transactions (recursive/already shown)
│   │   │   │   │   │   │   │   │   └── Shipping PDF Data (recursive/already shown)
│   │   │   │   │   │   │   │       └── Vendor Transactions (recursive/already shown)
│   │   │   │   │   │   │   │   └── Parts Master (recursive/already shown)
│   │   │   │   │   │   │   │   └── Company Info2 (recursive/already shown)
│   │   │   │   │   │   │       └── Jobs/Work Orders (recursive/already shown)
│   │   │   │   │   │   │   └── Inventory Items (recursive/already shown)
│   │   │   │   │   │       └── Planned Parts (recursive/already shown)
│   │   │   │   │   │   └── Inventory Movements (recursive/already shown)
│   │   │   │   │   │   └── Shipment Line Items (recursive/already shown)
│   │   │   │   │   │   └── Parts Issued to Job (recursive/already shown)
│   │   │   │   │   │   └── Planned Parts (recursive/already shown)
│   │   │   │   │       └── Vendor Transactions (recursive/already shown)
│   │   │   │       └── Jobs/Work Orders (recursive/already shown)
│   │   │   │   └── Service Orders (recursive/already shown)
│   │   │       └── Aircraft Fueling Transactions (recursive/already shown)
│   │   │   └── Fuel Reconciliation
│   │   │   │   └── Fuel Deliveries
│   │   │   │   │   └── Fuel Storage Tanks
│   │   │   │   │   │   └── Fuel Types
│   │   │   │   │   │   │   └── Fuel Storage Tanks (recursive/already shown)
│   │   │   │   │   │   │   └── Fuel Meters
│   │   │   │   │   │   │   │   └── Fuel Types (recursive/already shown)
│   │   │   │   │   │   │   │   └── Fuel Trucks
│   │   │   │   │   │   │   │   │   └── Fuel Types (recursive/already shown)
│   │   │   │   │   │   │   │   │   └── Nightly Truck Fill
│   │   │   │   │   │   │   │   │   │   └── Fuel Storage Tanks (recursive/already shown)
│   │   │   │   │   │   │   │   │   │   └── Truck Fill Allocations (Tank-Truck)
│   │   │   │   │   │   │   │   │   │   │   └── Fuel Storage Tanks (recursive/already shown)
│   │   │   │   │   │   │   │   │   │       └── Nightly Truck Fill (recursive/already shown)
│   │   │   │   │   │   │   │   │   │   └── Fuel Reconciliation (recursive/already shown)
│   │   │   │   │   │   │   │   │       └── Fuel Trucks (recursive/already shown)
│   │   │   │   │   │   │   │   │   └── Aircraft Fueling Transactions (recursive/already shown)
│   │   │   │   │   │   │   │       └── Fuel Meters (recursive/already shown)
│   │   │   │   │   │   │       └── Fuel Meter Lines
│   │   │   │   │   │   │       │   └── Fuel Meters (recursive/already shown)
│   │   │   │   │   │   │           └── Aircraft Fueling Transactions (recursive/already shown)
│   │   │   │   │   │       └── Fuel Trucks (recursive/already shown)
│   │   │   │   │   │   └── Fuel Deliveries (recursive/already shown)
│   │   │   │   │   │   └── Nightly Truck Fill (recursive/already shown)
│   │   │   │   │   │   └── Truck Fill Allocations (Tank-Truck) (recursive/already shown)
│   │   │   │   │   │   └── Delivery Allocations
│   │   │   │   │   │   │   └── Fuel Storage Tanks (recursive/already shown)
│   │   │   │   │   │       └── Fuel Deliveries (recursive/already shown)
│   │   │   │   │       └── Tank Level Checks
│   │   │   │   │           └── Fuel Storage Tanks (recursive/already shown)
│   │   │   │   │   └── Fuel Reconciliation (recursive/already shown)
│   │   │   │       └── Delivery Allocations (recursive/already shown)
│   │   │   │   └── Nightly Truck Fill (recursive/already shown)
│   │   │       └── Aircraft Fueling Transactions (recursive/already shown)
│   │   │   └── Fuel Meter Lines (recursive/already shown)
│   │   │   └── Customers (recursive/already shown)
│   │   │   └── Fuel Trucks (recursive/already shown)
│   │   │   └── Aircraft (recursive/already shown)
│   │       └── Service Orders (recursive/already shown)
│   │   └── Customers (recursive/already shown)
│   │   └── Jobs/Work Orders (recursive/already shown)
│   │   └── Company Info2 (recursive/already shown)
│       └── Service Orders (recursive/already shown)
│   └── Service Orders (recursive/already shown)
│   └── Aircraft Fueling Transactions (recursive/already shown)
    └── Jobs/Work Orders (recursive/already shown)
│
Vendors
│   └── Shipments (recursive/already shown)
│   └── Shipping PDF Data (recursive/already shown)
    └── Vendor Transactions (recursive/already shown)
│
Fuel Storage Tanks
│   └── Fuel Types (recursive/already shown)
│   └── Fuel Deliveries (recursive/already shown)
│   └── Nightly Truck Fill (recursive/already shown)
│   └── Truck Fill Allocations (Tank-Truck) (recursive/already shown)
│   └── Delivery Allocations (recursive/already shown)
    └── Tank Level Checks (recursive/already shown)
│
Services
│
```

# 5. Workflow Analysis

Based on the schema, here are the likely operational workflows:

### Aircraft Intake & Service Order Lifecycle

1. **Intake:** A Customer requests service for their Aircraft. A record is created in **Service Orders** linking the Customer and Aircraft.

2. **Assignment:** Employees (CSRs/Line Service) are assigned to the Service Order.

3. **Fulfillment:** Requested services (Fuel, Oil, Lav, Maintenance) are performed. If fueling is requested, an **Aircraft Fueling Transaction** is created and linked back to the Service Order.

4. **Completion:** The Service Order is marked 'Complete' after verifying fuel amounts and associated jobs.


### Fuel Reconciliation Workflow

1. **Receiving:** Fuel arrives via **Fuel Deliveries** (from vendors) into **Fuel Storage Tanks**.

2. **Transfer:** Fuel is moved from Tanks to **Fuel Trucks** during the **Nightly Truck Fill**.

3. **Dispensing:** Trucks dispense fuel to aircraft (**Aircraft Fueling Transactions**), tracked via **Fuel Meters** and **Fuel Meter Lines**.

4. **Reconciliation:** The **Fuel Reconciliation** table aggregates Data across Deliveries, Truck Fills, and Fueling Transactions to compute total volumes, identify variance, and calculate 'Calculated Loss' and 'Loss %'.


### Parts & Inventory Lifecycle

1. **Catalog:** **Parts Master** holds the definitions of components (ATA Chapters, conditions).

2. **Stock:** Individual units are tracked in **Inventory Items** by serial number and physical location (Shelf/Bin).

3. **Movement & Repair:** Inventory moves between Parts Room, Vendor (for overhaul/repair tracked in Vendor Transactions), and Installed (Parts Issued to Job).


# 6. Fillout Form Recommendations

### Form 1: Aircraft Fueling Entry

- **Purpose:** Allow line technicians to quickly log fuel dispensed to an aircraft while on the ramp.

- **Fields:** Aircraft (Linked), Fuel Truck (Linked), Meter Start, Meter End, Fueling Date, Employee (Linked).

- **Target Table:** Aircraft Fueling Transactions


### Form 2: New Service Order Request

- **Purpose:** Enable CSRs or Customers to request services prior to arrival.

- **Fields:** Customer, Aircraft, ETA, Requested Services (Multi-select), Requested Fuel Gallons, Special Instructions.

- **Target Table:** Service Orders


### Form 3: Nightly Truck Fill Log

- **Purpose:** Daily entry for line service to log the transfer of fuel from the main farm to mobile trucks.

- **Fields:** Date, Fuel Storage Tank, Fuel Truck, Meter Start, Meter End, Override Reason.

- **Target Table:** Nightly Truck Fill


# 7. n8n Automation Opportunities

### Automation 1: Low Fuel Alerting

- **Trigger:** Airtable record updated (Fuel Storage Tanks or Fuel Trucks).

- **Tables Involved:** Fuel Storage Tanks, Fuel Trucks.

- **Actions:** If the 'Low Fuel Warning' formula or 'Low Truck Fuel' formula triggers, send a Slack/Teams message or SMS to the Line Manager.

- **Expected Outcome:** Proactive dispatching of fuel farm deliveries or truck refills to prevent operational delays.


### Automation 2: Variance Flag Escalation

- **Trigger:** New record in 'Nightly Truck Fill'.

- **Tables Involved:** Nightly Truck Fill.

- **Actions:** If 'Variance Flag' equals 'REVIEW REQUIRED', create a task in a project management tool or email the supervisor for override approval.

- **Expected Outcome:** Tightened fuel accounting and theft/leak prevention.


### Automation 3: Service Order Completion to Invoicing

- **Trigger:** Service Order status changes to 'Complete'.

- **Tables Involved:** Service Orders, Aircraft Fueling Transactions.

- **Actions:** Aggregate 'Billed Gallons' and requested services, push data to QuickBooks/Xero, and generate a draft invoice.

- **Expected Outcome:** Seamless accounting handoff and faster billing cycles.


# 8. Critical Tables for Phase 1

If implementing this system in phases, the following tables are paramount for Phase 1:


1. **Customers & Aircraft**

   - *Why:* These form the foundational master data. You cannot log a service order or fuel ticket without knowing who owns the plane and the tail number.

2. **Service Orders**

   - *Why:* This is the core workflow driver for the FBO. It tracks the lifecycle of a customer visit from ETA to departure.

3. **Fuel Types, Fuel Storage Tanks & Fuel Trucks**

   - *Why:* Master data for the fuel farm. Essential for tracking current inventory before any transactions can occur.

4. **Aircraft Fueling Transactions**

   - *Why:* The primary revenue-generating activity. Capturing pumped vs. billed gallons accurately is mission-critical.


# 9. Schema Risks and Gaps

- **Duplicate Data Risks:** The schema relies heavily on manual text entry for some fields (e.g., 'Meter SN:', 'Truck Name') rather than strict linked records in every instance, which could lead to typos and orphaned records.

- **Missing Relationships:** Parts Master has a field for 'Manufacturer' as a single line text. This should likely be a linked record to a 'Manufacturers' or 'Vendors' table to ensure data consistency.

- **Potential Workflow Bottlenecks:** The 'Fuel Reconciliation' process seems to rely on complex rollups and month keys across many tables. If a user forgets to link a 'Nightly Truck Fill' to the 'Fuel Reconciliation' record, the monthly variance calculations will be silently incorrect. An automation (n8n) should automatically link these based on the Month Key.

- **Archival Strategy:** Tables like 'Aircraft Fueling Transactions' and 'Inventory Items' will grow exceptionally fast. Without indexes (Airtable doesn't support traditional indexing, but relies on views), performance could degrade over 50,000+ records. Consider a data-warehousing strategy for historical transactions.
