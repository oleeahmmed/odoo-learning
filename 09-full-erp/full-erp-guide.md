# Full ERP — সহজ গল্পে সম্পূর্ণ ইন্টিগ্রেশন
### রহিম ট্রেডার্স: এক Database এ সব মডিউল — Lead থেকে টাকা পর্যন্ত

---

## গল্প: একটাই দোকান, একটাই খাতা

আলাদা আলাদা পার্টে আপনি শিখেছেন:

```
Contacts → Inventory → Sales → Purchase → Accounting → Manufacturing → CRM
```

এখন **একটা Database** এ সব একসাথে।  
একটা কাস্টমার অর্ডার থেকে শেষ পর্যন্ত কে কী করে — পুরো ছবি।

| কে | ভূমিকা | মডিউল |
|---|---|---|
| **রহিম ভাই** | মালিক | সব দেখে, রিপোর্ট |
| **সালমা** | সেলস + CRM | Lead, Quotation, SO |
| **জাবেদ** | স্টোর | Receipt, Delivery, MO স্টক |
| **রফিক** | প্রোডাকশন | Manufacturing Order |
| **করিম** | হিসাব | Invoice, Bill, Payment |
| **সালমা (HR হালকা)** | অথবা HR ক্লার্ক | Employee রেকর্ড |

```
Demo data ☐ OFF
DB নাম: rahim_full_erp
একবারে সব Apps Install → তারপর মাস্টার ডাটা → তারপর একটা End-to-End অর্ডার
```

---

## সোনার নিয়ম (পুরো ERP)

```
১  আগে Apps + Settings
২  তারপর মানুষ (Contact/Employee)
৩  তারপর পণ্য + BOM + স্টক
৪  তারপর কেনা (কাঁচামাল)
৫  তারপর তৈরি (MO)
৬  তারপর CRM/Sales (বিক্রি)
৭  তারপর Delivery
৮  শেষে Invoice/Bill/Payment (হিসাব)
কোনো ধাপ এড়িয়ে সামনে যাবেন না
```

---

# দিন ১ সকাল — খাতা খোলা (রহিম)

```
http://localhost:8069/web/database/manager
```

| Field | মান |
|---|---|
| Database Name | `rahim_full_erp` |
| Email | `admin@rahim.com` |
| Password | `admin123` |
| Country | Bangladesh |
| Demo data | ☐ খালি |

Create → Login  
Company: **রহিম ট্রেডার্স** → Timezone Asia/Dhaka → Save

---

# দিন ১ দুপুর — সব Apps + প্রতিটার মিনিমাল Settings (রহিম)

```
Apps → Install:
Contacts | CRM | Sales | Purchase | Inventory | Manufacturing | Invoicing | Employees
```

Install এর **পরই** Settings — এড়িয়ে যাবেন না।

### Inventory Settings
```
☑ Storage Locations | ☑ Lots & Serial Numbers | ☑ Multi-Step Routes → Save
Stock Parent = WH | Tak-1 বানান
```

### Sales Settings
```
☐ Lock Confirmed Sales
Payment Terms: New 15 Days
Product Invoicing Policy: Delivered quantities (ফিজিক্যাল)
→ Save
```

### Purchase Settings
```
☑ Warnings | ☑ Purchase Agreements
○ Bill Control = Received quantities
☐ Approval | ☐ Lock
→ Save
Contact Vendor এ Warning টেক্সট
```

### Manufacturing Settings
```
Work Orders ☐ (মিনিমাল) → Save
```

### CRM Settings
```
☑ Leads → Save
Lost Reason: Too expensive
```

### Invoicing / Accounting
```
Show Full Accounting Features (Groups)
Dashboard: Periods | Bank ম্যানুয়াল | Taxes Done | CoA তে Income/Expense খাতা
```

### Employees Settings
```
ডিফল্ট Save | Departments + Jobs | Employee কার্ড
```

রহিম: “প্রতি অ্যাপ Install → সাথে সাথে Settings মিনিমাল — তারপর মাস্টার ডাটা।”

