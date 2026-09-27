# Inventory — একদম সহজ গাইড
### রহিম ভাইয়ের গুদাম: মাল কোথায়, কত আছে

---

## আগে কোথায় ছিলেন?

Contacts পার্টে আপনি:

```
☑ Database খুলেছেন
☑ Company: রহিম ট্রেডার্স
☑ Contacts ইনস্টল করেছেন
☑ Vendor: ঢাকা ইলেকট্রনিক্স
☑ Customer: সানরাইজ স্টোর
```

আজ শুধু **গুদামের খাতা** — Inventory।

একই database-এ থাকুন। নতুন database লাগবে না।

---

## আজকের গল্প

রহিম ভাইয়ের একটা গুদাম আছে।  
ভিতরে একটা স্টোর রুম (**Stock**)।  
রুমের ভিতরে দুই তাক: **Tak-1**, **Tak-2**।

ঢাকা ইলেকট্রনিক্স থেকে মাল আসবে → Receipt।  
সানরাইজকে মাল যাবে → Delivery।

---

# ধাপ ১ — Inventory ইনস্টল

```
Apps → Inventory → Install
```

উপরে মেনুতে **Inventory** দেখা গেলে OK।

---

# ধাপ ২ — Settings-এ টিক

```
Inventory অ্যাপ → উপরে ⚙ Settings
```

অথবা:

```
মেনু Settings → Inventory
```

এগুলোতে টিক দিন:

```
☑ Storage Locations
☑ Lots & Serial Numbers
☑ Multi-Step Routes
```

→ উপরে **Save**

Save এর পর Warehouse অংশে লিঙ্ক দেখা যেতে পারে (`→ Locations` ইত্যাদি)।  
এখন সেগুলোতে যেতে হবে না — আমরা Inventory অ্যাপের **Configuration** মেনু দিয়ে যাব।

---

# ধাপ ৩ — Inventory মেনু চিনুন

```
Apps মেনু থেকে Inventory খুলুন
```

উপরে টপ মেনু দেখবেন:

```
Overview | Operations | Products | Reporting | Configuration
```

| মেনু | সহজ মানে |
|---|---|
| Overview | গুদামের সারাংশ |
| Operations | Receipt, Delivery, Transfer |
| Products | পণ্য |
| Reporting | রিপোর্ট |
| Configuration | Warehouse, Location সেটআপ |

আজ কাজের ক্রম:

```
Configuration → Products → Operations
```

---

# ধাপ ৪ — Warehouse দেখুন

```
Inventory → Configuration → Warehouses
```

একটা warehouse ইতিমধ্যে আছে — দোকান খোলার সময় Odoo বানিয়েছে।

নাম সাধারণত **রহিম ট্রেডার্স** বা **WH** জাতীয়।

### চাইলে এডিট

Warehouse খুলুন → নাম/ঠিকানা ঠিক করুন → **Save**

### চাইলে আরেকটা বানান

**New** → নাম দিন → Save  
(শিখতে এখন **একটাই** warehouse রাখুন — সহজ থাকবে।)

মনে রাখুন: এখন আপনার **একটা গুদাম** আছে।

---

# ধাপ ৫ — Location দেখুন (Stock)

```
Inventory → Configuration → Locations
```

লিস্টে **Stock** নামের একটা location খুঁজুন → খুলুন।

দেখবেন মোটামুটি:

| Field | মানে |
|---|---|
| Location Name | Stock |
| Parent Location | আপনার warehouse (যেমন WH) |
| Location Type | Internal Location |

**গল্প:**

```
Warehouse (গুদাম)
 └── Stock (স্টোর রুম)    ← এখন যেটা খুলেছেন
```

Parent = warehouse মানে: এই Stock রুমটা **ঐ গুদামের ভিতরে**।

Parent মুছবেন না। শুধু দেখে বুঝুন → পিছনে Locations লিস্টে ফিরুন।

---

# ধাপ ৬ — দুই তাক বানান (Tak-1, Tak-2)

Locations লিস্টে থাকুন।

### Tak-1

1. **New**
2. লিখুন:

| Field | মান |
|---|---|
| Location Name | Tak-1 |
| Parent Location | **Stock** (আগের ডিফল্ট স্টোর রুম) |
| Location Type | Internal Location |

3. **Save**

### Tak-2

আবার **New**:

| Field | মান |
|---|---|
| Location Name | Tak-2 |
| Parent Location | **Stock** |
| Location Type | Internal Location |

→ **Save**

### এখন আপনার ছবি এমন

```
Warehouse
 └── Stock
      ├── Tak-1
      └── Tak-2
```

লিস্টে নাম দেখা যাবে মোটামুটি:

```
WH/Stock
WH/Stock/Tak-1
WH/Stock/Tak-2
```

হলে configure ঠিক আছে।

---

# ধাপ ৭ — Product যোগ করুন

```
Inventory → Products → Products → New
```

