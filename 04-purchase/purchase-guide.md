# Purchase — একদম সহজ গাইড
### রহিম ভাইয়ের কেনা: কেন সেটআপ, কী লাভ, তারপর অর্ডার

---

## আগে কোথায় ছিলেন?

Contacts, Inventory, Sales শেষ। একই database।

```
☑ রহিম ট্রেডার্স
☑ Vendor: ঢাকা ইলেকট্রনিক্স
☑ Customer: সানরাইজ স্টোর
☑ Product: Phone Charger 20W
```

আজ **Purchase** — সাপ্লায়ারের কাছ থেকে মাল কেনা।

মনে রাখবেন:

```
Sales    = কাস্টমারকে বিক্রি  → Delivery → Invoice
Purchase = Vendor এর কাছ থেকে কেনা → Receipt → Bill
```

দুটোই একই Contact খাতা ব্যবহার করে। Sales এ Customer, Purchase এ Vendor — আলাদা ডাটাবেস নয়।

---

# ধাপ ১ — Purchase ইনস্টল করো

```
Apps → Purchase → Install
```

এটা করলে Purchase অ্যাপ/মডিউল যুক্ত হবে।  
উপরে মেনুতে **Purchase** দেখা গেলে বুঝবেন ইনস্টল হয়েছে।

ইনস্টল শেষ। এবার কেনার জন্য কিছু Settings লাগবে।

---

# ধাপ ২ — Settings (মিনিমাল) + কেন লাগে

```
মেনু Settings → Purchase
```

অথবা:

```
Purchase → Configuration → Settings
```

এখানে Purchase এর Settingsগুলো আসবে।

শুরুতে সব টিক দিয়ে মাথা ঘামাবেন না।  
**একদম মিনিমাল** এর জন্য আমরা এখন শুধু এ দুটো টিক দিয়ে **Save** দিব:

```
☑ Warnings
☑ Purchase Agreements
```

→ **Save**

Save না দিলে টিক এর কাজ চালু হয় না।

নিচে বুঝবেন — প্রতিটা ফিচারের **আসল লক্ষ্য** কী, তাই সেটআপ করবেন।

---

## Warnings — টিক দিলে কী হয়, লাভ কী?

### টিক না দিলে কী হয়?

Contacts খুললে Warning সেট করার অপশন **থাকে না** (বা কাজে লাগে না)।  
সেজন্য আগে Settings এ Warnings টিক + Save লাগে।

### টিক + Save এর পর কী করবেন?

```
Contacts → Dhaka Electronics Ltd খুলুন
```

Internal Notes / Warning অংশে দেখবেন অপশন এসেছে — যেমন:

```
Warning on Purchase Order
Message লিখার ঘর
```

এখানে Warning সেট করতে পারেন।

### সেট করার পর কখন দেখা যায়?

Purchase Order, RFQ (Quotation), বা কেনার যে ফর্মগুলো আছে — সেখানে যখনই **Vendor সিলেক্ট** করবেন, তখনই সেই Warning দেখাবে।

### বাস্তবে লাভ কী? (গল্পে ভাবুন)

কোম্পানি থেকে বলল:

> ঢাকা ইলেকট্রনিক্সের অনেক পণ্যের কোয়ালিটি খারাপ।  
> MD স্যার বলেছেন — আপাতত মাল না কিনতে।

আপনি Contact এ Warning লিখলেন:

```
MD স্যার: আপাতত এই Vendor থেকে মাল কিনবেন না / কোয়ালিটি চেক করুন
```

এখন নতুন কর্মী ভুলে এই Vendor দিয়ে Purchase Order খুললেই Warning দেখবে।  
ভুলে কেনা কমে যাবে।

এরকম আরও ভালো মেসেজ হতে পারে — যেমন “আগে বাকি টাকা মিলাও”, “শুধু লট CH সিরিজ নাও”।

```
Warning এর আসল লক্ষ্য:
মানুষকে ভুলে বিপদজনক/নিষিদ্ধ Vendor বা পণ্যে অর্ডার কাটতে না দেওয়া
```

