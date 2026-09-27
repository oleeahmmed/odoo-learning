# Sales — একদম সহজ গাইড
### রহিম ভাইয়ের বিক্রি: অর্ডার নাও → মাল দাও → বিল কাটো

---

## আগে কোথায় ছিলেন?

Contacts + Inventory পার্ট শেষে আপনার কাছে আছে:

```
☑ রহিম ট্রেডার্স (Company)
☑ Customer: সানরাইজ স্টোর
☑ Vendor: ঢাকা ইলেকট্রনিক্স
☑ Product: Phone Charger 20W (By Lots)
☑ Warehouse + Stock + Tak-1 / Tak-2
☑ Receipt করে স্টক আছে (যেমন On Hand ≈ ৩০ বা যত রেখেছেন)
```

আজ শুধু **বিক্রির খাতা** — Sales।  
একই database। নতুন database লাগবে না।

> স্টক ০ থাকলে আগে Inventory পার্টে আবার Receipt করুন।  
> নাহলে Sales থেকে Delivery Validate হবে না।

---

## আজকের গল্প

সানরাইজ স্টোর ফোন করল:

> “ভাই, চার্জার **১০ পিস** লাগবে। দাম কত?”

রহিম ভাই আগে কাগজে লিখতেন। এখন Odoo-তে:

```
১  Quotation (অফার / দর) দেবেন
২  কাস্টমার রাজি → Confirm → Sales Order
৩  Delivery → মাল বের হবে (Inventory)
৪  Invoice → বিল
```

---

# ধাপ ১ — Sales ইনস্টল

```
Apps → Sales → Install
```

উপরে মেনুতে **Sales** দেখা গেলে OK।

---

## Contacts কেন ইনস্টল করেছিলাম? Sales-এ Customer আলাদা নাকি?

**গল্প:** Contacts অ্যাপে দেখেন “সানরাইজ স্টোর”।  
Sales অ্যাপে Quotation-এ Customer হিসেবেও “সানরাইজ স্টোর” বাছতে পারেন।  
দুটো জায়গায় দেখা যায় — তাই মনে হয় দুটো আলাদা জিনিস।

**আসলে একটাই রেকর্ড।**

Odoo-তে সব মানুষ/দোকান রাখে এক জায়গায় — নাম **Contact** (ভিতরে `res.partner`)।

```
Contacts অ্যাপ  = খাতার মূল তালিকা (সবাই)
Sales অ্যাপ     = সেই তালিকা থেকে “কে কিনবে” বেছে নেওয়া
Purchase অ্যাপ  = সেই তালিকা থেকে “কার কাছ থেকে কিনব” বেছে নেওয়া
```

| আপনি যা দেখেন | আসলে কী |
|---|---|
| Contacts → সানরাইজ | একটা Contact কার্ড |
| Sales → Customer = সানরাইজ | **একই** কার্ড — বিক্রির ফর্মে ব্যবহার |
| Purchase → Vendor = ঢাকা ইলেকট্রনিক্স | **একই** ধরনের Contact — কেনার ফর্মে ব্যবহার |

### Odoo কীভাবে হ্যান্ডেল করে?

1. **Contacts ইনস্টল** = নাম–ঠিকানা লেখার জায়গা পাওয়া।  
2. আপনি সানরাইজ লিখলেন → database-এ একটা Contact তৈরি।  
3. **Sales ইনস্টল** = Quotation/Order বানানোর জায়গা; Customer ঘরে Contacts থেকে বেছে নেন।  
4. সানরাইজকে একবার Sales Order-এ ব্যবহার করলে Odoo তাকে “customer” হিসেবে মনে রাখে (আলাদা কপি বানায় না)।  
5. ঢাকা ইলেকট্রনিক্সকে Purchase-এ ব্যবহার করলে “vendor” — আবারও **একই Contact টেবিল**।

