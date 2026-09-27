# Odoo 17 — Windows Setup Guide
### হাতে ধরে, ধাপে ধাপে — দোকান খোলার আগে ঘর বানানো

---

## একটু গল্প দিয়ে শুরু

ধরুন আপনি **রহিম সাহেব**। নবাবপুরে একটা পাইকারি দোকান খুলবেন — নাম **রহিম ট্রেডার্স**।

দোকান খোলার আগে কী লাগে?

```
১. দোকানের ঘর (কম্পিউটারে সফটওয়্যারগুলো)
২. চাবি ও তালা (password, user)
৩. সাইনবোর্ড ও খাতা (database, company নাম)
৪. দোকানের কপি / নিরাপত্তা (backup)
```

Odoo মানে আপনার **ডিজিটাল দোকানের খাতা**।  
এই guide সেই খাতা চালু করা পর্যন্ত।  
Contacts, Inventory এসব — পরে, যখন দোকান খুলে বসবেন।

---

## এই guide শেষে আপনি যা পারবেন

```
☑ PostgreSQL চালু (ডাটা রাখার জায়গা)
☑ Odoo চালু (http://localhost:8069)
☑ একটা test database বানানো ও login
☑ Company নাম ও Timezone সেট করা
☑ Backup রাখা ও Restore করা
```

**এখনো করবেন না:** Contacts / Sales / Inventory install  
সেগুলো পরের পার্টের গল্প।

---

## মোট ধাপ — মানচিত্র

```
ঘর বানানো
  ১–৫   → PostgreSQL, Python, Git, wkhtmltopdf
  ৬–১০  → Odoo ফোল্ডার, কোড, venv, library, config
  ১১–১২ → Odoo চালু, browser খোলা

দোকানের খাতা খোলা
  ১৩ → Database তৈরি + Login
  ১৪ → Company setup
  ১৫ → Timezone
  ১৬ → Backup
  ১৭ → Restore
  ১৮ → pgAdmin-এ এক নজর
```

প্রতিটা ধাপ শেষে ছোট **চেক** আছে — সেটা পাস করে পরের ধাপে যান।

---

# অংশ ১ — ঘর বানানো (সফটওয়্যার)

---

## ধাপ ১ — PostgreSQL (ডাটার গুদাম)

**গল্প:** Odoo নিজে কিছু মনে রাখে না। সব খাতা রাখে PostgreSQL নামের গুদামে।  
গুদাম না থাকলে খাতা লেখার জায়গা নেই।

### কী করবেন

1. Download:  
   `https://www.enterprisedb.com/downloads/postgres-postgresql-downloads`
2. **PostgreSQL 15** (বা 16/18) — Windows x86-64 বেছে নিন
3. Install চালান:

```
Password দিন → মনে রাখুন (উদাহরণ: postgres123)
Port → 5432 (বদলাবেন না)
Stack Builder → টিক তুলে দিন → Finish
```

### চেক

Start Menu → **pgAdmin 4** খুলতে পারলে OK।

> পরে path দেখবেন `C:\Program Files\PostgreSQL\15\`  
> ১৮ ইনস্টল করলে `15` এর জায়গায় `18` লিখবেন।

---

## ধাপ ২ — Odoo-র জন্য আলাদা চাবি (user `odoo17`)

**গল্প:** গুদামের মাস্টার চাবি আপনার (`postgres`)।  
Odooকে আলাদা কর্মচারী বানাবেন — নাম `odoo17`, চাবি `odoo123`।  
সে শুধু নিজের কাজের খাতা খুলতে পারবে।

### কী করবেন

1. **pgAdmin 4** খুলুন → Master Password দিন (pgAdmin-এর নিজস্ব)
2. বাঁয়ে: `Servers → PostgreSQL →` double-click → DB password দিন
3. `Login/Group Roles` → Right-click → **Create → Login/Group Role**

```
General    → Name: odoo17
Definition → Password: odoo123
Privileges → Can login? = Yes     ← খুব জরুরি
             Create databases? = Yes
             Superuser? = No
