# Purchase — সহজ গল্পে সম্পূর্ণ সেটআপ
### রহিম ট্রেডার্স: Sales এর উল্টো দিক — কেনা → Receipt → Bill

---

## গল্প: অফিসে কে কে?

| কে | ভূমিকা | Odoo তে কী করে |
|---|---|---|
| **রহিম ভাই** | মালিক | Settings, Agreement ধারণা |
| **সালমা** | কেনাকাটা | RFQ / Purchase Order |
| **জাবেদ** | স্টোর | Receipt Validate |
| **করিম** | হিসাব | Vendor Bill / Payment |
| **ঢাকা ইলেকট্রনিক্স** | Vendor | সাপ্লায়ার |
| **Phone Charger 20W** | পণ্য | By Lots |

```
আগে: Contacts + Inventory (+ Sales)
একই DB rahim_traders
Demo ☐ OFF
```

Sales তুলনা:

| Sales | Purchase |
|---|---|
| Customer | Vendor |
| Quotation / SO | RFQ / PO |
| Delivery | Receipt |
| Customer Invoice | Vendor Bill |

---

## সোনার নিয়ম

```
১  Vendor + Product আগে
২  Confirm Order = অটো Receipt (মাল এখনো ঢোকেনি)
৩  Bill Control = Received quantities ভালো (মাল এলে বিল)
৪  Warning টিক থাকলে Contact এ মেসেজ → PO তে Vendor বাছলে দেখায়
```

---

# দিন ১ — Purchase ইনস্টল + Settings (রহিম)

```
Apps → Purchase → Install
```

```
Settings → Purchase
```
অথবা `Purchase → Configuration → Settings`

মিনিমাল টিক:

```
☑ Warnings
☑ Purchase Agreements
```

Bill Control থাকলে: **Received quantities**

→ **Save**

---

## Warnings — কেন টিক? (রহিম সালমাকে)

টিক + Save না থাকলে Contact এ Warning ঘর আসে না।

```
Contacts → Dhaka Electronics Ltd
```

Warning on Purchase Order → মেসেজ:

```
MD স্যার: কোয়ালিটি চেক করে তারপর অর্ডার করুন
```

→ Save

এখন PO তে Vendor বাছলেই সতর্কবার্তা — ভুলে খারাপ অর্ডার কম।

**শুধু Done/টিক এর লাভ:** মানুষকে সাবধান করা। উদ্দেশ্য না বুঝে টিক নয়।

---

## Blanket Order — কখন? (রহিম সংক্ষেপ)

সাধারণ RFQ = আজকের এক কেনা।

**Blanket** = সারা বছরের ওয়াদা (যেমন ৬০০ পিস @ ২২০) — মাল এখনই আসে না।  
পরে ছোট PO কেটে Receipt।

মেনু (টিক থাকলে): `Purchase → Orders → Purchase Agreements`

শিখতে আগে সাধারণ RFQই করুন। Blanket দরকার হলে চুক্তি বানিয়ে পরে ছোট PO।

---

# দিন ২ — RFQ (সালমা)

**গল্প:** স্টক কম — ঢাকা ইলেকট্রনিক্সকে ৫০ পিস অর্ডার।

```
Purchase → Orders → Requests for Quotation → New
```

| Field | ডেমো মান |
|---|---|
| Vendor | Dhaka Electronics Ltd |
| Product | Phone Charger 20W |
| Quantity | 50 |
| Unit Price | 220 |

Vendor বাছলে Warning দেখা যেতে পারে → OK বুঝে এগোবেন।

→ **Save** (RFQ)

---

# দিন ২ — Confirm Order (সালমা)

→ **Confirm Order**

```
RFQ → Purchase Order
Confirm = অটো Receipt তৈরি
মাল এখনো গুদামে ঢোকেনি
```

**জাবেদ কী দেখবে:** Inventory → Receipts এ নতুন ডকুমেন্ট।

---

# দিন ৩ — Receipt (জাবেদ)

PO → **Receipt** বাটন

1. Mark as Todo  
2. ≡ এ Lot `CH-002`, Qty 50, To Tak-1 বা Stock  
3. **Validate** → Done  

On Hand +৫০।

```
সালমা Confirm ≠ মাল ঢোকা
জাবেদ Validate = মাল ঢোকা
```

আংশিক এলে Qty কম + Create Backorder (Sales advanced এর মতো গল্প)।

---

# দিন ৩ বিকেল — Vendor Bill (করিম)

PO → **Create Bill** → Confirm  
→ Register Payment (টাকা দিলে)

```
Customer Invoice = পাব
Vendor Bill = দেব
```

Received quantities থাকলে মাল যত এসেছে তত বিল সহজ।

---

# কে কখন কোন মেনু

| মেনু | সালমা | জাবেদ | করিম | রহিম |
|---|---|---|---|---|
| RFQ / PO | ✅ | — | — | Settings/Warning |
| Receipt | — | ✅ | — | — |
| Create Bill | — | — | ✅ | — |
| Agreements | কদাচিৎ | — | — | বুঝিয়ে দেয় |

---

# এক নজরে

```
১  Purchase Install
২  Warnings + Agreements ☑ → Contact এ Warning
৩  RFQ (ঢাকা ইলেকট্রনিক্স + চার্জার ৫০)
৪  Confirm → অটো Receipt
৫  Receipt + Lot → Validate
৬  Create Bill → Confirm
```

## সমস্যা হলে

| সমস্যা | করণীয় |
|---|---|
| Warning ঘর নেই | Settings এ Warnings ☑ + Save |
| Receipt বাটন নেই | Confirm Order + Inventory |
| Bill+Pay করেও Receipt বাকি | Validate আলাদা করতে হয় |

পরের পার্ট **Accounting**: Invoice/Bill এর টাকা ও রিপোর্ট — আলাদা গাইডে বিস্তারিত।
