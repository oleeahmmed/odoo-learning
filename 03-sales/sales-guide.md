# Sales — scratch থেকে সম্পূর্ণ সেটআপ
### রহিম ট্রেডার্স: Login → Install → Settings → Configuration → Orders মেনু ধরে

---

## কে কে?

| কে | কাজ |
|---|---|
| **রহিম** | Settings / Configuration |
| **সালমা** | Quotation / Confirm / Invoice |
| **জাবেদ** | Delivery Validate |
| Customer | সানরাইজ স্টোর |
| Product | Phone Charger 20W |

```
Demo ☐ OFF | DB: rahim_sales
Apps: Contacts + Inventory + Sales
স্টক ছাড়া Delivery আটকাবে — এই গাইডে ছোট Receiptও আছে
```

---

# দিন ১ — Scratch Login + Install

| Field | মান |
|---|---|
| Database Name | `rahim_sales` |
| Email | `admin@rahim.com` |
| Password | `admin123` |
| Country | Bangladesh |
| Demo data | ☐ |

Company রহিম ট্রেডার্স + Timezone Asia/Dhaka

```
Apps → Contacts → Inventory → Sales → Install
```

Inventory Settings মিনিমাল:

```
☑ Storage Locations | ☑ Lots & Serial Numbers → Save
```

মেনু Sales:

```
Orders | To Invoice | Products | Reporting | Configuration
```

---

# দিন ১ — Sales Settings (রহিম) প্রতিটা বুঝে মিনিমাল

```
Sales → Configuration → Settings
```

| অপশন | মিনিমাল | টিক দিলে কী হয় / কোথায় |
|---|---|---|
| Variants | ☐ | Product এ Attribute |
| Discounts | ☑ চাইলে | SO লাইনে Disc% |
| Lock Confirmed Sales | ☐ শিখতে | Confirm পর এডিট বন্ধ |
| Quotation Templates | ☐ | টেমপ্লেট মেনু |
| Online Signature / Payment | ☐ | কাস্টমার পোর্টাল |
| Delivery Methods | ☐ | শিপিং মেথড |
| Coupons / Loyalty | ☐ | প্রমো |
| Margins | ☐ | মার্জিন কলাম |
| Pricelists | ☐ | একাধিক দাম তালিকা |
| Customer Addresses | ☑ ডিফল্ট | Invoice/Delivery ঠিকানা |

**মিনিমাল Save:**

```
☐ Lock Confirmed Sales
☑ Discounts (ঐচ্ছিক)
বাকি জটিল ☐
→ Save
```

Product Invoicing Policy (পণ্যে):

```
Ordered quantities = অর্ডারের বিল
Delivered quantities = ডেলিভারি যত তত বিল (ফিজিক্যালে ভালো)
```

Charger এ পরে **Delivered quantities** সেট করুন।

---

# দিন ১ — Configuration মেনু ধরে

```
Sales → Configuration
```

| মেনু | এখন কী করবেন | পরে কোথায় লাগে |
|---|---|---|
| Settings | উপরেই | — |
| Sales Teams | New: `রহিম সেলস` (ঐচ্ছিক) | SO এ Team |
| Activity Types | ডিফল্ট | ফলোআপ |
| Tags | New: Hot Deal (ঐচ্ছিক) | SO ট্যাগ |
| Payment Terms | New: 15 Days | SO/Invoice Due |
| Quotation Templates | স্কিপ | — |
| Product Categories | স্কিপ | — |

**মিনিমাল:** Payment Terms `15 Days` একটা বানান → Save

---

# দিন ২ — মাস্টার (Scratch হাতে)

**Contacts:** সানরাইজ স্টোর (Company), Dhaka Electronics Ltd  

**Product:** Phone Charger 20W | Storable | By Lots | Price 350 | Invoicing Policy Delivered  

**স্টক:** Inventory → Receipt → Vendor ঢাকা… → Qty 50 Lot CH-001 → Validate  

```
Customer + Product + Stock আগে → তারপর Quotation
```

---

# দিন ২ — Orders মেনু ধরে

## Quotations

New → Customer সানরাইজ → Product Qty 10 → Payment Terms 15 Days → Save  

## Confirm

Confirm → Sales Order  
**অটো Delivery** তৈরি — মাল এখনো বের হয়নি।

## Orders (Sales Orders লিস্ট)

Confirmed অর্ডার এখানে।

---

# দিন ৩ — Delivery (জাবেদ) + To Invoice

SO → Delivery → Todo → ≡ Lot CH-001 Qty 10 → Validate  

## To Invoice মেনু

বিল কাটার অপেক্ষা লিস্ট।  
SO → Create Invoice → **Regular invoice** (Down payment %/fixed এখন নয়) → Confirm  

Register Payment ঐচ্ছিক।

```
Regular = সাধারণ বিল
Down payment = অগ্রিম — শিখতে পরে
```

---

# দিন ৩ — Products / Reporting মেনু

Products = একই পণ্য তালিকা।  
Reporting = Sales analysis (ডাটা থাকলে)।

---

# Customers vs Contacts

একই `res.partner`। Sales Customer ঘর = Contacts এর নাম।

---

# কে কোন মেনু

| মেনু | রহিম | সালমা | জাবেদ |
|---|---|---|---|
| Settings / Payment Terms | ✅ | — | — |
| Quotations / Orders | — | ✅ | — |
| Delivery | — | — | ✅ |
| Create Invoice | — | ✅ | — |

---

# এক নজরে

```
১  DB + Contacts + Inventory + Sales
২  Sales Settings মিনিমাল Save
৩  Payment Terms 15 Days
৪  Customer + Product + Receipt স্টক
৫  Quotation → Confirm → Delivery → Regular Invoice
```

পরের পার্ট **Purchase** scratch।
