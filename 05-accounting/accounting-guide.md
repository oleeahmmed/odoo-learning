# Accounting — সহজ গল্পে সম্পূর্ণ সেটআপ
### রহিম ট্রেডার্স: scratch থেকে Login → হিসাব চালু

---

## গল্প: অফিসে কে কে?

| কে | ভূমিকা | Odoo তে কী করে |
|---|---|---|
| **রহিম ভাই** | মালিক | সেটআপ দেখে, রিপোর্ট দেখে |
| **করিম** | হিসাব কর্মী | প্রতিদিন Invoice, Bill, Payment |
| **সানরাইজ স্টোর** | Customer | যাদেরকে বিক্রি |
| **ঢাকা ইলেকট্রনিক্স** | Vendor | যাদের কাছ থেকে কেনা |

আপনি প্রথমে **রহিম (Admin)** হয়ে সব সেট করবেন।  
পরে করিম যেন রোজ কাজ করতে পারে — সেইভাবে সাজাবেন।

```
Demo data ☐ OFF — সব হাতে বানাব
অ্যাপ = Invoicing (ছোট কোম্পানির জন্য যথেষ্ট)
```

---

# দিন ১ সকাল — খাতা খোলা (রহিম)

## ১) নতুন Database

```
http://localhost:8069/web/database/manager
```

| Field | মান |
|---|---|
| Database Name | rahim_accounting |
| Email | admin@rahim.com |
| Password | admin123 |
| Country | Bangladesh |
| Demo data | ☐ খালি |

Create → Login (`admin@rahim.com` / `admin123`)

## ২) দোকানের নাম

```
Settings → Companies → নাম: রহিম ট্রেডার্স → Save
উপরের নাম → Preferences → Timezone: Asia/Dhaka → Save
```

রহিম ভাই ভাবিলেন: “খাতা খালি — এখন হিসাবের অ্যাপ লাগাই।”

---

# দিন ১ দুপুর — অ্যাপ লাগানো (রহিম)

```
Apps → Contacts → Install
Apps → Invoicing → Install
```

উপরে **Invoicing** দেখা গেল।

কিন্তু Configuration এ Chart of Accounts খুঁজে না পেলে রহিম করলেন:

```
Settings → Activate the developer mode
Settings → Users & Companies → Groups
→ Show Full Accounting Features খুলে
→ Users ট্যাবে নিজেকে Add → Save
→ রিফ্রেশ
```

এখন মেনু দেখা যায়:

```
Dashboard | Customers | Vendors | Accounting | Reporting | Configuration
```

**করিম পরে কী দেখবে:** একই মেনু (যদি তাকেও এই অধিকার দেন)।  
শুরুতে শুধু রহিম Admin।

---

# দিন ১ বিকেল — Dashboard এর ৪টা কাজ (রহিম)

Invoicing খুললে Dashboard এ কার্ড থাকে। রহিম একটা একটা শেষ করেন।

---

## কাজ A — Accounting Periods

রহিম: “এই বছরের হিসাব কোন তারিখ থেকে?”

→ **Configure** → বছরের শুরু–শেষ দিন → Save

**করিম পরে কী দেখবে:** রোজ কিছু না। শুধু রিপোর্ট ঠিক বছরে কাটে।

---

## কাজ B — Bank Account

রহিম: “টাকা কোন ব্যাংকে উঠবে?”

→ **Bank Account** → ম্যানুয়ালি বানান  
নাম: `রহিম চলতি হিসাব`  
Currency আলাদা ভরার দরকার নেই (কোম্পানি BDT)।

→ Save

**এরপর করিম কী দেখবে:**  
Invoice এ **Register Payment** চাপলে Journal এ এই ব্যাংক বাছা যাবে।

**কোথায় জমে:** `Configuration → Journals` এ Bank দেখা যায়।

---

## কাজ C — Taxes এ Review

রহিম Review চাপলেন — কিছু ট্যাক্স লিস্টে আছে।

এখন VAT নিয়ে মাথা ঘামান না।  
শুধু **Done** চাপলেন।

**মানে:** কার্ড সরল। ট্যাক্স মুছে যায়নি।  
করিম পরে সাধারণ Invoice কাটতে পারবে ট্যাক্স ছাড়াও।