---

# দিন ১ বিকেল — মাস্টার ডাটা (রহিম + দল)

## Contact

| Type | Name | পরে ব্যবহার |
|---|---|---|
| Company | Dhaka Electronics Ltd | Vendor |
| Company | সানরাইজ স্টোর | Customer |
| Company | নূর টেক শপ | CRM Lead/Customer |

## Employee (হালকা HR)

```
Employees → New
```

| Name | Job |
|---|---|
| সালমা | Sales Executive |
| জাবেদ | Store Keeper |
| রফিক | Production |
| করিম | Accountant |

(User অ্যাকাউন্ট পরে দিলে আলাদা লগইন চলে — শিখতে Admin দিয়েই সব করা যায়।)

## Warehouse / তাক

Stock Parent = WH।  
Tak-1 বানান (Parent = Stock) — তৈরি/কেনা মাল রাখতে।

## পণ্য

| Product | Type | নোট |
|---|---|---|
| USB Cable 1m | Storable | কাঁচামাল / কেনা |
| Plastic Case | Storable | কাঁচামাল |
| Charger Combo Pack | Storable | তৈরি + বিক্রি |
| Phone Charger 20W | Storable | সরাসরি কেনা-বেচা (ঐচ্ছিক) |

Tracking: Combo/Charger এ **By Lots** চাইলে Inventory পার্টের মতো।

## BOM

```
Manufacturing → Bills of Materials
```

Charger Combo Pack = USB Cable 1 + Plastic Case 1 → Save

```
Product ও BOM আগে — MO পরে
Contact আগে — SO/PO পরে
```

---

# দিন ২ — End-to-End গল্প ১: কাঁচামাল কিনে তৈরি

## সকাল — Purchase (সালমা / করিম)

```
Purchase → RFQ → Vendor: ঢাকা ইলেকট্রনিক্স
```

| Product | Qty | Price |
|---|---|---|
| USB Cable 1m | 100 | 70 |
| Plastic Case | 100 | 40 |

→ Confirm Order  

**জাবেদ:** Receipt → Todo → Validate (To = Tak-1)  
**করিম:** Create Bill → Confirm → (ঐচ্ছিক) Payment

স্টক: Cable 100, Case 100।

## দুপুর — Manufacturing (রফিক)

```
Manufacturing → Manufacturing Orders → New
Product: Charger Combo Pack | Qty: 20
→ Confirm → Check Availability → Produce / Done
```

স্টক: Cable/Case ≈ ৮০ করে; Combo Pack ≈ **২০**।

**রহিম কী দেখবে:** Inventory On Hand এ Combo এসেছে।

---

# দিন ৩ — End-to-End গল্প ২: CRM থেকে বিক্রি ও টাকা

## সকাল — CRM (সালমা)

```
CRM → Leads → New
```

| Field | মান |
|---|---|
| Name | নূর টেক — Combo ১০ প্যাক |
| Customer | নূর টেক শপ |
| Expected Revenue | 4500 |

→ Convert to Opportunity → স্টেজ: Qualified → Proposition

## দুপুর — Sales Quotation (সালমা)

Opportunity → **New Quotation**  
অথবা Sales → Quotations → New

| Field | মান |
|---|---|
| Customer | নূর টেক শপ |
| Product | Charger Combo Pack |
| Quantity | 10 |
| Price | 450 |

→ Confirm (Sales Order)

```
Confirm = অটো Delivery তৈরি
```

## বিকেল — Delivery (জাবেদ)

SO → Delivery → Todo → Lot (লাগলে) → **Validate**

Combo On Hand ≈ ১০ (২০−১০)।

## সন্ধ্যা — Invoice + Payment (করিম)

SO → Create Invoice → **Regular invoice** → Confirm  
→ Register Payment → Bank Journal

CRM Opportunity → **Won**

**রহিম কী দেখবে:**

```
CRM     → Won ডিল
Sales   → SO Done path
Inventory → স্টক কমেছে
Invoicing → Posted Invoice + Payment
Reporting → P&L এ আয়
```

