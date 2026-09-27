# পার্ট ০৮ — HR & Employee Management
### স্টার হোটেল — হোটেল ও রেস্তোরাঁ

---

## Business পরিচয়

**স্টার হোটেল** ঢাকার একটা ছোট হোটেল।

```
মালিক:    নাসির সাহেব
কর্মী:    ৩৫ জন — রিসেপশন, রুম সার্ভিস, রান্নাঘর, নিরাপত্তা
সমস্যা:   কর্মীদের তথ্য আলাদা আলাদা ফাইলে
          ছুটির আবেদন কাগজে, approve হয়েছে কিনা জানা যায় না
          কে কতদিন কাজ করেছে, overtime কত সেটা হিসাব করা কঠিন
          কর্মী recruit করতে কোথায় apply এসেছে track নেই
```

---

## ধাপ ১ — Fresh Database তৈরি করুন

```
http://localhost:8069/web/database/manager

Master Password:  admin
Database Name:    star_hotel
Email:            admin@starhotel.com
Password:         admin123
Language:         English
Country:          Bangladesh
Demo data:        ☑ ON
```

→ **Create Database** → Login করুন।

### Company ও Timezone সেট করুন

Settings → Companies → company নামে click:
```
Company Name:  স্টার হোটেল
Country:       Bangladesh
Currency:      BDT
```
Save। উপরে ডানদিকে নামে click → Preferences → `Timezone: Asia/Dhaka` → Save।

---

## ধাপ ২ — Apps Install করুন

```
Employees       → Install
Attendances     → Install
Time Off        → Install
Recruitment     → Install
Expenses        → Install
```

---

## ধাপ ৩ — Department তৈরি করুন

Employees → Configuration → Departments → New

```
Department:   Reception
Manager:      (পরে assign করবেন)
```

New:
```
Department:   Kitchen
```

New:
```
Department:   Room Service
```

New:
```
Department:   Security
```

Save সব।

---

## ধাপ ৪ — Job Positions তৈরি করুন

Employees → Configuration → Job Positions → New

```
Job Position:   Receptionist
Department:     Reception
Expected Employees: 3
```

New:
```
Job Position:   Head Chef
Department:     Kitchen
```

New:
```
Job Position:   Room Attendant
Department:     Room Service
```

Save সব।

---

## ধাপ ৫ — Employee তৈরি করুন

Employees → Employees → New

**প্রথম কর্মী (Manager):**

```
Name:           Sumaiya Begum
Job Position:   Receptionist
Department:     Reception
Work Email:     sumaiya@starhotel.com
Work Phone:     01711000101
Manager:        (খালি — তিনিই senior)
```

**Private Information Tab:**
```
Private Email:      sumaiya.personal@gmail.com
Certificate Level:  Graduate
Date of Birth:      1990-05-15
Gender:             Female
NID Number:         123456789
```

**Work Information Tab:**
```
Work Location:    Main Office
Working Hours:    Standard 40 Hours/Week
Timezone:         Asia/Dhaka
```

**HR Settings Tab:**
```
Employee Type:    Employee
```

Save।

**দ্বিতীয় কর্মী:**

```
Name:          Karim Miah
Job Position:  Head Chef
Department:    Kitchen
Manager:       (Admin user)
```

**তৃতীয় কর্মী:**

```
Name:          Rina Akter
Job Position:  Room Attendant
Department:    Room Service
```

---

## ধাপ ৬ — pgAdmin-এ Employee Data দেখুন

```sql
SELECT
    he.name AS employee,
    hj.name AS job_position,
    hd.name AS department,
    he.work_email,
    he.active
FROM hr_employee he
LEFT JOIN hr_job hj ON he.job_id = hj.id
LEFT JOIN hr_department hd ON he.department_id = hd.id
WHERE he.active = TRUE
ORDER BY hd.name, he.name;
```

---

## ধাপ ৭ — Leave Types (ছুটির ধরন) তৈরি করুন

Time Off → Configuration → Activity Types → New

```
Name:             Annual Leave
Approval:         Time Off Officer
Leave Validation: Requires approval
Max Days/Year:    15
```

New:
```
Name:             Sick Leave
Approval:         No Validation
Max Days/Year:    10
```

New:
```
Name:             Emergency Leave
Approval:         Time Off Officer
Max Days/Year:    3
```

Save।

---

## ধাপ ৮ — Leave Allocation (ছুটি বরাদ্দ করুন)

প্রতিটা কর্মীকে বার্ষিক ছুটি বরাদ্দ করতে হবে।

Time Off → Managers → Allocation Requests → New

```
Leave Type:   Annual Leave
Allocation Mode: Employee
Employee:     Sumaiya Begum
Number of Days: 15
Validity:     01/01/2024 - 12/31/2024
```

**Approve** চাপুন।

