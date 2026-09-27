# Sales Advanced — জটিল কেস (রহিম ট্রেডার্স)
### শিল্পে যা হয়: আংশিক অর্ডার, দুইবার ডেলিভারি, রিটার্ন

---

## এই ফাইল কখন পড়বেন?

আগে করুন:

```
03-sales/sales-guide.md   ← সাধারণ Quotation → Confirm → Delivery → Invoice
```

ওটা **সহজ ফ্লো**। এই ফাইলটা **বাস্তব জটিল কেস**।  
আগের ফাইল বদলাবেন না / লাগবে না মুছতে — দুটো আলাদা।

একই database, একই গল্প:

```
Customer: সানরাইজ স্টোর
Product:  Phone Charger 20W (Lot CH-001)
স্টক:     যথেষ্ট থাকতে হবে (কমপক্ষে ২০+ পিস রাখুন)
```

---

## আজকের বড় গল্প (একটাই কেস, ধাপে ধাপে)

সানরাইজ বলল:

> “১০ পিসের দর চাই।”

পরে:

> “ভাই, এখন **৯ পিস**ই নেব।”

ডেলিভারিতে:

> প্রথম দিন ৫ পিস, পরের দিন বাকি ৪ পিস।

আরও পরে:

> “৩ পিস ফেরত দিচ্ছি — কাজ হচ্ছে না।”

এটাই ইন্ডাস্ট্রিতে রোজ হয়। Odoo দিয়ে পুরোটা চালাবেন।

```
Quotation 10
    → অর্ডার 9
        → Delivery 1: 5 পিস
        → Delivery 2: 4 পিস  (backorder)
            → Invoice
                → Return 3 পিস + Credit Note
```

---

# কেস ক — Quotation ১০, Sales Order ৯

## ক১) Quotation ১০ পিস

```
Sales → Orders → Quotations → New
```

| Field | মান |
|---|---|
| Customer | সানরাইজ স্টোর |
| Product | Phone Charger 20W |
| Quantity | **10** |
| Unit Price | 350 |

→ **Save**

এখন এটা শুধু অফার — স্টক কাটেনি।

---

## ক২) কাস্টমার বলল “৯ নেব” — Confirm-এর আগে Qty বদলান

Quotation খোলা অবস্থায় লাইনে:

| Field | পুরনো | নতুন |
|---|---|---|
| Quantity | 10 | **9** |

→ **Save**

→ উপরে **Confirm**

এখন এটা **Sales Order** — অর্ডার = **৯ পিস**।

```
গল্পের মানে:
অফার ছিল ১০
অর্ডার হলো ৯
Confirm-এর আগে Qty কমানোই সবচেয়ে পরিষ্কার উপায়
```

> Confirm হয়ে গেলেও অনেক সময় Qty এডিট যায় (Lock না থাকলে)।  
> শিখতে: **Confirm-এর আগেই** ৯ করে নিন।

Delivery বাটনে Demand ≈ **9** দেখা যাবে।

---

# কেস খ — এক অর্ডার, দুইবার Delivery (Backorder)

**গল্প:** গুদামে একসাথে ৯ প্যাক করতে পারলেন না। আজ ৫, কাল ৪।

## খ১) Settings — Backorder চালু আছে কিনা

```
Inventory → Configuration → Operations Types
→ Delivery Orders খুলুন
```

| দেখুন | রাখুন |
|---|---|
| Create Backorder | **Ask** অথবা **Always** |

`Ask` = Validate-এর সময় জিজ্ঞেস করবে — শেখার জন্য ভালো।

→ **Save**

---

## খ২) প্রথম Delivery — শুধু ৫ পিস

Sales Order → **Delivery** বাটন।

1. **Mark as Todo** (লাগলে)  
2. Quantity / Done = **5** (৯ না)  
3. ≡ এ Lot = **CH-001**, Qty = **5**  
4. **Validate**

পপআপ আসবে মোটামুটি:

