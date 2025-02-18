# Warehouse Processes and Operations Specifications


# Material Receiving
## Receiving standardized process
### Process overview
```mermaid
flowchart LR
    A((start)) --> B{checklist validation}
    B --> |PO valid and up to delivery date | C[Execute Collection]
    B --> |PO is invalid/not on delivery date | D[return to supplier]
    C --> E{Physical Receipt}
    E -->|Quantity Specification Match| F[System Entry]
    E -->|Exception | G[Exception Registration]
    F --> H[Label Generation]
    H --> I[Paste Labeling]
    I --> J{IQC Inspection}
    J --> |Inspection Passed | K[Shelf in Warehouse]
    J --> |Inspection Failed| L[Quality Review]
    L --> M[RTV Processing]
    L --> N[Special Pick Approval]
    K --> O((End))
    M --> O
    N --> O
    G --> O
```


### 1. Order checking operation (SAP system operation)
* **SAP system **
    - Enter the transaction code `ZME2O`.
    - Enter the delivery order information in the “Plant” and “PO” fields.
    - Click on the alarm clock icon in the upper left corner 🕥 or press `F8`.
    - Key information quadruple check:
       - ✅ Material code consistency
       - ✅ Order quantity consistency
       - ✅ Delivery date validity (compare OA DATE)
       - ✅ System lead time and physical label consistency
    - [ZME2O.gif](https://github.com/dlelyw/VTX_6501/blob/0ecf0e8decf70686fdc0656ab4f7a64b32ba7241/files/gif/ZME2O.gif)


> **Exception Handling**:
> 🚨 When there is “PO countless/no delivery date”, immediately suspend the receiving process and contact the supplier to coordinate the process.


### 2. Receiving point count standardization
1. Three-way comparison:
   - Physical package label
   - Supplier delivery note
   - SAP system order
2. appearance quality inspection:
   - 🔍 Check the package integrity
   - ⚠️ Rule out abnormalities such as deformation/damage/moisture etc.
3. Post-signing operation:
   - Post-signing operation: Label the location of the waiting area (format: `QA01` or `QA02`).
   - Goods are moved to the yellow inspection area.
   


### 3. Post-signing operation: Post the QA locator tag (format: `QA01` or `QA02`).
**WMS 4.2 system** 1.
    1. Data entry:
       - Input Invoice No. → Packing List No. → PO No. → `[ Enter ]` in order.
    2. Packaging Matching:
       - Match the physical material number/quantity in the upper right view area
       - The cursor is positioned to the receiving quantity field
    3. Packaging information confirmation:
       - Enter the actual arrival package specifications (number of boxes/packing units)
    4. System Operation:
       - Click `[ Save ]` to generate the batch number.  
       - The generated batch number is written to the document.
    5. [InRT_101.gif](https://github.com/dlelyw/VTX_6501/blob/78761c82f6bacd105d83a0eeb12adb896d5ab8bc/files/gif/InRT_101.gif)       

> **Prompt**:
> Temporary Storage Warehouse Query Instruction: ZMM138 Overdue unaccounted batches are automatically transferred to temporary storage status Waiting for expiration Accepted to normal warehouse and then passed the order to IQC.



### 4. Label Posting Specifications
* Label Positioning.
    - Upper right corner of the package, 30cm clean area
    - Label positioning: 30cm clean area in upper right corner of package
* Posting requirements.
    - One label for each item, do not cover the original factory marking
    - Batch number must be fully visible
 


### 5. Over-the-counter IQC processes
* WMS 4.2 System
    - Navigate to the function menu:  
        - `Location` → `Move location` → `Enter batch` → `Enter new location` → `Save`.
    - Batch number entry specification :
        - Format requirements: fixed 10-digit number
        - Complementary rules: leading complement “0”.  
        - Example: Original batch “1234567” → Enter “00001234567”.
     -  [Movelocation323.gif](https://github.com/dlelyw/VTX_6501/blob/78761c82f6bacd105d83a0eeb12adb896d5ab8bc/files/gif/Movelocation323.gif)

> **attachment**  **<a href="https://github.com/dlelyw/VTX_6501/blob/0ecf0e8decf70686fdc0656ab4f7a64b32ba7241/files/gif/Download%20File%20Example.gif">Example of all file downloads</a>**
- **software category**
- [WMS.exe](https://github.com/dlelyw/VTX_6501/blob/19b5c6346e674e532626e966f523b64e8f6b57c0/files/apps/WMS.exe)
- [DFMS.exe （MES Printing Services）](https://github.com/dlelyw/VTX_6501/blob/78761c82f6bacd105d83a0eeb12adb896d5ab8bc/files/apps/DFMS.exe)
- [Hairpin Label Printing Software.exe](https://github.com/dlelyw/VTX_6501/blob/78761c82f6bacd105d83a0eeb12adb896d5ab8bc/files/apps/Hairpin%20Label%20Printing%20Software.exe)
- [Herramienta de inicio de sesión específica.exe](https://github.com/dlelyw/VTX_6501/blob/78761c82f6bacd105d83a0eeb12adb896d5ab8bc/files/apps/Herramienta%20de%20inicio%20de%20sesión%20específica.exe)
- [wms_release_1.3.7.apk （mobile version）](https://github.com/dlelyw/VTX_6501/blob/78761c82f6bacd105d83a0eeb12adb896d5ab8bc/files/apps/wms_release_1.3.7.apk)
- [dlelwprint.exe（Arbitrary text printing）](https://github.com/dlelyw/VTX_6501/blob/78761c82f6bacd105d83a0eeb12adb896d5ab8bc/files/apps/dlelwprint.exe)
- [MESAPP_PRO.apk（MES Mobile Edition）](https://github.com/dlelyw/VTX_6501/blob/78761c82f6bacd105d83a0eeb12adb896d5ab8bc/files/apps/MESAPP_PRO.apk)
- [dlelyw.exe（backup version）](https://github.com/dlelyw/VTX_6501/blob/78761c82f6bacd105d83a0eeb12adb896d5ab8bc/files/apps/dlelyw.exe)
- **Online Tools**
- [web_MES](http://10.97.245.205:92/login)
- [web_MES_apk](http://10.97.245.205:93)
- [web_translator](https://www.deepl.com/zh/translator)
- **file class**  
- [Invoice Number Lookup Guide.pdf](https://github.com/dlelyw/VTX_6501/blob/19b5c6346e674e532626e966f523b64e8f6b57c0/files/pdf/Invoice%20Number%20Lookup%20Guide.pdf)  
- [MES Receiving.pdf](https://github.com/dlelyw/VTX_6501/blob/78761c82f6bacd105d83a0eeb12adb896d5ab8bc/files/pdf/MES%20Receiving.pdf)