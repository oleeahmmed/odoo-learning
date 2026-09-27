# পার্ট ০৬ — Manufacturing
### ঢাকা গার্মেন্টস — পোশাক কারখানা

---

## Business পরিচয়

**ঢাকা গার্মেন্টস** একটা ছোট পোশাক কারখানা।

```
মালিক:    শফিক সাহেব
ব্যবসা:   কাপড় কিনে নিজেরা শার্ট তৈরি করেন
সমস্যা:   একটা শার্ট বানাতে কত কাপড় লাগে, কত সুতা লাগে
          কোনো record নেই
          কতটা তৈরি হয়েছে, কতটা stock-এ আছে
          হিসাব নেই
          কোনো order আসলে কতটা বানাতে পারবেন বুঝতে পারেন না
```

---

## ধাপ ১ — Fresh Database তৈরি করুন

```
http://localhost:8069/web/database/manager

Master Password:  admin
Database Name:    dhaka_garments
Email:            admin@dhaka.com
Password:         admin123
Language:         English
Country:          Bangladesh
Demo data:        ☑ ON
```

→ **Create Database** → Login করুন।

### Company ও Timezone সেট করুন

Settings → Companies → company নামে click:
```
Company Name:  ঢাকা গার্মেন্টস
Country:       Bangladesh
Currency:      BDT
```
Save। উপরে ডানদিকে নামে click → Preferences → `Timezone: Asia/Dhaka` → Save।

---

## ধাপ ২ — Apps Install করুন

```
Manufacturing   → Install
Inventory       → Install
Purchase        → Install
```

Manufacturing → Configuration → Settings:
```
Work Orders:    ☑ চালু করুন
```

---

## ধাপ ৩ — Raw Material Products তৈরি করুন

Inventory → Products → Products → New

**কাপড়:**
```
Product Name:   White Cotton Fabric (meter)
Product Type:   Storable Product
Cost:           150
Unit of Measure: m (meter)
Purchase UoM:   m
```
Save।

**সুতা:**
```
Product Name:   Sewing Thread (reel)
Product Type:   Storable Product
Cost:           80
UoM:            pcs
```
Save।

**বোতাম:**
```
Product Name:   Shirt Button (dozen)
Product Type:   Storable Product
Cost:           25
UoM:            doz
```
Save।

---

## ধাপ ৪ — Finished Product তৈরি করুন

```
Product Name:   Men's White Shirt (M)
Product Type:   Storable Product
Sales Price:    850
Cost:           0    ← Odoo নিজেই হিসাব করবে BOM থেকে
Tracking:       By Serial Number
```

Save।

---

## ধাপ ৫ — Work Centers তৈরি করুন

Work Center মানে কারখানার একটা নির্দিষ্ট কাজের জায়গা বা মেশিন।

Manufacturing → Configuration → Work Centers → New

```
Name:               Cutting Section
Capacity:           5     ← একসাথে ৫টা কাজ হতে পারে
Time Efficiency:    100%
Cost per Hour:      200   ← মেশিন + শ্রমিকের ঘণ্টাপ্রতি খরচ
```

New:
```
Name:         Sewing Section
Capacity:     10
Cost/Hour:    300
```

New:
```
Name:         Finishing Section
Capacity:     5
Cost/Hour:    150
```

Save সব।

---

## ধাপ ৬ — Bill of Materials (BOM) তৈরি করুন

BOM মানে একটা product তৈরির রেসিপি।
কী কী raw material লাগবে, কতটুকু লাগবে।

Manufacturing → Products → Bills of Materials → New

```
Product:   Men's White Shirt (M)
Reference: BOM-SHIRT-001
BOM Type:  Manufacture this Product
Quantity:  1     ← ১টা শার্টের জন্য
```

**Components Tab:**

Add a line:
```
Component:   White Cotton Fabric
Quantity:    2.5 m
```

Add a line:
```
Component:   Sewing Thread
Quantity:    1 pcs
```

Add a line:
```
Component:   Shirt Button
Quantity:    1 doz
```

**Operations Tab** (Work Orders):

Add a line:
```
Operation:    Cutting
Work Center:  Cutting Section
Duration:     10 min      ← ১টা শার্ট cut করতে ১০ মিনিট
```

Add a line:
```
Operation:    Sewing
Work Center:  Sewing Section
Duration:     25 min
```

Add a line:
```
Operation:    Finishing
Work Center:  Finishing Section
Duration:     5 min
```

Save।

### pgAdmin-এ দেখুন

```sql
-- BOM ও components
SELECT
    pt.name AS finished_product,
    mb.product_qty AS bom_quantity,
    cpt.name AS component,
    mbl.product_qty AS component_qty,
    pu.name AS uom
FROM mrp_bom mb
JOIN product_template pt ON mb.product_tmpl_id = pt.id
JOIN mrp_bom_line mbl ON mbl.bom_id = mb.id
JOIN product_product cpp ON mbl.product_id = cpp.id
JOIN product_template cpt ON cpp.product_template_id = cpt.id
JOIN uom_uom pu ON mbl.product_uom_id = pu.id
ORDER BY mbl.sequence;
```

---

## ধাপ ৭ — Raw Material Stock ঢোকান

Manufacturing করতে হলে আগে raw material stock-এ আনতে হবে।

Inventory → Operations → Receipts → New

```
Operation Type: Receipts
```

Add lines:
```
Product:  White Cotton Fabric    Quantity: 500 m
Product:  Sewing Thread          Quantity: 300 pcs
Product:  Shirt Button           Quantity: 200 doz
```

