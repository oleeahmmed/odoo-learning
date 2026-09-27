# Manufacturing — সহজ গল্পে সম্পূর্ণ সেটআপ
### রহিম ট্রেডার্স: scratch থেকে Login → কাঁচামাল → BOM → তৈরি → স্টক

---

## গল্প: অফিসে কে কে?

| কে | ভূমিকা | Odoo তে কী করে |
|---|---|---|
| **রহিম ভাই** | মালিক | Apps, Settings, BOM সেট করে |
| **রফিক** | প্রোডাকশন কর্মী | Manufacturing Order চালায়, Produce করে |
| **জাবেদ** | স্টোরকিপার | কাঁচামাল Receipt, তৈরি মাল On Hand চেক |
| **ঢাকা ইলেকট্রনিক্স** | Vendor | কাঁচামাল কেনা |
| **সানরাইজ স্টোর** | Customer | পরে তৈরি পণ্য বিক্রি (Sales) |

রহিম ভাই শুধু চার্জার কেনা-বেচা নন — এখন **ছোট অ্যাসেম্বলি** করেন:

```
কাঁচামাল:  USB Cable 1m  +  Plastic Case
তৈরি পণ্য: Charger Combo Pack
```

একটা Combo Pack বানাতে লাগে: কেবল ১টা + কেস ১টা।

আপনি প্রথমে **রহিম (Admin)** হয়ে সেটআপ করবেন।  
পরে **রফিক** রোজ Manufacturing Order চালাবে।

```
Demo data ☐ OFF — সব হাতে বানাব
অ্যাপ = Manufacturing + Inventory (+ Contacts)
আগে Contacts/Inventory জানা থাকলে একই DB চলতে পারে
অথবা নতুন DB: rahim_manufacturing
```

---

## সোনার নিয়ম

```
১  কাঁচামাল Product আগে → স্টক আগে (Receipt)
২  তৈরি পণ্য Product আগে → তারপর BOM
৩  BOM ছাড়া Manufacturing Order পূর্ণ হয় না
৪  কাঁচামাল স্টক না থাকলে Produce আটকে যায়
৫  Produce Done = কাঁচামাল কমে + তৈরি পণ্য বাড়ে
```

---

# দিন ১ সকাল — খাতা খোলা (রহিম)

## ১) Database

নতুন করে শিখতে:

```
http://localhost:8069/web/database/manager
```

| Field | মান |
|---|---|
| Database Name | `rahim_manufacturing` |
| Email | `admin@rahim.com` |
| Password | `admin123` |
| Country | Bangladesh |
| Demo data | ☐ খালি |

Create → Login

(আগের `rahim_traders` চালাতে চাইলে সেখানেই Manufacturing Install করুন — ডেমো নাম একই রাখুন।)

## ২) Company

```
Settings → Companies → রহিম ট্রেডার্স → Save
Preferences → Timezone: Asia/Dhaka → Save
```

রহিম: “এখন কারখানার মডিউল লাগাই।”

---

# দিন ১ দুপুর — Apps ইনস্টল (রহিম)

```
Apps → Contacts → Install
Apps → Inventory → Install
Apps → Manufacturing → Install
```

চাইলে Purchaseও Install — কাঁচামাল Receipt সহজ হয়।

উপরে **Manufacturing** মেনু এলে OK।

```
Inventory → Configuration → Settings
☑ Storage Locations
☑ Lots & Serial Numbers   (ঐচ্ছিক — শিখতে পরে)
→ Save
```

Manufacturing Settings:

```
Manufacturing → Configuration → Settings
```

শিখতে মিনিমাল রাখুন। Work Orders টিক না দিলেও সাধারণ Produce চলে।  
(Work Orders = প্রতি ধাপ আলাদা — পরে।)

→ **Save**

**রফিক পরে কী দেখবে:** Manufacturing → Operations → Manufacturing Orders।

---

# দিন ১ বিকেল — Contact + Warehouse চেক (রহিম)

## Vendor

```
Contacts → New → Company
Name: Dhaka Electronics Ltd → Save
```

## Location

```
Inventory → Configuration → Locations
```

Stock এর Parent = **WH** আছে কিনা দেখুন (`WH/Stock`)।  
চাইলে Tak-1 বানান (Parent = Stock) — তৈরি মাল সেখানে রাখতে।