---

## কাজ D — Chart of Accounts: আগে খাতা বানান (পরে Product)

রহিম বললেন: “পণ্যে Income Account বাছব — কিন্তু খাতা আগে না থাকলে বাছব কী?”

```
নিয়ম: Account আগে বানাও → তারপর Product এ সেট করো
কোনো খাতা না বানিয়ে Product Accounting এ যাবেন না
```

Dashboard → **Chart of Accounts → Review**  
অথবা: `Configuration → Chart of Accounts`

লিস্টে অনেক ডিফল্ট খাতা থাকতে পারে। তবু রহিম **নিজের নামে তিনটা** বানালেন — যাতে পরে Product এ স্পষ্ট বাছা যায়।

### খাতা ১ — বিক্রির আয় (Charger এর Income এর জন্য)

→ **New**

| Field | ডেমো মান |
|---|---|
| Code | `400100` (খালি কোড হলে অন্যও চলে) |
| Account Name | `Product Sales Income` |
| Type | **Income** |

→ **Save**

### খাতা ২ — কেনার খরচ (Charger এর Expense এর জন্য)

→ **New**

| Field | ডেমো মান |
|---|---|
| Code | `500100` |
| Account Name | `Product Purchase Expense` |
| Type | **Expenses** |

→ **Save**

### খাতা ৩ — অফিস ভাড়া (Office Rent এর জন্য)

→ **New**

| Field | ডেমো মান |
|---|---|
| Code | `630000` |
| Account Name | `Office Rent Expense` |
| Type | **Expenses** |

→ **Save**

এখন CoA লিস্টে এই তিনটা দেখা যাচ্ছে।  
→ Dashboard কার্ডে **Done** (কার্ড সরানো)।

**করিম পরে কী দেখবে:** Product/Bill এ Account ড্রপডাউনে এই নামগুলো।

```
আজ বানালাম:
Product Sales Income      → Charger Income Account
Product Purchase Expense  → Charger Expense Account
Office Rent Expense       → Office Rent Expense Account
```

---

# দিন ২ সকাল — মানুষ লেখা (রহিম)

রহিম বললেন: “আগে নাম লিখি, তারপর বিল।”

## Customer

```
Invoicing → Customers → Customers → New
```

- সানরাইজ স্টোর (Company) → Save  
- নূর টেক শপ → Save  

**করিম পরে কী দেখবে:** Invoice এ Customer ড্রপডাউনে এই নাম।

## Vendor

```
Invoicing → Vendors → Vendors → New
```

- Dhaka Electronics Ltd → Save  

**করিম পরে কী দেখবে:** Bill এ Vendor হিসেবে এই নাম।

## কর্মী করিম (ঐচ্ছিক)

```
Settings → Users → New
নাম: করিম হিসাব | Email: karim@rahim.com
Invoicing অধিকার দিন → Save
```

করিম আলাদা লগইন করে শুধু বিল কাটবে।  
এখন শিখতে Admin দিয়েই সব করতে পারেন।

---

# দিন ২ দুপুর — পণ্য লেখা (রহিম)

রহিম আগে চেক করলেন:

```
☑ Product Sales Income বানিয়েছি (কাজ D)
☑ Product Purchase Expense বানিয়েছি (কাজ D)
☑ Office Rent Expense বানিয়েছি (কাজ D)
```

না থাকলে **এখানে আসবেন না** — ফিরে কাজ D করুন।

```
Customers → Products → New
```

**Phone Charger 20W** (বিক্রি + কেনা দুটোই)

| Field | কী দেবেন |
|---|---|
| Product Name | Phone Charger 20W |
| Sales Price | 350 |
| Cost | 220 |
| Can be Sold | ☑ |
| Can be Purchased | ☑ |

ট্যাব **Accounting** — এখন ড্রপডাউনে কাজ D এর খাতা বাছুন:

| ঘর | কী বাছবেন (কাজ D তে বানানো) | Confirm এর পর কী হয় |
|---|---|---|
| **Income Account** | `Product Sales Income` | Invoice → আয় এই খাতায় |
| **Expense Account** | `Product Purchase Expense` | Vendor Bill → খরচ এই খাতায় |

→ **Save**

### Receivable / Payable — Product এ সেট নয়

