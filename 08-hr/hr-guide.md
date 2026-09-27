# HR — scratch থেকে সম্পূর্ণ সেটআপ
### রহিম ট্রেডার্স: Login → Install → Settings → Configuration → Employees

---

## কে কে?

| কে | কাজ |
|---|---|
| **রহিম** | Settings / Approve |
| **নাজমা** | Employee New |
| ডেমো কর্মী | সালমা, জাবেদ, রফিক, করিম |

```
Demo ☐ OFF | DB: rahim_hr
Apps: Employees (+ Contacts)
Time Off আলাদা অ্যাপ থাকলে ঐচ্ছিক Install
```

---

# দিন ১ — Scratch Login + Install

| Database | `rahim_hr` | Demo ☐ |
| admin@rahim.com / admin123 | BD |

Company রহিম ট্রেডার্স + Timezone

```
Apps → Employees → Install
Apps → Contacts → Install
```

মেনু:

```
Employees | Department | Reporting | Configuration
```

(নাম ভার্সন অনুযায়ী একটু ভিন্ন হতে পারে।)

---

# দিন ১ — Employees Settings (রহিম) মিনিমাল

```
Employees → Configuration → Settings
```
অথবা Settings → Employees

| অপশন | মিনিমাল | কী হয় |
|---|---|---|
| Presence icons / Presence | ☑ ডিফল্ট | অনলাইন উপস্থিতি আইকন |
| Skills Management | ☐ | স্কিল ট্যাব |
| Employee Editing | ডিফল্ট | কে এডিট করতে পারে |
| Advanced Presence | ☐ | জটিল প্রেজেন্স |

**মিনিমাল:** ডিফল্ট রেখে **Save** — বেশি টিক নয়।

Time Off অ্যাপ Install করলে আলাদা Settings: Leave types — পরে।

---

# দিন ১ — Configuration মেনু ধরে

| মেনু | কী করবেন |
|---|---|
| Settings | মিনিমাল Save |
| Departments | New: Sales, Warehouse, Production, Accounts |
| Job Positions | Sales Executive, Store Keeper, Production Staff, Accountant |
| Work Locations | ঐচ্ছিক: Head Office Dhaka |
| Employment Types | ডিফল্ট (Employee) |
| Skills / Resume | Skills টিক না থাকলে স্কিপ |
| Departure Reasons | ডিফল্ট |

**মিনিমাল সিরিয়াল:** আগে Department → Job → তারপর Employee।

---

# দিন ২ — Employees মেনু

New করে চারজন:

| Name | Job | Department | Work Email |
|---|---|---|---|
| সালমা | Sales Executive | Sales | salma@rahim.demo |
| জাবেদ | Store Keeper | Warehouse | javed@rahim.demo |
| রফিক | Production Staff | Production | rafiq@rahim.demo |
| করিম | Accountant | Accounts | karim@rahim.demo |

→ Save

Department মেনু = বিভাগ অনুযায়ী কার্ড।

---

# দিন ৩ — User (ঐচ্ছিক)

Settings → Users → সালমাকে Sales অধিকার।  
Employee এ Related User লিংক।

---

# Reporting

Employee ফিল্টার/পিভট — ডাটা থাকলে।

---

# এক নজরে

```
১  DB + Employees Install
২  Settings মিনিমাল Save
৩  Departments → Jobs
৪  Employees ৪ জন
```
