# Roles ও permissions

প্রতিটি কাজের access check হবে **account → school membership → product entitlement → role → record scope → approval** ধরে। একই account teacher ও guardian হতে পারে। Active school ও product context অনুযায়ী screen এবং permitted actions বদলাবে।

## Role matrix

| Role | Allowed scope | মূল কাজ | যে কাজ আটকে থাকবে বা আলাদা approval লাগবে |
|---|---|---|---|
| **Super Admin** | Platform operators-এর school ও plan scope | School provision, plan, product lifecycle, usage এবং operator reports | School records দেখতে scoped support session লাগবে |
| **Platform Support** | Approved school, issue ও সময় | অনুমতি নিয়ে case-specific support | School consent বা logged emergency approval, expiry ও audit প্রয়োজন |
| **School Owner / Director** | নিজের school, অনুমোদিত campuses | Principal-এর school view, plan, subscription ও owner approval। Privileged role grant, approval policy ও retention policy approve করেন। | Operator functions বন্ধ। Group grant ছাড়া অন্য school-এর access বন্ধ। Retention-এ market অনুযায়ী counsel review। |
| **Principal** | নিজের school ও assigned campuses | School summary, emergency alert, result ও published correction approve, policy অনুযায়ী finance approval | আলাদা finance role ছাড়া শুধু payment totals। Individual payment detail ও operator functions বন্ধ। Report card release করেন School Admin। |
| **School Admin** | নিজের school ও assigned campuses | Setup, users, timetable, notices ও audit। Principal-এর approval-এর পর report card তৈরি ও release। | Marks বা ledger entry বদলানো বন্ধ। Privileged role-এ Owner approval ও second factor লাগে। আলাদা learning role ছাড়া course খোলা বন্ধ। |
| **Registrar** | Assigned school বা campus | Registration, import, household, guardian rights, placement ও field-limited export। R2-তে approved result দেখে promotion। | Marks entry, ledger ও health detail বন্ধ। Custody restriction-এ document ও দ্বিতীয় approver লাগে। |
| **Department Head** | Assigned department | Subjects, schemes, lesson review ও coverage | অন্য department বা submission-এর আগের marks দেখা বন্ধ, ওই subject-এর নিজস্ব teacher হলে নিজের marks দেখতে পারবেন। |
| **Academic Lead** | Assigned grades ও classes | Missing marks ও ব্যতিক্রম review করে ফেরত দেন বা Principal-এর approval-এ পাঠান। | Fees, HR ও অন্য grade band বন্ধ। |
| **Exam Officer** | Assigned exam, term ও campus | Exam setup, seating, duty, candidates ও paper custody | Locked paper-এর content দেখা ও marks বদলানো বন্ধ। |
| **Class / Homeroom Teacher** | নিজের homeroom ও assigned classes | Attendance summary, follow-up, comments ও family communication | নিজে ওই subject-এর teacher না হলে subject submit হওয়ার আগে তার marks দেখা বন্ধ। Closed register correction-এ approval লাগে। |
| **Subject Teacher** | Assigned teaching groups ও subjects | Register, lesson plan, marks entry, correction ও subject submit | অন্য class, fees ও বিস্তারিত health/safeguarding record বন্ধ। শুধু প্রয়োজনীয় alert flag দেখা যায়। |
| **Substitute বা Delegate** | নির্দিষ্ট period, class ও সময়সীমা | Cover session, approved attendance বা অন্য granted actions | Access time-bound এবং revoke করা যায় |
| **Finance Officer** | School fees ও course fees-এর আলাদা workspace | Bill, bank claim verification, reconciliation ও refund request | অন্য product-এর book এই workspace থেকে খোলা যাবে না। Academic data-তে শুধু নাম ও class। নিজের request approve করা বন্ধ। |
| **Cashier** | নিজের counter ও shift | Cash payment, receipt ও shift closing | অন্য shift, posted receipt edit ও reduction বন্ধ। Difference supervisor দেখবেন। |
| **Learning Admin** | নিজের Desk Learn workspace | Catalogue, instructor, enrollment, pricing ও learning reports | School student records, attendance, official results ও school fees বন্ধ। |
| **Course Instructor বা Assistant** | Assigned course ও granted activities | Content, assignment, quiz, course marks ও progress | বাইরের instructor স্কুলের record খুলতে পারবেন না। Class teacher ও instructor grant আলাদা করে চালু ও শেষ হবে। |
| **Student** | নিজের school view ও active courses | Routine, lesson, quiz, feedback ও released marks | অন্য pupil-এর কাজ, marks বা people directory বন্ধ। School অনুমতি দিলেই fees দেখা যায়। |
| **Guardian** | Linked child ও সেই child-এর granted rights | Attendance, notice, published result, fees, receipts, consent ও messages | অন্য child, staff-only notes ও custody restriction-এ বন্ধ করা data দেখা যায় না। Rights প্রতি child-এ আলাদা করে test হবে। |

## পরের release-এর roles

Admission staff R2-এ applications ও offers সামলাবেন। Approved group admin স্কুলের agreed aggregates দেখবেন। Safeguarding lead, HR reviewer, nurse, librarian, transport worker, gate/front-office worker ও canteen staff নিজেদের assigned module ও record scope পাবেন। Advanced finance, procurement, operations ও partner roles অনুযায়ী নতুন actions যোগ হবে।

## কাজ করার নিয়ম

- Published result change, বড় refund, গুরুত্বপূর্ণ policy change ও custody change-এর মতো কাজে **maker এবং approver আলাদা** থাকবে।
- UI menu বা screen hide করার পাশাপাশি server, export, report, file ও event delivery-তে authorization থাকবে।
- Access record অনুযায়ী। Teacher-এর assignment বদলালে old history থাকে, current edit access নতুন assignment অনুযায়ী হয়। Guardian link বন্ধ হলে notice, export ও cached data access নতুন policy অনুযায়ী হয়।
- Subscription restriction-এর policy school data, export ও essential records-এর agreed access ধরে চলবে।
- Login, role change, access revoke, approved support session, sensitive views, result approval ও payments audit করা যাবে।

## Permission review checklist

প্রতিটি workflow-এর জন্য actor, requested action, school, product, campus, target record, allowed roles ও required approval লিখতে হবে। Check করুন actor নিজের school-এ কাজটি করতে পারে, অন্য demo school-এ scope সীমা কাজ করে, same-school অন্য class/child-তেও scope ঠিক থাকে এবং revoked বা expired account প্রত্যাশিত access পায়। প্রতি role-এর allowed ও blocked case একই user context-এ চালানো হবে। দ্বিতীয় role আলাদা করে দিলে তার আগের ও পরের ফলও রাখা হবে। V01, V02, V03, V04 এবং V15-এর evidence [QA plan](07-qa-pilot-and-acceptance.md)-এ যাবে।

Source coverage Blueprint পৃষ্ঠা ২৫–৩৪, ৯৮, ১০২–১০৩।