```
Create Backorder ? 
```

→ **Create Backorder** চাপুন।

মানে:

```
এই Delivery = 5 Done
বাকি 4     = নতুন Delivery (Backorder) তৈরি
```

Sales Order এ Delivery স্ট্যাটাস দেখাবে **Partially Delivered** জাতীয়।

---

## খ৩) দ্বিতীয় Delivery — বাকি ৪ পিস

আবার Sales Order → **Delivery** বাটন (এখন ২টা Delivery দেখা যেতে পারে)।

অথবা:

```
Inventory → Operations → Delivery Orders
```

যেটা এখনো Done নয় (বাকি ৪) সেটা খুলুন।

1. Mark as Todo  
2. Qty = **4**, Lot = CH-001  
3. **Validate**

এখন মোট বেরিয়েছে:

```
5 + 4 = 9   ← অর্ডার পূরণ
```

```
এক Delivery তে কম দিয়ে Validate
    + Create Backorder
        = দুইবার ডেলিভারি
```

---

## খ৪) Create Backorder vs No Backorder — বাস্তব গল্পে বুঝুন

সানরাইজ অর্ডার **৯ পিস** চার্জার।  
রহিম ভাই গুদামে আজ প্যাক করতে পারলেন শুধু **৫ পিস**। Delivery Validate চাপলেন।

Odoo জিজ্ঞেস করে:

```
বাকি ৪ পিসের কী হবে?
```

এখানে দুটো রাস্তা।

---

### গল্প A — Create Backorder (কাল দেব)

রহিম ভাই বললেন:

> “আজ ৫ নিয়ে যাও। বাকি ৪ কাল পাঠাব।”

→ **Create Backorder**

বাস্তবে কী হয়:

```
আজকের Delivery  = ৫ পিস Done (ট্রাকে উঠে গেল)
কালের Delivery  = Odoo নিজে বানিয়ে রাখল (বাকি ৪)
অর্ডার          = এখনো শেষ নয় — Partially Delivered
```

কাল গুদামে মাল এলে সেই **দ্বিতীয় Delivery** খুলে Validate → ৪ পিসও চলে গেল।  
অর্ডার পূর্ণ: ৫ + ৪ = ৯।

```
Create Backorder = “বাকিটা ভুলিও না — আরেকটা Delivery খাতা খোলা রাখো”
```

---

### গল্প B — No Backorder (আর দেব না)

রহিম ভাই বললেন:

> “আজ শুধু ৫ই দিলাম। বাকি ৪ আর পাঠাব না — অর্ডার এখানেই শেষ।”

→ **No Backorder**

বাস্তবে কী হয়:

```
আজকের Delivery = ৫ পিস Done
বাকি ৪         = বাতিল — কোনো দ্বিতীয় Delivery তৈরি হয় না
অর্ডার         = মাল হিসেবে শেষ (Delivered ৫, বাকি পাঠানোর খাতা নেই)
```

সানরাইজের কাছে গেল শুধু ৫।  
কাল Odoo তে সেই বাকি ৪ এর Delivery **খুঁজে পাবেন না** — কারণ আপনি বলেছেন পাঠাবেন না।

পরে সানরাইজ আবার ৪ চাইলে নতুন অর্ডার লাগে (আগের বাতিল Delivery ফিরে আসে না)।

```
No Backorder = “বাকিটা মুছে দাও — আর খাতা খোলা রাখবেন না”
```

---

### এক লাইনে তফাৎ

| | Create Backorder | No Backorder |
|---|---|---|
| আজ | ৫ গেল | ৫ গেল |
| বাকি ৪ | কালের Delivery খাতা খোলা | বাতিল |
| পরে সেই ৪ পাঠানো | একই অর্ডারের ২য় Delivery | নতুন অর্ডার লাগে |
| গল্প | “বাকি আসছে” | “বাকি নেই, এখানেই শেষ” |

ট্রাকের গল্প:

```
Create Backorder = আজকের ট্রাক + কালকের ট্রাকের স্লিপ রাখা
No Backorder     = আজকের ট্রাকই শেষ; কালকের স্লিপ ছিঁড়ে ফেলা
```

---

## খ৫) No Backorder এর পর আবার Delivery?

**সেই বাতিল Delivery আর ফিরে আসে না।**

পরে সানরাইজ আবার ৪ চাইলে:

| উপায় | কী করবেন |
|---|---|
| সহজ | নতুন Quotation/SO → Qty ৪ → Confirm → নতুন Delivery |
| একই SO | Qty বাড়ান (লক না থাকলে) → নতুন Delivery হতে পারে |

কাল দেবেন জানলে আগে থেকেই **Create Backorder** বাছুন।  
বিলের হিসাব → কেস **গ৩**।

---

# কেস গ — Invoice (আংশিক ডেলিভারির পর)

## গ০) জরুরি প্রশ্ন: এক Delivery + Invoice করলে কি অর্ডার শেষ?

**না।** Invoice দিলে বাকি Delivery অটো বাতিল হয় **না**, অর্ডারও সম্পূর্ণ হয় **না**।

Odoo-তে দুটো আলাদা রাস্তা:

```
Delivery  = মাল গেল কিনা
Invoice   = বিল কাটা হল কিনা
```

একটা করলে অন্যটা থেমে যায় না।

### উদাহরণ (আপনার কেস)

```
অর্ডার = ৯
Delivery #1 = ৫ Done + Backorder (বাকি ৪)
এখন Invoice কাটলেন
```

| কী হয় | কী হয় না |
|---|---|
| বিল তৈরি হয় (৫ বা ৯ — Policy অনুযায়ী) | বাকি ৪ পিসের Delivery মুছে যায় না |
| | অর্ডার “সম্পূর্ণ ডেলিভারি” হয়ে যায় না |

বাকি **৪ পিস এখনো পাঠাতে হবে** — Backorder Delivery খুলে Validate করতে হবে।  
Invoice নতুন Delivery বানিয়ে দেয় **না**; আগের Backorder-ই থাকে।

```
Invoice  ≠  অর্ডার শেষ
Create Backorder থাকলে → দ্বিতীয় Delivery আলাদা করেই করতে হবে
```

### কখন কী করবেন (সহজ নিয়ম)

| পরিস্থিতি | করণীয় |
|---|---|
| আজ ৫ দিলাম, কাল ৪ দেব | Delivery #1 → Backorder → (চাইলে এখন ৫ এর Invoice) → পরে Delivery #2 → বাকি Invoice |
| দুই Delivery শেষ করে একসাথে বিল | Delivery #1 + #2 Done → তারপর একবার Invoice |
| শুধু ৫ দিয়ে বাকি দেব না | Delivery #1 এ **No Backorder** → তারপর Invoice (শুধু যা দিয়েছেন) |

> **Ordered quantities** Policy থাকলে প্রথম Invoice-এই পুরো ৯ এর বিল কাটা যেতে পারে — মাল ৫ গেলেও।  
> তাতেও বাকি Delivery থাকলে সেটা আলাদা; বিল ≠ মাল শেষ।  
> তাই আংশিক ডেলিভারিতে **Delivered quantities** রাখুন।

---

## গ১) দুই Delivery শেষে এক Invoice (সহজ)

দুই Delivery Done হলে Sales Order এ যান।

→ **Create Invoice** → Regular invoice → Create → **Confirm**

---

## গ২) প্রথম Delivery এর পরই Invoice (৫ পিস)

Product Policy = **Delivered quantities** থাকলে:

1. Delivery #1 (৫ পিস) Done  
2. Sales Order → **Create Invoice** → শুধু ~৫ পিস বিল  
3. পরে Delivery #2 (৪ পিস) Validate  
4. আবার **Create Invoice** → বাকি ~৪ পিস বিল  

