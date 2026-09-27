# CRM — সহজ গল্পে সম্পূর্ণ সেটআপ
### রহিম ট্রেডার্স: scratch থেকে Login → Lead → Opportunity → বিক্রি

---

## গল্প: অফিসে কে কে?

| কে | ভূমিকা | Odoo তে কী করে |
|---|---|---|
| **রহিম ভাই** | মালিক | CRM Settings, পাইপলাইন দেখে |
| **সালমা** | সেলস | Lead/Opportunity চালায়, Quotation দেয় |
| **জাবেদ** | স্টোর | পরে Delivery (Sales/Inventory) |
| **সানরাইজ স্টোর** | সম্ভাব্য/পুরনো Customer | যার সাথে ডিল |
| **নূর টেক শপ** | নতুন Lead | ফোন থেকে আসা আগ্রহ |
| **জাহিদ হাসান** | Individual Lead | ব্যক্তিগত ক্রেতার আগ্রহ |

রহিম ভাই বললেন:

> “ফোনে অনেকে দাম জিজ্ঞেস করে। কেউ কিনে, কেউ হারিয়ে যায়।  
> কোনটা গরম ডিল, কোনটা ঠান্ডা — খাতায় রাখতে হবে।”

CRM = সেই খাতা।

```
Lead        = নতুন আগ্রহ (এখনো নিশ্চিত Customer নয়)
Opportunity = গরম ডিল — পাইপলাইনে ধাপে ধাপে
Won         = জিতেছি → Sales Quotation
Lost        = হেরেছি — কারণ লিখে রাখি
```

আপনি প্রথমে **রহিম (Admin)** হয়ে সেটআপ করবেন।  
পরে **সালমা** রোজ Lead/Opportunity চালাবে।

```
Demo data ☐ OFF — সব হাতে বানাব
অ্যাপ = CRM (+ Contacts; Sales থাকলে Quotation সহজ)
DB: rahim_crm অথবা আগের rahim_traders এ CRM Install
```

---

## সোনার নিয়ম