একটাই গল্প — ছয়টা মডিউল।

---

# দিন ৪ — আরেকটা ছোট ফ্লো: শুধু চার্জার কেনা-বেচা

BOM ছাড়াও চলে:

1. Purchase → Phone Charger 50 → Receipt → Bill  
2. Sales → সানরাইজকে 15 → Delivery → Invoice  

Manufacturing লাগে না — খুচরা কেনা-বেচা।

রহিম: “কম্বো বানাই যখন অ্যাসেম্বলি লাগে; সাধারণ চার্জার সরাসরি কিনি।”

---

# দিন ৫ — কে কোন অ্যাপ খোলে (রোজকার)

| সময় | কে | অ্যাপ | কাজ |
|---|---|---|---|
| সকাল | সালমা | CRM / Sales | Lead, Quotation |
| সকাল | জাবেদ | Inventory | Receipt / Delivery |
| দুপুর | রফিক | Manufacturing | MO Produce |
| দুপুর | সালমা | Purchase | RFQ যখন স্টক কম |
| বিকেল | করিম | Invoicing | Invoice / Bill / Payment |
| সন্ধ্যা | রহিম | Reporting | P&L, Pipeline, Stock |

একই Login এ সব মেনু — অধিকার ভাগ করলে প্রত্যেকে নিজের অ্যাপ দেখে।

---

# মডিউল ম্যাপিং — এক পাতায়

| ব্যবসার কথা | Odoo অ্যাপ |
|---|---|
| মানুষ/দোকান | Contacts |
| আগ্রহ/ডিল | CRM |
| দরপত্র/অর্ডার | Sales |
| সাপ্লায়ার অর্ডার | Purchase |
| গুদাম/লট/তাক | Inventory |
| অ্যাসেম্বলি | Manufacturing |
| বিল/টাকা | Invoicing (+ Full Accounting Features) |
| কর্মী তালিকা | Employees |

```
CRM Won ≠ টাকা এসেছে
Sales Confirm ≠ মাল গেছে
Purchase Confirm ≠ মাল এসেছে
MO Done = তৈরি হয়ে স্টক বেড়েছে
Delivery/Receipt Validate = স্টক বদল
Invoice/Bill + Payment = টাকার খাতা
```

---

# এক নজরে Full ERP চেকলিস্ট

```
☐ DB rahim_full_erp — Demo OFF
☐ সব Apps Install + Inventory/Purchase Settings
☐ Contacts: Vendor + Customers
☐ Employees: সালমা, জাবেদ, রফিক, করিম
☐ Products + BOM
☐ Purchase কাঁচামাল → Receipt → Bill
☐ MO Combo 20 → Done
☐ CRM Lead নূর টেক → Opportunity
☐ SO Combo 10 → Delivery → Invoice → Payment → CRM Won
☐ রহিম: Stock + P&L + Pipeline চেক
```

---

## সমস্যা হলে

| সমস্যা | করণীয় |
|---|---|
| MO তে স্টক নেই | আগে Purchase Receipt |
| Delivery আটকে | আগে MO Done বা Purchase স্টক |
| Quotation এ Product নেই | Product Save + Can be Sold |
| Invoice মেনু কম | Invoicing Install + Full Accounting Features |
| কে কোন ধাপে জানি না | উপরের “কে কোন অ্যাপ” ছক |

---

## আলাদা পার্ট vs Full ERP

| আলাদা পার্ট | Full ERP (এই ফাইল) |
|---|---|
| এক মডিউল গভীর শেখা | সব মডিউল এক খাতায় জোড়া |
| আলাদা DB চলতে পারে | **এক DB** — এক গল্প শেষ পর্যন্ত |

আগে আলাদা গাইড পড়ে নিন → এই ফাইলে একবারে প্র্যাকটিস করুন।

এই ফাইল দিন ১ → ৫।  
রহিম ট্রেডার্সের পুরো ERP — Lead থেকে Payment পর্যন্ত এক লাইনে।
