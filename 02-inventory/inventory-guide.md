# Inventory — scratch থেকে সম্পূর্ণ সেটআপ
### রহিম ট্রেডার্স: Login → Install → Settings প্রতিটা → Configuration → Operations

---

## গল্প: কে কে?

| কে | কাজ |
|---|---|
| **রহিম** | Settings + Configuration |
| **জাবেদ** | Receipt / Delivery / On Hand |
| Vendor | Dhaka Electronics Ltd |
| Customer | সানরাইজ স্টোর |
| Product | Phone Charger 20W (By Lots) |

```
Demo ☐ OFF | DB: rahim_inventory
Apps: Contacts + Inventory
আগে Contact হাতে বানাবেন (এই গাইডেই আছে)
```

---

# দিন ১ — Scratch Login

```
http://localhost:8069/web/database/manager
```

| Field | মান |
|---|---|
| Database Name | `rahim_inventory` |
| Email | `admin@rahim.com` |
| Password | `admin123` |
| Country | Bangladesh |
| Demo data | ☐ খালি |

Company: **রহিম ট্রেডার্স** | Timezone Asia/Dhaka

```
Apps → Contacts → Install
Apps → Inventory → Install
```

মেনু:

```
Overview | Operations | Products | Reporting | Configuration
```

---

# দিন ১ — Inventory Settings (রহিম) — মিনিমাল + প্রতিটা বুঝে

```
Inventory → Configuration → Settings
```
অথবা `Settings → Inventory`

প্রতিটা সুইচ: টিক → Save → কোথায় দেখা যায়।  
শিখতে শেষে **মিনিমাল টিক** রাখবেন (নিচে সারাংশ)।

---

## Settings গল্প — Operations

### Storage Locations

| | |
|---|---|
| টিক নাই | শুধু এক Stock; তাক বানানোর মেনু কম |
| টিক + Save | Configuration → **Locations** পূর্ণ; তাক Parent=Stock |

**মিনিমাল:** ☑ টিক দিন (তাক শিখতে লাগে)

### Multi-Step Routes

| | |
|---|---|
| টিক + Save | Receipt/Delivery একাধিক ধাপ/রুট অপশন |
| শিখতে | ☑ রাখুন — অনেক ফিচার এর সাথে যায় |

### Lots & Serial Numbers

| | |
|---|---|
| টিক + Save | Product এ Tracking = By Lots / Serial; Receipt এ Lot |
| না দিলে | Lot ঘর পাবেন না |

**মিনিমাল:** ☑ টিক দিন

### Packages / Delivery Methods / ইত্যাদি

শিখতে ☐ খালি — পরে দরকার হলে।

---

## Settings — Products / অন্যান্য

| অপশন | মিনিমাল | কেন |
|---|---|---|
| Units of Measure | ☐ | পিসই যথেষ্ট |
| Product Packagings | ☐ | পরে |
| Variants | ☐ | পরে |
| Landed Costs | ☐ | পরে |
| Consignment | ☐ | পরে |

→ উপরে **Save** (টিক দিলে অবশ্যই Save)

### মিনিমাল সেট (Save এর পর এটাই রাখুন)

```
☑ Storage Locations
☑ Lots & Serial Numbers
☑ Multi-Step Routes
বাকি ☐
→ Save
```

**জাবেদ পরে কী দেখবে:** Locations মেনু; Product এ Tracking; Receipt এ Lot।

---

# দিন ১ — Configuration মেনু ধরে ধরে

```
Inventory → Configuration
```

---

## ১) Warehouses

**এখন:** একটা WH আছে।  
**করুন:** খুলে নাম রহিম ট্রেডার্স/ঠিকানা চেক → Save। নতুন আর বানাবেন না (মিনিমাল)।  
**পরে:** Receipt Operation Type এই WH এর সাথে।

---

## ২) Locations

**এখন:** WH, Stock, Virtual…  
**করুন:**

1. **Stock** খুলুন — Parent = **WH** (`WH/Stock`) — খালি রাখবেন না  
2. New → Tak-1 | Parent=Stock | Internal → Save  
3. New → Tak-2 | Parent=Stock | Internal → Save  

**পরে:** Receipt ≡ To = Tak-1; On Hand বাটনে তাক।

---

## ৩) Routes (ঐচ্ছিক দেখা)

ডিফল্ট Receive/Deliver রুট আছে। শিখতে এডিট নয়।

---

## ৪) Operation Types

Receipts, Delivery Orders দেখুন।  
Create Backorder = Ask (আংশিক ডেলিভারির জন্য ভালো)।  
শিখতে বেশি বদলাবেন না।

---

## ৫) Putaway Rules (ঐচ্ছিক)

New → Product Phone Charger → Store to Tak-1  
মিনিমাল শেখায়: ☐ স্কিপ — Receipt এ হাতে To বাছুন।

---

## ৬) Product Categories

All ক্যাটাগরিই যথেষ্ট। New লাগে না।

---

## ৭) Unit of Measure (UoM চালু থাকলে)

Units ডিফল্ট। নতুন UoM এখন নয়।

---

# দিন ২ — Contacts মেনু (Scratch এ হাতে)

Contacts অ্যাপ:

| Type | Name |
|---|---|
| Company | Dhaka Electronics Ltd |
| Company | সানরাইজ স্টোর |

---

# দিন ২ — Products মেনু

```
Products → Products → New
```

| Field | মান |
|---|---|
| Name | Phone Charger 20W |
| Type | Storable Product |
| Sales Price | 350 |
| Cost | 220 |
| Tracking | **By Lots** |

→ Save

```
Products → (ঐচ্ছিক) Lot/Serial Numbers — Receipt এর পর CH-001 দেখা যাবে
```

---

# দিন ৩ — Operations মেনু ধরে ধরে

## Overview

ড্যাশবোর্ড — Receipt/Delivery কাউন্ট। সেটআপের পর ভরে।

## Receipts

New → Vendor ঢাকা ইলেকট্রনিক্স → Product Demand 50  
→ Mark as Todo → ≡ Lot CH-001 Qty 50 To **Tak-1** → Validate  

**আগে:** On Hand 0  
**পরে:** On Hand ≈ 50; Location Tak-1

## Delivery Orders

New → সানরাইজ → Qty 20 → Todo → ≡ Lot CH-001 → Validate  
On Hand ≈ 30

## Transfers / Scrap / Adjustments

শিখতে পরে। Transfers = তাক থেকে তাকে।

---

# দিন ৩ — Reporting মেনু

| মেনু | কী দেখায় |
|---|---|
| Stock / Locations | কোন তাকে কত |
| Moves History | From–To Done মুভ |
| Inventory Valuation | মূল্য (অ্যাকাউন্ট থাকলে) |

---

# কে কোন মেনু

| মেনু | রহিম | জাবেদ |
|---|---|---|
| Settings / Locations / Warehouse | ✅ | দেখে |
| Products | Tracking সেট | On Hand |
| Receipts / Delivery | — | ✅ |
| Reporting | ✅ | Locations |

---

# এক নজরে

```
১  DB + Contacts + Inventory Install
২  Settings: Locations + Lots + Routes ☑ Save
৩  Stock Parent=WH | Tak-1, Tak-2
৪  Contact + Product By Lots
৫  Receipt Lot+To → Delivery
৬  Reporting চেক
```

পরের পার্ট **Sales** — scratch নিজের DB বা স্টক আছে এমন DB।
