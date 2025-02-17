# 退货
## 流程概览
```mermaid
flowchart LR
    A([开始]) --> B["系统发起退货"]
    B --> C["收取退货"]
    C --> D["退货数入退货仓"]
    D --> E["退货至供应商"]
    E --> F([结束])
```
    

## 1. 系统发起退货
* **执行频率**：每日定时操作
  1. 登录Notes和SAP系统
  2. 进入「退货」模块
  3. 下载当日退货清单
  4. 收集这些退货到RTV仓
> **提示**：
> RTV仓是指仓库中专门用于处理 Return To Vendor（退回供应商） 商品的区域或仓库。它是供应链和仓库管理中的一个重要 环节，主要用于存放和管理需要退回给供应商的商品。


## 2. 收取退货处理

### 2.1 MRB退货收集流程
* **系统登录与下载资料**
   - 打开Notes系统找到菜单`MX IQC  Inspection  Document on...`
   - 找到按钮`Gen Report`单击，选择序号`5 Sotre Reject Report`
   - 按照下载的资料去IQC哪里收集退货
   - [RTV_MRB.gif](https://github.com/dlelyw/VTX_6501/blob/d82ba10a0527b64e0d6fc44a51e3f5ec0db2ce7d/files/gif/RTV_MRB.gif)
### 2.2 RN退货收集流程
* **系统登录与下载资料**
   - 打开SAP 输入事务代码 `ZIMWH`
   - 在"Plant"字段输入`6501`
   - 点击左上角闹钟图标🕥或按`F8`执行
   - 选择所有待退货的数据 下载到本地表格
   - 按照退货清单去IQC RN房间收集退货到RTV仓
   - [RTV_RN.gif](https://github.com/dlelyw/VTX_6501/blob/d82ba10a0527b64e0d6fc44a51e3f5ec0db2ce7d/files/gif/RTV_RN.gif)

## 3 退货数入退货仓
* **SAP系统**
   - 打开SAP 输入事务代码 `MB1B`
   - 在字段输入`Doc.Header Text 输入日期和退货类型` → ` Plant 输入 6501` → `Movement type 输入 311` → `Storage Loation 输入JB01(RN)/JA01(MRB)`
   - 按下键盘回车键进入下一个界面
   - 在字段输入`Material 输入料号` → ` Quantity 输入数量` → `Batch 输入批次` → `Rcvg SLoc 输入移动的目的位置`
   - 保存
   - [RTV_movelocation.gif](https://github.com/dlelyw/VTX_6501/blob/d82ba10a0527b64e0d6fc44a51e3f5ec0db2ce7d/files/gif/RTV_movelocation.gif)

## 4 退货至供应商
* **Notes系统**
    - 打开Notes系统找到菜单`MX Delivery Order on MEXCMS11`
    - 选择左上角`New`单击
    - 进行数据填写：
         1. 先点击左中位置的`add`按钮 选择需要退货的供应商或者物料
         2. `Goods Ready Pick Date * :`两个都要单击，选择退货日期
         3. `Region *:`选择退货的地方
         4. `CC to PUR/PMT *: `抄送邮件给相关的PUR和PMT
         5. `Prepayment *:`选择`No`
         6. `Carrier * :`选择或者输入`LOCAL`
         7. 选择相应的签批人员
    - 提交给PUT或者PMT签批
    - 打印退货单2份(供应商签名两份 仓库和供应商各留一份) 
    - 6591无例子 使用9291 操作是一样的 [RTV_tovender_9291.gif](https://github.com/dlelyw/VTX_6501/blob/d82ba10a0527b64e0d6fc44a51e3f5ec0db2ce7d/files/gif/RTV_tovender_9291.gif)
        