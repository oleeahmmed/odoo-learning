# পার্ট ০৭ — CRM (Customer Relationship Management)
### ক্লাউড সফটওয়্যার বিডি — Software Company

---

## Business পরিচয়

**ক্লাউড সফটওয়্যার বিডি** একটা software company।

```
মালিক:    তামান্না আপু
ব্যবসা:   ERP software বিক্রি ও implementation
সমস্যা:   অনেক potential customer-এর সাথে কথা হচ্ছে
          কার সাথে কতদূর এগিয়েছি মনে থাকে না
          কোনো deal কেন হারিয়ে গেল জানি না
          আগামী মাসে কত revenue আসতে পারে বলতে পারি না
```

---

## CRM মানে কী?

```
CRM = Customer Relationship Management

Lead        → কেউ আগ্রহ দেখিয়েছে, এখনো qualify হয়নি
Opportunity → Qualify হয়েছে, deal হওয়ার সম্ভাবনা আছে
Pipeline    → সব opportunity-র একটা view, stage-এ সাজানো
Stage       → Deal কোন পর্যায়ে আছে (New, Qualified, Proposal, Won)
```

---

## ধাপ ১ — Fresh Database তৈরি করুন

```
http://localhost:8069/web/database/manager

Master Password:  admin
Database Name:    cloud_software
Email:            admin@cloud.com
Password:         admin123
Language:         English
Country:          Bangladesh
Demo data:        ☑ ON
```

→ **Create Database** → Login করুন।

### Company ও Timezone সেট করুন

Settings → Companies → company নামে click:
```
Company Name:  ক্লাউড সফটওয়্যার বিডি
Country:       Bangladesh
Currency:      BDT
```
Save। উপরে ডানদিকে নামে click → Preferences → `Timezone: Asia/Dhaka` → Save।

---

## ধাপ ২ — Apps Install করুন

```
CRM     → Install
Sales   → Install
```

---

## ধাপ ৩ — Pipeline Stages সাজান

CRM → Configuration → Stages

Default stages দেখবেন। সানরাইজের জন্য custom stages:

New বাটন বা existing edit করুন:

```
Stage 1:  New Lead          sequence: 1
Stage 2:  Meeting Scheduled  sequence: 2
Stage 3:  Proposal Sent      sequence: 3
Stage 4:  Negotiation        sequence: 4
Stage 5:  Won                sequence: 5  (Is Won Stage: ☑)
```

---

## ধাপ ৪ — Salesperson তৈরি করুন

Settings → Users → New

```
Name:     Rafiq Ahmed
Login:    rafiq@cloud.com
Password: rafiq123
Sales:    Salesperson
```

Save।

---

## ধাপ ৫ — Lead তৈরি করুন

CRM → My Pipeline → New

```
Title:          ABC Ltd - ERP Implementation
Customer Name:  ABC Limited
Company:        ABC Limited
Email:          ceo@abc.com
Phone:          01711000001
Expected Revenue: 500000
Salesperson:    Rafiq Ahmed
Priority:       ★★ (Medium)
```

**Tags:** `New Lead`

**Notes Tab:**
```
CFO Alam Bhai এর সাথে কথা হয়েছে।
তারা তাদের inventory system upgrade করতে চান।
Demo দেখতে চান।
```

Save।

---

## ধাপ ৬ — Activity Schedule করুন

Deal follow up করতে Activity দরকার।

Lead খুলুন → উপরে **Activities** বাটন → **Schedule Activity**

```
Activity Type:   Phone Call
Summary:         Demo schedule করতে call
Deadline:        আগামীকাল
Assigned to:     Rafiq Ahmed
Note:            Alam Bhai-এর সাথে demo time ঠিক করতে হবে
```

**Schedule** চাপুন।

এখন Kanban view-এ lead-এর উপর একটা Activity indicator দেখবেন।

---

## ধাপ ৭ — Lead → Opportunity (Qualify করা)

Call করার পর জানা গেল ABC Limited সত্যিই কিনতে আগ্রহী।

Lead খুলুন → **Convert to Opportunity** বাটন চাপুন।

```
Conversion Action:  Convert to Opportunity
Merge with:         (existing থাকলে)
Salesperson:        Rafiq Ahmed
Sales Team:         Sales
```

**Convert** চাপুন।

এখন এটা **Opportunity** — Pipeline-এ দেখা যাবে।

---

## ধাপ ৮ — Pipeline দেখুন (Kanban View)

CRM → My Pipeline