Backorder Delivery তবু ধাপ ৩ এ নিজে Validate করতে হবে — Invoice সেটা করে দেয় না।

---

### দুই ধরনের বিল নীতি (বুঝে রাখুন)

Product → Invoice Policy:

| Policy | মানে | কখন |
|---|---|---|
| **Ordered quantities** | অর্ডারের পুরো Qty বিল (ডিফল্ট অনেক সময়) | অর্ডার ফাইনাল হলেই বিল |
| **Delivered quantities** | যত ডেলিভারি হয়েছে তত বিল | আংশিক ডেলিভারিতে নিরাপদ |

**শিখতে সুপারিশ:** Phone Charger এ

```
Product → General / Sales ট্যাব → Invoicing Policy = Delivered quantities
```

তাহলে:

```
৫ পিস ডেলিভারি → ৫ পিসের Invoice করা যায়
আবার ৪ পিস → আরেক Invoice বা বাকি বিল
```

Ordered রাখলে Confirm এর পরই পুরো ৯ বিল কাটা যায় — মাল না গেলেও।  
ইন্ডাস্ট্রিতে ফিজিক্যাল মালে প্রায়ই **Delivered** ভালো।

---

## গ৩) No Backorder + Invoice — আগে করলে / পরে করলে কী হয়?

গল্প একই: অর্ডার **৯**, আজ দিলেন **৫**, বাকি **৪ আর দেবেন না** → **No Backorder**।

মনে রাখুন দুটো ঘড়ি আলাদা:

```
মালের ঘড়ি  = Delivery / No Backorder
টাকার ঘড়ি  = Invoice / Credit Note
```

একটা ঠিক করলে অন্যটা অটো মিলে যায় না — নিজে মিলিয়ে নিতে হয়।

---

### ছক: কোন ক্রমে কী হয়

ধরে নিন Product Policy দুটোই আলাদা করে বুঝছেন।

#### ১) আগে No Backorder, পরে Invoice ✅ সবচেয়ে পরিষ্কার

```
Delivery ৫ → Validate → No Backorder
→ Create Invoice
```

| Policy | Invoice এ কত পিস |
|---|---|
| **Delivered quantities** | ≈ **৫** — ঠিক; বাকি ৪ বিল হয় না |
| **Ordered quantities** | সাবধান: অনেক সময় এখনো **৯** এর বিল আসতে পারে |

Ordered হলে Invoice ড্রাফটে Qty দেখে নিন — ৯ থাকলে **৫** করে Confirm করুন।  
নাহলে কাস্টমারকে ৪ পিসের টাকাও বিল হয়ে যাবে, অথচ মাল যায়নি।

```
সঠিক অভ্যাস: No Backorder → তারপর Invoice
Delivered policy রাখুন
```

---

#### ২) আগে Invoice (পুরো/বেশি), পরে No Backorder ⚠️ সমস্যা হতে পারে

**উদাহরণ ক — Delivered policy, শুধু ৫ এর বিল আগে**

```
Delivery ৫ Done (এখনো Backorder আছে বা পরে No Backorder)
→ Invoice ≈ ৫ Confirm
→ পরে Validate এ No Backorder (বাকি ৪ বাতিল)
```

ফল:

```
মাল গেছে ৫, বিল ৫ → মিলে গেছে ✅
আর Delivery নেই
```

এটাও ঠিক।

---

**উদাহরণ খ — Ordered policy, আগেই ৯ এর Invoice**

```
SO Confirm
→ Invoice পুরো ৯ Confirm করে ফেললেন
→ পরে Delivery শুধু ৫ + No Backorder
```

ফল:

```
মাল গেছে   = ৫
বিল কেটেছেন = ৯ এর টাকা
অতিরিক্ত   = ৪ পিসের টাকা — মাল যায়নি অথচ বিল আছে ❌
```

সমাধান:

```
Invoice → Credit Note → Partial → Qty 4 → Confirm
```

