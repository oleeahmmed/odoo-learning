# Purchase — scratch থেকে সম্পূর্ণ সেটআপ
### রহিম ট্রেডার্স: Login → Install → Settings প্রতিটা → Configuration → Orders

---

## কে কে?

| কে | কাজ |
|---|---|
| **রহিম** | Settings ব্যাখ্যা + Save |
| **সালমা** | RFQ / Confirm |
| **জাবেদ** | Receipt |
| **করিম** | Vendor Bill |
| Vendor | Dhaka Electronics Ltd |
| Product | Phone Charger 20W |

```
Demo ☐ OFF | DB: rahim_purchase
Apps: Contacts + Inventory + Purchase (+ Invoicing Bill এর জন্য)
```

---

# দিন ১ — Scratch Login + Install

| Database Name | `rahim_purchase` |
| Demo data | ☐ |
| Email / Pass | admin@rahim.com / admin123 |
| Country | Bangladesh |

Company রহিম ট্রেডার্স + Timezone

```
Apps → Contacts → Inventory → Purchase → Invoicing → Install
```

Inventory Settings: ☑ Storage Locations | ☑ Lots → Save

মেনু Purchase:

```
Orders | Products | Reporting | Configuration
```

---

# দিন ১ — Purchase Settings প্রতিটা (রহিম) → মিনিমাল Save

```
Purchase → Configuration → Settings
```

---

## Orders

### Purchase Order Approval

টিক = বড় টাকায় ম্যানেজার Approve।  
**মিনিমাল:** ☐ (শিখতে আটকাবে)

### Lock Confirmed Orders

টিক = Confirm পর এডিট বন্ধ।  
**মিনিমাল:** ☐

### Warnings

টিক + Save → Contact এ Warning ঘর আসে।  
Vendor এ মেসেজ লিখলে PO তে Vendor বাছলেই দেখায়।  

**ডেমো:** ঢাকা ইলেকট্রনিক্স → Warning: `কোয়ালিটি চেক করুন`  

**মিনিমাল:** ☑

### Purchase Agreements

টিক + Save → মেনু Agreements (Blanket/Tender)।  
**মিনিমাল:** ☑ (মেনু দেখতে) — শিখতে আগে সাধারণ RFQই করুন।  
Blanket = বছরের চুক্তি; ছোট PO পরে।

### Receipt Reminder

টিক = Vendor কে তারিখ রিমাইন্ডার।  
**মিনিমাল:** ☐

---

## Invoicing

### Bill Control

| অপশন | মানে |
|---|---|
| Ordered quantities | অর্ডারের পুরো Qty বিল |
| **Received quantities** | যে মাল Receipt হয়েছে তত বিল |

**মিনিমাল:** ○ **Received quantities**

### 3-way matching

Enterprise — Community তে না থাকলে স্কিপ।

---

## Products

| Variants / Grid / Packagings / UoM | মিনিমাল ☐ |
|---|---|

→ **Save**

### মিনিমাল সেট সারাংশ

```
☑ Warnings
☑ Purchase Agreements
○ Bill Control = Received quantities
☐ Approval, Lock, Reminder, Variants…
→ Save
```

---

# দিন ১ — Configuration মেনু ধরে

| মেনু | কী করবেন |
|---|---|
| Settings | উপরেই |
| Purchase Agreements | লিস্ট দেখুন; Blanket পরে |
| Products | Inventory এর সাথে শেয়ার |
| Vendor Pricelists | স্কিপ |
| Tags | ঐচ্ছিক |

---

# দিন ২ — মাস্টার Scratch

Contacts: Dhaka Electronics Ltd + Warning মেসেজ  
Product: Phone Charger 20W Storable By Lots Cost 220  
Locations: Stock Parent=WH

---

# দিন ২–৩ — Orders মেনু ধরে

## Requests for Quotation

New → Vendor ঢাকা… (Warning দেখা যেতে পারে) → Product Qty 50 Price 220 → Save

## Confirm Order

Confirm → PO  
**অটো Receipt** তৈরি — মাল এখনো ঢোকেনি।

## Purchase Orders লিস্ট

Confirmed PO এখানে।

## Agreements (ঐচ্ছিক)

Blanket: Vendor + Product Qty 600 @ 220 → পরে ছোট PO।  
প্রথম প্র্যাকটিসে স্কিপ করতে পারেন।

---

# দিন ৩ — Receipt (জাবেদ) + Bill (করিম)

PO → Receipt → Todo → ≡ Lot CH-001 Qty 50 → Validate  

PO → Create Bill → Confirm → Payment ঐচ্ছিক  

```
Confirm ≠ মাল ঢোকা
Receipt Validate = স্টক +
Bill = Vendor কে দেয় টাকার খাতা
```

---

# Products / Reporting মেনু

Products = পণ্য।  
Reporting = Purchase analysis।

---

# কে কোন মেনু

| মেনু | রহিম | সালমা | জাবেদ | করিম |
|---|---|---|---|---|
| Settings | ✅ | — | — | — |
| RFQ/PO | — | ✅ | — | — |
| Receipt | — | — | ✅ | — |
| Bill | — | — | — | ✅ |
| Agreements | ব্যাখ্যা | কদাচিৎ | — | — |

---

# এক নজরে

```
১  DB + Apps Install
২  Settings: Warnings + Agreements + Received quantities
৩  Vendor + Warning টেক্সট + Product
৪  RFQ → Confirm → Receipt → Bill
```

পরের পার্ট Accounting / Manufacturing — নিজ নিজ scratch গাইড।