| Field | মান |
|---|---|
| Product Name | Phone Charger 20W |
| Product Type | **Storable Product** |
| Sales Price | 350 |
| Cost | 220 |

ট্যাব **Inventory**:

| Field | মান |
|---|---|
| Tracking | **By Lots** |

→ **Save**

**গল্প:** By Lots = এক ব্যাচে অনেক পিস (যেমন CH-001 তে ৫০ পিস)।

---

# ধাপ ৭ক — পণ্য কোন তাক / কোন গুদামে যাবে?

ডিফল্টে Receipt করলে মাল **ঐ warehouse-এর Stock** এ যায়।  
আলাদা তাক বা আলাদা গুদাম চাইলে **সেট** করতে হয়।

---

## কেন সবসময় Stock এ যায়?

Receipt ফর্মে একটা **Operation Type** থাকে — যেমন:

```
রহিম ট্রেডার্স: Receipts
```

এটা কোন warehouse-এর Receipts → মাল সেই গুদামের **Stock** এ যায়।  
তাক আলাদা করতে বা গুদাম আলাদা করতে নিচের দুই রাস্তা।

---

## ক) একই গুদাম — আলাদা তাক (সবচেয়ে সহজ)

**গল্প:** চার্জার সবসময় Tak-1 এ রাখব। কেবল অন্য পণ্য Tak-2 এ।

### একবার নিয়ম বানান (Putaway)

```
Inventory → Configuration → Putaway Rules → New
```

| Field | মান |
|---|---|
| Product | Phone Charger 20W |
| Store to / Location | **Tak-1** (WH/Stock/Tak-1) |

→ **Save**

এর পর এই পণ্য Receipt করলে Odoo চেষ্টা করবে **Tak-1** এ পাঠাতে।

আরেক পণ্য (যেমন Cable) Tak-2 এ চাইলে আরেকটা Putaway Rule:

| Product | Store to |
|---|---|
| USB Cable | Tak-2 |

### অথবা হাতে (প্রতিবার)

Receipt → Mark as Todo → ≡ এ **To = Tak-1** বাছুন।  
Putaway না থাকলে এভাবেই তাক ঠিক হয়।

```
Putaway  = একবার সেট → পরে অটো
To ফিল্ড = শুধু এই Receipt-এর জন্য
```

---

## খ) আলাদা গুদাম

**গল্প:** ঢাকায় এক গুদাম, চট্টগ্রামে আরেক গুদাম।

### গুরুত্বপূর্ণ সত্য (মনে রাখুন)

Odoo Product ফর্মে এমন ফিল্ড **নেই**:

```
Default Warehouse = চট্টগ্রাম   ← এমন কিছু নেই
```

মানে: “এই পণ্য সবসময় শুধু এই গুদাম ব্যবহার করবে” — **এক ক্লিকে সেট হয় না**।

Odoo-র নিয়ম অন্যরকম:

```
পণ্য  = কী মাল
গুদাম = কোন ডকুমেন্টে (Receipt / SO / PO) আপনি বেছে নিলেন
তাক   = Putaway বা To দিয়ে সেট করা যায়
```

তাই গুদাম বেছে নিতে হয় **Receipt / Delivery ফর্মে** — Product সেভে নয়।

### ১) দ্বিতীয় warehouse বানান

```
Inventory → Configuration → Warehouses → New
```

| Field | মান |
|---|---|
| Warehouse Name | চট্টগ্রাম গুদাম |
| Short Name | CTG |

→ **Save**

Odoo নিজেই এই গুদামের Stock (এবং Receipts/Delivery) বানিয়ে দেবে।

চাইলে সেই গুদামের Stock-এর নিচে আবার তাক বানাতে পারেন (Parent = ঐ Stock)।

### ২) Receipt-এ সঠিক গুদাম বাছুন

```
Inventory → Operations → Receipts → New
```

ফর্মে **Operation Type** দেখুন — এখানেই গুদাম বেছে নেন:

| চাইলে | Operation Type |
|---|---|
| ঢাকা গুদামে মাল | রহিম ট্রেডার্স: Receipts (বা WH: Receipts) |
| চট্টগ্রামে মাল | চট্টগ্রাম গুদাম: Receipts (CTG: Receipts) |

→ Destination Location অটো **ঐ গুদামের Stock** হবে।

তারপর Todo → Lot → Validate — আগের মতোই।

```
কোন গুদাম?  →  Receipt-এর Operation Type  (Product-এ সেট হয় না)
কোন তাক?    →  Putaway Rule অথবা ≡ এর To  (এটাই Product অনুযায়ী সেট করা যায়)
```

### চাইলে আনুমানিক “পণ্য → গুদাম” অভ্যাস