```
Receivable = Customer Contact এ (ডিফল্ট অটো)
Payable    = Vendor Contact এ (ডিফল্ট অটো)
Product এ শুধু Income + Expense — কাজ D তে যা বানিয়েছি
```

Income জায়গায় ভুলে Receivable দিলে আয় ভুল খাতায় যাবে — দেবেন না।

```
সিরিয়াল মনে রাখুন:
কাজ D তে খাতা বানাও → তারপর Charger এ Income/Expense বাছো
```

**Office Rent** (সার্ভিস — শুধু খরচের Bill এর জন্য)

রহিম New চাপলে ফর্মে **Sales Price** ও **VAT/Taxes** দেখা যায়।  
ভয় পাবেন না — সব পণ্য ফর্মেই এই ঘরগুলো থাকে।  
অফিস ভাড়া আমরা **কাস্টমারকে বিক্রি করি না**, শুধু Vendor Bill এ খরচ দেখাই। তাই অনেক ঘর খালি/০ রাখব।

| Field | কী দেবেন | কেন |
|---|---|---|
| Product Name | `Office Rent` | নাম |
| Product Type | **Service** | স্টক নয়, শুধু সার্ভিস/খরচ |
| Can be Sold | **☐ খালি** | কাস্টমারকে ভাড়া বিক্রি করি না |
| Can be Purchased | **☑** | Vendor Bill এ লাগবে |
| Sales Price | **0** বা খালি | বিক্রি নয় — তাই Sales Price দেখা গেলেও ০ রাখুন |
| Cost / Purchase price | **15000** (ঐচ্ছিক) | মাসিক ভাড়ার আন্দাজ; Bill এ পরে বদলানো যায় |
| Customer Taxes / Sales VAT | **খালি** | বিক্রি নেই — VAT লাগাবেন না |
| Vendor Taxes / Purchase VAT | **খালি** (শিখতে) | পরে VAT থাকলে এখানে বাছবেন |
| Expense Account | **Office Rent Expense** | Chart এ যে খাতা বানিয়েছিলেন |
| Income Account | খালি রাখুন | বিক্রি নেই |

→ **Save**

```
Sales Price / VAT ঘর আসা = Odoo এর সাধারণ ফর্ম
Office Rent এ Sales Price = 0, Sold ☐, Sales Tax খালি
শুধু Purchased ☑ + Expense Account
```

**করিম পরে কী দেখবে:**  
Invoice/Bill লাইনে পণ্য বাছা মাত্র দাম ও হিসাবের খাতা অটো আসে।

```
পণ্য ছাড়া বিল লাইন ভরবেন না — তাই আগে পণ্য
```

---

# দিন ৩ — করিমের কাজ: কাস্টমার বিল

করিম সকালে অফিসে এসে Invoicing খুললেন।

## Invoice কাটা

```
Customers → Invoices → New
```

| Field | মান |
|---|---|
| Customer | সানরাইজ স্টোর |
| Product | Phone Charger 20W |
| Quantity | 10 |

→ **Confirm**

করিম দেখলেন: স্ট্যাটাস Posted।

## টাকা এলে

একই Invoice → **Register Payment**  
Journal = রহিম চলতি হিসাব → Create Payment

**রহিম ভাই কী দেখবেন:**  
Reporting → Partner Ledger এ সানরাইজের পাওনা/পেমেন্ট।  
Dashboard এ Customer Invoices এর সংখ্যা বেড়েছে।

---

# দিন ৩ বিকেল — করিমের কাজ: Vendor বিল

```
Vendors → Bills → New
```

| Field | মান |
|---|---|
| Vendor | Dhaka Electronics Ltd |
| Product | Phone Charger 20W |
| Quantity | 50 |
| Price | 220 |

→ Confirm → (টাকা দিলে) Register Payment

অফিস ভাড়া:

Bill → Product **Office Rent** → Amount 15000 → Confirm

**রহিম কী দেখবেন:** P&L এ খরচ বেড়েছে।

---

# দিন ৪ — ভুল বিল ঠিক (করিম)

সানরাইজ ২ পিস ফেরত চাইল।

করিম পুরনো Invoice খুলে **Credit Note** → Qty 2 → Confirm