তাহলে টাকার হিসাব ৫ পিসে নেমে আসে।

```
বেশি বিল কেটে ফেললে → Credit Note দিয়ে কমান
No Backorder মাল বাতিল করে; বিল নিজে থেকে কমায় না
```

---

#### ৩) আগে Invoice ৯, পরে Create Backorder রেখেছিলেন, শেষে আর মাল দিলেন না

একই সমস্যা — বিল ৯, মাল ৫।

```
বাকি Delivery Cancel অথবা No Backorder
+ Credit Note Qty 4
```

---

### ছোট সারাংশ ছক

| ক্রম | মাল | বিল (Delivered policy) | বিল (Ordered policy) | করণীয় |
|---|---|---|---|---|
| No Backorder → Invoice | ৫ | ≈ ৫ ✅ | ড্রাফটে ৯ হতে পারে → ৫ করুন | পরিষ্কার পথ |
| Invoice ৫ → No Backorder | ৫ | ৫ ✅ | — | ঠিক |
| Invoice ৯ → পরে No Backorder | ৫ | — | ৯ বিল আছে ❌ | Credit Note ৪ |
| শুধু No Backorder, Invoice নেই | ৫ | ০ | ০ | পরে শুধু ৫ এর Invoice কাটুন |

```
No Backorder  = বাকি মাল বাতিল
Invoice       = টাকা আলাদা
বেশি বিল হলে  = Credit Note
কম বিল হলে   = আরেক Invoice (যদি পরে আর মাল দেন)
```

---

## গ৪) Return এর সাথে এগুলোর সম্পর্ক

রিটার্ন মানে: **যা ইতিমধ্যে ডেলিভারি হয়েছে** তার কিছু ফেরত।  
যে Qty No Backorder এ বাতিল — সেটা কখনো ডেলিভারি হয়নি, তাই সেটার “Return” হয় না।

### গল্প মিলিয়ে

```
অর্ডার ৯
দিলেন ৫ + No Backorder (৪ বাতিল)
বিল ঠিকমতো ৫
কাস্টমার বলল: ৩ পিস ফেরত
```

| কাজ | Qty | ফল |
|---|---|---|
| Return (Delivery থেকে) | ৩ | গুদামে +৩ |
| Credit Note | ৩ | বিল/পাওনা −৩ পিসের দাম |

শেষ হিসাব:

```
মাল কাস্টমারের কাছে = ৫ − ৩ = ২
নেট বিল             = ৫ − ৩ = ২ পিসের টাকা
```

---

### যদি ভুল করে ৯ এর বিল কেটেছিলেন + No Backorder + পরে Return ৩?

ধাপ আলাদা আলাদা:

```
১  Credit Note ৪   ← যে মাল যায়নি (No Backorder অংশ)
২  Return ৩        ← যে মাল গিয়েছিল, ফেরত
৩  Credit Note ৩   ← ফেরত মালের টাকা
```

অথবা এক Credit Note এ মিলিয়ে ৪+৩=৭ও করা যায় — কিন্তু শিখতে **আলাদা রাখুন**, হিসাব পরিষ্কার থাকে:

| Credit Note | কারণ |
|---|---|
| Qty 4 | মাল যায়নি, বিল বেশি হয়েছিল |
| Qty 3 | মাল ফেরত |

```
No Backorder অংশ → Return নয়; শুধু Credit Note (যদি বিল বেশি হয়)
Delivery হওয়া অংশ → Return + Credit Note
```

---

### Return কি No Backorder এর বিকল্প?

**না।**

| কাজ | কখন |
|---|---|
| No Backorder | মাল **এখনো যায়নি** — বাকি পাঠাব না |
| Return | মাল **ইতিমধ্যে গেছে** — কাস্টমার ফেরত দিচ্ছে |

একটাকে অন্যটার জায়গায় ব্যবহার করবেন না।

---