```
১  Contact/Customer আগে না থাকলেও Lead এ নাম লেখা যায়
২  Lead → Opportunity করতে গেলে Contact তৈরি/লিংক হয়
৩  Opportunity Won এর আগেই অনেক সময় Quotation — সিরিয়াল বুঝে চলুন
৪  Pipeline স্টেজ এড়িয়ে সরাসরি Won করবেন না (শেখায় ধাপে ধাপে সরান)
৫  Lost হলে কারণ লিখুন — পরে রিপোর্টে কাজে লাগে
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
| Database Name | `rahim_crm` |
| Email | `admin@rahim.com` |
| Password | `admin123` |
| Country | Bangladesh |
| Demo data | ☐ খালি |

Create → Login (`admin@rahim.com` / `admin123`)

## ২) Company

```
Settings → Companies → রহিম ট্রেডার্স → Save
Preferences → Timezone: Asia/Dhaka → Save
```

রহিম: “এখন বিক্রির পাইপলাইন লাগাই।”

---

# দিন ১ দুপুর — Apps ইনস্টল (রহিম)

```
Apps → Contacts → Install
Apps → CRM → Install
Apps → Sales → Install
```

(Sales না থাকলেও CRM চলে; Quotation বাটন পেতে Sales ভালো।)

উপরে **CRM** মেনু এলে OK।

মেনু মোটামুটি:

```
Sales | Leads | Reporting | Configuration
```

(ভার্সন অনুযায়ী নাম একটু আলাদা হতে পারে — Pipeline / My Pipeline।)

**সালমা পরে কী দেখবে:** CRM → Pipeline / Leads।

---

# দিন ১ বিকেল — Settings মিনিমাল (রহিম)

```
CRM → Configuration → Settings
```

শিখতে বেশি টিক নয়। দরকার হলে:

```
☑ Leads   (Lead আলাদা মেনু চালু — থাকলে)
```

→ **Save**

রহিম Pipeline খুলে ডিফল্ট স্টেজ দেখলেন — যেমন:

```
New → Qualified → Proposition → Won
```

(নাম একটু ভিন্ন হতে পারে। এখন বদলাবেন না — শিখে নিন।)

**সালমা পরে কী দেখবে:** কার্ড ডানদিকে সরিয়ে স্টেজ বদলানো।

---

# দিন ২ সকাল — Contact আগে? (রহিম চেক)

পুরনো কাস্টমার থাকলে Contacts এ থাকবে। Scratch হলে:

```
Contacts → New → Company → সানরাইজ স্টোর → Save
```

নতুন ফোন আসা মানুষের জন্য এখন Contact বাধ্য নয় — Lead এ সরাসরি নাম লেখা যায়।  
সুযোগ পাকা হলে Contact বানানো/লিংক হবে।

```
নতুন অচেনা ফোন = আগে Lead
পুরনো সানরাইজ = Contact আছে → সরাসরি Opportunityও করা যায়
```

---

# দিন ২ দুপুর — প্রথম Lead (সালমা)

**গল্প:** অচেনা নম্বর — নূর টেক শপ চার্জার কম্বো নিয়ে দাম জিজ্ঞেস করল।

```
CRM → Leads → New
```

(Leads মেনু না থাকলে Pipeline এ New → ধরন Lead।)

| Field | ডেমো মান |
|---|---|
| Lead / Opportunity Name | নূর টেক — Combo Pack আগ্রহ |
| Contact Name / Customer | নূর টেক শপ |
| Phone | 01811000002 |
| Email | nur@tech.demo |
| Expected Revenue | 4500 |
| Salesperson | সালমা (বা Admin) |

→ **Save**

**রহিম কী দেখবে:** Leads লিস্টে নতুন কার্ড।  
এখনো Pipeline এর Won নয় — শুধু আগ্রহ।

```
Lead Save ≠ বিক্রি হয়ে গেছে
শুধু খাতায় নাম উঠল
```

---

# দিন ২ বিকেল — Lead → Opportunity (সালমা)

সালমা ফোন করে বুঝল: সত্যিই কিনবে, Qty প্রায় ১০।

Lead খুলে → **Convert to Opportunity** / **New Opportunity**

| Field | মান |
|---|---|
| Customer | নূর টেক শপ (নতুন Contact তৈরি হতে পারে) |
| Expected Revenue | 4500 |

→ Create / Confirm

এখন কার্ড **Pipeline** এ — স্টেজ সাধারণত New / Qualified।

**Contact এ কী হয়:** নূর টেক শপ Contact হিসেবে সেভ হয়ে যেতে পারে।  
পরে Sales এ Customer বাছা যাবে।

```
সিরিয়াল: Lead (আগ্রহ) → Opportunity (ডিল চলছে)
Contact আগে না থাকলে এখানেই তৈরি হতে পারে
```

---

# দিন ৩ সকাল — Pipeline এ সরানো (সালমা)

সালমা কার্ড ধরে ডানে সরায়:

```
New → Qualified → Proposition
```

প্রতি স্টেজে নোট লিখতে পারে:

```
Qualified: বাজেট আছে, ১০ প্যাক চাই
Proposition: দাম ৪৫০ × ১০ অফার দিয়েছি
```

**রহিম কী দেখবে:** Pipeline বোর্ডে কতগুলো ডিল কোন ধাপে।

অতিরিক্ত ডেমো Opportunity (সরাসরি):

```
CRM → Pipeline → New
```

| Field | মান |
|---|---|
| Name | সানরাইজ — Combo Pack ২০ পিস |
| Customer | সানরাইজ স্টোর |
| Expected Revenue | 9000 |

→ Save → স্টেজ সরান।

---

# দিন ৩ দুপুর — Quotation Opportunity থেকে (সালমা)

Proposition স্টেজে নূর টেক কার্ড খুলুন।

→ **New Quotation** (বাটন থাকলে)

Sales Quotation খুলবে — Customer/Product ভরুন:

| Field | মান |
|---|---|
| Customer | নূর টেক শপ |
| Product | Charger Combo Pack বা Phone Charger 20W |
| Quantity | 10 |
| Price | 450 বা 350 |

→ Save → কাস্টমার রাজি হলে Sales এ **Confirm**

**জাবেদ পরে:** Delivery Validate (Inventory/Sales পার্ট)।

```
CRM Opportunity = ডিলের খাতা
Sales Quotation = আসল দরপত্র
দুটো লিংকড থাকতে পারে — একই গল্পের দুই পর্দা
```

Product/স্টক না থাকলে আগে Inventory/Manufacturing পার্ট দেখুন।  
শুধু CRM শিখতে Quotation লাইন ছাড়াও Opportunity Won করা যায় — তবে আসল বিক্রির জন্য Product লাগে।

---

# দিন ৩ বিকেল — Won / Lost (সালমা)

## Won

ডিল জিতলে Opportunity এ **Won** চাপুন (বা কার্ড Won কলামে টানুন)।

রহিম: “পাইপলাইন থেকে জয়ের খাতায় উঠল।”

## Lost

জাহিদ হাসান আগ্রহ দেখিয়ে দাম বেশি বললে চলে গেল।

Opportunity/Lead → **Lost** → Reason বাছুন/লিখুন:

```
Too expensive / প্রতিযোগী সস্তা
```

→ Submit

**লাভ:** মাসে Lost Reasons রিপোর্ট — কেন হারছেন বোঝা যায়।

```
Won ≠ স্টক কমেছে
স্টক কমে Sales Delivery Validate এ
```

---

# দিন ৪ — আরেকটা Lead শুধু ফোন (সালমা)

| Field | মান |
|---|---|
| Name | জাহিদ — চার্জার ২ পিস জিজ্ঞাসা |
| Contact | জাহিদ হাসান |
| Phone | 01911000001 |
| Expected Revenue | 700 |

Save → পরে Qualify না হলে Lost বা অপেক্ষা।

ছোট ডিলও খাতায় রাখলে হারানো ফোন কমে।

---

# দিন ৫ — রহিম রিপোর্ট দেখেন

```
CRM → Reporting
```

| Report (নাম ভিন্ন হতে পারে) | কী বোঝায় |
|---|---|
| Pipeline | এখন কত ডিল কোন স্টেজে |
| Forecast | আশা করা রেভিনিউ |
| Won / Lost | জয়-পরাজয় |
| Activities | ফলোআপ বাকি আছে কিনা |

রহিম সালমাকে বলেন: “প্রতি ডিল এ Next Activity (কল/মিটিং) দাও — ভুলে যাবে না।”

Opportunity এ **Schedule Activity** → Call → তারিখ কাল।

**সালমা পরে কী দেখবে:** আজকের Activities লিস্টে কল করার কথা।

---

# Configuration — রহিম একবার দেখবেন

```
CRM → Configuration
```

| মেনু | কী | এখন |
|---|---|---|
| Settings | Leads টিক ইত্যাদি | মিনিমাল Save |
| Stages | Pipeline কলাম | ডিফল্ট রাখুন |
| Lost Reasons | হারানোর কারণ | চাইলে New: Too expensive |
| Activity Types | Call, Email, Meeting | ডিফল্টই যথেষ্ট |
| Tags | Hot / Cold | ঐচ্ছিক |

Stages এড়িয়ে সরাসরি মুছে ফেলবেন না — সালমার বোর্ড গোলমাল হবে।

---

# কে কখন কোন মেনু

| মেনু | রহিম | সালমা |
|---|---|---|
| Settings / Stages | ✅ একবার | — |
| Leads | দেখে | ✅ New + Convert |
| Pipeline / Opportunities | বোর্ড দেখে | ✅ সরানো + Quotation |
| Won / Lost | রিপোর্ট | ✅ সিদ্ধান্ত |
| Reporting | ✅ সাপ্তাহিক | কদাচিৎ |
| Schedule Activity | — | ✅ ফলোআপ |

---

# ডেমো চরিত্র — এক নজরে

| নাম | CRM তে |
|---|---|
| নূর টেক শপ | Lead → Opportunity → Quotation |
| সানরাইজ স্টোর | সরাসরি Opportunity |
| জাহিদ হাসান | ছোট Lead / Lost হতে পারে |
| সালমা | Salesperson |
| Expected Revenue উদাহরণ | 4500 / 9000 |

---

# এক নজরে পুরো রাস্তা

```
১  DB + Company (Demo OFF)
২  Contacts + CRM + Sales Install
৩  Settings মিনিমাল → Pipeline স্টেজ দেখা
৪  Lead: নূর টেক আগ্রহ
৫  Convert → Opportunity → স্টেজ সরানো
৬  New Quotation → Confirm (Sales)
৭  Won (জিতলে) / Lost + Reason (হারলে)
৮  Activity দিয়ে ফলোআপ
৯  Reporting দেখা (রহিম)
```

---

## সমস্যা হলে

| সমস্যা | কী করবেন |
|---|---|
| CRM মেনু নেই | Apps → CRM Install |
| Leads মেনু নেই | Settings এ Leads ☑ → Save |
| Quotation বাটন নেই | Sales অ্যাপ Install |
| Customer খালি | Convert এ Contact তৈরি বা Contacts এ আগে Save |
| Product নেই Quotation এ | Inventory/Manufacturing পার্টে পণ্য+স্টক |
| Pipeline খালি | Lead Convert বা New Opportunity |

---

## Accounting / Sales এর মতো মনে রাখুন

```
Contacts     = কে
CRM          = কোন ডিল চলছে (পাইপলাইন)
Sales        = দরপত্র + অর্ডার
Inventory    = মাল বের হওয়া
Accounting   = বিল ও টাকা
```

```
Lead/Opportunity আগে → Quotation পরে
Won শুধু CRM পতাকা — মাল ও টাকা আলাদা মডিউলে
```

এই ফাইল উপর থেকে দিন ১ → ৫।  
রহিম সেটআপ, সালমা Lead→Opportunity→Won/Lost — তাহলে CRM পরিষ্কার।