Odoo এর প্রতিটা ফিচারেরই এমন কোনো না কোনো কাজের উদ্দেশ্য আছে।  
উদ্দেশ্য না বুঝে টিক দিলে শুধু স্ক্রিন জটিল হয়।

---

## Purchase Agreements — টিক দিলে কী হয়, লাভ কী?

Settings এ **Purchase Agreements** ☑ + Save এর পর মেনুতে দেখা যায়:

```
Purchase → Orders → Purchase Agreements
```

টিক না দিলে এই মেনু সাধারণত থাকে না।

দুই ধরন থাকে। আজ শুধু **Blanket Order** ভালো করে বুঝুন।  
(Call for Tender = অনেক Vendor এর কাছ থেকে দর চাওয়া — পরে আলাদা।)

---

## Blanket Order কী? — রহিম কোম্পানির গল্পে

আপনি যেটা ইতিমধ্যে বুঝেছেন:

```
RFQ → Confirm → Receipt → Bill
```

এটা = **একবারের কেনা**। আজ ৫০ পিস লাগল → একটা অর্ডার → মাল এল → বিল। শেষ।

---

### সমস্যা কখন হয়?

রহিম ট্রেডার্স প্রতি মাসে ঢাকা ইলেকট্রনিক্স থেকে চার্জার কেনে:

```
জানুয়ারি  — ৫০ পিস
ফেব্রুয়ারি — ৪০ পিস
মার্চ       — ৬০ পিস
...
```

প্রতিবার যদি সাধারণ RFQ করেন:

```
প্রতিবার ফোন → দাম কত?
প্রতিবার নতুন করে দরদাম
দাম ভুলে আলাদা আলাদা হয়ে যেতে পারে
বছর শেষে “মোট কত কেনার চুক্তি ছিল” বোঝা কঠিন
```

MD স্যার বললেন:

> “ঢাকা ইলেকট্রনিক্সের সাথে একবারে কথা বলে ফেলো।  
> এই বছর **৬০০ পিস** নেব, দাম **২২০ টাকা** ফিক্স।  
> মাসে মাসে যত লাগে তত তুলে নেব।”

এই “একবারের বড় চুক্তি” ই = **Blanket Order**।

---

### সহজ ছবি

```
Blanket Order (বড় চুক্তির খাতা)
   │
   │  লেখা আছে: চার্জার ৬০০ পিস @ ২২০ টাকা
   │            Vendor = ঢাকা ইলেকট্রনিক্স
   │
   ├── জানুয়ারি PO  ৫০ পিস  → Receipt → Bill
   ├── ফেব্রুয়ারি PO ৪০ পিস → Receipt → Bill
   └── মার্চ PO      ৬০ পিস  → Receipt → Bill
         ...
```

| জিনিস | কী |
|---|---|
| **Blanket Order** | চুক্তি / ওয়াদা — মাল এখনই আসে না |
| **Purchase Order** | চুক্তি থেকে কাটা ছোট অর্ডার — এবার মাল আসবে |
| **Receipt / Bill** | আগের মতোই — ছোট PO এর ওপর |

```
Blanket  ≠  মাল এসে গেছে
Blanket  =  “এত পিস, এত দামে নেব” — খাতায় চুক্তি
আসল মাল = পরে ছোট PO + Receipt
```

---

### কখন কাজে লাগে?

| পরিস্থিতি | Blanket লাগে? |
|---|---|
| একবার ৫০ পিস কিনে শেষ | না — সাধারণ RFQই যথেষ্ট |
| সারা বছর একই Vendor, একই দাম, বারবার কেনা | **হ্যাঁ** |
| আগে থেকে পরিমাণ/দাম ফিক্স রাখতে চান | **হ্যাঁ** |
| কত চুক্তির মধ্যে কত নিয়েছি দেখতে চান | **হ্যাঁ** |

রহিম কোম্পানিতে: চার্জার রোজকার মাল, একই সাপ্লায়ার — Blanket মানে।  
কদাচিৎ একটা স্পেশাল জিনিস একবার কিনলে — Blanket লাগে না।

