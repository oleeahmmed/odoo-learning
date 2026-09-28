# পার্ট ০৮ — HR & Payroll
### রূপসা গার্মেন্টস — শূন্য থেকে সম্পূর্ণ HRM

---

## Business পরিচয়

**রূপসা গার্মেন্টস** খুলনার একটা রেডিমেড পোশাক কারখানা।

```
মালিক:     করিম সাহেব
কর্মী:     ১২০ জন — সেলাই, কাটিং, ফিনিশিং, অফিস
সমস্যা:    কর্মীর তথ্য কাগজে
            হাজিরা খাতায়
            ছুটির আবেদন মৌখিকভাবে
            বেতনের হিসাব Excel-এ
            কে কতদিন কাজ করল মনে রাখতে হয়
```

---

## এই গাইডে কী কী শিখবেন

```
ধাপ ১   → Database তৈরি + Apps install
ধাপ ২   → Departments (বিভাগ)
ধাপ ৩   → Job Positions (পদ)
ধাপ ৪   → Employee তৈরি
ধাপ ৫   → Employee-র বিস্তারিত তথ্য
ধাপ ৬   → Contract (চুক্তিপত্র)
ধাপ ৭   → Working Schedule (কাজের সময়)
ধাপ ৮   → Leave Types (ছুটির ধরন)
ধাপ ৯   → Leave Allocation (ছুটি বরাদ্দ)
ধাপ ১০  → Leave Request ও Approval
ধাপ ১১  → Attendance (হাজিরা)
ধাপ ১২  → Payroll Structure (বেতন কাঠামো)
ধাপ ১৩  → Payslip (বেতন স্লিপ)
ধাপ ১৪  → Expense (কর্মীর খরচ)
ধাপ ১৫  → Recruitment (নিয়োগ)
ধাপ ১৬  → pgAdmin — Database-এ কী হলো
```

---

## ধাপ ১ — Fresh Database তৈরি করুন

```
http://localhost:8069/web/database/manager

Master Password:  admin
Database Name:    rupsha_garments
Email:            admin@rupsha.com
Password:         admin123
Language:         English
Country:          Bangladesh
Demo data:        ☑ ON
```

→ **Create Database** → Login করুন।

### Company ও Timezone সেট করুন

Settings → Companies → company নামে click:
```
Company Name:  রূপসা গার্মেন্টস
Country:       Bangladesh
Currency:      BDT
```
Save। উপরে ডানদিকে নামে click → Preferences → `Timezone: Asia/Dhaka` → Save।

### Apps Install করুন

Apps menu থেকে একটার পর একটা install করুন:

```
Employees       → Install
Time Off        → Install
Attendances     → Install
Payroll         → Install
Expenses        → Install
Recruitment     → Install
```

> প্রতিটা install শেষ হওয়ার পর পরেরটা install করুন।

---

## ধাপ ২ — Departments (বিভাগ) তৈরি করুন

রূপসা গার্মেন্টসের চারটা বিভাগ আছে।

Employees → Configuration → **Departments** → New

```
Department Name:  সেলাই বিভাগ
```
Save।

আবার New:
```
Department Name:  কাটিং বিভাগ
```
Save।

আবার New:
```
Department Name:  ফিনিশিং বিভাগ
```
Save।

আবার New:
```
Department Name:  অফিস ও প্রশাসন
```
Save।

### pgAdmin-এ দেখুন

```sql
SELECT id, name, active
FROM hr_department
ORDER BY id;
```

---

## ধাপ ৩ — Job Positions (পদ) তৈরি করুন

Employees → Configuration → **Job Positions** → New

```
Job Position:       সেলাই মেশিন অপারেটর
Department:         সেলাই বিভাগ
Expected Employees: 60
```
Save।

New:
```
Job Position:       কাটিং মাস্টার
Department:         কাটিং বিভাগ
Expected Employees: 20
```
Save।

New:
```
Job Position:       কোয়ালিটি চেকার
Department:         ফিনিশিং বিভাগ
Expected Employees: 15
```
Save।

New:
```
Job Position:       HR ম্যানেজার
Department:         অফিস ও প্রশাসন
Expected Employees: 1
```
Save।

New:
```
Job Position:       একাউন্টেন্ট
Department:         অফিস ও প্রশাসন
Expected Employees: 2
```
Save।

### pgAdmin-এ দেখুন

```sql
SELECT hj.name AS job_position, hd.name AS department,
       hj.no_of_recruitment AS expected
FROM hr_job hj
LEFT JOIN hr_department hd ON hj.department_id = hd.id
ORDER BY hd.name, hj.name;
```

---

