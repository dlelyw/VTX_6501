<div style="text-align: center;"> <h1> 成品出入库</h1> </div>

## 流程概述
```mermaid
graph LR
    A[开始] --> B[接收FG单收货]
    B --> C[PDA扫描SN入库]
    C --> D[PDA扫描SN装柜]
    D --> E[结束]
```
## FG IN
### FG扫描SN入库
* **wms移动版**
    - 收到生产部提供的FG单和成品
    - 先按照FG单跑VTS
        1. **看工单的状态**
            - 输入事务指令`CO03`
            - 看`Sys.Status`中第三个单词是否是`DLV` DLV代表VTS确认工单完成否则需要跑VTS
        2. **扣数**
            - SAP单独开一个窗口输入事务指令`ZMB1A`
            - 在`Plant`输入`6501`
            - `Order`输入`CO03`查看的所有工单
            - 点击左上角闹钟图标🕥或按`F8`执行
        3. **跑VTS**
            - 打开MES系统 [MES](http://10.224.245.101:8080/Index.aspx#)
            - 找到菜单最后一栏`SAP`
            - 选择`A02.Confirmation`
            - 在`Order Number：`输入工单号（按照`CO03`查看到的工单号从下至上的确认产量）
            - **按下回车**
            - 在`Quantity：`输入需要确认的数量
            - 点击`Save` 在`Result[SAP message]`会给出结果
            - 返回SAP`CO03`重新刷新 
            - 看`Sys.Status`中第三个单词就会变成`DLV`
            - [vts.png](https://github.com/dlelyw/VTX_6501/blob/main/files/png/vts.png)
    - **入库**
        1. PDA扫描入库  [wms.apk （移动版）](https://github.com/dlelyw/VTX_6501/blob/main/files/apps/wms_release_1.3.7.apk)
            - 登录PDA (服务器地址: 10.224.245.101:8085)
            - 选择菜单`01-FG In`
            - `SN Type`选择 Customer SN
            - `FG No` 输入FG单号
            - `Doc No` 输入文件号 没有就输入工单号后4位
            - `FG Order`输入工单号
            - 按下回车
            - `Total Qty`会自动带出数量
            - `P/N`输入成品料号
            - `Location` 输入位置
            - `Scan SN` 扫描成品箱号
            - 扫描完所有箱号后保存 [pda_fgin01.png](https://github.com/dlelyw/VTX_6501/blob/main/files/png/pda_fgin01.png) [pda_fgin02.png](https://github.com/dlelyw/VTX_6501/blob/main/files/png/pda_fgin02.png)
        2. SAP手工入数 （仅限于客户不需要箱号使用）
            * SAP输入事务代码`MB31`  [mb31.png](https://github.com/dlelyw/VTX_6501/blob/main/files/png/mb31.png)
            * `Movement Type`输入101
            * `Order`输入工单号
            * `Plant`输入6501
            * `Storage Loc.`输入入库的位置 
            * 按下回车
            * 然后修改入数的数量
            * 保存
            * 然后会提示需要生成箱号
            * 单击工具栏最下最右的按钮 自动生成箱号
            * 保存

## FG OUT
### FG扫描SN出库
* **查看出货清单**
    * SAP输入事务代码`ZSP1A`
    * `Planned GI Date`日期向前几天填写
    * `Shipping Point`输入S650
    * `Sales Organization`输入6501
    * `GI Status`状态输入A（A表示没有完成）
    * 点击左上角闹钟图标🕥或按`F8`执行
    * [ZSP1A.png](https://github.com/dlelyw/VTX_6501/blob/main/files/png/ZSP1A.png)
* **出库**
    1. PDA扫描出库
        * 登录PDA (服务器地址: 10.224.245.101:8085)
        * 选择菜单`03-FG Out`
        * `SN Type`选择 Customer SN
        * `DN` 输入DN单号
        * 按下回车
        * `DN Qty`会自动带出数量
        * `Scan SN`扫描成品箱号
        * `Scan Qty`会自动计数已扫描的箱号
        * 保存 [pad_fgout.png](https://github.com/dlelyw/VTX_6501/blob/main/files/png/pad_fgout.png)
             * SAP出货界面会自动同步`Pick Up`状态
             * 如果长时间没有同步
             * 使用SAP输入事务指令`ZSD046`
             * `Sales Organization`输入6501
             * `DN`输入出货DN
             * 点击左上角闹钟图标🕥或按`F8`执行
             * 然后出货界面刷新下就可以看到`Pick Up`状态为成功了（C）
             * [ZSD046.png](https://github.com/dlelyw/VTX_6501/blob/main/files/png/ZSD046.png)
    2. SAP手工出数 （仅限于客户不需要箱号使用或者自提）
       * SAP输入事务代码`ZSP1A`
       * `Planned GI Date`日期向前几天填写
       * `Shipping Point`输入S650
       * `Sales Organization`输入6501
       * `GI Status`状态输入A（A表示没有完成）
       * 点击左上角闹钟图标🕥或按`F8`执行
       * 参考这图（如果是成品需要选择箱号）[fg_handcarry.gif](https://github.com/dlelyw/VTX_6501/blob/main/files/gif/fg_handcarry.gif)



## RMA
* **SAP系统**
    * **正常工单入数**
        * 需要查询到这个RMA机正常工单的单号 一般是65100006335样式的 
        * 按正常工单号，把这个工单入数到系统 使用SAP命令MB31入系统 
        * `Movement Type`输入101
        * `Order`输入正常工单号
        * `Plant`输入相应的仓别
        * `Srtorage Loc.`输入入仓的位置
        * 点击左上角闹钟图标🕥或按`F8`执行
        * 然后跳转至入数信息界面
        * 输入需要入数的数量 然后一直回车即可
        * [rma_1.png](https://github.com/dlelyw/VTX_6501/blob/main/files/png/rma_1.png) [rma_2.png](https://github.com/dlelyw/VTX_6501/blob/main/files/png/rma_2.png)
    * **扣数到返工订单中**
        * 使用SAP命令MB1A
        * `Movement Type`输入261
        * `Plant`输入6501
        * `Storage Location`输入FG01
        * 找到`To Order`单击它 然后输入返工工单号 
        * 然后保存即可
        * 后续操作跟正常扫描入库一样
        * [vts_p_3.png](https://github.com/dlelyw/VTX_6501/blob/main/files/png/vts_p_3.png)

## 辅料层手动过数
* **SAP系统**
    * SAP`CO03`查看工单 物料号后面尾号为P***的，表示跑生产部的VTS(从下至上)
    * 打开生产部的SAP 输入`CO11N` 输入SO# 按下回车 
    * `Order`输入工单号
    * 单击 `Actual Data` 
    * 再点击`Goods Movements`
    * 选择所有的数据
    * 单击`batch determination`确认
    * 然后保存
    * [vts_p_1.png](https://github.com/dlelyw/VTX_6501/blob/main/files/png/vts_p_1.png) [vts_p_2.png](https://github.com/dlelyw/VTX_6501/blob/main/files/png/vts_p_2.png) [vts_p_3.png](https://github.com/dlelyw/VTX_6501/blob/main/files/png/vts_p_3.png)