```
❌ Sales-এ আলাদা Customer ডাটাবেস নেই
❌ Contacts মুছলে Sales-এ অন্য কপি থাকে না — একটাই
✅ একবার Contacts-এ লিখুন → Sales/Purchase দুটোতেই ব্যবহার
```

একই দোকান চাইলে Customer **ও** Vendor হতে পারে (কেনেও, বেচেও) — তবু Contact একটি।

### তাহলে Contacts কেন আগে লাগল?

```
Contacts ছাড়া → মানুষ/দোকানের মূল খাতা নেই
Sales একা     → অর্ডার বানাতে Customer বাছতে হয়; সেটা Contacts থেকেই আসে
```

তাই শেখার ক্রম:

```
Contacts → নাম লিখি
Sales    → সেই নামে বিক্রি করি
Purchase → সেই (বা অন্য) নামে কেনি
```

> আলাদা “Is Customer” টিক Odoo 17-এ সাধারণত নেই।  
> Customer হয় ব্যবহার করে — Sales Order/Invoice এ বেছে নিলেই।

---

# ধাপ ২ — Settings (সহজ টিক)

```
Sales → Configuration → Settings
```

অথবা:

```
Settings → Sales
```

শিখতে এগুলো টিক রাখলে ভালো (অনেকগুলো ডিফল্টেই থাকে):

```
☑ Variants          (না থাকলেও চলবে)
☑ Delivery Methods  (ঐচ্ছিক)
```

প্রয়োজন না হলে বেশি টিক নিয়ে মাথা ঘামাবেন না।  
→ **Save**

Inventorry আগে ইনস্টল থাকলে Delivery অটো যুক্ত থাকে — আলাদা কিছু লাগে না।

---

# ধাপ ৩ — Sales মেনু চিনুন

```
Apps থেকে Sales খুলুন
```

উপরে মোটামুটি:

```
Orders | To Invoice | Products | Reporting | Configuration
```

| মেনু | সহজ মানে |
|---|---|
| Orders | Quotation / Sales Order |
| To Invoice | বিল কাটার অপেক্ষায় |
| Products | পণ্য (Inventory-র সাথেই শেয়ার) |
| Reporting | বিক্রির রিপোর্ট |
| Configuration | Settings |

আজ কাজ:

```
Orders → Quotation → Confirm → Delivery → Invoice
```

---

# ধাপ ৪ — Quotation বানান (দরপত্র)

**গল্প:** সানরাইজকে ১০ পিস চার্জারের অফার।

```
Sales → Orders → Quotations → New
```

### হেডার

| Field | মান |
|---|---|
| Customer | সানরাইজ স্টোর |
| Quotation Date | আজকের তারিখ (অটো) |

Warehouse থাকলে দেখতে পারেন:

| Field | মান |
|---|---|
| Warehouse | আপনার গুদাম (রহিম ট্রেডার্স / WH) |

> গুদাম এখানে বেছে নেন — Product-এ নয়।  
> Inventory পার্টে যে কথা বলেছিলাম: **কোন গুদাম = ডকুমেন্টে**।

### লাইন Add

| Field | মান |
|---|---|
| Product | Phone Charger 20W |
| Quantity | 10 |
| Unit Price | 350 (অটো আসতে পারে) |

→ **Save**

স্ট্যাটাস সাধারণত **Quotation**।

এখন এটা শুধু অফার — মাল এখনো বের হয়নি, স্টক কাটেনি।

---

# ধাপ ৫ — Confirm (অর্ডার ফাইনাল)

কাস্টমার রাজি হলে:

উপরে **Confirm** চাপুন।

নাম বদলে যায়:

```
Quotation  →  Sales Order
```

স্ট্যাটাস **Sales Order**।

```
Confirm = Odoo অটো একটা Delivery বানিয়ে রাখে
(Inventory ইনস্টল থাকলে)
```

মাল এখনো বের হয়নি — শুধু Delivery ডকুমেন্ট তৈরি।  
উপরে **Delivery** স্মার্ট বাটন দেখা যাবে। Validate করলে মাল বের হবে।

