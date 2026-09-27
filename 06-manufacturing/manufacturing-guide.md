# Manufacturing — scratch থেকে সম্পূর্ণ সেটআপ
### রহিম ট্রেডার্স: Login → Install → Settings → Configuration → BOM → MO

---

## কে কে?

| কে | কাজ |
|---|---|
| **রহিম** | Settings + BOM |
| **রফিক** | Manufacturing Orders + Produce |
| **জাবেদ** | কাঁচামাল Receipt |
| Vendor | Dhaka Electronics Ltd |
| কাঁচামাল | USB Cable 1m, Plastic Case |
| তৈরি | Charger Combo Pack |

```
Demo ☐ OFF | DB: rahim_manufacturing
Apps: Contacts + Inventory + Manufacturing + Purchase
```

---

# দিন ১ — Scratch Login + Install

| Database | `rahim_manufacturing` | Demo ☐ |
| Email | admin@rahim.com / admin123 | Country BD |

Company রহিম ট্রেডার্স + Timezone

```
Apps → Contacts → Inventory → Purchase → Manufacturing → Install
```

Inventory Settings মিনিমাল: ☑ Storage Locations → Save  
Stock Parent = WH

মেনু Manufacturing:

```
Operations | Planning | Products | Reporting | Configuration
```

---

# দিন ১ — Manufacturing Settings (রহিম)

```
Manufacturing → Configuration → Settings
```

| অপশন | মিনিমাল | টিক দিলে কী / কোথায় |
|---|---|---|
| Work Orders | ☐ | প্রতি ধাপ আলাদা WO — শিখতে জটিল |
| Work Order Dependencies | ☐ | WO ক্রম |
| By-Products | ☐ | উপজাত পণ্য |
| Quality Control | ☐ (Enterprise/অ্যাডঅন হতে পারে) | কোয়ালিটি চেক |
| Subcontracting | ☐ | বাইরে বানানো |
| Master Production Schedule | ☐ | প্ল্যানিং স্ক্রিন |
| Security Lead Time | 0 | অগ্রিম দিন |

**মিনিমাল:** সব ☐ → **Save** (সাধারণ Produce যথেষ্ট)

Work Orders ☑ করলে: Configuration এ Work Centers লাগে — এখন স্কিপ।

---

# দিন ১ — Configuration মেনু ধরে

| মেনু | কী করবেন |
|---|---|
| Settings | উপরে মিনিমাল |
| Work Centers | Work Orders চালু না হলে স্কিপ |
| Operations | ডিফল্ট; এডিট নয় |
| Bill of Materials | পরে Product এর পর বানাব |
| Unbuild Orders | স্কিপ (ভাঙা) |
| Scrap | স্কিপ |

---

# দিন ২ — Products মেনু (আগে Product, পরে BOM)

| Product | Type | Cost | Sold/Purchased |
|---|---|---|---|
| USB Cable 1m | Storable | 70 | Purchased ☑ |
| Plastic Case | Storable | 40 | Purchased ☑ |
| Charger Combo Pack | Storable | 110 | Sold ☑ Purchased ☐ |

Routes এ Manufacture থাকলে Combo তে ☑।

---

# দিন ২ — কাঁচামাল স্টক (জাবেদ) আগে

Purchase/Receipt: Cable 100 + Case 100 → Validate  

```
স্টক আগে → BOM/MO পরে
```

---

# দিন ২ — BOM (Products / Configuration)

```
Manufacturing → Products → Bills of Materials → New
```

| Field | মান |
|---|---|
| Product | Charger Combo Pack |
| Qty | 1 |
| Components | Cable 1 + Case 1 |

→ Save

---

# দিন ৩ — Operations মেনু

## Manufacturing Orders

New → Combo Qty 10 → Confirm → Check Availability → Produce/Done  

**পরে:** Combo On Hand +10; কাঁচামাল −10 করে।

## Planning / Unbuild / Scrap

শিখতে পরে।

---

# Reporting মেনু

Production analysis — ডাটা থাকলে।

---

# এক নজরে

```
১  DB + Apps
২  MFG Settings সব ☐ Save (মিনিমাল)
৩  Product তিনটা
৪  Receipt কাঁচামাল
৫  BOM
৬  MO → Done
```