```

4. **Save**

### চেক

Login/Group Roles list-এ `odoo17` দেখা যাচ্ছে।

> **ভুল হলে:** `odoo17` → Properties → Privileges → Can login? = Yes

---

## ধাপ ৩ — Python 3.11 (Odoo যে ভাষায় চলে)

**গল্প:** Odoo Python দিয়ে লেখা। ভুল version হলে অনেক প্যাকেজ ভাঙে।  
তাই **৩.১১** লাগবে — ৩.১২ বা ৩.১৪ নয়।

### কী করবেন

1. Download: `https://www.python.org/downloads/release/python-3119/`
2. Windows installer (64-bit)
3. প্রথম স্ক্রিনে:

```
☑ Add python.exe to PATH   ← অবশ্যই
→ Install Now
```

### চেক

```cmd
py -3.11 --version
```

দেখতে হবে: `Python 3.11.x`

---

## ধাপ ৪ — Git (কোড আনার টুল)

**গল্প:** Odoo-র কোড GitHub-এ রাখা। Git দিয়ে সেটা নামিয়ে আনবেন।

1. Download: `https://git-scm.com/download/win`
2. সব **Next** (default ঠিক আছে)

### চেক

```cmd
git --version
```

---

## ধাপ ৫ — wkhtmltopdf (PDF ছাপার মেশিন)

**গল্প:** ইনভয়েস PDF বানাতে Odoo এই টুল ব্যবহার করে।

1. Download:  
   `https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6-1/wkhtmltox-0.12.6-1.msvc2015-win64.exe`
2. Default রেখে Install

### চেক

```cmd
"C:\Program Files\wkhtmltopdf\bin\wkhtmltopdf.exe" --version
```

---

## ধাপ ৬ — ফোল্ডার বানান

**গল্প:** কম্পিউটারে একটা আলমারি — সব Odoo জিনিস এখানে।

```cmd
cd C:\
mkdir odoo-dev
cd odoo-dev
mkdir backup
```

### চেক