## ধাপ ৪ — Employee তৈরি করুন (মূল তথ্য)

### প্রথম কর্মী — HR ম্যানেজার

Employees → Employees → **New**

**Work Information Tab:**

```
Employee Name:   ফারহানা বেগম
Job Position:    HR ম্যানেজার
Department:      অফিস ও প্রশাসন
Work Email:      farhana@rupsha.com
Work Phone:      01711-000101
Manager:         (খালি — তিনি সিনিয়র)
```

**Work Information (ভেতরে):**
```
Work Location:   Main Office
Work Schedule:   Standard 40 Hours/Week
```

**HR Settings Tab:**
```
Employee Type:   Employee
PIN:             1234    ← হাজিরা Kiosk-এর জন্য
```

→ **Save**

---

### দ্বিতীয় কর্মী — সেলাই অপারেটর

Employees → **New**

```
Employee Name:   রহিমা খাতুন
Job Position:    সেলাই মেশিন অপারেটর
Department:      সেলাই বিভাগ
Manager:         ফারহানা বেগম
Work Email:      rahima@rupsha.com
```

→ Save

---

### তৃতীয় কর্মী — কাটিং মাস্টার

```
Employee Name:   জাফর আলী
Job Position:    কাটিং মাস্টার
Department:      কাটিং বিভাগ
Manager:         ফারহানা বেগম
```

→ Save

আরও ২–৩ জন কর্মী বানিয়ে রাখুন practice-এর জন্য।

---

## ধাপ ৫ — Employee-র বিস্তারিত তথ্য

ফারহানা বেগম-এর record খুলুন।

### Private Information Tab

```
Private Email:      farhana.personal@gmail.com
Date of Birth:      1990-03-15
Gender:             Female
Marital Status:     Married
Certificate Level:  Bachelor

NID Number:         1234567890123
Private Phone:      01911-000101

Emergency Contact:  রফিকুল ইসলাম (স্বামী)
Emergency Phone:    01811-000102
```

### Resume Tab (অভিজ্ঞতা ও শিক্ষা)

**Experience** সেকশনে Add a line:
```
Title:       HR Executive
Company:     ABC Ltd
Description: 3 years HR experience
Date From:   01/01/2018
Date To:     12/31/2020
```

**Education** সেকশনে Add a line:
```
Title:       BBA (HRM)
School:      Khulna University
Date From:   01/01/2014
Date To:     12/31/2017
```

### Skills Tab

Add a line:
```
Skill Type:  Language
Skill:       Bangla
Level:       Expert
```

Add a line:
```
Skill Type:  IT
Skill:       Microsoft Office
Level:       Good
```

→ **Save**

---

## ধাপ ৬ — Contract (চুক্তিপত্র)

প্রতিটা কর্মীর একটা contract থাকে।

ফারহানা বেগম-এর record-এ উপরে **Contract** বাটন চাপুন → **New**

```
Contract Reference:  RUPA-001
Contract Start Date: 01/01/2023
Wage:                25000
Contract Type:       Regular Employee
```

**Save** → **Confirm**

একইভাবে রহিমা খাতুনের contract:
```
Contract Reference:  RUPA-002
Contract Start Date: 01/03/2023
Wage:                12000
Contract Type:       Regular Employee
```

### pgAdmin-এ দেখুন

```sql
SELECT
    he.name AS employee,
    hc.name AS contract_ref,
    hc.wage,
    hc.date_start,
    hc.state
FROM hr_contract hc
JOIN hr_employee he ON hc.employee_id = he.id
ORDER BY hc.date_start;
```

---

## ধাপ ৭ — Working Schedule (কাজের সময়সূচি)

রূপসা গার্মেন্টসে শনিবার থেকে বৃহস্পতিবার কাজ হয়।

Employees → Configuration → **Working Schedules** → New

```
Name:            রূপসা স্ট্যান্ডার্ড শিফট
Company:         রূপসা গার্মেন্টস
Time Zone:       Asia/Dhaka
Work Time Rate:  100%
```

**Working Hours Tab-এ** প্রতিদিনের row edit করুন:

শনিবার:
```
Day:         Saturday
Time From:   08:00
Time To:     17:00
```

রবিবার থেকে বৃহস্পতিবারও একই সময় যোগ করুন।

শুক্রবারের row delete করুন (সাপ্তাহিক ছুটি)।

→ **Save**

---

## ধাপ ৮ — Leave Types (ছুটির ধরন)

Time Off → Configuration → **Activity Types** → New

**নৈমিত্তিক ছুটি:**
```
Name:                    Casual Leave
Approval:                Time Off Officer
Leave Validation:        No Validation
Allow Negative:          No
Request Type:            Fixed by HR
Days Allowed per Year:   10
```
Save।