---

# দিন ২ সকাল — তিনটা পণ্য বানান (রহিম) — আগে Product, পরে BOM

রহিম: “BOM এ পণ্য বাছব — আগে পণ্য না থাকলে বাছব কী?”

```
Inventory → Products → Products → New
```

---

## পণ্য A — কাঁচামাল: USB Cable 1m

| Field | ডেমো মান |
|---|---|
| Name | USB Cable 1m |
| Product Type | **Storable Product** |
| Cost | 70 |
| Sales Price | 120 |
| Can be Purchased | ☑ |
| Can be Sold | ☑ (আলাদাও বেচতে পারেন) |

→ **Save**

---

## পণ্য B — কাঁচামাল: Plastic Case

| Field | ডেমো মান |
|---|---|
| Name | Plastic Case |
| Product Type | **Storable Product** |
| Cost | 40 |
| Can be Purchased | ☑ |
| Can be Sold | ☐ |

→ **Save**

---

## পণ্য C — তৈরি জিনিস: Charger Combo Pack

| Field | ডেমো মান |
|---|---|
| Name | Charger Combo Pack |
| Product Type | **Storable Product** |
| Sales Price | 450 |
| Cost | 110 (আন্দাজ) |
| Can be Sold | ☑ |
| Can be Purchased | ☐ |

→ **Save**

Inventory ট্যাবে Routes থাকলে **Manufacture** ☑ করতে হতে পারে (Settings অনুযায়ী)।  
না দেখলে পরে BOM Save করলেও অনেক সময় চলে।

```
এখনও BOM নেই — শুধু তিনটা Product কার্ড
স্টকও এখনো ০
```

---

# দিন ২ দুপুর — কাঁচামাল স্টকে আনুন (জাবেদ)

রফিক Produce করতে গেলে স্টক না থাকলে আটকে যাবে। তাই আগে মাল ঢোকান।

```
Inventory → Operations → Receipts → New
```

| Field | মান |
|---|---|
| Receive From | Dhaka Electronics Ltd |

দুই লাইন:

| Product | Demand |
|---|---|
| USB Cable 1m | 100 |
| Plastic Case | 100 |

→ Mark as Todo → Quantity ভরুন → **Validate**

**জাবেদ কী দেখবে:** দুটো পণ্যে On Hand ≈ 100।  
**Charger Combo Pack** এখনো ০ — এখনো তৈরি হয়নি।

```
Receipt আগে → তারপর Manufacturing
```

---

# দিন ২ বিকেল — BOM বানান (রহিম) — Product এর পর

```
Manufacturing → Products → Bills of Materials → New
```

অথবা: Charger Combo Pack খুলে → Bill of Materials।

| Field | ডেমো মান |
|---|---|
| Product | **Charger Combo Pack** |
| Quantity | 1.00 (১টা প্যাক বানাতে) |
| BOM Type | Manufacture this product |

Components ট্যাব / লাইন:

| Component | Quantity |
|---|---|
| USB Cable 1m | 1 |
| Plastic Case | 1 |

→ **Save**

**গল্প:** “১টা Combo Pack = ১ কেবল + ১ কেস।”

**রফিক পরে কী দেখবে:** Manufacturing Order এ এই BOM অটো লাগবে।

```
BOM না বানিয়ে MO তে গেলে কম্পোনেন্ট খালি/error
সিরিয়াল: Product → স্টক → BOM → তারপর Order
```

---

# দিন ৩ — Manufacturing Order (রফিক)

**গল্প:** সানরাইজকে ১০টা Combo Pack লাগবে — আগে তৈরি করি।

```
Manufacturing → Operations → Manufacturing Orders → New
```

| Field | ডেমো মান |
|---|---|
| Product | Charger Combo Pack |
| Quantity | 10 |

BOM অটো আসবে (একটাই থাকলে)।  
Components এ দেখাবে: Cable 10 + Case 10।

→ **Confirm**

স্ট্যাটাস Confirmed / Waiting ইত্যাদি।

→ **Check Availability** (বাটন থাকলে)

কাঁচামাল যথেষ্ট থাকলে Ready / Available।

