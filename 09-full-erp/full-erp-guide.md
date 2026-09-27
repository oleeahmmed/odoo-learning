# পার্ট ০৯ — Full ERP Integration
### নকশি টেক্সটাইল — সব Module একসাথে

---

## Business পরিচয়

**নকশি টেক্সটাইল** একটা পোশাক কোম্পানি।

```
মালিক:    রুমানা বেগম
ব্যবসা:   কাপড় কিনে, শার্ট তৈরি করে, বিক্রি করে
Module:   CRM + Sales + Inventory + Purchase +
          Manufacturing + Accounting + HR
সমস্যা:   সব কিছু আলাদা আলাদাভাবে manage করছেন
          একটা order থেকে শেষ পর্যন্ত কী হচ্ছে
          কেউ বলতে পারেন না
```

---

## ধাপ ১ — Fresh Database তৈরি করুন

```
http://localhost:8069/web/database/manager

Master Password:  admin
Database Name:    nakshi_textile
Email:            admin@nakshi.com
Password:         admin123
Language:         English
Country:          Bangladesh
Demo data:        ☑ ON
```

→ **Create Database** → Login করুন।

### Company ও Timezone সেট করুন

Settings → Companies → company নামে click:
```
Company Name:  নকশি টেক্সটাইল
Country:       Bangladesh
Currency:      BDT
```
Save। উপরে ডানদিকে নামে click → Preferences → `Timezone: Asia/Dhaka` → Save।

---

## ধাপ ২ — সব Apps Install করুন

```
CRM             → Install
Sales           → Install
Purchase        → Install
Inventory       → Install
Manufacturing   → Install
Invoicing       → Install
Accounting      → Install
Employees       → Install
```

---

## ধাপ ৩ — Settings Configure করুন

Inventory → Configuration → Settings:
```
Storage Locations:   ☑
Lots & Serial:       ☑
Multi-Step Routes:   ☑
```

Manufacturing → Configuration → Settings:
```
Work Orders:         ☑
```

---

## ধাপ ৪ — সব Master Data তৈরি করুন

### Raw Material Products

```
Product: সুতি কাপড় (মিটার)
Type:    Storable Product
Cost:    120
Tracking: By Lots

Product: বোতাম (ডজন)
Type:    Storable Product
Cost:    30

Product: সেলাই সুতা (রিল)
Type:    Storable Product
Cost:    80
```

### Finished Product

```
Product: পুরুষদের শার্ট (নীল, M)
Type:    Storable Product
Sales:   850
Cost:    450
Tracking: By Serial Number
```

### Bill of Materials (BOM)

Manufacturing → Products → Bills of Materials → New

```
Product:  পুরুষদের শার্ট (নীল, M)
Quantity: 1
```

Components Tab:
```
সুতি কাপড়: 2.5 মিটার
বোতাম:     1 ডজন
সেলাই সুতা: 0.5 রিল
```

Save।

---

## ধাপ ৫ — Customer ও Supplier তৈরি করুন

```
Customer:  Style House BD (customer_rank=1)
Supplier:  Padma Textile Mills (supplier_rank=1)
```

---

## ধাপ ৬ — CRM থেকে শুরু করুন

Style House BD আগ্রহ দেখিয়েছে।

CRM → My Pipeline → New

```
Opportunity Name:  Style House BD - ৫০০ শার্ট অর্ডার
Customer:          Style House BD
Expected Revenue:  425000
```

**Activities** যোগ করুন — কখন follow up করবেন।

---

## ধাপ ৭ — CRM → Sales Order

Deal confirm হলো।

Opportunity-তে **New Quotation** চাপুন।

```
Customer:  Style House BD
Product:   পুরুষদের শার্ট (নীল, M)
Quantity:  500
Price:     850
```

**Confirm** করুন।

এখন হলো:
```
CRM Opportunity    → Won ✓
Sales Order        → Confirmed ✓
Delivery Order     → তৈরি হয়েছে (stock নেই, তাই Waiting)
```

---

## ধাপ ৮ — Manufacturing Order

শার্ট বানাতে হবে।

Manufacturing → Manufacturing Orders → New

```
Product:   পুরুষদের শার্ট (নীল, M)
Quantity:  500
```

**Confirm** চাপুন।

দেখবেন Components-এ কী কী লাগবে:
```
সুতি কাপড়:  1250 মিটার  (500 × 2.5)
বোতাম:       500 ডজন
সেলাই সুতা:  250 রিল
```

কিন্তু এগুলো stock-এ নেই!

---

## ধাপ ৯ — Raw Material কেনা