**গল্প:** বিল কমানো — পুরো Invoice মুছে ফেলা নয়।

Vendor ফেরত হলে: Vendors → Refunds।

---

# দিন ৫ — রহিম ভাই রিপোর্ট দেখেন

রহিম নিজে লগইন (বা করিমের পাশে বসে):

```
Reporting → Profit and Loss     → এই মাসে লাভ/ক্ষতি
Reporting → Partner Ledger      → সানরাইজের কাছে কত / Vendor কে কত
Accounting → Journal Entries    → করিম বিল কাটলে অটো এন্ট্রি
```

রহিম বললেন: “করিম বিল কাটে, আমি শুধু এই রিপোর্ট দেখি।”

---

# কে কখন কোন মেনু দেখে — এক ছক

| মেনু | রহিম (সেটআপ/রিপোর্ট) | করিম (রোজকার) |
|---|---|---|
| Dashboard ৪টা কার্ড | একবার সেট করে | পরে আর লাগে না |
| Configuration | Chart, Bank, Terms | সাধারণত খোলে না |
| Customers → Customers | নাম লেখে | কদাচিৎ নতুন কাস্টমার |
| Customers → Products | পণ্য+Account সেট | কদাচিৎ |
| Customers → Invoices | দেখে | **প্রতিদিন কাটে** |
| Customers → Payments | দেখে | Payment করে |
| Customers → Credit Notes | দেখে | ফেরত হলে |
| Vendors → Bills / Payments | দেখে | **প্রতিদিন** |
| Accounting → Journal Entries | অডিট করে | শুধু দেখতে পারে |
| Reporting | **সাপ্তাহিক দেখে** | কদাচিৎ |

---

# Dashboard ৪টা কার্ড — এক লাইনে মনে রাখুন

| কার্ড | রহিম কী করেন | করিমের কাজে কী লাগে |
|---|---|---|
| Periods | বছর সেট | রিপোর্ট ঠিক থাকে |
| Bank | ব্যাংক নাম Save | Payment এ Journal পায় |
| Taxes Review → Done | এখন না ছুঁয়ে Done | সাধারণ বিল কাটে |
| CoA → ৩টা খাতা বানাও তারপর Done | Sales Income, Purchase Expense, Rent Expense | Product এ এইগুলো বাছা |

শুধু Done = কার্ড সরানো, ডাটা মুছে যায় না।

---

# Configuration — করিম না খুললেও রহিম একবার দেখবেন

```
Configuration → Journals     → Bank আছে তো?
Configuration → Payment Terms → New: 15 Days (ঐচ্ছিক)
Configuration → Taxes        → এখন এডিট না করলেও চলে
Configuration → Currencies   → BDT আছে, আর কিছু লাগে না
```

Payment Terms Save করলে করিম Invoice এ “১৫ দিন বাকি” বাছতে পারবে।

---

# এক নজরে পুরো রাস্তা (scratch → চালু)

```
১  Login (নতুন DB, Demo off)
২  Company + Timezone
৩  Contacts + Invoicing Install
৪  Full Accounting Features চালু
৫  Dashboard: Periods → Bank → Taxes Done → CoA তে ৩ খাতা বানাও → Done
৬  Customer + Vendor
৭  Product (Income/Expense = কাজ D এর খাতা)
৮  করিম: Invoice → Payment
৯  করিম: Bill → Payment
১০ রহিম: Reporting দেখা
```

---

## সমস্যা হলে

| সমস্যা | কী করবেন |
|---|---|
| Chart মেনু নেই | Full Accounting Features |
| Payment এ Bank নেই | Dashboard থেকে Bank Save করেছেন তো? |
| Invoice এ Customer খালি | আগে Customers → New |
| Product নেই | আগে Products → New |
| করিম মেনু কম দেখে | Users এ Invoicing অধিকার দিন |

---

## ছোট কোম্পানি মনে রাখবেন

রহিম ট্রেডার্স = **Invoicing** যথেষ্ট।  
বড় ফ্যাক্টরির Enterprise Accounting এখন লাগবে না।

এই ফাইল উপর থেকে পড়ুন আর অফিসে **রহিম হয়ে সেট করুন, করিম হয়ে বিল কাটুন** — তাহলেই পুরো হিসাব পরিষ্কার।