**অসুস্থতা ছুটি:**
```
Name:                    Sick Leave
Approval:                Time Off Officer
Days Allowed per Year:   14
```
Save।

**অর্জিত ছুটি:**
```
Name:                    Earned Leave
Approval:                Time Off Officer
Accrual:                 ☑ ON
Days per Year:           18
```
Save।

**মাতৃত্বকালীন ছুটি:**
```
Name:                    Maternity Leave
Approval:                Time Off Officer
Days Allowed per Year:   112
```
Save।

### pgAdmin-এ দেখুন

```sql
SELECT name, leave_validation_type,
       allocation_validation_type
FROM hr_leave_type
ORDER BY name;
```

---

## ধাপ ৯ — Leave Allocation (ছুটি বরাদ্দ)

প্রতিটা কর্মীকে বার্ষিক ছুটি বরাদ্দ করতে হবে।

Time Off → Managers → **Allocation Requests** → New

```
Leave Type:        Casual Leave
Allocation Mode:   Employee
Employee:          ফারহানা বেগম
Number of Days:    10
Validity Start:    01/01/2024
Validity End:      12/31/2024
```

→ **Approve**

একইভাবে রহিমা খাতুনকেও Casual Leave ১০ দিন দিন।

**সবার জন্য একসাথে করতে:**

New Allocation → Allocation Mode: **By Employee** → সব select → Approve

---

## ধাপ ১০ — Leave Request ও Approval

### রহিমা খাতুন ছুটির আবেদন করছেন

Time Off → My Time Off → **New Leave Request**

```
Leave Type:   Casual Leave
From:         আগামী সোমবার
To:           আগামী মঙ্গলবার   (২ দিন)
Reason:       পারিবারিক কাজ
```

→ **Save & Close** → **Request Approval**

### ফারহানা বেগম (HR Manager) অনুমোদন দেবেন

Time Off → Managers → **Leave Requests**

রহিমা খাতুনের আবেদন দেখবেন।

→ **Approve**

### pgAdmin-এ দেখুন

```sql
SELECT
    he.name AS employee,
    hlt.name AS leave_type,
    hl.date_from,
    hl.date_to,
    hl.number_of_days,
    hl.state
FROM hr_leave hl
JOIN hr_employee he ON hl.employee_id = he.id
JOIN hr_leave_type hlt ON hl.holiday_status_id = hlt.id
ORDER BY hl.date_from DESC;
```

**কত ছুটি বাকি আছে:**

```sql
SELECT
    he.name AS employee,
    hlt.name AS leave_type,
    hla.number_of_days AS allocated,
    hla.number_of_days - COALESCE(used.days_used, 0) AS remaining
FROM hr_leave_allocation hla
JOIN hr_employee he ON hla.employee_id = he.id
JOIN hr_leave_type hlt ON hla.holiday_status_id = hlt.id
LEFT JOIN (
    SELECT employee_id, holiday_status_id,
           SUM(number_of_days) AS days_used
    FROM hr_leave
    WHERE state = 'validate'
    GROUP BY employee_id, holiday_status_id
) used ON used.employee_id = hla.employee_id
      AND used.holiday_status_id = hla.holiday_status_id
WHERE hla.state = 'validate'
ORDER BY he.name;
```

---

## ধাপ ১১ — Attendance (হাজিরা)

### Manual হাজিরা দেওয়া

Attendances → Attendances → **New**

```
Employee:    রহিমা খাতুন
Check In:    আজকে সকাল 08:00
Check Out:   আজকে বিকাল 17:00
```
Save।

আরও ২–৩ দিনের হাজিরা দিন।

### Kiosk Mode (কারখানার গেটে)

Attendances → **Kiosk Mode**

এই screen-এ কর্মী নিজের নাম বা PIN দিয়ে Check In/Out করেন।

Practice: নিজেই **Identify Manually** চেপে রহিমা খাতুন সিলেক্ট করে Check In করুন।

### Attendance Report

Attendances → Reporting → **Attendance Analysis**

দেখবেন:
```
Employee     Date        Check In    Check Out    Worked Hours
রহিমা খাতুন  আজকে        08:00       17:00        9:00
```

### pgAdmin-এ দেখুন

```sql
SELECT
    he.name AS employee,
    ha.check_in,
    ha.check_out,
    ha.worked_hours
FROM hr_attendance ha
JOIN hr_employee he ON ha.employee_id = he.id
ORDER BY ha.check_in DESC
LIMIT 10;
```

---

## ধাপ ১২ — Payroll Structure (বেতন কাঠামো)