তানভীর (Purchase Manager) কাপড় কিনবেন।

Purchase → New RFQ:

```
Vendor:   Padma Textile Mills
Product:  সুতি কাপড়
Qty:      1500 মিটার (কিছু extra)
```

Confirm → Receipt Validate করুন।

বোতাম ও সুতাও কিনুন।

---

## ধাপ ১০ — Manufacturing Order Produce করুন

Manufacturing → Manufacturing Orders

Manufacturing Order খুলুন।

**Produce** চাপুন।

```
Quantity:   500
Lot:        SHIRT-2024-001
```

**Validate** চাপুন।

এখন:
```
Raw Materials:  stock থেকে বের হয়ে গেল
Finished Goods: stock-এ এলো 500 শার্ট
```

---

## ধাপ ১১ — Delivery করুন

Sales Order-এ ফিরুন।

Delivery Order এখন **Ready** (stock আছে)।

Validate করুন।

---

## ধাপ ১২ — Invoice ও Payment

Sales Order → **Create Invoice** → Confirm → Payment নিন।

---

## ধাপ ১৩ — পুরো Flow Database-এ

```sql
-- নকশি টেক্সটাইলের একটা complete order trace
SELECT
    'CRM Lead' AS step,
    cl.name AS reference,
    cl.stage_id::text AS status
FROM crm_lead cl
WHERE cl.partner_id = (
    SELECT id FROM res_partner WHERE name = 'Style House BD' LIMIT 1)

UNION ALL

SELECT
    'Sales Order' AS step,
    so.name,
    so.state
FROM sale_order so
WHERE so.partner_id = (
    SELECT id FROM res_partner WHERE name = 'Style House BD' LIMIT 1)

UNION ALL

SELECT
    'Manufacturing Order' AS step,
    mo.name,
    mo.state
FROM mrp_production mo
ORDER BY mo.id DESC
LIMIT 1

UNION ALL

SELECT
    'Delivery' AS step,
    sp.name,
    sp.state
FROM stock_picking sp
WHERE sp.picking_type_code = 'outgoing'
ORDER BY sp.id DESC
LIMIT 1

UNION ALL

SELECT
    'Invoice' AS step,
    am.name,
    am.state
FROM account_move am
WHERE am.move_type = 'out_invoice'
ORDER BY am.id DESC
LIMIT 1;
```

দেখবেন সম্পূর্ণ chain:

```
step                 reference      status
CRM Lead            Style House BD  won
Sales Order         S00001          sale
Manufacturing Order MO/001          done
Delivery            WH/OUT/001      done
Invoice             INV/2024/001    posted
```

---

## একটা সম্পূর্ণ Order-এ কোন কোন Table Update হয়

```
CRM:
    crm_lead                → opportunity তৈরি ও win

Sales:
    sale_order              → quotation → confirmed
    sale_order_line         → product lines

Manufacturing:
    mrp_production          → manufacturing order
    mrp_workorder           → work orders (যদি থাকে)
    stock_move              → raw material consumption
    stock_move              → finished product production

Purchase:
    purchase_order          → raw material PO
    purchase_order_line     → lines
    stock_picking           → receipt
    stock_move              → incoming movement

Inventory:
    stock_quant             → quantity changes (raw in, finished out)
    stock_picking           → delivery
    stock_move              → outgoing movement
    stock_move_line         → serial number tracking

Accounting:
    account_move            → invoice (out_invoice)
    account_move_line       → invoice lines
    account_move            → vendor bills (in_invoice)
    account_payment         → payments
```

---

## এই Part-এ যা শিখলেন

```
✓ CRM → Sales → Manufacturing → Purchase → Delivery → Invoice
✓ সব module কীভাবে একে অপরের সাথে connected
✓ একটা transaction-এ কতগুলো table update হয়
✓ Odoo আসলে একটা integrated system — আলাদা আলাদা নয়
```

---

## আপনি এখন Odoo ERP বোঝেন

```
CRM        → Opportunity থেকে Sales শুরু
Sales      → Order নেওয়া, Delivery ও Invoice
Purchase   → কেনাকাটা, Receipt, Bill
Inventory  → Stock management, Lot, Transfer
Manufacturing → BOM, Production Order
Accounting → Invoice, Bill, Payment, Reports
HR         → Employee, Attendance, Leave
```

প্রতিটা business-এর জন্য দরকারি module আলাদা।
Odoo সেই flexibility দেয়।

এটাই ERP-র মূল কথা —
**সব department একটা system-এ, একটা database-এ।**