---

### Odoo তে হাতে কী করবেন? (সংক্ষেপ)

**১) চুক্তি বানান**

```
Purchase → Orders → Purchase Agreements → New
```

| Field | মান (উদাহরণ) |
|---|---|
| Agreement Type | **Blanket Order** |
| Vendor | Dhaka Electronics Ltd |
| Product | Phone Charger 20W |
| Quantity | 600 |
| Unit Price | 220 |

→ Confirm / চুক্তি চালু করুন (স্ক্রিনে যে বাটন থাকে)।

এখন মাল আসেনি — শুধু চুক্তি আছে।

**২) যখন মাল লাগে — চুক্তি থেকে ছোট PO**

একই Agreement খুলুন → **New Quotation** / Purchase Order তৈরির বাটন  
(অথবা PO তে এই Agreement লিংক করে Qty ৫০)।

ছোট PO তে দাম অনেক সময় চুক্তি থেকেই আসে (২২০)।

**৩) তারপর আগের মতো**

```
ছোট PO → Confirm → Receipt → Bill
```

বারবার দরদাম নয় — চুক্তির ভিতর থেকে তুলে নিচ্ছেন।

Odoo চুক্তিতে দেখায় মোটামুটি কত অর্ডার হয়েছে / কত বাকি (যেমন ৬০০ এর মধ্যে ৫০ গেছে)।

---

### এক লাইনে মনে রাখুন

```
সাধারণ RFQ     = আজকের এক কেনা
Blanket Order  = সারা বছরের ওয়াদা
ছোট PO         = ওয়াদা থেকে আজকের অংশ তোলা
Receipt/Bill   = আগের মতোই ছোট PO তে
```

Call for Tender আলাদা গল্প: অনেক Vendor কে দর পাঠিয়ে তুলনা — এখন স্কিপ।

---

## Bill Control — এক লাইনে জেনে রাখুন

Settings এর Invoicing অংশে দেখতে পারেন:

```
○ Ordered quantities   = অর্ডারে যা লিখেছেন সেই বিল
○ Received quantities  = যে মাল গুদামে এসেছে শুধু সেই বিল
```

চার্জারের মতো আসল মাল হলে **Received quantities** ভালো।  
কারণ: মাল না এলেও পুরো বিল কাটার ভুল কমে।

Sales এ “Delivered quantities” যে ধারণা — কেনার দিকে সেটাই প্রায় Receipt অনুযায়ী বিল।

আরও Settings (Approval, Lock, Variants…) পরে দরকার হলে চালাবেন।  
এখন মিনিমাল টিকই যথেষ্ট।

---

# ধাপ ৩ — Purchase মেনু চিনুন

```
Purchase অ্যাপ খুলুন
```

উপরে মোটামুটি:

```
Orders | Products | Reporting | Configuration
```

আজ কাজ **Orders** দিয়ে।

---

# ধাপ ৪ — Vendor ও Product চেক

নতুন বানানোর দরকার নেই — আগেই আছে।

```
Contacts → Dhaka Electronics Ltd
Products → Phone Charger 20W
```

Product এ **Can be Purchased** টিক থাকতে হবে।  
Warning সেট করে থাকলে Contact এ মেসেজ আছে কিনা এক নজর দেখে নিন।

---

# ধাপ ৫ — RFQ বানান (কেনার খসড়া)

**গল্প:** স্টক কম। রহিম ঢাকা ইলেকট্রনিক্সকে বললেন — চার্জার ৫০ পিস লাগবে।

```
Purchase → Orders → Requests for Quotation → New
```

| Field | মান |
|---|---|
| Vendor | Dhaka Electronics Ltd |
| Product | Phone Charger 20W |
| Quantity | 50 |
| Unit Price | 220 |

Vendor সিলেক্ট করার সাথে সাথে Warning সেট করা থাকলে **এখানেই** সতর্কবার্তা দেখাবে — ধাপ ২ এর লাভ এখন চোখে পড়বে।

