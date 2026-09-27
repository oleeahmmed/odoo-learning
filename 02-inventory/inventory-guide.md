# Inventory — সহজ গল্পে সম্পূর্ণ সেটআপ
### রহিম ট্রেডার্স: Contacts এর পর গুদাম → পণ্য → মাল ঢোকানো/বের করা

---

## গল্প: অফিসে কে কে?

| কে | ভূমিকা | Odoo তে কী করে |
|---|---|---|
| **রহিম ভাই** | মালিক | Warehouse/Location সেট করে |
| **জাবেদ** | স্টোরকিপার | Receipt, Delivery, তাক দেখে |
| **ঢাকা ইলেকট্রনিক্স** | Vendor | মাল আসে যাদের কাছ থেকে |
| **সানরাইজ স্টোর** | Customer | মাল যায় যাদের কাছে |
| **Phone Charger 20W** | পণ্য | Lot দিয়ে ট্র্যাক |

```
আগে Contacts পার্ট করুন (একই DB rahim_traders চলবে)
Demo data ☐ OFF
অ্যাপ = Inventory (+ Contacts আগেই আছে)
```

---

## সোনার নিয়ম

```
১  Settings টিক → Save আগে
২  Stock এর Parent = WH (খালি রাখবেন না)
৩  তাক বানালেই মাল যায় না — Receipt এ To বাছতে হয়
৪  Lot পণ্যে ≡ তে Lot+Qty — মেইন লাইনে Serial নয়
৫  Product আগে → তারপর Receipt/Delivery
```

---

# দিন ১ — Inventory ইনস্টল + Settings (রহিম)

Contacts আছে ধরে Login (`rahim_traders`)।

```
Apps → Inventory → Install
```

```
Inventory → Configuration → Settings
```

টিক দিন:

```
☑ Storage Locations
☑ Lots & Serial Numbers
☑ Multi-Step Routes
```

→ **Save**

রহিম: “এখন তাক ও লট চালু।”

**জাবেদ পরে কী দেখবে:** Operations মেনুতে Receipts / Delivery।

---

# দিন ১ বিকেল — Warehouse ও Location (রহিম)

## Warehouse

```
Inventory → Configuration → Warehouses
```

একটা warehouse আগেই আছে (রহিম ট্রেডার্স / WH)।  
নাম ঠিক আছে কিনা দেখে Save। শিখতে **একটাই** রাখুন।

## Stock চেক (Parent জরুরি)

```
Configuration → Locations → Stock খুলুন
```

| Field | থাকতে হবে |
|---|---|
| Parent Location | **WH** (নাম `WH/Stock`) |
| Type | Internal Location |

Parent খালি থাকলে Receipt Done হলেও On Hand ০ দেখায় — মুছবেন না।

## তাক বানান — Tak-1, Tak-2

Locations → **New**:

| Field | Tak-1 | Tak-2 |
|---|---|---|
| Name | Tak-1 | Tak-2 |
| Parent | Stock | Stock |
| Type | Internal | Internal |

→ Save

```
Warehouse
 └── Stock
      ├── Tak-1
      └── Tak-2
```

**জাবেদ পরে কী দেখবে:** Receipt এর To তে Tak-1 বাছতে পারবে।

---

# দিন ২ সকাল — Vendor/Customer আছে তো? (জাবেদ চেক)

Contacts থেকে আগে থাকার কথা:

```
☑ Dhaka Electronics Ltd
☑ সানরাইজ স্টোর
```

না থাকলে Contacts পার্টের মতো হাতে বানান।

---

# দিন ২ দুপুর — পণ্য (রহিম + জাবেদ)

```
Inventory → Products → Products → New
```

| Field | ডেমো মান |
|---|---|
| Name | Phone Charger 20W |
| Product Type | **Storable Product** |
| Sales Price | 350 |
| Cost | 220 |
| Can be Sold | ☑ |
| Can be Purchased | ☑ |

ট্যাব **Inventory**:

| Field | মান |
|---|---|
| Tracking | **By Lots** |

→ **Save**

```
By Lots = এক ব্যাচ CH-001 তে অনেক পিস
Unique Serial = প্রতি পিস আলাদা — এখন ব্যবহার করবেন না
```

---

# দিন ২ বিকেল — কোন তাকে যাবে? (রহিম শেখায়)

তাক বানালেই মাল অটো যায় **না**।

| উপায় | কী হয় |
|---|---|
| ডিফল্ট | Receipt → সাধারণত **Stock** এ |
| হাতে | ≡ Detailed Ops এ **To = Tak-1** |
| Putaway Rule | Configuration → Putaway Rules → Product → Store to Tak-1 |

শিখতে Receipt এ **To = Tak-1** হাতে বাছুন।

---

# দিন ৩ — Receipt (জাবেদ): মাল ঢোকানো

**গল্প:** ঢাকা ইলেকট্রনিক্স থেকে ৫০ পিস এল। ব্যাচ CH-001।

```
Inventory → Operations → Receipts → New
```

| Field | মান |
|---|---|
| Receive From | Dhaka Electronics Ltd |
| Product | Phone Charger 20W |
| Demand | 50 |

→ **Mark as Todo**

লাইন ডানে **≡** (Detailed Operations — Forecast এর পরের লিস্ট আইকন; একদম ডানের field hide নয়):

| Field | মান |
|---|---|
| Lot | CH-001 |
| Quantity | 50 |
| To | **Tak-1** |

→ Confirm → উপরে **Validate** → Done

**রহিম কী দেখবে:**  
Products → Phone Charger → On Hand ≈ 50  
On Hand বাটনে Location = Tak-1

```
Confirm/Todo ≠ মাল ঢোকা
Validate = মাল ঢোকা
Serial কলাম ছোঁবেন না
```

---

# দিন ৪ — Delivery (জাবেদ): মাল বের করা

**গল্প:** সানরাইজকে ২০ পিস দিতে হবে।

```
Inventory → Operations → Delivery Orders → New
```

| Field | মান |
|---|---|
| Deliver To | সানরাইজ স্টোর |
| Product | Phone Charger 20W |
| Quantity | 20 |

→ Mark as Todo → ≡ এ Lot **CH-001**, Qty 20 → **Validate**

On Hand ≈ ৩০ (৫০−২০)।

---

# দিন ৫ — তাক থেকে তাকে (ঐচ্ছিক)

মাল Tak-1 এ আছে জেনে (On Hand বাটন):

```
Operations → Transfers → New
From: Tak-1 | To: Tak-2 | Product + Lot | Qty 10 → Validate
```

---

# কে কখন কোন মেনু

| মেনু | রহিম | জাবেদ |
|---|---|---|
| Settings / Locations | একবার সেট | দেখে |
| Products | ট্র্যাকিং সেট | On Hand চেক |
| Receipts / Delivery | দেখে | **প্রতিদিন** |
| Reporting → Locations | চেক | কোন তাকে কত |

---

# এক নজরে

```
১  Inventory Install + Settings টিক
২  Stock Parent=WH | Tak-1, Tak-2
৩  Product By Lots
৪  Receipt → Todo → ≡ Lot+To=Tak-1 → Validate
৫  Delivery → Lot → Validate
৬  On Hand বাটনে তাক দেখা
```

## সমস্যা হলে

| সমস্যা | করণীয় |
|---|---|
| On Hand ০ অথচ Receipt Done | Stock Parent = WH |
| Qty অটো ১ | Serial কলাম ছোঁবেন না; ≡ এ Lot |
| কোন তাকে জানি না | Product → On Hand বাটন |

পরের পার্ট **Sales**: সানরাইজকে Quotation → Delivery অটো।