Kanban view-এ দেখবেন stages-এ opportunity-গুলো সাজানো।

Drag and drop করে stage বদলান:

```
New Lead → Meeting Scheduled
```

---

## ধাপ ৯ — আরও কয়েকটা Opportunity তৈরি করুন

Pipeline ভরিয়ে তুলুন practice-এর জন্য:

```
Opportunity 1:
    Title:    XYZ Corp - Sales Module
    Revenue:  200000
    Stage:    Proposal Sent

Opportunity 2:
    Title:    PQR Ltd - Full ERP
    Revenue:  800000
    Stage:    Negotiation

Opportunity 3:
    Title:    DEF Inc - HR Module
    Revenue:  150000
    Stage:    New Lead
```

---

## ধাপ ১০ — Quotation পাঠানো (CRM → Sales)

ABC Limited-এর সাথে deal এগিয়েছে। Quotation পাঠাতে হবে।

ABC Limited-এর Opportunity খুলুন।

উপরে **New Quotation** বাটন চাপুন।

```
Product:    ERP Implementation Service
Quantity:   1
Price:      500000
```

**Confirm** করুন।

এখন Opportunity-তে ফিরলে উপরে দেখবেন:
```
Quotations  1
```

---

## ধাপ ১১ — Deal Won/Lost করুন

**Won হলে:**

Opportunity উপরে **Mark Won** চাপুন।

Stage automatically "Won"-এ চলে যাবে।

**Lost হলে:**

**Mark Lost** → কারণ দিন:

```
Lost Reason:  Price too high / No budget / Chose competitor
```

---

## ধাপ ১২ — Sales Forecast Report

CRM → Reporting → Pipeline

দেখবেন:
```
Stage              Count   Expected Revenue   Probability
New Lead           2       650,000            10%
Meeting Scheduled  1       500,000            30%
Proposal Sent      1       200,000            60%
Negotiation        1       800,000            80%
```

**Weighted Revenue** = Expected Revenue × Probability

এটাই আগামী মাসের revenue forecast।

---

## ধাপ ১৩ — Lost Analysis

CRM → Reporting → Lost Reasons

দেখবেন কোন কারণে deal হারাচ্ছেন বেশি।

---

## ধাপ ১৪ — pgAdmin-এ CRM Database

```sql
-- সব opportunity দেখুন
SELECT
    cl.name AS opportunity,
    rp.name AS customer,
    cl.expected_revenue,
    cl.probability,
    cl.expected_revenue * cl.probability / 100 AS weighted_revenue,
    cs.name AS stage,
    cl.active,
    cl.type        -- lead or opportunity
FROM crm_lead cl
LEFT JOIN res_partner rp ON cl.partner_id = rp.id
LEFT JOIN crm_stage cs ON cl.stage_id = cs.id
ORDER BY cl.expected_revenue DESC;
```

```sql
-- Won deals
SELECT
    cl.name,
    rp.name AS customer,
    cl.expected_revenue,
    cl.date_closed
FROM crm_lead cl
LEFT JOIN res_partner rp ON cl.partner_id = rp.id
WHERE cl.probability = 100
   OR cs.is_won = TRUE
FROM crm_lead cl
LEFT JOIN crm_stage cs ON cl.stage_id = cs.id;
```

```sql
-- Activity summary
SELECT
    mm.summary,
    mm.activity_type_id,
    mm.date_deadline,
    ru.login AS assigned_to
FROM mail_activity mm
JOIN res_users ru ON mm.user_id = ru.id
WHERE mm.res_model = 'crm.lead'
ORDER BY mm.date_deadline;
```

---

## CRM-এর Database Tables

```
crm_lead          → সব lead ও opportunity (same table!)
                    type='lead' বা type='opportunity'
crm_stage         → Pipeline stages
crm_team          → Sales teams
mail_activity     → Scheduled activities
```

---

## এই Part-এ যা শিখলেন

```
✓ Lead vs Opportunity পার্থক্য
✓ Pipeline stages configure করা
✓ Lead তৈরি, notes রাখা
✓ Activity schedule করা (follow-up)
✓ Lead → Opportunity convert করা
✓ Kanban drag-drop দিয়ে stage বদলানো
✓ CRM → Sales Quotation connection
✓ Won/Lost করা এবং Lost Reason রাখা
✓ Sales Forecast report
✓ crm_lead, crm_stage — database tables
```

---

## পরের পার্ট

**`08-hr/hr-guide.md`** — স্টার হোটেল
