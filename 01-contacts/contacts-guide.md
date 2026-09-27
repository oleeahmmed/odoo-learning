# Contacts — সহজ গল্পে সম্পূর্ণ সেটআপ
### রহিম ট্রেডার্স: scratch থেকে Login → মানুষ ও দোকানের খাতা

---

## গল্প: অফিসে কে কে?

| কে | ভূমিকা | Odoo তে কী করে |
|---|---|---|
| **রহিম ভাই** | মালিক | Contact খাতা সাজায়, চেক করে |
| **সালমা** | অফিস সহকারী | নতুন নাম–ফোন লেখে |
| **ঢাকা ইলেকট্রনিক্স** | Vendor (দোকান) | যাদের কাছ থেকে মাল কেনা |
| **করিম আহমেদ** | Vendor এর মানুষ | ঢাকা ইলেকট্রনিক্সের সেলসম্যান |
| **সানরাইজ স্টোর** | Customer (দোকান) | যাদেরকে বিক্রি |
| **নূর টেক শপ** | Customer (দোকান) | ছোট খুচরা ক্রেতা |
| **জাহিদ হাসান** | Individual | ব্যক্তিগত ক্রেতা (ফোন নম্বর) |

আপনি প্রথমে **রহিম (Admin)** হয়ে সেটআপ করবেন।  
পরে সালমা রোজ নতুন Contact লিখবে।

```
Demo data ☐ OFF — সব হাতে বানাব
এই পার্টের অ্যাপ = Contacts
Customer/Vendor টিক লাগে না — পরে Sales/Purchase এ ব্যবহার করলেই হয়
```

---

## সোনার নিয়ম

```
১  আগে Company না Individual ঠিক করো
২  দোকান = Company | মানুষ = Individual
৩  মানুষের Parent = তার দোকান/কোম্পানি (উল্টো নয়)
৪  Contacts এ শুধু নাম–ঠিকানা–ফোন — এখন Invoice কাটবেন না
```

---

# দিন ১ সকাল — খাতা খোলা (রহিম)

## ১) নতুন Database

```
http://localhost:8069/web/database/manager
```

| Field | মান |
|---|---|
| Database Name | `rahim_traders` |
| Email | `admin@rahim.com` |
| Password | `admin123` |
| Country | Bangladesh |
| Demo data | ☐ খালি |

Create → Login (`admin@rahim.com` / `admin123`)

## ২) দোকানের নাম

```
Settings → Companies → নাম: রহিম ট্রেডার্স → Country: Bangladesh → Save
উপরের নাম → Preferences → Timezone: Asia/Dhaka → Save
```

রহিম: “খাতা খালি — আগে মানুষের তালিকা লাগাই।”

---

# দিন ১ দুপুর — Contacts ইনস্টল (রহিম)

```
Apps → Contacts → Install
```

উপরে **Contacts** মেনু এলে OK।

**সালমা পরে কী দেখবে:** Contacts অ্যাপ — New চাপলে ফর্ম।

---

# দিন ১ বিকেল — Individual vs Company বোঝা (রহিম সালমাকে শেখায়)

```
Contacts → New
```

উপরে দুটো অপশন:

```
○ Individual     ○ Company
```

| বাছা | মানে | উদাহরণ |
|---|---|---|
| **Company** | দোকান / কোম্পানি / অফিস | ঢাকা ইলেকট্রনিক্স, সানরাইজ স্টোর |
| **Individual** | একজন মানুষ | করিম আহমেদ, জাহিদ হাসান |

রহিম: “সাইনবোর্ড আছে = Company। শুধু মানুষের নাম = Individual।”

---

# দিন ২ সকাল — Vendor দোকান লেখা (সালমা)

**গল্প:** মাল আসে ঢাকা ইলেকট্রনিক্স থেকে — আগে দোকানটা লিখি।

```
Contacts → New → ○ Company
```

| Field | ডেমো মান |
|---|---|
| Name | Dhaka Electronics Ltd |
| Street | Elephant Road |
| City | Dhaka |
| Country | Bangladesh |
| Phone | 01711000001 |
| Email | sales@dhakaelec.demo |

→ **Save**

**রহিম কী দেখবে:** Contacts লিস্টে একটা কোম্পানি কার্ড।  
**পরে Purchase এ কী হবে:** Vendor বাছলে এই নাম আসবে।

এখনো “Is Vendor” টিক নেই — স্বাভাবিক। কেনা শুরু করলেই Vendor হয়ে যাবে।

---

# দিন ২ দুপুর — Vendor এর মানুষ (সালমা)

**গল্প:** ঢাকা ইলেকট্রনিক্সে ফোন ধরে **করিম আহমেদ**। দোকানের ভিতরের মানুষ।

```
Contacts → New → ○ Individual
```

| Field | ডেমো মান |
|---|---|
| Name | করিম আহমেদ |
| Job Position | Sales Executive |
| Phone | 01711000002 |
| Email | karim@dhakaelec.demo |
| **Company** (Parent) | **Dhaka Electronics Ltd** |

→ **Save**

```
ঠিক:  Individual → Parent = Company
ভুল:  Company এর Parent = Individual  (লুপ/error হতে পারে)
```

ঢাকা ইলেকট্রনিক্স খুলে **Contacts** ট্যাবে করিম দেখা যাবে।