→ **Save**

স্ট্যাটাস **RFQ** = এখনো খসড়া। মাল আসেনি। স্টক বাড়েনি।

---

# ধাপ ৬ — Confirm Order

উপরে **Confirm Order** চাপুন।

```
RFQ → Purchase Order
```

### এখানে Odoo কী করে?

```
Confirm = অটো একটা Receipt বানিয়ে রাখে
(Inventory ইনস্টল থাকলে)
```

উপরে **Receipt** বাটন দেখা যাবে।  
মাল এখনো গুদামে ঢোকেনি — শুধু Receipt এর খাতা তৈরি।

Sales মনে করুন:

```
Sales Confirm    → অটো Delivery
Purchase Confirm → অটো Receipt
```

বিল কাটলেও Receipt নিজে Done হয় না — আলাদা Validate লাগে।

---

# ধাপ ৭ — Receipt (মাল গুদামে নামানো)

PO থেকে **Receipt** বাটন চাপুন।

1. **Mark as Todo** (লাগলে)  
2. লাইনের **≡** এ Lot ও Qty দিন:

| Field | মান |
|---|---|
| Lot | CH-002 |
| Quantity | 50 |
| To | Stock বা Tak-1 |

3. **Validate** → Done

```
Products → Phone Charger 20W → On Hand আগের থেকে +50
```

```
Confirm  ≠ মাল ঢোকা
Receipt Validate = মাল ঢোকা
```

---

# ধাপ ৮ — Vendor Bill (সাপ্লায়ারের বিল)

আবার PO তে যান → **Create Bill**।

| দেখবেন | মানে |
|---|---|
| Vendor | ঢাকা ইলেকট্রনিক্স |
| Qty / Price | চার্জার × কেনার দাম |

→ **Confirm**

চাইলে **Register Payment** — টাকা দিয়েছি।

```
Sales Invoice  = কাস্টমারের কাছ থেকে পাব
Purchase Bill  = Vendor কে দিতে হবে
```

---

# ধাপ ৯ — পুরো ছবি

PO পেজে:

```
Receipt → Done
Bill    → Posted
```

এক লাইনে:

```
Install → Settings (Warning + Agreement) → RFQ → Confirm → Receipt → Bill
```

---

# এক নজরে

```
১  Apps → Purchase → Install
২  Settings → Purchase → Warnings + Agreements ☑ → Save
৩  (ঐচ্ছিক) Contact এ Warning মেসেজ — যাতে PO তে Vendor বাছলে দেখা যায়
৪  RFQ → ঢাকা ইলেকট্রনিক্স + চার্জার ৫০
৫  Confirm Order (অটো Receipt)
৬  Receipt → Lot → Validate
৭  Create Bill → Confirm
```

---

## সমস্যা হলে

| সমস্যা | কী করবেন |
|---|---|
| Purchase মেনু নেই | ধাপ ১ Install |
| Contact এ Warning অপশন নেই | Settings এ Warnings ☑ + Save |
| Vendor বাছলে Warning আসে না | Contact এ Warning মেসেজ Save করেছেন তো? |
| Agreements মেনু নেই | Purchase Agreements ☑ + Save |
| Receipt বাটন নেই | Confirm Order + Inventory ইনস্টল |
| Lot চায় | ≡ এ Lot + Qty |
| Bill+Payment করেও Receipt বাকি | স্বাভাবিক — Receipt আলাদা Validate |

---

## মনে রাখার মূল কথা

```
ফিচার টিক দেওয়ার আগে জিজ্ঞেস করুন: এর আসল লক্ষ্য কী?
Warning = ভুলে খারাপ Vendor তে অর্ডার আটকানো
Agreement = বড় চুক্তির খাতা
Receipt = মাল ঢোকানো
Bill = Vendor কে দেয় টাকার হিসাব
```

এই ফাইল উপর থেকে ধাপ ১ → ৯ করুন।  
সহজ ভাষায় উদ্দেশ্য বুঝে সেটআপ — তারপর হাতে কেনা।