Validate করুন।

---

## ধাপ ৮ — Manufacturing Order তৈরি করুন

একটা order এসেছে — ৫০টা শার্ট লাগবে।

Manufacturing → Manufacturing Orders → New

```
Product:          Men's White Shirt (M)
Bill of Material: BOM-SHIRT-001   ← automatically আসবে
Quantity:         50
Scheduled Date:   আজকে থেকে ৩ দিন পর
```

**Confirm** চাপুন।

Status: **Confirmed**

এখন দেখুন **Components Tab:**

```
Component            Required    Available
White Cotton Fabric  125 m       500 m    ✓
Sewing Thread        50 pcs      300 pcs  ✓
Shirt Button         50 doz      200 doz  ✓
```

Odoo নিজেই হিসাব করেছে:
- ৫০টা শার্ট × ২.৫ মিটার = ১২৫ মিটার কাপড় লাগবে

**Work Orders Tab:**

```
Operation   Work Center      Expected Duration
Cutting     Cutting Section  500 min (50 × 10)
Sewing      Sewing Section   1250 min
Finishing   Finishing Section 250 min
```

---

## ধাপ ৯ — Work Orders Process করুন

**Work Orders Tab** → **Cutting** → Start

কাটা শেষ হলে → **Done**

**Sewing** → Start → Done

**Finishing** → Start → Done

---

## ধাপ ১০ — Manufacturing Order Validate করুন

উপরে **Validate** চাপুন।

```
Serial Numbers: SHIRT-001 থেকে SHIRT-050 পর্যন্ত দিন
(প্রতিটা শার্টের আলাদা serial number)
```

Validate হলে:

```
Raw Material (consumed):
    White Cotton Fabric: 500 → 375  (125 মিটার খরচ হয়েছে)
    Sewing Thread:       300 → 250  (50 পিস খরচ)
    Shirt Button:        200 → 150  (50 ডজন খরচ)

Finished Product (produced):
    Men's White Shirt:   0 → 50    (৫০টা তৈরি হয়েছে)
```

### pgAdmin-এ দেখুন — Stock Changes

```sql
SELECT
    pt.name AS product,
    SUM(sq.quantity) AS current_stock,
    sl.complete_name AS location
FROM stock_quant sq
JOIN product_product pp ON sq.product_id = pp.id
JOIN product_template pt ON pp.product_template_id = pt.id
JOIN stock_location sl ON sq.location_id = sl.id
WHERE sl.usage = 'internal'
GROUP BY pt.name, sl.complete_name
ORDER BY pt.name;
```

দেখবেন raw materials কমেছে, finished goods বেড়েছে।

---

## ধাপ ১১ — Manufacturing-এর Database Structure

```sql
-- Manufacturing Order দেখুন
SELECT
    mo.name,
    pt.name AS product,
    mo.product_qty,
    mo.state,
    mo.date_start,
    mo.date_finished
FROM mrp_production mo
JOIN product_product pp ON mo.product_id = pp.id
JOIN product_template pt ON pp.product_template_id = pt.id
ORDER BY mo.id DESC
LIMIT 5;
```

```sql
-- Component consumption দেখুন
SELECT
    pt.name AS component,
    sm.product_uom_qty AS planned_qty,
    sm.quantity_done AS consumed_qty,
    sm.state
FROM stock_move sm
JOIN product_product pp ON sm.product_id = pp.id
JOIN product_template pt ON pp.product_template_id = pt.id
JOIN mrp_production mo ON sm.production_id = mo.id
WHERE mo.name = 'WH/MO/00001';
```

---

## Manufacturing-এ Database-এ কী হয়

```
Manufacturing Order Confirm করলে:
    mrp_production     → নতুন row, state='confirmed'
    stock_move         → components-এর জন্য (location: stock → production)
    stock_move         → finished product-এর জন্য (location: production → stock)
    mrp_workorder      → প্রতিটা work order

Work Order শুরু করলে:
    mrp_workorder      → state: 'ready' → 'progress'
    mrp_workcenter_productivity → time tracking

Validate করলে:
    mrp_production     → state: 'done'
    stock_move         → state: 'done' (components consumed)
    stock_move         → state: 'done' (finished goods produced)
    stock_quant        → raw materials কমল
    stock_quant        → finished goods বাড়ল
    stock_lot          → serial numbers তৈরি হলো
```

---

## ধাপ ১২ — Cost Calculation

একটা শার্ট বানাতে আসলে কত খরচ?

Manufacturing Order-এ **Cost Analysis** বা উপরের
**→ Cost** tab দেখুন:

```
Component Cost:
    Fabric (2.5m × 150):    375
    Thread (1 × 80):         80
    Button (1doz × 25):      25
    Total Materials:        480

Operation Cost:
    Cutting (10min × 200/60):   33
    Sewing  (25min × 300/60):  125
    Finishing(5min × 150/60):   12
    Total Operations:          170

Total Manufacturing Cost:       650
```

সেলস প্রাইস ৮৫০ — লাভ ২০০ টাকা প্রতি শার্টে।

---

## এই Part-এ যা শিখলেন

```
✓ Bill of Materials — recipe তৈরি
✓ Work Centers — কারখানার section
✓ Manufacturing Order তৈরি ও confirm
✓ Work Orders process করা
✓ Raw material → Finished product
✓ Stock automatically update হওয়া
✓ Cost calculation
✓ mrp_production, mrp_bom, stock_move — database tables
```

---

## পরের পার্ট

**`07-crm/crm-guide.md`** — ক্লাউড সফটওয়্যার বিডি
