# HR — সহজ গল্পে সম্পূর্ণ সেটআপ
### রহিম ট্রেডার্স: scratch থেকে Login → কর্মী খাতা

---

## গল্প: অফিসে কে কে?

| কে | ভূমিকা | Odoo তে কী |
|---|---|---|
| **রহিম ভাই** | মালিক | Employee সেটআপ, অনুমোদন |
| **সালমা** | Sales | Employee রেকর্ড |
| **জাবেদ** | Store | Employee রেকর্ড |
| **রফিক** | Production | Employee রেকর্ড |
| **করিম** | Accounts | Employee রেকর্ড |
| **নাজমা** | HR সহায়ক | নতুন কর্মী এন্ট্রি, ছুটি দেখে |

রহিম: “কর্মীদের নাম ফোনে আর কাগজে নয় — Odoo তে এক খাতা।”

```
Demo data ☐ OFF
অ্যাপ = Employees (HR বেসিক)
DB: rahim_hr অথবা rahim_full_erp এ Employees Install
Community তে Payroll/Attendance পূর্ণাঙ্গ নাও থাকতে পারে — বেসিক Employeeই যথেষ্ট
```

---

## সোনার নিয়ম

```
১  আগে Department/Job (ঐচ্ছিক) → তারপর Employee
২  Employee ≠ Contacts Customer — কর্মী আলাদা খাতা
৩  User লগইন আলাদা — Employee কার্ড শুধু তথ্য হতে পারে
৪  ছুটি/অ্যাটেনডেন্স অ্যাপ আলাদা Install লাগতে পারে
```

---

# দিন ১ — Database + Apps (রহিম)

```
http://localhost:8069/web/database/manager
```

| Field | মান |
|---|---|
| Database Name | `rahim_hr` |
| Email | `admin@rahim.com` |
| Password | `admin123` |
| Country | Bangladesh |
| Demo data | ☐ খালি |

Company: রহিম ট্রেডার্স → Timezone Asia/Dhaka

```
Apps → Employees → Install
Apps → Contacts → Install
```

চাইলে: Time Off (ছুটি) আলাদা অ্যাপ থাকলে Install।

উপরে **Employees** মেনু OK।

---

# দিন ১ বিকেল — Department ও Job (রহিম)

```
Employees → Configuration → Departments → New
```

| Name |
|---|
| Sales |
| Warehouse |
| Production |
| Accounts |

```
Configuration → Job Positions → New
```

| Job | Department |
|---|---|
| Sales Executive | Sales |
| Store Keeper | Warehouse |
| Production Staff | Production |
| Accountant | Accounts |

→ Save

**নাজমা পরে কী দেখবে:** Employee ফর্মে এই Department/Job বাছা যায়।

---

# দিন ২ — কর্মী এন্ট্রি (নাজমা)

```
Employees → New
```

ডেমো চারজন:

| Name | Job | Department | Work Email | Work Mobile |
|---|---|---|---|---|
| সালমা | Sales Executive | Sales | salma@rahim.demo | 01720000001 |
| জাবেদ | Store Keeper | Warehouse | javed@rahim.demo | 01720000002 |
| রফিক | Production Staff | Production | rafiq@rahim.demo | 01720000003 |
| করিম | Accountant | Accounts | karim@rahim.demo | 01720000004 |

প্রতিজন → **Save**

**রহিম কী দেখবে:** Employees কানবান/লিস্টে চারটা কার্ড।

```
এখন শুধু খাতা — বেতন স্লিপ Community তে সীমিত হতে পারে
Full ERP তে এই নামগুলোই দলের চরিত্র
```

---

# দিন ৩ — User লগইন (ঐচ্ছিক) (রহিম)

কর্মীকে নিজে লগইন করতে দিলে:

```
Settings → Users → New
```

সালমার ইমেইল দিয়ে User → Sales অধিকার।  
Employee ফর্মে Related User লিংক (থাকলে)।

শিখতে Admin দিয়েই সব মডিউল চালানো যায় — User পরে।

---

# দিন ৪ — Time Off (ছুটি) থাকলে

Time Off অ্যাপ Install থাকলে:

জাবেদ → ছুটির আবেদন → রহিম Approve/Refuse।

না থাকলে স্কিপ — শুধু Employee মাস্টারই Full ERP এর জন্য যথেষ্ট।

---

# কে কখন কোন মেনু

| মেনু | রহিম | নাজমা |
|---|---|---|
| Departments / Jobs | ✅ একবার | — |
| Employees New | চেক | ✅ রোজ নতুন কর্মী |
| Users | ✅ | — |
| Time Off | Approve | আবেদন দেখে |

---

# এক নজরে

```
১  DB + Employees Install
২  Department + Job
৩  Employee: সালমা, জাবেদ, রফিক, করিম
৪  (ঐচ্ছিক) User লিংক
৫  Full ERP গাইডে এই দল দিয়ে কাজ
```

## সমস্যা হলে

| সমস্যা | করণীয় |
|---|---|
| Employees মেনু নেই | Apps → Install |
| Job খালি | আগে Job Positions Save |
| Payroll নেই | Community সীমা — Enterprise/অ্যাডঅন |

পরের পার্ট **Full ERP**: এই কর্মীদের নিয়ে সব মডিউল এক খাতায়।
