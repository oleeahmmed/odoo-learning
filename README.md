# Odoo Learning Path
### শূন্য থেকে ERP Implementation — ধাপে ধাপে

---

## এই Learning Path কীভাবে কাজ করে

প্রতিটা পার্টে **একটা বাস্তব business** আছে।
সেই business-এর **real problem** আছে।
Odoo দিয়ে সেই problem কীভাবে solve হয় সেটা **হাতে-কলমে** দেখবেন।

প্রতিটা পার্ট **Fresh Setup** — আগের database-এর দরকার নেই।
প্রতিটা পার্ট শেষে আপনি একটা **নির্দিষ্ট business scenario** পুরোপুরি চালাতে পারবেন।

---

## পার্টগুলো

```
00-setup/
    odoo-setup-guide.md         ← সব পার্টের আগে একবার করুন
                                   Odoo install, database তৈরি, pgAdmin basics

01-contacts/
    contacts-guide.md           ← রহিম ট্রেডার্স
                                   Business: যেকোনো ব্যবসার মানুষ ও প্রতিষ্ঠান manage
                                   শিখবেন: Customer, Vendor, Company, Individual,
                                            Address, Tags, portal access

02-inventory/
    inventory-guide.md          ← মেডিকো ফার্মা
                                   Business: ওষুধ পরিবেশক
                                   শিখবেন: Warehouse, Location, Product, Lot,
                                            Serial Number, Multiple Warehouse,
                                            BIN Card, Stock Valuation, Routes,
                                            Reordering, Internal Transfer,
                                            Receipt, Delivery — behind the scenes

03-sales/
    sales-guide.md              ← টেকনো গ্যাজেট
                                   Business: Electronics বিক্রেতা
                                   শিখবেন: Quotation, Sales Order, Delivery,
                                            Invoice, Payment, Pricelist,
                                            Customer Portal — সম্পূর্ণ flow

04-purchase/
    purchase-guide.md           ← গ্রিন ফুড সাপ্লাই
                                   Business: খাদ্যপণ্য পরিবেশক
                                   শিখবেন: RFQ, Purchase Order, Receipt,
                                            Vendor Bill, Payment, Vendor Pricelist,
                                            Purchase → Inventory connection

05-accounting/
    accounting-guide.md         ← সানরাইজ রিয়েল এস্টেট
                                   Business: Property ব্যবসা
                                   শিখবেন: Chart of Accounts, Journal, Invoice,
                                            Bill, Payment, Reconciliation,
                                            Reports — Odoo accounting flow

06-manufacturing/
    manufacturing-guide.md      ← ঢাকা গার্মেন্টস
                                   Business: পোশাক কারখানা
                                   শিখবেন: Bill of Materials, Manufacturing Order,
                                            Work Center, Raw Material → Finished Product,
                                            Manufacturing → Inventory connection

07-crm/
    crm-guide.md                ← ক্লাউড সফটওয়্যার বিডি
                                   Business: Software company
                                   শিখবেন: Lead, Opportunity, Pipeline,
                                            Activity, CRM → Sales connection,
                                            Sales forecast

08-hr/
    hr-guide.md                 ← স্টার হোটেল
                                   Business: হোটেল ও রেস্তোরাঁ
                                   শিখবেন: Employees, Departments, Attendance,
                                            Leave Management, Payroll basics,
                                            Expense

09-full-erp/
    full-erp-guide.md           ← নকশি টেক্সটাইল
                                   Business: টেক্সটাইল কোম্পানি (সব module একসাথে)
                                   শিখবেন: CRM → Sales → Inventory → Purchase →
                                            Manufacturing → Accounting — সম্পূর্ণ ERP flow
```

---

## শেখার নিয়ম

**১। প্রতিটা পার্ট শুরুর আগে:**
```
http://localhost:8069/web/database/manager
→ Create Database
→ নতুন নাম দিন (যেমন: medico_pharma, techno_gadget)
→ Demo data: ON
```

**২। প্রতিটা transaction-এর পর pgAdmin-এ দেখুন:**
```
database-এর table-এ গিয়ে দেখুন কী কী বদলাল
```
এটা করলে Odoo-র behind the scenes পরিষ্কার বুঝবেন।

**৩। Notebook-এ লিখুন:**
```
Business problem কী?
Odoo-তে কোন module solve করল?
কোন document তৈরি হলো?
কোন table-এ কী data গেল?
```

---

## কোন Business-এর জন্য কোন Module

```
শুধু পণ্য বেচি
    → Contacts + Sales + Inventory + Invoicing

পণ্য কিনি ও বেচি
    → Contacts + Purchase + Sales + Inventory + Invoicing + Accounting

নিজেরা তৈরি করি
    → সব উপরেরটা + Manufacturing

Service দিই
    → Contacts + CRM + Sales + Project + Timesheet + Invoicing

মানুষ manage করি
    → HR + Attendance + Leave + Payroll

সব একসাথে
    → সব module
```

---

## প্রতিটা Part-এর Format

প্রতিটা guide-এ থাকবে:

```
১. Business পরিচয়
২. Business problems
৩. Fresh database setup
৪. Module install করুন
৫. ধাপে ধাপে practical workflow
৬. প্রতিটা step-এ pgAdmin-এ কী দেখবেন
৭. Behind the scenes — database-এ কী হলো
```

---

## শুরু করুন

**প্রথমে:** `00-setup/odoo-setup-guide.md`

**তারপর:** আপনার চাহিদা অনুযায়ী যেকোনো পার্ট।

সব পার্ট ক্রমানুসারে করলে সবচেয়ে ভালো।
কিন্তু শুধু Inventory দরকার হলে `02-inventory` থেকেই শুরু করতে পারেন।