# কেস ঘ — কাস্টমার ৩ পিস Return

**গল্প:** যা **ইতিমধ্যে ডেলিভারি হয়েছে** তার মধ্যে ৩ পিস ফেরত।  
(No Backorder এ বাতিল Qty-র Return হয় না — কেস **গ৪** দেখুন।)

রিটার্নে **দুটো কাজ**:

```
১  মাল গুদামে ফেরত (Inventory Return)
২  বিল কমানো (Credit Note)
```

শুধু একটা করলে হিসাব অসম্পূর্ণ থাকে।

---

## ঘ১) মাল ফেরত — Delivery থেকে Return

যে Delivery থেকে মাল গেছে সেটা খুলুন  
(অথবা Sales Order → Delivery → যেকোনো Done Delivery)।

উপরে **Return** চাপুন।

| Field | মান |
|---|---|
| Product | Phone Charger 20W |
| Quantity | **3** |
| Lot | CH-001 (≡ এ দিতে হতে পারে) |

→ **Return** / Validate করে Incoming Return **Done** করুন।

চেক:

```
Product → On Hand  ≈ আগের থেকে +3
```

মাল গুদামে ফিরেছে।

---

## ঘ২) বিল কমানো — Credit Note

যে Invoice থেকে বিল কেটেছেন সেটা খুলুন  
(Sales Order → Invoice বা Accounting → Customers → Invoices)।

উপরে **Credit Note** (বা Add Credit Note)।

| অপশন | কখন |
|---|---|
| Partial Refund | শুধু ৩ পিস — **এটাই** |
| Full Refund | পুরো বিল বাতিল |

Partial বেছে:

| Field | মান |
|---|---|
| Quantity | **3** |
| Price | আগের মতো |

→ Confirm / Reverse

এখন কাস্টমারের পাওনা ৩ পিসের দাম কমেছে (অথবা টাকা ফেরতের হিসাব তৈরি)।

```
Return     = মাল ফিরে এল
Credit Note = টাকা/বিল ঠিক হল
দুটোই লাগে
```

---

# কেস চ — Order Revise (কিছু মাল Delivery হয়ে গেছে)

**গল্প:** সানরাইজ অর্ডার ৯। রহিম **৫** দিয়ে দিয়েছেন (Delivery Done)।  
এখন সানরাইজ ফোন করে অর্ডার **বদলাতে** চায়।

Odoo তে “Revise Order” বলে এক বাটন নেই।  
সমাধান = **কী বদলাচ্ছে** তার ওপর আলাদা আলাদা কাজ।

মূল নিয়ম:

```
যা ইতিমধ্যে Delivery হয়েছে → সহজে মুছে ফেলা যায় না
যা এখনো যায়নি           → কমানো / বাতিল করা যায়
নতুন চাহিদা               → লাইন বাড়ানো বা নতুন SO
```

---

## চ১) বাকি মাল আর লাগবে না (৯ এর মধ্যে ৫ গেছে, বাকি ৪ বাদ)

```
বাকি Delivery → Cancel
অথবা Validate সময়ে No Backorder
```

যা গেছে (৫) থাকে। বাকি পাঠানোর খাতা বন্ধ।

বিল যদি আগে ৯ কেটে থাকেন → **Credit Note ৪**।  
বিল এখনো না কেটে থাকেন / Delivered policy → শুধু ৫ এর Invoice।

---

## চ২) যা গেছে তার কিছুও কমাতে হবে (৫ এর মধ্যে ২ ফেরত + বাকি বাদ)

এটা শুধু Qty এডিট নয় — **রিটার্ন লাগে**:

```
১  Delivery → Return (যেমন Qty 2) → Validate
২  বাকি Delivery Cancel / No Backorder
৩  Invoice থাকলে Credit Note (ফেরত + বাতিল অংশ)
```

---

## চ৩) আরও মাল চাই (৯ এর পর আরও ৩)

যা গেছে থাকবে। নতুন চাহিদা:

| উপায় | কখন |
|---|---|
| একই SO তে Qty বাড়ান / নতুন লাইন | লক না থাকলে; Confirm এর পর এডিট চালু |
| **নতুন SO** Qty 3 | হিসাব পরিষ্কার — ইন্ডাস্ট্রিতে প্রায়ই এটাই ভালো |

Qty বাড়ালে Odoo সাধারণত **নতুন Delivery** তৈরি করে।

```
Sales → Settings
☐ Lock Confirmed Sales   ← বন্ধ থাকলে SO এডিট সহজ
```

---

## চ৪) অন্য পণ্য চাই (চার্জারের বদলে কেবল)

```
বাকি চার্জার Delivery Cancel
নতুন লাইন: USB Cable → Save
(প্রয়োজনে নতুন Delivery অটো / ম্যানুয়াল)
যা চার্জার ইতিমধ্যে গেছে → রাখুন, অথবা Return
```

পুরো অর্ডার ভুল হলে: গেছে মাল Return + Credit Note, বাকি Cancel, নতুন SO।

---

## চ৫) দাম বদল (আংশিক Delivery এর পর)

| অবস্থা | সমাধান |
|---|---|
| এখনো Invoice হয়নি | SO লাইনে দাম বদলান (লক না থাকলে); গেছে মালের জন্য পরের Invoice এ নতুন দাম |
| Invoice হয়ে গেছে | **Credit Note** পুরনো দাম ঠিক করুন; প্রয়োজনে নতুন Invoice |
| শুধু বাকি মালের দাম কম | বাকি লাইন/নতুন SO তে নতুন দাম — যা গেছে আলাদা রাখা সহজ |

---

## চ৬) এক নজরে — Odoo কী দেয়

| পরিবর্তন | Odoo সমাধান |
|---|---|
| বাকি পাঠাব না | Cancel Delivery / No Backorder |
| গেছে মাল কমাব | Return + Credit Note |
| আরও মাল | SO তে Qty↑ বা নতুন SO → নতুন Delivery |
| অন্য পণ্য | নতুন লাইন + বাকি পুরনো Cancel |
| দাম বদল (বিলের পর) | Credit Note ± নতুন Invoice |
| সব বাতিল | Return (গেছে) + Cancel (বাকি) + Credit Note |

```
Revise ≠ এক ক্লিক Undo
গেছে মাল = Return/Credit
বাকি মাল = Cancel/No Backorder/Qty বদল
বাড়তি  = নতুন Qty বা নতুন SO
```

---

# কেস ঙ — আরও ইন্ডাস্ট্রি সমস্যা (সংক্ষেপ + সমাধান)

নিচে যেগুলো রোজ লাগে — রহিম গল্পেই বুঝুন।

---

## ঙ১) Quotation পাঠানোর পর দাম কমাতে হবে

```
Quotation এখনো Confirm হয়নি
→ Unit Price বদলান → Save → আবার পাঠান / Confirm
```

Confirm হয়ে **Locked** হলে Settings এ Lock Confirmed Sales বন্ধ না থাকলে এডিট চেষ্টা করুন।  
লক থাকলে: Credit Note / নতুন অর্ডার — জটিল; শিখতে Lock বন্ধ রাখুন।

```
Sales → Configuration → Settings
☐ Lock Confirmed Sales   ← শেখার সময় খালি রাখুন
```

---

## ঙ২) ডিসকাউন্ট দিতে হবে

Quotation / SO লাইনে:

| Field | উদাহরণ |
|---|---|
| Disc.% | 5 |

অথবা Settings এ Discount ফিচার টিক থাকলে লাইনে Disc দেখা যায়।

```
Sales → Settings → ☑ Discounts
```

---

## ঙ৩) অর্ডার Confirm — কিছু Delivery হয়েছে — বাকি বাতিল

বাকি Delivery খুলুন → **Cancel**  
অথবা Validate সময়ে **No Backorder**।