রূপসা গার্মেন্টসের বেতন কাঠামো বানাবেন।

### Salary Rules (বেতনের নিয়ম)

Payroll → Configuration → **Salary Rules** → New

**মূল বেতন:**
```
Name:          Basic Salary
Category:      Basic
Code:          BASIC
Condition:     Always True
Amount Type:   Fixed
```
Amount: contract.wage (auto আসবে)

**বাড়িভাড়া ভাতা:**
```
Name:          House Rent Allowance
Category:      Allowance
Code:          HRA
Amount Type:   Percentage (%)
Percentage:    40
Based on:      Basic Salary
```

**চিকিৎসা ভাতা:**
```
Name:          Medical Allowance
Category:      Allowance
Code:          MED
Amount Type:   Fixed
Amount:        1000
```

**Provident Fund (কর্তন):**
```
Name:          Provident Fund
Category:      Deduction
Code:          PF
Amount Type:   Percentage (%)
Percentage:    5
Based on:      Basic Salary
```

### Salary Structure

Payroll → Configuration → **Salary Structures** → New

```
Name:     রূপসা স্ট্যান্ডার্ড স্যালারি
Type:     Employee
```

**Salary Rules Tab** এ যোগ করুন:
- Basic Salary
- House Rent Allowance
- Medical Allowance
- Provident Fund

Save।

---

## ধাপ ১৩ — Payslip (বেতন স্লিপ) তৈরি করুন

### একজনের বেতন স্লিপ

Payroll → Payslips → **New**

```
Employee:   রহিমা খাতুন
Period:     January 2024
Structure:  রূপসা স্ট্যান্ডার্ড স্যালারি
```

→ **Compute Sheet**

দেখবেন:

```
Basic Salary:              12,000
House Rent Allowance:       4,800  (40%)
Medical Allowance:          1,000
                          -------
Gross:                    17,800

Provident Fund:              -600  (5%)
                          -------
Net Salary:               17,200
```

→ **Confirm** → **Pay**

### সবার একসাথে বেতন

Payroll → **Batch Payslips** → New

```
Name:    January 2024 Batch
Period:  January 2024
```

**Generate Payslips** → সব কর্মী select → **Generate**

সব payslip তৈরি হবে। এক এক করে Confirm করুন বা সব একসাথে।

### pgAdmin-এ দেখুন

```sql
SELECT
    he.name AS employee,
    hp.name AS payslip,
    hp.date_from,
    hp.date_to,
    hp.state
FROM hr_payslip hp
JOIN hr_employee he ON hp.employee_id = he.id
ORDER BY hp.date_from DESC;
```

**বেতনের বিবরণ:**

```sql
SELECT
    he.name AS employee,
    hpl.name AS salary_rule,
    hpl.category_id,
    hpl.total
FROM hr_payslip_line hpl
JOIN hr_payslip hp ON hpl.slip_id = hp.id
JOIN hr_employee he ON hp.employee_id = he.id
WHERE he.name = 'রহিমা খাতুন'
  AND hp.date_from = '2024-01-01'
ORDER BY hpl.sequence;
```

---

## ধাপ ১৪ — Expense (কর্মীর খরচের দাবি)

### কর্মী খরচ submit করেন

ফারহানা বেগম একটা প্রশিক্ষণে যেয়ে ৩,০০০ টাকা খরচ করলেন।

Expenses → My Expenses → **New**

```
Expense Name:    HR Training - Dhaka
Expense Date:    আজকে
Employee:        ফারহানা বেগম
Category:        Training
Total:           3000
Description:     National HR Conference, Dhaka
```

→ **Attach Receipt** (scan বা ছবি attach করুন)

→ **Submit to Manager**

### Manager অনুমোদন দেন

Expenses → Managers → **Expense Reports**

ফারহানার expense report দেখবেন।

→ **Approve** → **Post Journal Entries**

---

## ধাপ ১৫ — Recruitment (নিয়োগ প্রক্রিয়া)

রূপসা গার্মেন্টসে ৫টা নতুন সেলাই অপারেটর লাগবে।

### Job Position-এ নিয়োগ শুরু করুন

Employees → Job Positions → **সেলাই মেশিন অপারেটর**

→ **Start Recruitment**

এখন একটা নতুন **Job Application** pipeline খুলবে।

### আবেদনপত্র যোগ করুন

→ **New Application**

```
Applicant Name:  নাসরিন সুলতানা
Email:           nasrin@gmail.com
Phone:           01611-000201
Applied Job:     সেলাই মেশিন অপারেটর
Source:          বিজ্ঞাপন (খবরের কাগজ)
```

আরও ২টা application যোগ করুন।