File Explorer-এ `C:\odoo-dev\` এবং ভিতরে `backup` দেখা যাচ্ছে।

---

## ধাপ ৭ — Odoo কোড নামান (clone)

```cmd
git clone https://github.com/odoo/odoo.git --depth 1 --branch 17.0 C:\odoo-dev\odoo17
```

৫–২০ মিনিট লাগতে পারে। অপেক্ষা করুন।

### চেক

```cmd
dir C:\odoo-dev\odoo17
```

দেখবেন: `odoo`, `addons`, `requirements.txt`, `odoo-bin`

> ফোল্ডার নাম যেন ঠিক `odoo17` হয় (`odoo-devodoo17` হলে rename করুন)।

---

## ধাপ ৮ — Virtual environment (আলাদা রান্নাঘর)

**গল্প:** বাড়িতে একটা রান্নাঘর শুধু Odoo-র জন্য।  
অন্য Python প্রজেক্টের মসলা মিশবে না।

```cmd
cd C:\odoo-dev
py -3.11 -m venv odoo17-venv
C:\odoo-dev\odoo17-venv\Scripts\activate
```

সামনে দেখাবে: `(odoo17-venv)`

### চেক

```cmd
python --version
```

`Python 3.11.x` হতে হবে।

> নতুন CMD খুললে আবার `activate` করতে হয়।

---

## ধাপ ৯ — লাইব্রেরি বসান

`(odoo17-venv)` দেখা অবস্থায়:

```cmd
python -m pip install --upgrade pip
python -m pip install -r C:\odoo-dev\odoo17\requirements.txt
```

৫–১৫ মিনিট। শেষে error না থাকলে OK।

সমস্যা হলে:

```cmd
python -m pip install psycopg2-binary
```

---

## ধাপ ১০ — Config file (Odoo-র ঠিকানা খাতা)

**গল্প:** Odooকে বলে দিতে হবে — গুদাম কোথায়, কোন পোর্ট, PDF মেশিন কোথায়।

Notepad খুলুন → নিচের লেখা হুবহু পেস্ট করুন:

```ini
[options]
addons_path = C:\odoo-dev\odoo17\addons
db_host = localhost
db_port = 5432
db_user = odoo17
db_password = odoo123
db_template = template1
admin_passwd = admin
http_port = 8069
logfile = C:\odoo-dev\odoo17.log
wkhtmltopdf = C:\Program Files\wkhtmltopdf\bin\wkhtmltopdf.exe
```

Save As:

```
নাম:     odoo17.conf
টাইপ:    All Files (*.*)
জায়গা:  C:\odoo-dev\
```

### সহজ অর্থ

| লাইন | মানে |
|---|---|
| `db_user` / `db_password` | গুদামের কর্মচারী চাবি |
| `admin_passwd` | Database Manager-এর মাস্টার চাবি (পরে backup-এ লাগে) |
| `http_port = 8069` | Browser-এ এই নম্বর দিয়ে ঢুকবেন |
| `db_template = template1` | Windows-এ database তৈরির সমস্যা এড়ায় |

### চেক

`C:\odoo-dev\odoo17.conf` আছে — নামে `.txt` নেই।

---

## ধাপ ১১ — Odoo প্রথমবার চালু

```cmd
C:\odoo-dev\odoo17-venv\Scripts\activate
python C:\odoo-dev\odoo17\odoo-bin -c C:\odoo-dev\odoo17.conf
```

এই লাইন দেখলে সফল:

```
HTTP service (werkzeug) running on ...:8069
```

**এই কালো জানালা বন্ধ করবেন না** — বন্ধ মানে দোকান বন্ধ।

সমস্যা:

| দেখলে | করণীয় |
|---|---|
| could not connect | Services → PostgreSQL → Start |
| not permitted to log in | pgAdmin → odoo17 → Can login? Yes |
| port already in use | `netstat -ano \| findstr :8069` তারপর সেই PID kill |

---

## ধাপ ১২ — Browser খুলুন

Odoo চালু রেখে:

```
http://localhost:8069
```

**Database Manager** পেজ দেখা যাবে — এখান থেকেই খাতা খোলা, backup, restore।

সরাসরি Manager:

```
http://localhost:8069/web/database/manager
```

---

# অংশ ২ — খাতা খোলা (Database থেকে Backup)

---

## ধাপ ১৩ — Test database বানান + Login

**গল্প:** আসল দোকান খোলার আগে একটা **ডামি খাতা** বানাবেন — নাম `odoo_test`।  
শুধু দেখার জন্য যে সবকিছু চলছে। রহিম ট্রেডার্সের আসল খাতা পরে Contacts পার্টে।

### ফর্ম পূরণ

```
Master Password:  admin          ← config-এর admin_passwd
Database Name:    odoo_test
Email:            admin@test.com
Password:         admin123
Language:         English
Country:          Bangladesh
Demo data:        ☑ ON           ← নমুনা ডাটা থাকবে, শিখতে সুবিধা
```

→ **Create Database** → কয়েক মিনিট অপেক্ষা (বন্ধ করবেন না)।

### Login

```
Email:     admin@test.com
Password:  admin123
```

### চেক

```
☑ Odoo-র ভিতরের পর্দা এসেছে
☑ উপরে Settings দেখা যাচ্ছে
☑ Contacts না থাকা স্বাভাবিক — এখনো App বসায়নি
```

**উদাহরণ:** নতুন দোকানের চাবি পেলেন, ভিতরে ঢুকলেন — তাক খালি হতে পারে। পরে তাক সাজাবেন।

---

## ধাপ ১৪ — Company setup (সাইনবোর্ড)

**গল্প:** খাতায় এখনো লেখা “YourCompany”।  
সাইনবোর্ড বদলে **Test Company BD** লিখবেন — শুধু practice।

### কী করবেন

```
Settings → Companies → YourCompany-এ ক্লিক
```

লিখুন:

```
Company Name:  Test Company BD
Country:       Bangladesh
Currency:      BDT
```

→ **Save**

### চেক

আবার Companies খুলে নাম `Test Company BD` দেখা যাচ্ছে।

> রহিম ট্রেডার্স নামটা Contacts পার্টে আসল database-এ দেবেন।  
> এখানে শুধু **কীভাবে সাইনবোর্ড বদলাতে হয়** শিখলেন।

---

## ধাপ ১৫ — Timezone (ঘড়ি ঠিক করা)

**গল্প:** ঘড়ি আমেরিকার সময় থাকলে বাংলাদেশের তারিখ ভুল দেখাবে।

```
উপরে ডানদিকে আপনার নাম/ছবি → Preferences
Timezone: Asia/Dhaka
→ Save
```

### চেক

Preferences আবার খুলে `Asia/Dhaka` আছে।

---

## ধাপ ১৬ — Backup (খাতার ফটোকপি)

**গল্প:** দোকানের খাতার ফটোকপি আলমারিতে রাখা।  
কম্পিউটার নষ্ট হলেও খাতা ফেরত আনা যায়।

### কী করবেন

1. খুলুন: `http://localhost:8069/web/database/manager`
2. `odoo_test` → **Backup**
3. দিন:

```
Master Password:  admin
Format:           zip
```

4. Download হওয়া `.zip` রাখুন: `C:\odoo-dev\backup\`

### চেক

`backup` ফোল্ডারে একটা zip ফাইল আছে।

**উদাহরণ:** আগামীকাল ভুল করে ডাটা মুছলে এই zip দিয়ে ফিরে আসবেন।

---

## ধাপ ১৭ — Restore (ফটোকপি থেকে ফেরত)

**গল্প:** খাতা নষ্ট → ফটোকপি থেকে নতুন খাতা খোলা।

### কী করবেন

1. Database Manager → **Restore**
2. ফর্ম:

```
Master Password:  admin
File:             আগের zip বেছে নিন
Database Name:    odoo_test_restored   ← নতুন নাম (পুরোনোটার উপর চাপাবেন না)
```

3. Restore শেষে login:

```
Database: odoo_test_restored
Email:    admin@test.com
Password: admin123
```

### দ্রুত কপি (Duplicate)

শুধু practice কপি চাইলে:

```
odoo_test → Duplicate → নাম: odoo_test_copy → Continue
```

### চেক

`odoo_test_restored` (বা copy) তে login হচ্ছে।

---

## ধাপ ১৮ — pgAdmin-এ এক নজর (ঐচ্ছিক কিন্তু ভালো)

**গল্প:** UI-তে যা দেখেন, আসলে গুদামের টেবিলে থাকে। একবার চোখ রাখুন।

```
pgAdmin → Databases → odoo_test → Schemas → public → Tables
```

| Table | গল্পে মানে |
|---|---|
| `res_company` | সাইনবোর্ড / কোম্পানি তথ্য |
| `res_partner` | সব মানুষ ও প্রতিষ্ঠানের খাতা (পরে Contacts) |
| `res_users` | কে login করতে পারে |
| `ir_module_module` | কোন App বসেছে / বসেনি |

**এখনই করুন:** `res_company` → View/Edit Data → নামে `Test Company BD` দেখুন।

Query Tool-এ:

```sql
SELECT name, state FROM ir_module_module
WHERE name = 'contacts';
```

সাধারণত `uninstalled` — ঠিক আছে। Contacts পার্টে install করবেন।

---

## প্রতিদিন দোকান খোলা / বন্ধ

**খোলা:**

```cmd
cd /d C:\odoo-dev
C:\odoo-dev\odoo17-venv\Scripts\activate.bat
python odoo17\odoo-bin -c odoo17.conf
```

Browser: `http://localhost:8069`

**বন্ধ:** কালো জানালায় `Ctrl + C`

---

## Setup শেষ — চেকলিস্ট

```
☑ Odoo চালু (8069)
☑ odoo_test তৈরি + login
☑ Company = Test Company BD
☑ Timezone = Asia/Dhaka
☑ Backup zip রাখা আছে
☑ Restore বা Duplicate একবার চেষ্টা
```

সব টিক পড়লে ঘর তৈরি শেষ। এখন আসল দোকানের গল্প:

```
→ 01-contacts/contacts-guide.md
```

সেখানে রহিম ট্রেডার্সের খাতা খুলে **মানুষ ও কোম্পানি** লিখতে শিখবেন।

---

## সমস্যা হলে এক লাইনে

| সমস্যা | সমাধান |
|---|---|
| Master Password ভুল | `admin` দিন (config-এর মতো) |
| Database create error / collation | config-এ `db_template = template1` |
| odoo17 login হয় না | Can login? = Yes |
| PDF এ `RPC_ERROR 204` | IDM বন্ধ করুন (download manager) |
| Python ভুল version | `py -3.11` দিয়ে venv আবার বানান |
