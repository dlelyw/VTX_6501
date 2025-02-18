<div style="text-align: center;"> <h1>Finished Goods Inbound and Outbound</h1> </div>

## Process Overview
```mermaid
graph LR
    A[Start] --> B[Receive FG Receipt]
    B --> C[PDA Scan SN for Inbound]
    C --> D[PDA Scan SN for Loading]
    D --> E[End]
```
## FG IN
### FG Scan SN for Inbound
* **WMS Mobile Version**
    - Receive the FG order and finished goods from the production department.
    - First, run VTS for the FG order:
        1. **Check the status of the work order**
            - Enter transaction code `CO03` in SAP.
            - Check if the third word in `Sys.Status` is `DLV`. DLV indicates that VTS has confirmed the completion of the work order; otherwise, VTS needs to be run.
        2. **Deduct quantities**
            - Open a separate window in SAP and enter transaction code `ZMB1A`.
            - Enter `6501` in the `Plant` field.
            - In the `Order` field, enter all the work orders viewed in `CO03`.
            - Click the clock icon 🕥 in the top-left corner or press `F8` to execute.
        3. **Run VTS**
            - Open the MES system [MES](http://10.224.245.101:8080/Index.aspx#).
            - Navigate to the last menu item `SAP`.
            - Select `A02.Confirmation`.
            - In the `Order Number:` field, enter the work order number (confirm the output from the bottom to the top based on the work orders viewed in `CO03`).
            - **Press Enter**.
            - In the `Quantity:` field, enter the quantity to be confirmed.
            - Click `Save`. The result will be displayed in `Result[SAP message]`.
            - Return to SAP `CO03` and refresh.
            - Check `Sys.Status`; the third word should now be `DLV`.
            - [vts.png](https://github.com/dlelyw/VTX_6501/blob/main/files/png/vts.png)
    - **Inbound**
        1. PDA Scan for Inbound  [wms.apk (Mobile Version)](https://github.com/dlelyw/VTX_6501/blob/main/files/apps/wms_release_1.3.7.apk)
            - Log in to the PDA (Server address: 10.224.245.101:8085).
            - Select the menu `01-FG In`.
            - In `SN Type`, select Customer SN.
            - In `FG No`, enter the FG order number.
            - In `Doc No`, enter the document number. If there is none, enter the last 4 digits of the work order number.
            - In `FG Order`, enter the work order number.
            - Press Enter.
            - `Total Qty` will automatically populate with the quantity.
            - In `P/N`, enter the finished goods part number.
            - In `Location`, enter the location.
            - In `Scan SN`, scan the finished goods box number.
            - After scanning all box numbers, save. [pda_fgin01.png](https://github.com/dlelyw/VTX_6501/blob/main/files/png/pda_fgin01.png) [pda_fgin02.png](https://github.com/dlelyw/VTX_6501/blob/main/files/png/pda_fgin02.png)
        2. Manual Inbound in SAP (Only for customers who do not require box numbers)
            - Enter transaction code `MB31` in SAP. [mb31.png](https://github.com/dlelyw/VTX_6501/blob/main/files/png/mb31.png)
            - In `Movement Type`, enter 101.
            - In `Order`, enter the work order number.
            - In `Plant`, enter 6501.
            - In `Storage Loc.`, enter the inbound location.
            - Press Enter.
            - Then modify the quantity to be entered.
            - Save.
            - A prompt will appear to generate a box number.
            - Click the bottom-right button in the toolbar to automatically generate a box number.
            - Save.

## FG OUT
### FG Scan SN for Outbound
* **View Shipping List**
    - Enter transaction code `ZSP1A` in SAP.
    - In `Planned GI Date`, enter a date a few days prior.
    - In `Shipping Point`, enter S650.
    - In `Sales Organization`, enter 6501.
    - In `GI Status`, enter A (A indicates not completed).
    - Click the clock icon 🕥 in the top-left corner or press `F8` to execute.
    - [ZSP1A.png](https://github.com/dlelyw/VTX_6501/blob/main/files/png/ZSP1A.png)
* **Outbound**
    1. PDA Scan for Outbound
        - Log in to the PDA (Server address: 10.224.245.101:8085).
        - Select the menu `03-FG Out`.
        - In `SN Type`, select Customer SN.
        - In `DN`, enter the DN number.
        - Press Enter.
        - `DN Qty` will automatically populate with the quantity.
        - In `Scan SN`, scan the finished goods box number.
        - `Scan Qty` will automatically count the scanned box numbers.
        - Save. [pad_fgout.png](https://github.com/dlelyw/VTX_6501/blob/main/files/png/pad_fgout.png)
            - The SAP shipping interface will automatically synchronize the `Pick Up` status.
            - If synchronization does not occur for a long time,
            - Use SAP transaction code `ZSD046`.
            - In `Sales Organization`, enter 6501.
            - In `DN`, enter the shipping DN.
            - Click the clock icon 🕥 in the top-left corner or press `F8` to execute.
            - Then refresh the shipping interface to see the `Pick Up` status as successful (C).
            - [ZSD046.png](https://github.com/dlelyw/VTX_6501/blob/main/files/png/ZSD046.png)
    2. Manual Outbound in SAP (Only for customers who do not require box numbers or for self-pickup)
        - Enter transaction code `ZSP1A` in SAP.
        - In `Planned GI Date`, enter a date a few days prior.
        - In `Shipping Point`, enter S650.
        - In `Sales Organization`, enter 6501.
        - In `GI Status`, enter A (A indicates not completed).
        - Click the clock icon 🕥 in the top-left corner or press `F8` to execute.
        - Refer to this image (if it is finished goods, select the box number). [fg_handcarry.gif](https://github.com/dlelyw/VTX_6501/blob/main/files/gif/fg_handcarry.gif)

## RMA
* **SAP System**
    - **Normal Work Order Inbound**
        - Query the normal work order number for this RMA, usually in the format 65100006335.
        - Use the normal work order number to enter the quantity into the system using SAP transaction code MB31.
        - In `Movement Type`, enter 101.
        - In `Order`, enter the normal work order number.
        - In `Plant`, enter the corresponding warehouse.
        - In `Storage Loc.`, enter the inbound location.
        - Click the clock icon 🕥 in the top-left corner or press `F8` to execute.
        - Then proceed to the inbound information interface.
        - Enter the quantity to be inbound and press Enter.
        - [rma_1.png](https://github.com/dlelyw/VTX_6501/blob/main/files/png/rma_1.png) [rma_2.png](https://github.com/dlelyw/VTX_6501/blob/main/files/png/rma_2.png)
    - **Deduct Quantity to Rework Order**
        - Use SAP transaction code MB1A.
        - In `Movement Type`, enter 261.
        - In `Plant`, enter 6501.
        - In `Storage Location`, enter FG01.
        - Find `To Order`, click it, and enter the rework order number.
        - Then save.
        - Subsequent operations are the same as normal scanning for inbound.
        - [vts_p_3.png](https://github.com/dlelyw/VTX_6501/blob/main/files/png/vts_p_3.png)

## Manual Quantity Transfer for Auxiliary Materials
* **SAP System**
    - Use SAP `CO03` to check the work order. Materials with part numbers ending in P*** indicate running VTS for the production department (from bottom to top).
    - Open the production department's SAP and enter `CO11N`. Enter the SO# and press Enter.
    - In `Order`, enter the work order number.
    - Click `Actual Data`.
    - Then click `Goods Movements`.
    - Select all data.
    - Click `batch determination` to confirm.
    - Then save.
    - [vts_p_1.png](https://github.com/dlelyw/VTX_6501/blob/main/files/png/vts_p_1.png) [vts_p_2.png](https://github.com/dlelyw/VTX_6501/blob/main/files/png/vts_p_2.png) [vts_p_3.png](https://github.com/dlelyw/VTX_6501/blob/main/files/png/vts_p_3.png)