### Pipeline-এ stage বদলান

Kanban view-এ drag করুন:
```
New Application
     ↓
Qualification (ফোনে যোগাযোগ করা হয়েছে)
     ↓
Interview (interview নেওয়া হয়েছে)
     ↓
Contract Signed (নাসরিনকে নিয়োগ)
```

### নিয়োগ দিন → Employee তৈরি

নাসরিনের application খুলুন → **Create Employee**

নতুন employee form-এ সব তথ্য ইতিমধ্যে আসবে।

---

## ধাপ ১৬ — pgAdmin — সব Table এক নজরে

### HR-এর মূল Table

```sql
SELECT
    'hr_employee'          AS table_name, COUNT(*) AS rows FROM hr_employee
UNION ALL SELECT
    'hr_department',       COUNT(*) FROM hr_department
UNION ALL SELECT
    'hr_job',              COUNT(*) FROM hr_job
UNION ALL SELECT
    'hr_contract',         COUNT(*) FROM hr_contract
UNION ALL SELECT
    'hr_leave_type',       COUNT(*) FROM hr_leave_type
UNION ALL SELECT
    'hr_leave_allocation', COUNT(*) FROM hr_leave_allocation
UNION ALL SELECT
    'hr_leave',            COUNT(*) FROM hr_leave
UNION ALL SELECT
    'hr_attendance',       COUNT(*) FROM hr_attendance
UNION ALL SELECT
    'hr_payslip',          COUNT(*) FROM hr_payslip
UNION ALL SELECT
    'hr_expense',          COUNT(*) FROM hr_expense
UNION ALL SELECT
    'hr_applicant',        COUNT(*) FROM hr_applicant;
```

### Table-এর সম্পর্ক

```
hr_employee
    ├── hr_department    → department_id
    ├── hr_job           → job_id
    ├── hr_contract      → employee_id
    ├── hr_leave         → employee_id
    ├── hr_attendance    → employee_id
    ├── hr_payslip       → employee_id
    └── hr_expense       → employee_id
```

### একটা কর্মীর সম্পূর্ণ তথ্য এক query-তে

```sql
SELECT
    he.name AS employee,
    hd.name AS department,
    hj.name AS job,
    hc.wage AS salary,
    (SELECT COUNT(*) FROM hr_attendance ha
     WHERE ha.employee_id = he.id) AS attendance_records,
    (SELECT SUM(number_of_days) FROM hr_leave hl
     WHERE hl.employee_id = he.id
       AND hl.state = 'validate') AS leaves_taken
FROM hr_employee he
LEFT JOIN hr_department hd ON he.department_id = hd.id
LEFT JOIN hr_job hj ON he.job_id = hj.id
LEFT JOIN hr_contract hc ON hc.employee_id = he.id
    AND hc.state = 'open'
WHERE he.active = TRUE
ORDER BY he.name;
```

---

## একটা Transaction-এ Database-এ কী হয়

### কর্মী তৈরি করলে:
```
hr_employee         → নতুন row INSERT
res_partner         → নতুন row INSERT (কর্মীও একজন partner)
res_users           → (যদি portal access দেওয়া হয়)
```

### Contract Confirm করলে:
```
hr_contract         → state: 'draft' → 'open'
```

### Leave Approved হলে:
```
hr_leave            → state: 'confirm' → 'validate'
hr_leave_allocation → number_of_days_display কমে
```

### Payslip Confirm করলে:
```
hr_payslip          → state: 'draft' → 'done'
hr_payslip_line     → প্রতিটা বেতন rule-এর amount
account_move        → accounting journal entry (payroll journal-এ)
```

---

## এই Part-এ যা শিখলেন

```
✓ Departments ও Job Positions তৈরি
✓ Employee তৈরি — basic ও private information
✓ Contract — বেতন ও চুক্তির তথ্য
✓ Working Schedule — কাজের সময়সূচি
✓ Leave Types — ছুটির ধরন
✓ Leave Allocation — বার্ষিক ছুটি বরাদ্দ
✓ Leave Request ও Approval workflow
✓ Attendance — manual ও Kiosk mode
✓ Payroll Structure — বেতন কাঠামো
✓ Payslip — individual ও batch
✓ Expense — কর্মীর খরচ submit ও approve
✓ Recruitment — application থেকে নিয়োগ পর্যন্ত
✓ Database-এর সব table ও সম্পর্ক
```

---

## পরের পার্ট

**`09-full-erp/full-erp-guide.md`** — নকশি টেক্সটাইল

সব module একসাথে — CRM → Sales → Manufacturing → Purchase → Inventory → HR → Accounting — একটা complete ERP flow।
