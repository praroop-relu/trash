# Aviation FBO ER Diagram

```mermaid
erDiagram
    Fuel_Types {
        singleLineText Fuel_Types
        formula Billing_Precision_
        multipleRecordLinks Fuel_Storage_Tanks
        multipleRecordLinks Fuel_Trucks
        multipleRecordLinks Fuel_Meters
    }
    Fuel_Types ||--o{ Fuel_Storage_Tanks : "links_to"
    Fuel_Types ||--o{ Fuel_Trucks : "links_to"
    Fuel_Types ||--o{ Fuel_Meters : "links_to"
    Fuel_Storage_Tanks {
        singleLineText Tank_Name
        multipleRecordLinks Fuel_Type
        multipleLookupValues Fuel_Types_from_Fuel_Type
        number Capacity
        number Starting_Gallons
    }
    Fuel_Storage_Tanks ||--o{ Fuel_Deliveries : "links_to"
    Fuel_Storage_Tanks ||--o{ Nightly_Truck_Fill : "links_to"
    Fuel_Storage_Tanks ||--o{ Truck_Fill_Allocations_Tank_Truck : "links_to"
    Fuel_Storage_Tanks ||--o{ Delivery_Allocations : "links_to"
    Fuel_Storage_Tanks ||--o{ Tank_Level_Checks : "links_to"
    Fuel_Deliveries {
        formula Fuel_Delivery
        multipleRecordLinks Fuel_Storage_Tanks
        number Gallons_Delivered
        dateTime Date
        number BOL_#
    }
    Fuel_Deliveries ||--o{ Fuel_Reconciliation : "links_to"
    Fuel_Deliveries ||--o{ Delivery_Allocations : "links_to"
    Nightly_Truck_Fill {
        formula Name
        dateTime Date
        multipleRecordLinks Fuel_Storage_Tanks
        multipleRecordLinks Fuel_Trucks
        number Meter_Start
    }
    Nightly_Truck_Fill ||--o{ Fuel_Trucks : "links_to"
    Nightly_Truck_Fill ||--o{ Fuel_Reconciliation : "links_to"
    Nightly_Truck_Fill ||--o{ Truck_Fill_Allocations_Tank_Truck : "links_to"
    Fuel_Trucks {
        singleLineText Truck_Name
        multipleRecordLinks Fuel_Type
        multipleLookupValues Fuel_Types_from_Fuel_Type
        number Capacity
        number Starting_Gallons
    }
    Fuel_Trucks ||--o{ Aircraft_Fueling_Transactions : "links_to"
    Fuel_Trucks ||--o{ Fuel_Meters : "links_to"
    Fuel_Meters {
        singleLineText Meter_Name
        singleSelect Meter_Type
        multipleRecordLinks Fuel_Type
        multipleLookupValues Fuel_Types_from_Fuel_Type
        multipleRecordLinks Truck
    }
    Fuel_Meters ||--o{ Fuel_Meter_Lines : "links_to"
    Aircraft_Fueling_Transactions {
        formula Fuel_Transaction_#
        multipleLookupValues Aircraft
        multipleRecordLinks Service_Orders
        multipleLookupValues Customer
        multipleRecordLinks Employee
    }
    Aircraft_Fueling_Transactions ||--o{ Service_Orders : "links_to"
    Aircraft_Fueling_Transactions ||--o{ Employees : "links_to"
    Aircraft_Fueling_Transactions ||--o{ Fuel_Reconciliation : "links_to"
    Aircraft_Fueling_Transactions ||--o{ Aircraft : "links_to"
    Aircraft_Fueling_Transactions ||--o{ Customers : "links_to"
    Aircraft_Fueling_Transactions ||--o{ Fuel_Meter_Lines : "links_to"
    Fuel_Meter_Lines {
        autoNumber Line_ID
        multipleRecordLinks Fueling_Transaction
        multipleRecordLinks Meter
        multipleLookupValues Truck_from_meter
        number Meter_Start
    }
    Service_Orders {
        autoNumber Service_Order_#
        dateTime Check_in_Time
        date Service_Order_Created
        multipleRecordLinks Customers
        multipleRecordLinks Aircraft
    }
    Service_Orders ||--o{ Customers : "links_to"
    Service_Orders ||--o{ Aircraft : "links_to"
    Service_Orders ||--o{ Employees : "links_to"
    Service_Orders ||--o{ Company_Info2 : "links_to"
    Service_Orders ||--o{ Jobs_Work_Orders : "links_to"
    Fuel_Reconciliation {
        singleLineText Name
        date Month
        singleSelect Fuel_Type
        multipleRecordLinks Deliveries
        multipleRecordLinks Truck_Fills
    }
    Customers {
        singleLineText Customer_Name
        phoneNumber Phone
        email Email
        singleLineText Company
        singleLineText Notes
    }
    Customers ||--o{ Aircraft : "links_to"
    Customers ||--o{ Jobs_Work_Orders : "links_to"
    Aircraft {
        singleLineText Tail_Number
        multipleRecordLinks Customers
        singleLineText Type
        singleSelect Fuel_Type
        multipleRecordLinks Service_Orders
    }
    Aircraft ||--o{ Company_Info2 : "links_to"
    Aircraft ||--o{ Jobs_Work_Orders : "links_to"
    Services {
        singleLineText Name
    }
    Employees {
        singleLineText Name
        multilineText Notes
        singleCollaborator Assignee
        singleSelect Status
        multipleAttachments Attachments
    }
    Employees ||--o{ Parts_Issued_to_Job : "links_to"
    Parts_Master {
        singleLineText Part_Number
        singleLineText Discription
        singleLineText ATA_Chapter
        singleLineText Manufacturer
        singleSelect Condition
    }
    Parts_Master ||--o{ Inventory_Items : "links_to"
    Parts_Master ||--o{ Shipping_PDF_Data : "links_to"
    Parts_Master ||--o{ Planned_Parts : "links_to"
    Inventory_Items {
        autoNumber Inventory_ID
        multipleRecordLinks Part
        singleLineText Serial_Number
        number Quantity_On_Hand
        singleSelect Contition
    }
    Inventory_Items ||--o{ Vendor_Transactions : "links_to"
    Inventory_Items ||--o{ Parts_Issued_to_Job : "links_to"
    Inventory_Items ||--o{ Shipment_Line_Items : "links_to"
    Inventory_Items ||--o{ Planned_Parts : "links_to"
    Inventory_Items ||--o{ Inventory_Movements : "links_to"
    Vendors {
        singleLineText Vendor_Name
        singleSelect Vendor_Type
        singleLineText FAA_Repair_Station_#
        singleLineText Contact_Name
        phoneNumber Phone_Number
    }
    Vendors ||--o{ Vendor_Transactions : "links_to"
    Vendors ||--o{ Shipping_PDF_Data : "links_to"
    Vendors ||--o{ Shipments : "links_to"
    Vendor_Transactions {
        autoNumber Transaction_ID
        multipleRecordLinks Inventory_Item
        multipleRecordLinks Vendor
        singleSelect Transaction_Type
        date Date_Out
    }
    Vendor_Transactions ||--o{ Shipments : "links_to"
    Vendor_Transactions ||--o{ Inventory_Movements : "links_to"
    Jobs_Work_Orders {
        singleLineText Job_Number
        multipleRecordLinks Aircraft_N_Number
        singleLineText Aircraft_Type
        multipleRecordLinks Customer
        singleSelect Job_Status
    }
    Jobs_Work_Orders ||--o{ Parts_Issued_to_Job : "links_to"
    Jobs_Work_Orders ||--o{ Shipping_PDF_Data : "links_to"
    Jobs_Work_Orders ||--o{ Maintenance_Work_Plans : "links_to"
    Jobs_Work_Orders ||--o{ Inventory_Movements : "links_to"
    Jobs_Work_Orders ||--o{ Work_Plan_Tasks : "links_to"
    Parts_Issued_to_Job {
        autoNumber Issue_ID
        multipleRecordLinks Inventory_Item
        multipleRecordLinks Job
        multipleLookupValues Job_Number_from_Job
        number Qty_Issued
    }
    Shipping_PDF_Data {
        autoNumber Shipping_ID
        multipleRecordLinks Company_Info
        multipleLookupValues Customer_Name_from_Company_Info
        multipleRecordLinks Vendor_Info
        multipleLookupValues Vendor_Name_from_Vendor_Info
    }
    Shipping_PDF_Data ||--o{ Company_Info2 : "links_to"
    Maintenance_Work_Plans {
        autoNumber Work_Plan_ID
        multipleRecordLinks Job
        singleSelect Plan_Type
        multipleLookupValues Aircraft_N_Number
        multipleLookupValues Aircraft_Type
    }
    Maintenance_Work_Plans ||--o{ Work_Plan_Tasks : "links_to"
    Company_Info2 {
        singleLineText Company_Info
        phoneNumber Phone
        email Email
        singleLineText Company
        singleLineText Notes
    }
    Shipments {
        autoNumber Shipment_Number
        singleSelect Type
        multipleRecordLinks Vendor___Custormer
        multilineText Ship_From
        multilineText Ship_To
    }
    Shipments ||--o{ Shipment_Line_Items : "links_to"
    Shipment_Line_Items {
        singleLineText Line_Items
        multipleRecordLinks Shipment
        multipleRecordLinks Inventory_Item
        multipleLookupValues Part_Number
        multipleLookupValues Serial_Number
    }
    Work_Plan_Tasks {
        autoNumber Task_ID
        multipleRecordLinks Work_Plan
        singleSelect Task_Type
        singleLineText Reference
        singleLineText CMM
    }
    Work_Plan_Tasks ||--o{ Planned_Parts : "links_to"
    Work_Plan_Tasks ||--o{ Labor_Entries : "links_to"
    Planned_Parts {
        autoNumber Planned_Part_ID
        multipleRecordLinks Work_Plan_Task
        multipleRecordLinks Part
        number Qty_Required
        number Qty_Available_lookup_from_Inventory
    }
    Labor_Entries {
        autoNumber Labor_ID
        multipleRecordLinks Task
        singleLineText Tech
        date Date
        number Hours
    }
    Inventory_Movements {
        autoNumber Move_ID
        multipleRecordLinks Inventory_Item
        singleSelect Move_Type
        number Qty_Change
        singleSelect From_Location
    }
    Truck_Fill_Allocations_Tank_Truck {
        autoNumber Allocation_ID
        multipleRecordLinks Nightly_Truck_Fill
        multipleRecordLinks From_Tank
        number Allocated_Gallons
        checkbox Auto_Split?
    }
    Delivery_Allocations {
        autoNumber Delivery_Allocation_ID
        multipleRecordLinks Fuel_Delivery
        multipleRecordLinks To_Tank
        number Allocated_Gallons
        checkbox Auto_Split?
    }
    Tank_Level_Checks {
        autoNumber Check_ID
        multipleRecordLinks Tank
        dateTime Check_Date_Time
        number Measured_Gallons
        multipleLookupValues System_Gallons
    }
```