Sales Order এ Delivered < Ordered থাকবে — ঠিক আছে।

---

## ঙ৪) অতিরিক্ত মাল চেয়ে বসল (৯ এর পর আরও ২)

পুরনো SO তে লাইন বাড়ানো যায় (লক না থাকলে)।  
পরিষ্কার উপায়:

```
নতুন Quotation / SO → Quantity 2 → Confirm → Delivery
```

ইন্ডাস্ট্রিতে আলাদা অর্ডার রাখা হিসাব পরিষ্কার রাখে।

---

## ঙ৫) বিল কেটেছি — মাল এখনো পুরো যায়নি

Invoice Policy = **Ordered** থাকলে এমন হয়।

সমাধান এগিয়ে:

- পরের পণ্যে Policy = **Delivered quantities**
- অথবা Credit Note দিয়ে বিল কমান
- অথবা বাকি মাল দ্রুত Delivery করুন

---

## ঙ৬) ভুল কাস্টমার / ভুল পণ্যে Confirm হয়ে গেছে

Delivery ও Invoice **আগে না থাকলে**: SO → Cancel।  
আংশিক Delivery/Invoice হয়ে গেলে:

```
Return (মাল) + Credit Note (বিল) + বাকি Cancel
```

“ডিলিট” করে মুছে ফেলা যায় না Posted Invoice — শুধু Credit Note।

---

## ঙ৭) একই অর্ডারে দুই গুদাম থেকে মাল

স্ট্যান্ডার্ডে এক SO সাধারণত এক Warehouse।  
দুই গুদাম লাগলে:

```
দুইটা আলাদা SO   অথবা   Internal Transfer করে এক গুদামে এনে Delivery
```

(Inventory পার্টের গুদাম কথা মনে রাখুন।)

---

## ঙ৮) কাস্টমার আগে অগ্রিম টাকা দিল

পরে Accounting পার্টে বিস্তারিত। সংক্ষেপ:

```
Sales Order → Create Invoice → Down payment (percentage / fixed)
```

এখন না করলেও চলবে — জেনে রাখুন অপশন আছে।

---

# এক নজরে — আপনার বড় কেস

```
১  Quotation Qty = 10
২  Confirm আগে Qty = 9 → Confirm
৩  Delivery #1: Qty 5 → Validate → Create Backorder
৪  (ঐচ্ছিক) এখন Invoice শুধু ৫ — অর্ডার শেষ নয়; বাকি Delivery থাকে
৫  Delivery #2: Qty 4 → Validate
৬  বাকি Invoice / অথবা দুই Delivery শেষে এক Invoice
৭  Delivery → Return Qty 3 → Validate
৮  Invoice → Credit Note Partial Qty 3 → Confirm
```

---

## সমস্যা হলে

| সমস্যা | কী করবেন |
|---|---|
| Backorder পপআপ আসে না | Operation Type → Create Backorder = Ask/Always |
| Validate এ পুরো ৯ বাধ্য | Done Qty ম্যানুয়ালি ৫ করুন |
| Return বাটন নেই | Delivery **Done** হতে হবে |
| Credit Note নেই | Invoice **Posted/Confirm** হতে হবে |
| রিটার্নে Lot চায় | ≡ এ CH-001 দিন |
| স্টক কম | আগে Receipt বাড়ান |
| সহজ ফ্লো ভুলে গেছি | `sales-guide.md` আবার দেখুন |

---

## দুই ফাইলের কাজ ভাগ

| ফাইল | কী শেখায় |
|---|---|
| `sales-guide.md` | সাধারণ সুন্দর ফ্লো |
| `sales-advanced-guide.md` (এই ফাইল) | আংশিক অর্ডার, split delivery, return, বিল নীতি |

এই ফাইলের কেস ক → ঘ একটার পর একটা করুন।  
ঙ অংশ রেফারেন্স — দরকার হলে খুলবেন।