**সালমা পরে কী দেখবে:** দোকানের নিচে মানুষ জোড়া।

---

# দিন ২ বিকেল — Customer দোকান (সালমা)

**গল্প:** যাদেরকে বিক্রি করি — সানরাইজ ও নূর টেক।

### সানরাইজ স্টোর

```
Contacts → New → ○ Company
```

| Field | ডেমো মান |
|---|---|
| Name | সানরাইজ স্টোর |
| City | Dhaka |
| Country | Bangladesh |
| Phone | 01811000001 |
| Email | buy@sunrise.demo |

→ **Save**

### নূর টেক শপ

একইভাবে Company → Name `নূর টেক শপ` → Phone `01811000002` → Save

**পরে Sales এ কী হবে:** Customer ফিল্ডে এই নাম বাছা যাবে।

---

# দিন ৩ সকাল — Individual ক্রেতা (সালমা)

**গল্প:** জাহিদ ভাই নিজে এসে চার্জার কিনেন — দোকান নয়, মানুষ।

```
Contacts → New → ○ Individual
```

| Field | ডেমো মান |
|---|---|
| Name | জাহিদ হাসান |
| Phone | 01911000001 |
| City | Dhaka |
| Country | Bangladesh |
| Company / Parent | **খালি** (কোনো দোকানের কর্মী নয়) |

→ **Save**

পরে Sales এ Customer হিসেবে জাহিদও বাছা যায়।

---

# দিন ৩ দুপুর — ঠিকানা ও ট্যাগ (রহিম + সালমা)

## আরেক ঠিকানা (ঐচ্ছিক)

সানরাইজ খুলুন → Contacts / Addresses এ Invoice বা Delivery ঠিকানা যোগ করা যায়।  
শিখতে এখন এক ঠিকানাই যথেষ্ট।

## Tag

```
Contacts → Configuration → Contact Tags → New
```

| Name | রং (ঐচ্ছিক) |
|---|---|
| VIP | যেকোনো |
| Vendor | |
| Local Shop | |

সানরাইজ এ Tag = VIP → Save  
ঢাকা ইলেকট্রনিক্স এ Tag = Vendor → Save

**লাভ:** লিস্টে ফিল্টার করে VIP কাস্টমার খুঁজে পাওয়া।

---

# দিন ৩ বিকেল — Customer/Vendor টিক নেই কেন? (রহিম ব্যাখ্যা)

সালমা জিজ্ঞেস: “ভাই, Is Customer টিক কোথায়?”

রহিম: “Odoo 17 এ সাধারণত নেই। একটাই Contact খাতা।”

```
Contacts     = নাম লেখা
Sales        = সেই নামে বিক্রি → Customer হয়ে যায়
Purchase     = সেই নামে কেনা → Vendor হয়ে যায়
একই দোকান দুটোই হতে পারে
```

তাই এখন শুধু নাম ঠিকমতো লিখুন। Sales/Purchase পার্টে ব্যবহার করবেন।

---

# দিন ৪ — কে কখন কী দেখে

| কাজ | কে করে | মেনু |
|---|---|---|
| Apps Install, Company | রহিম | Apps / Settings |
| নতুন দোকান/মানুষ লেখা | সালমা | Contacts → New |
| Parent ঠিক আছে কিনা | রহিম চেক | Company খুলে Contacts ট্যাব |
| Tag / Configuration | রহিম একবার | Configuration → Tags |
| পরে Invoice/PO | হিসাব/কিনা-বেচা কর্মী | Sales / Purchase (এই পার্টে নয়) |

---

# ডেমো চরিত্র — এক নজরে

| নাম | Type | Parent | পরে ব্যবহার |
|---|---|---|---|
| Dhaka Electronics Ltd | Company | — | Purchase Vendor |
| করিম আহমেদ | Individual | Dhaka Electronics Ltd | যোগাযোগ |
| সানরাইজ স্টোর | Company | — | Sales Customer |
| নূর টেক শপ | Company | — | Sales Customer |
| জাহিদ হাসান | Individual | — | Sales Customer |

---

# এক নজরে পুরো রাস্তা

```
১  DB rahim_traders — Demo OFF
২  Company রহিম ট্রেডার্স + Timezone
৩  Contacts Install
৪  Company: ঢাকা ইলেকট্রনিক্স
৫  Individual: করিম (Parent = ঢাকা…)
৬  Company: সানরাইজ, নূর টেক
৭  Individual: জাহিদ
৮  Tags (ঐচ্ছিক)
৯  লিস্টে সব নাম দেখা যাচ্ছে কিনা চেক
```

---

## সমস্যা হলে

| সমস্যা | কী করবেন |
|---|---|
| Contacts মেনু নেই | Apps → Install |
| Customer টিক নেই | স্বাভাবিক — পরে Sales এ ব্যবহার |
| Recursive hierarchy error | Individual এর Parent শুধু Company; উল্টো নয় |
| Demo তে অনেক নাম | Demo OFF দিয়ে নতুন DB |

---

পরের পার্ট **Inventory**: এই Contactগুলো রেখে গুদাম–পণ্য–Receipt।  
একই গল্প, একই database চালিয়ে যেতে পারেন (বা নতুন DB — গাইডে বলা থাকবে)।
