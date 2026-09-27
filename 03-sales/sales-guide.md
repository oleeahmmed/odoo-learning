# Sales — সহজ গল্পে সম্পূর্ণ সেটআপ
### রহিম ট্রেডার্স: Inventory এর পর বিক্রি → Delivery → Invoice

---

## গল্প: অফিসে কে কে?

| কে | ভূমিকা | Odoo তে কী করে |
|---|---|---|
| **রহিম ভাই** | মালিক | Settings, রিপোর্ট |
| **সালমা** | সেলস | Quotation / Order |
| **জাবেদ** | স্টোর | Delivery Validate |
| **করিম** | হিসাব | Invoice / Payment (Accounting পার্টে বিস্তার) |
| **সানরাইজ স্টোর** | Customer | ক্রেতা |
| **Phone Charger 20W** | পণ্য | স্টক থাকতে হবে |

```
আগে: Contacts + Inventory (স্টক > ০)
একই DB rahim_traders
Demo ☐ OFF
```

---

## সোনার নিয়ম

```
১  Customer ও Product আগে থাকতে হবে
২  Confirm = অটো Delivery তৈরি (মাল এখনো বের হয়নি)
৩  Invoice/Payment ≠ Delivery Done
৪  Return = Delivery Done এর পর + Credit Note
```

---

# দিন ১ — Sales ইনস্টল (রহিম)

```
Apps → Sales → Install
```

```
Sales → Configuration → Settings → Save
```

(অতিরিক্ত টিক এখন কম রাখুন।)

মেনু:

```
Orders | To Invoice | Products | Reporting | Configuration
```

**সালমা পরে কী দেখবে:** Orders → Quotations।

---

# দিন ১ বিকেল — Contacts vs Sales Customer (রহিম শেখায়)

সালমা: “Contacts এও সানরাইজ, Sales এও Customer — দুটো কি আলাদা?”

রহিম: “না। একটাই Contact। Sales শুধু সেই খাতা থেকে বাছে।”

```
Contacts = মূল খাতা
Sales Customer ঘর = একই রেকর্ড
```

---

# দিন ২ — Quotation (সালমা)

**গল্প:** সানরাইজ ফোন — চার্জার ১০ পিস দর চাই।

স্টক আছে তো? জাবেদ বলল On Hand যথেষ্ট।

```
Sales → Orders → Quotations → New
```

| Field | ডেমো মান |
|---|---|
| Customer | সানরাইজ স্টোর |
| Product | Phone Charger 20W |
| Quantity | 10 |
| Unit Price | 350 |
| Warehouse | আপনার WH (থাকলে) |

→ **Save**

স্ট্যাটাস Quotation — এখনো স্টক কাটেনি।

---

# দিন ২ — Confirm (সালমা)

কাস্টমার রাজি → **Confirm**

```
Quotation → Sales Order
```

```
Confirm = Odoo অটো একটা Delivery বানিয়ে রাখে
মাল এখনো বের হয়নি — Validate বাকি
```

উপরে **Delivery** বাটন দেখা যাবে।

**জাবেদ কী দেখবে:** Inventory → Delivery Orders এ নতুন ডকুমেন্ট।

---

# দিন ৩ — Delivery (জাবেদ)

Sales Order → **Delivery**  
অথবা Inventory → Delivery Orders

1. Mark as Todo (লাগলে)  
2. ≡ এ Lot `CH-001`, Qty 10  
3. **Validate** → Done  

On Hand ১০ কমে।

```
সালমা Confirm করেছে ≠ মাল গেছে
জাবেদ Validate = মাল গেছে
```

---

# দিন ৩ বিকেল — Invoice (সালমা / করিম)

Sales Order → **Create Invoice**

পপআপে তিনটা অপশন:

| অপশন | মানে | এখন |
|---|---|---|
| **Regular invoice** | সাধারণ বিল | ✅ এটা বাছুন |
| Down payment (percentage) | % অগ্রিম | পরে |
| Down payment (fixed amount) | ফিক্সড অগ্রিম | পরে |

→ Create → **Confirm**

ঐচ্ছিক: **Register Payment**

```
Invoice/Payment করেও Delivery বাকি থাকতে পারে
টাকা ≠ মাল বের হওয়া
```

---

# দিন ৪ — Return সহজ (জাবেদ + করিম)

সানরাইজ ২ পিস ফেরত:

1. Delivery Done → **Return** → Qty 2 → Lot → Validate  
2. Invoice → **Credit Note** → Qty 2 → Confirm  

```
Return = মাল ফিরে
Credit Note = বিল কমে
দুটোই লাগে
```

জটিল কেস (backorder, আংশিক): `sales-advanced-guide.md`

---

# কে কখন কোন মেনু

| মেনু | সালমা | জাবেদ | রহিম |
|---|---|---|---|
| Quotations / Orders | ✅ রোজ | — | দেখে |
| Delivery | বাটন দেখে | ✅ Validate | — |
| Create Invoice | ✅ | — | — |
| Reporting | — | — | ✅ |

---

# এক নজরে

```
১  Sales Install
২  Quotation (সানরাইজ + চার্জার ১০)
৩  Confirm → অটো Delivery
৪  Delivery Validate + Lot
৫  Create Invoice → Regular → Confirm
৬  (ঐচ্ছিক) Return + Credit Note
```

## সমস্যা হলে

| সমস্যা | করণীয় |
|---|---|
| Delivery Validate স্টক নেই | আগে Inventory Receipt |
| Customer খালি | Contacts এ সানরাইজ |
| Invoice+Pay করেও Delivery দেখায় | স্বাভাবিক — Validate/Cancel করুন |

পরের পার্ট **Purchase**: ঢাকা ইলেকট্রনিক্স থেকে কেনা।
