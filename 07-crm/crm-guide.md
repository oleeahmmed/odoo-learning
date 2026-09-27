# CRM — scratch থেকে সম্পূর্ণ সেটআপ
### রহিম ট্রেডার্স: Login → Install → Settings → Configuration → Leads/Pipeline

---

## কে কে?

| কে | কাজ |
|---|---|
| **রহিম** | Settings / Stages |
| **সালমা** | Lead → Opportunity → Quotation |
| ডেমো | নূর টেক, সানরাইজ, জাহিদ |

```
Demo ☐ OFF | DB: rahim_crm
Apps: Contacts + CRM + Sales (+ Inventory যদি Quotation এ স্টক পণ্য চান)
```

---

# দিন ১ — Scratch Login + Install

| Database | `rahim_crm` | Demo ☐ |
| admin@rahim.com / admin123 | Country BD |

Company রহিম ট্রেডার্স + Timezone

```
Apps → Contacts → CRM → Sales → Install
```

মেনু CRM:

```
Sales (Pipeline) | Leads | Reporting | Configuration
```

---

# দিন ১ — CRM Settings (রহিম) মিনিমাল

```
CRM → Configuration → Settings
```

| অপশন | মিনিমাল | কী হয় |
|---|---|---|
| Leads | ☑ | আলাদা Leads মেনু |
| Predictive Lead Scoring | ☐ | স্কোর |
| Recurring Revenues | ☐ | সাবস্ক্রিপশন রেভিনিউ |
| Multi Teams | ☐ | মাল্টি টিম |
| Online Appointment | ☐ | অ্যাপয়েন্টমেন্ট |

**মিনিমাল:** ☑ Leads → **Save**

---

# দিন ১ — Configuration মেনু ধরে

| মেনু | কী করবেন | কোথায় লাগে |
|---|---|---|
| Settings | উপরে | — |
| Sales Teams | New: `রহিম সেলস` ঐচ্ছিক | Opportunity Team |
| Stages | ডিফল্ট রাখুন (New→…→Won) | Pipeline কলাম |
| Activity Types | ডিফল্ট | Schedule Activity |
| Lost Reasons | New: `Too expensive` | Lost চাপলে |
| Tags | Hot / Cold ঐচ্ছিক | কার্ড ট্যাগ |
| Recurring Plans | স্কিপ | — |

**মিনিমাল:** Lost Reason একটা + Stages না মুছা।

---

# দিন ২ — Contacts Scratch

সানরাইজ স্টোর, নূর টেক শপ (Company)

---

# দিন ২ — Leads মেনু

New → নূর টেক Combo আগ্রহ | Phone | Expected Revenue 4500 → Save  
→ Convert to Opportunity  

---

# দিন ৩ — Pipeline (Sales) মেনু

কার্ড স্টেজ সরান: Qualified → Proposition  
→ New Quotation (Sales অ্যাপ) → Product/Qty → Confirm  

Won / Lost (+ Reason)

Schedule Activity: Call কালকের তারিখ

---

# Reporting মেনু

Pipeline / Forecast / Won-Lost — ডাটা থাকলে।

---

# এক নজরে

```
১  DB + CRM + Sales Install
২  Settings: Leads ☑ Save
৩  Lost Reason + Stages চেক
৪  Lead → Opportunity → Quotation → Won/Lost
```
