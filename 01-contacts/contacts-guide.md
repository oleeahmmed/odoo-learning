# Contacts — scratch থেকে সম্পূর্ণ সেটআপ
### রহিম ট্রেডার্স: Login → Install → Settings/Configuration → প্রতিটা মেনু → ডেমো ডাটা

---

## গল্প: কে কে?

| কে | কাজ |
|---|---|
| **রহিম** | সেটআপ, Configuration |
| **সালমা** | রোজ Contact New |
| ডেমো দোকান | ঢাকা ইলেকট্রনিক্স, সানরাইজ, নূর টেক |
| ডেমো মানুষ | করিম আহমেদ, জাহিদ হাসান |

```
Demo data ☐ OFF
DB: rahim_contacts
অ্যাপ: Contacts
কোনো Sales/Invoice এখন নয় — শুধু খাতা
```

---

# দিন ১ — Scratch Login (রহিম)

```
http://localhost:8069/web/database/manager
```

| Field | মান |
|---|---|
| Database Name | `rahim_contacts` |
| Email | `admin@rahim.com` |
| Password | `admin123` |
| Country | Bangladesh |
| Demo data | ☐ খালি |

→ Create → Login  
Settings → Companies → **রহিম ট্রেডার্স** → Save  
Preferences → Timezone **Asia/Dhaka** → Save

---

# দিন ১ — Apps Install

```
Apps → Contacts → Install
```

উপরে মেনু:

```
Contacts | Configuration
```

(কোনো Contact এখনো নেই — স্বাভাবিক।)

---

# দিন ১ — Settings (মিনিমাল)

Contacts অ্যাপে আলাদা লম্বা Settings কম। মূল কাজ:

```
Settings → General Settings
```

চেক:

| অপশন | কী করবেন | কেন |
|---|---|---|
| Company নাম/ঠিকানা | রহিম ট্রেডার্স | নিজের কোম্পানি |
| Document layout (ঐচ্ছিক) | পরে | প্রিন্ট |

Contacts এর নিজস্ব সুইচ বেশি নেই — **Configuration মেনুই** মূল সেটআপ।

→ প্রয়োজনমতো Save

---

# দিন ১ — Configuration মেনু একটা একটা (রহিম)

```
Contacts → Configuration
```

---

## ১) Contact Tags

**এখন কেমন:** ট্যাগ লিস্ট খালি।

**কী করবেন:** New

| Name | কেন |
|---|---|
| Vendor | সাপ্লায়ার চিহ্ন |
| Customer | ক্রেতা চিহ্ন |
| VIP | গুরুত্বপূর্ণ |

→ Save

**পরে কোথায় কাজে লাগে:** Contact ফর্মে Tags; লিস্টে Filter।  
**সালমা কী দেখবে:** নাম সেভ করার সময় Tag বাছতে পারবে।

```
মিনিমাল: এই ৩টা Tag যথেষ্ট
```

---

## ২) Contact Titles (যদি মেনু থাকে)

**এখন কেমন:** Mr / Mrs ডিফল্ট থাকতে পারে।

**কী করবেন:** শিখতে ডিফল্টই রাখুন। চাইলে New: `Engineer`।

**কাজে লাগে:** Individual এর Title ঘর।

```
মিনিমাল: কিছু না বদলালেও চলে
```

---

## ৩) Industries (যদি মেনু থাকে)

**এখন কেমন:** খালি বা কিছু ইন্ডাস্ট্রি।

**কী করবেন:** New → `Electronics` → Save

**কাজে লাগে:** Company ফর্মে Industry বাছা; রিপোর্ট/ফিল্টার।

```
মিনিমাল: Electronics একটাই
```

---

## ৪) Localization / অন্যান্য

Country/State Bangladesh থেকেই আসে। Scratch এ আলাদা কিছু লাগে না।

---

# দিন ২ — Contacts মেনু: New (সালমা) — সিরিয়াল

```
নিয়ম: আগে Company দোকান → তারপর Individual মানুষ (Parent = Company)
```

---

## মেনু: Contacts (মূল লিস্ট)

**এখন কেমন:** খালি।

### A) Vendor দোকান

New → ○ **Company**

| Field | ডেমো মান |
|---|---|
| Name | Dhaka Electronics Ltd |
| Street | Elephant Road |
| City | Dhaka |
| Country | Bangladesh |
| Phone | 01711000001 |
| Email | sales@dhakaelec.demo |
| Tags | Vendor |
| Industry | Electronics (থাকলে) |

→ Save

### B) Vendor এর মানুষ

New → ○ **Individual**

| Field | ডেমো মান |
|---|---|
| Name | করিম আহমেদ |
| Title | Mr (ঐচ্ছিক) |
| Job Position | Sales Executive |
| Phone | 01711000002 |
| **Company** | Dhaka Electronics Ltd |
| Email | karim@dhakaelec.demo |

→ Save

```
Parent = Company ঠিক
Company এর Parent = Individual ভুল
```

### C) Customer দোকান

Company → `সানরাইজ স্টোর` | Phone 01811000001 | Tags Customer, VIP → Save  
Company → `নূর টেক শপ` | Phone 01811000002 | Tags Customer → Save

### D) Individual ক্রেতা

Individual → `জাহিদ হাসান` | Phone 01911000001 | Company খালি → Save

**সালমা কী দেখবে:** Contacts লিস্টে ৫টা কার্ড।  
**রহিম কী দেখবে:** ঢাকা ইলেকট্রনিক্স খুলে Contacts ট্যাবে করিম।

---

## Customer / Vendor টিক নেই কেন?

Odoo 17 এ সাধারণত নেই। একই Contact।  
পরে Sales এ ব্যবহার = Customer; Purchase এ = Vendor।

এখন শুধু খাতা ভর্তি — Invoice কাটবেন না।

---

# দিন ৩ — Contacts মেনু অন্য ভিউ

| কাজ | কীভাবে |
|---|---|
| খোঁজা | সার্চ বারে নাম |
| Filter | Tags: VIP |
| Group | Company / Country |
| Kanban / List | উপরের ভিউ আইকন |

Configuration আবার খুলে Tag এডিট করা যায়।

---

# কে কোন মেনু

| মেনু | রহিম | সালমা |
|---|---|---|
| Settings / Company | ✅ | — |
| Configuration → Tags, Titles, Industries | ✅ একবার | Tag ব্যবহার |
| Contacts → New / লিস্ট | চেক | ✅ রোজ |

---

# এক নজরে

```
১  DB rahim_contacts Demo OFF
২  Company + Timezone
৩  Contacts Install
৪  Configuration: Tags (+ Industry)
৫  Company Contacts → Individual Parent
৬  লিস্ট/ফিল্টার চেক
```

পরের পার্ট **Inventory**: নতুন scratch বা এই Contact এক্সপোর্ট নয় — Inventory গাইডে নিজের DB/ধাপ; Vendor-Customer নাম একই ডেমো।