**রফিক কী দেখবে:** কোন কম্পোনেন্ট কম আছে কিনা লাল/সতর্কবার্তা।  
কম থাকলে জাবেদকে আবার Receipt করতে বলবেন — Produce আগে নয়।

---

# দিন ৩ বিকেল — Produce / Mark as Done (রফিক)

MO খোলা:

→ **Produce** অথবা **Mark as Done** / Validate (স্ক্রিনে যে বাটন)

Qty 10 ঠিক আছে কিনা দেখে Confirm।

### Done এর পর কোথায় কী বদলায়

| জায়গা | আগে | পরে |
|---|---|---|
| USB Cable On Hand | 100 | ≈ 90 |
| Plastic Case On Hand | 100 | ≈ 90 |
| Charger Combo Pack On Hand | 0 | ≈ **10** |
| Manufacturing Order | In Progress | **Done** |

**রহিম কী দেখবে:** Products এ Combo Pack স্টক এসেছে — এখন Sales এ বিক্রি করা যায়।

```
Produce Done = কারখানা শেষ
কাঁচামাল কমে + তৈরি পণ্য বাড়ে
```

---

# দিন ৪ — তৈরি পণ্য বিক্রি (ঐচ্ছিক লিংক)

স্টক আছে বলে সালমা Sales এ:

```
Quotation → Customer সানরাইজ → Product Charger Combo Pack Qty 5 → Confirm → Delivery
```

Inventory পার্টের মতো Delivery Validate।

এটা Manufacturing এর পরের ধাপ — শুধু বোঝার জন্য।

---

# দিন ৫ — রহিম রিপোর্ট / চেক

```
Inventory → Reporting → Stock
Manufacturing → Reporting (থাকলে)
```

রহিম: “কত Combo বানালাম, কাঁচামাল কত কমল — সব খাতায় আছে।”

---

# কে কখন কোন মেনু

| মেনু | রহিম | রফিক | জাবেদ |
|---|---|---|---|
| Apps / Settings / BOM | ✅ একবার | — | — |
| Products (কাঁচামাল/তৈরি) | ✅ | দেখে | On Hand |
| Receipts | — | — | ✅ কাঁচামাল |
| Manufacturing Orders | দেখে | ✅ Confirm + Produce | — |
| Sales (তৈরি পণ্য) | — | — | Delivery |

---

# ডেমো চরিত্র ও পণ্য — এক নজরে

| নাম | ধরন |
|---|---|
| Dhaka Electronics Ltd | Vendor |
| USB Cable 1m | কাঁচামাল |
| Plastic Case | কাঁচামাল |
| Charger Combo Pack | তৈরি পণ্য |
| BOM | 1 Pack = 1 Cable + 1 Case |
| MO Qty | 10 |

---

# এক নজরে পুরো রাস্তা

```
১  DB + Company (Demo OFF)
২  Contacts + Inventory + Manufacturing Install
৩  Vendor লেখা
৪  Product: Cable, Case, Combo Pack
৫  Receipt: Cable 100 + Case 100 → Validate
৬  BOM: Combo = Cable 1 + Case 1
৭  MO: Combo Qty 10 → Confirm → Check Availability
৮  Produce / Done
৯  On Hand: Combo +10, কাঁচামাল −10 করে
১০ (ঐচ্ছিক) Sales এ Combo বিক্রি
```

---

## সমস্যা হলে

| সমস্যা | কী করবেন |
|---|---|
| Manufacturing মেনু নেই | Apps → Install |
| BOM এ Product খালি | আগে Product Save |
| Components খালি | BOM এ লাইন যোগ |
| Cannot produce / স্টক নেই | আগে Receipt দিয়ে কাঁচামাল আনুন |
| Combo On Hand ০ অথচ Done | Product Type Storable তো? Location Internal তো? |
| Work Orders জটিল | Settings এ Work Orders টিক খুলে সাধারণ Produce করুন |

---

## Accounting এর মতো মনে রাখুন

```
Accounting: Account আগে → Product এ সেট
Manufacturing: Product + স্টক আগে → BOM → তারপর MO
কোনো কিছু না বানিয়ে সামনের ধাপে যাবেন না
```

এই ফাইল উপর থেকে দিন ১ → ৫।  
রহিম সেটআপ, জাবেদ কাঁচামাল, রফিক তৈরি — তাহলে Manufacturing পরিষ্কার।