একই কাজ Karim Miah ও Rina Akter-এর জন্য করুন।

**সবার জন্য একসাথে করতে:**

New Allocation → `Allocation Mode: By Employee` → সব select করুন।

---

## ধাপ ৯ — Leave Request (ছুটির আবেদন)

Sumaiya Begum ছুটি চাইলেন।

Time Off → My Time Off → New Leave Request

```
Leave Type:   Annual Leave
From:         আগামী সোমবার
To:           আগামী শুক্রবার  (৫ দিন)
Reason:       পারিবারিক অনুষ্ঠান
```

**Save & Close** → **Request Approval**

---

## ধাপ ১০ — Leave Approve করুন

Admin/Manager হিসেবে:

Time Off → Managers → Leave Requests

Sumaiya-র আবেদন দেখবেন।

**Approve** চাপুন।

---

## ধাপ ১১ — pgAdmin-এ Leave Data

```sql
-- সব leave request দেখুন
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
ORDER BY hl.date_from;
```

```sql
-- কার কত ছুটি বাকি আছে
SELECT
    he.name AS employee,
    hlt.name AS leave_type,
    hla.number_of_days AS allocated,
    hla.number_of_days_display AS remaining
FROM hr_leave_allocation hla
JOIN hr_employee he ON hla.employee_id = he.id
JOIN hr_leave_type hlt ON hla.holiday_status_id = hlt.id
WHERE hla.state = 'validate'
ORDER BY he.name;
```

---

## ধাপ ১২ — Attendance (উপস্থিতি)

Attendances → Check In

এখানে কর্মী নিজে Check In / Check Out করতে পারেন।

### Manual Attendance Entry

Attendances → Attendances → New

```
Employee:    Karim Miah
Check In:    আজকে সকাল 09:00
Check Out:   আজকে সন্ধ্যা 18:00
```

Save।

### Attendance Report দেখুন

Attendances → Reporting → Attendance Analysis

```
Employee    Date         Check In    Check Out    Worked Hours
Karim Miah  Today        09:00       18:00        9:00
```

---

## ধাপ ১৩ — Attendance Database

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

## ধাপ ১৪ — Expense (কর্মীর খরচ)

Karim Miah বাজার করতে গিয়ে ৫,০০০ টাকা খরচ করেছেন।

Expenses → My Expenses → New

```
Expense Name:   Kitchen Ingredients Purchase
Expense Date:   আজকে
Employee:       Karim Miah
Category:       Meals & Entertainment
Total:          5000
Description:    Weekly ingredient purchase for kitchen
```

**Attach Receipt** — bill-এর photo attach করুন।

**Submit to Manager** চাপুন।

---

## ধাপ ১৫ — Recruitment (নতুন কর্মী নিয়োগ)

নতুন Receptionist দরকার।

Recruitment → Job Positions → Receptionist

**Start Recruitment** চাপুন।

**Applications Tab:**

New Application:

```
Applicant's Name:  Fatema Khatun
Email:             fatema@gmail.com
Phone:             01511000001
Applied Job:       Receptionist
Source:            LinkedIn
```

Stage দিয়ে drag করুন:
```
New Application → Qualification → Interview → Contract Signed
```

---

## ধাপ ১৬ — HR Database Structure

```
hr_employee           → কর্মীর তথ্য
hr_department         → বিভাগ
hr_job                → পদ
hr_leave              → ছুটির আবেদন
hr_leave_type         → ছুটির ধরন
hr_leave_allocation   → ছুটি বরাদ্দ
hr_attendance         → উপস্থিতি
hr_expense            → কর্মীর খরচ
hr_applicant          → Recruitment application
```

```sql
-- সব HR table এবং row count
SELECT
    'hr_employee'         AS tbl, COUNT(*) FROM hr_employee
UNION ALL SELECT
    'hr_department',      COUNT(*) FROM hr_department
UNION ALL SELECT
    'hr_leave',           COUNT(*) FROM hr_leave
UNION ALL SELECT
    'hr_leave_allocation',COUNT(*) FROM hr_leave_allocation
UNION ALL SELECT
    'hr_attendance',      COUNT(*) FROM hr_attendance;
```

---

## এই Part-এ যা শিখলেন

```
✓ Department ও Job Position তৈরি
✓ Employee তৈরি — public ও private information
✓ Leave Types configure করা
✓ Leave Allocation — বার্ষিক ছুটি বরাদ্দ
✓ Leave Request ও Approval workflow
✓ Attendance — Check In/Out
✓ Expense — কর্মীর খরচ submit ও approve
✓ Recruitment — application pipeline
✓ hr_employee, hr_leave, hr_attendance — database tables
```

---

## পরের পার্ট

**`09-full-erp/full-erp-guide.md`** — নকশি টেক্সটাইল

সব module একসাথে — একটা complete business scenario।