### জরুরি: Invoice / Payment করলেও Delivery কেন থাকে?

**গল্প:** অর্ডার Confirm → টাকার বিল কাটলেন → Register Payment করলেন।  
তবু Delivery লিস্টে / SO তে Delivery বাটন দেখাচ্ছে — এটা **বাগ না**, Odoo-র নিয়ম।

Confirm করার সাথে সাথে Odoo **দুটো আলাদা কাজ** খুলে দেয়:

```
১  Delivery  = গুদাম থেকে মাল বের করা   (Inventory)
২  Invoice   = বিল + টাকা                  (Accounting)
```

| আপনি যা করলেন | কী হয়েছিল | কী হয়নি |
|---|---|---|
| Sales Order Confirm | Delivery ডকুমেন্ট **তৈরি** | মাল এখনো বের হয়নি |
| Invoice Confirm | বিল Posted | Delivery Done হয়নি |
| Register Payment | টাকা পাওয়ার রেকর্ড | স্টক কমেনি |

তাই Delivery **দেখাবেই** — যতক্ষণ না আপনি:

```
Delivery খুলে → Validate   → মাল বের, স্টক কমবে
অথবা
Delivery খুলে → Cancel     → মাল যাবে না (সেবা/শুধু বিল হলে কখনো কখনো)
```

```
টাকা নেওয়া  ≠  মাল দেওয়া
Invoice/Payment করেও Delivery বাকি থাকতে পারে
বাকি Delivery Validate (বা Cancel) না করলে লিস্টে দেখা যাবে
```

স্বাভাবিক সম্পূর্ণ ফ্লো:

```
Confirm → Delivery Validate → Invoice → Register Payment
```

আগে Invoice/Payment করে ফেললেও পরে Delivery Validate করতে পারবেন — ডকুমেন্ট মুছে যায় না।

---

# ধাপ ৬ — Delivery (মাল বের করা)

**গল্প:** অর্ডার কনফার্ম → এখন গুদাম থেকে ১০ পিস বের করতে হবে।

Sales Order পেজে উপরে **Delivery** বাটন চাপুন।  
(অথবা `Inventory → Operations → Delivery Orders` এ নতুন ডকুমেন্ট দেখা যাবে।)

### Delivery ফর্ম

| দেখবেন | মানে |
|---|---|
| Customer / Delivery Address | সানরাইজ স্টোর |
| Product | Phone Charger 20W |
| Demand / Quantity | 10 |

### করণীয়

1. **Mark as Todo** (যদি Draft থাকে)  
2. লাইনের **≡** এ Lot = **CH-001**, Quantity = **10**  
3. উপরে **Validate**

স্ট্যাটাস **Done** হলে মাল বেরিয়ে গেছে।

### চেক

```
Inventory → Products → Phone Charger 20W
```

On Hand আগের থেকে **১০ কম** (যেমন ৩০ ছিল → ২০)।

```
Sales Order Confirm  ≠  মাল বের হওয়া
মাল বের হয়         =  Delivery Validate
```

---

# ধাপ ৭ — Invoice (বিল) + তিনটা অপশন কী

আবার Sales Order পেজে যান।  
Delivery Validate হয়ে গেলে ভালো (মাল গেছে)।

উপরে **Create Invoice** চাপুন।

পপআপে সাধারণত **তিনটা অপশন** দেখা যায়:

```
○ Regular invoice
○ Down payment (percentage)
○ Down payment (fixed amount)
```

---

### অপশনগুলোর মানে (গল্পে)

সানরাইজের অর্ডার মোট ≈ **৩৫০০ টাকা** (১০ × ৩৫০)।