পরে Sales/Purchase পার্টে দেখবেন: অর্ডারে Warehouse বাছতে হয়।  
Inventory-তে চাইলে **Reordering Rules** দিয়ে বলা যায় “এই পণ্য এই গুদামে মজুত রাখো” — কিন্তু Receipt স্বয়ংক্রিয়ভাবে সেই গুদামে বাধ্য হয় না; Receipt-এ Operation Type তবু ঠিক রাখতে হয়।

---

## ছোট চেক

Receipt Done এর পর:

```
Products → Phone Charger 20W → On Hand বাটন
```

Location দেখুন — Tak-1 / CTG Stock ইত্যাদি যেখানে সেট করেছেন সেখানেই থাকবে।

---

# ধাপ ৮ — Receipt (মাল ঢোকানো)

**গল্প:** ঢাকা ইলেকট্রনিক্স থেকে চার্জার ৫০ পিস এল। ব্যাচ CH-001।

```
Inventory → Operations → Receipts → New
```

### ফর্ম

| Field | মান |
|---|---|
| Receive From | Dhaka Electronics Ltd (আপনার Vendor) |
| Operation Type | কোন গুদামে ঢোকাবেন — সেই গুদামের Receipts (ধাপ ৭ক) |

লাইন Add করুন:

| Field | মান |
|---|---|
| Product | Phone Charger 20W |
| Demand | 50 |

→ উপরে **Mark as Todo**

### Lot দিন

প্রোডাক্ট লাইনের ডানে **≡** (লিস্ট) আইকন চাপুন।

| Field | মান |
|---|---|
| Lot | CH-001 |
| Quantity | 50 |
| To | Stock অথবা Tak-1 |

> **To** = মাল কোন জায়গায় যাবে।  
> কিছু না বদলালে সাধারণত **Stock** এ যায়।  
> Tak-1 তে রাখতে চাইলে **To = Tak-1** বাছুন।

→ Confirm / Save

মূল পেজে Quantity ≈ 50 দেখলে:

→ উপরে **Validate**

স্ট্যাটাস **Done** হলে Receipt শেষ।

---

# ধাপ ৯ — স্টক দেখুন

```
Inventory → Products → Phone Charger 20W
```

**On Hand** ≈ 50

কোন তাকে আছে জানতে: উপরে **On Hand** বাটন চাপুন।  
Location কলামে দেখাবে Stock বা Tak-1।

---

# ধাপ ১০ — Delivery (মাল বের করা)

**গল্প:** সানরাইজ স্টোর ২০ পিস নিল।

```
Inventory → Operations → Delivery Orders → New
```

| Field | মান |
|---|---|
| Deliver To | সানরাইজ স্টোর (আপনার Customer) |

লাইন:

| Field | মান |
|---|---|
| Product | Phone Charger 20W |
| Quantity | 20 |

→ **Mark as Todo** (লাগলে)

→ লাইনের **≡** এ Lot = **CH-001**, Quantity = 20

→ **Validate**

### চেক

Product আবার খুলুন:

```
On Hand ≈ 30   (50 − 20)
```

হলে Delivery ঠিক।

---

# (ঐচ্ছিক) ধাপ ১১ — এক তাক থেকে অন্য তাকে

মাল যদি Tak-1 এ থাকে, ১০ পিস Tak-2 তে সরাতে:

```
Inventory → Operations → Transfers → New
```

| Field | মান |
|---|---|
| From | …/Stock/Tak-1 |
| To | …/Stock/Tak-2 |
| Product | Phone Charger 20W |
| Quantity | 10 |
| Lot | CH-001 |

→ Validate

আগে Product → **On Hand** বাটন দিয়ে দেখুন মাল কোন তাকে আছে — সেটাই **From**।

---

# এক নজরে পুরো ক্রম

```
১  Inventory Install
২  Settings → টিক → Save
৩  Configuration → Warehouses
৪  Configuration → Locations → Stock + Tak-1/Tak-2
৫  Product (By Lots)
৬  ধাপ ৭ক: তাক = Putaway / To ; গুদাম = Operation Type
৭  Receipt → Delivery
```

---

## সমস্যা হলে

| সমস্যা | কী করবেন |
|---|---|
| Inventory মেনু নেই | ধাপ ১ Install |
| Location / Lot অপশন নেই | ধাপ ২ টিক → Save |
| Stock-এর Parent খালি | Parent = আপনার Warehouse (WH) দিন → Save |
| Receive From খালি | Contacts-এ Vendor আগে সেভ আছে তো? |
| Lot চায় / Validate error | ≡ এ Lot + Quantity দিন, তারপর Validate |
| সবসময় Stock এ যায় | ধাপ ৭ক: Putaway দিয়ে তাক সেট করুন, বা ≡ এ To বাছুন |
| অন্য গুদামে পাঠাতে চাই | Receipt → Operation Type = ঐ গুদামের Receipts |

---

এই ফাইল উপর থেকে **ধাপ ১ → ১০** একটার পর একটা করুন।  
আগে Contacts, এখন গুদাম — এতেই রহিম ভাইয়ের ফ্লো সম্পূর্ণ।