| অপশন | সহজ মানে | কখন ব্যবহার |
|---|---|---|
| **Regular invoice** | পুরো (বা বাকি) মালের সাধারণ বিল | সবচেয়ে বেশি ব্যবহার — শিখতে **এটাই** বাছুন |
| **Down payment (percentage)** | মোটের **%** অগ্রিম বিল (যেমন ৩০%) | মাল যাওয়ার আগে আংশিক টাকা নিতে |
| **Down payment (fixed amount)** | নির্দিষ্ট টাকা অগ্রিম (যেমন ১০০০) | “আগে ১০০০ দাও” টাইপ চুক্তি |

---

### ১) Regular invoice — আজ এটা করুন

○ **Regular invoice** সিলেক্ট করুন → **Create Invoice**

Invoice খুলবে:

| দেখবেন | মানে |
|---|---|
| Customer | সানরাইজ স্টোর |
| Product / Qty / Price | চার্জার × দাম |

→ **Confirm**

স্ট্যাটাস **Posted** = বিল কাটা হয়ে গেছে।

চাইলে **Register Payment** → “টাকা পেয়েছি”।

```
শিখতে ডিফল্ট পথ:
Delivery Validate → Create Invoice → Regular invoice → Confirm
```

---

### ২) Down payment (percentage) — কী হয়?

**গল্প:** মাল যাওয়ার আগে সানরাইজ মোট বিলের ৩০% অগ্রিম দিল।

```
Create Invoice → ○ Down payment (percentage) → 30% → Create Invoice
```

Odoo একটা **অগ্রিম বিল** বানায় (মোটের ৩০%)।  
Confirm + Register Payment = অগ্রিম টাকা রেকর্ড।

পরে মাল Delivery করে আবার Create Invoice → **Regular invoice** করলে  
বাকি বিল কাটে (অগ্রিম হিসাব মিলিয়ে)।

```
Percentage down payment = মোটের ভাগ অগ্রিম
এখন শিখতে বাধ্য নয় — জেনে রাখুন
```

---

### ৩) Down payment (fixed amount) — কী হয়?

**গল্প:** “আগে ১০০০ টাকা দাও, পরে বাকি।”

```
Create Invoice → ○ Down payment (fixed amount) → 1000 → Create Invoice
```

অগ্রিম বিল = **১০০০ টাকা** (পারসেন্ট নয়)।  
পরে Regular invoice এ বাকি মালের বিল।

```
Fixed = নির্দিষ্ট টাকা অগ্রিম
Percentage = মোটের % অগ্রিম
Regular = সাধারণ পুরো/বাকি বিল
```

---

### ছোট নিয়ম

| পরিস্থিতি | কোন অপশন |
|---|---|
| মাল দিয়ে পুরো বিল | **Regular invoice** |
| আগে % টাকা | Down payment (percentage) |
| আগে ফিক্সড টাকা | Down payment (fixed amount) |

> Accounting পুরো পার্টে অগ্রিম/জার্নাল আরও বিস্তারিত হবে।  
> এখন মনে রাখুন: তিনটা অপশনের কাজ আলাদা — ভুল করে Down payment বাছবেন না যদি সাধারণ বিল চান।

---

# ধাপ ৮ — Return (মাল ফেরত) — সহজ

**গল্প:** সানরাইজ ১০ পিস পেয়েছে। ২ পিস কাজ করছে না — ফেরত দিচ্ছে।

রিটার্নে **দুটো কাজ**:

```
১  মাল গুদামে ফেরত  → Delivery থেকে Return
২  বিল কমানো         → Invoice থেকে Credit Note
```

---

### ৮ক) মাল ফেরত

Sales Order → **Delivery** বাটন → যে Delivery **Done** সেটা খুলুন।

উপরে **Return** চাপুন।

| Field | মান |
|---|---|
| Product | Phone Charger 20W |
| Quantity | **2** |
| Lot | CH-001 (≡ এ দিতে হতে পারে) |

→ Return / Validate করে **Done** করুন।

চেক: Product → On Hand আগের থেকে **+২**।

---

### ৮খ) বিল কমানো (Credit Note)

Sales Order → **Invoice** → Posted Invoice খুলুন।

উপরে **Credit Note** চাপুন।

| অপশন | মানে |
|---|---|
| Partial Refund | শুধু কিছু পিস — **এটাই** (Qty = 2) |
| Full Refund | পুরো বিল বাতিল |

→ Qty **2** রাখুন → Confirm।

```
Return      = মাল ফিরে এল
Credit Note = টাকার বিল ঠিক হল
দুটোই করুন
```

জটিল কেস (আংশিক Delivery + No Backorder ইত্যাদি) → `sales-advanced-guide.md`।

---

# ধাপ ৯ — পুরো ছবি একবার দেখুন

Sales Order পেজে স্মার্ট বাটন:

```
Delivery   → মাল গেছে / Return হয়েছে
Invoice    → বিল / Credit Note
```

এক লাইনে ফ্লো:

```
Quotation → Confirm → Delivery Validate → Invoice (Regular) → (প্রয়োজনে) Return + Credit Note
```

---

# (ঐচ্ছিক) ধাপ ১০ — শুধু Quotation বাতিল

কাস্টমার রাজি না হলে Confirm-এর আগে:

```
Cancel  অথবা  নতুন Quotation বানিয়ে পুরনো রেখে দিন
```

Confirm এর পর বাতিল জটিল — শিখতে Confirm শুধু রাজি অর্ডারেই করুন।

---

# এক নজরে পুরো ক্রম

```
আগে:  Contacts + Inventory (স্টক আছে)
১     Sales Install
২     Settings Save (সহজ)
৩     Quotations → New (সানরাইজ + চার্জার ১০)
৪     Confirm → Sales Order (অটো Delivery তৈরি)
৫     Delivery → Todo → Lot → Validate
৬     Create Invoice → Regular invoice → Confirm
৭     (ঐচ্ছিক) Register Payment
৮     (ঐচ্ছিক) Return + Credit Note
```

Invoice অপশন মনে রাখুন:

```
Regular           = সাধারণ বিল
Down payment %    = অগ্রিম শতাংশ
Down payment fixed = অগ্রিম নির্দিষ্ট টাকা
```

---

## সমস্যা হলে

| সমস্যা | কী করবেন |
|---|---|
| Sales মেনু নেই | ধাপ ১ Install |
| Customer লিস্টে সানরাইজ নেই | Contacts পার্টে Company সেভ আছে তো? |
| Product নেই / প্রাইস ০ | Inventory পার্টে Phone Charger Save + Sales Price |
| Delivery Validate error / স্টক নেই | আগে Receipt করুন; On Hand > ০ হতে হবে |
| Lot চায় | ≡ এ CH-001 + Qty দিন |
| Delivery বাটন নেই | Confirm করেছেন তো? Inventory ইনস্টল আছে তো? |
| Invoice তৈরি হয় না | আগে Confirm; অনেক সময় Delivery পরে Invoice সুবিধাজনক |
| Invoice+Payment করেও Delivery দেখাচ্ছে | স্বাভাবিক — Delivery আলাদা; Validate বা Cancel করুন |
| ভুলে Down payment বেছে ফেললাম | সাধারণ বিল চাইলে **Regular invoice** বাছুন |
| Return বাটন নেই | Delivery আগে **Done** হতে হবে |
| Credit Note নেই | Invoice আগে **Confirm/Posted** হতে হবে |

---

## আগের পার্টের সাথে যোগ

```
Contacts   → কে কিনবে (সানরাইজ)
Inventory  → কী মাল, কোথায় স্টক
Sales      → বিক্রি, Delivery, Invoice অপশন, Return
```

পরের পার্ট **Purchase**: ঢাকা ইলেকট্রনিক্স থেকে কেনার অর্ডার — Receipt অটো তৈরির গল্প।

---

এই ফাইল উপর থেকে **ধাপ ১ → ৯** একটার পর একটা করুন।  
জটিল কেস চাইলে `sales-advanced-guide.md`।
