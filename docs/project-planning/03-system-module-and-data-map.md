# System module ও data map

Blueprint-এর **৫৩টি module** নিচে ১০টি primary system-এ রাখা হয়েছে। *MVP proposal* count [features tracker](tracker/features.csv) থেকে নেওয়া। এতে সম্পূর্ণ candidate ও partial row দুটোই আছে। Module-এর বাকি feature তার original release বা approved override অনুযায়ী থাকবে।

## Shared structure

School হবে tenant। Campus সেই school-এর ভেতরের scope। একজন person-এর account একাধিক school-এর membership নিতে পারে। প্রতিটি school-এর role ও product access আলাদা। Apps ও operator console shared services ব্যবহার করবে। Interactive requests, background jobs ও document generation আলাদা capacity-তে চলতে পারবে।

স্কুলের data, cache, files, queries, export, notifications ও background jobs-এ একই tenant boundary থাকবে। Tenant-এর নিয়ম service ও database-এ enforce হবে। Files scope check-এর পর short-lived links দিয়ে দেওয়া হবে। Report সংশ্লিষ্ট role-এর scope ধরে চলবে। Shared infrastructure ব্যবহার করলেও school-এর record আলাদা থাকবে।

## Module inventory

### 01 Multi-tenant Platform

Platform school, campus, plan, entitlement, audit ও shared security rules manage করবে।

| Module | দায়িত্ব | Blueprint releases | MVP proposal |
|---|---|---|---|
| **C1** Products, plans and entitlements | Separate products, plan, usage ও module access | R1 | 9 feature row |
| **C2** Operator console | Operator console, school lifecycle, usage, restore ও support | R1, R2, R3, R5 | 8 feature row |
| **C9** Platform services | School isolation, audit, policies, approvals, files ও localization | R1, R2 | 11 feature row |
| **C10** Security, privacy and reliability | Security, privacy, performance, backup ও release reliability | R1 | 6 feature row |

### 02 Identity and Permissions

Identity account ও permissions-এর owner। Access যাচাই হবে tenant, role ও record অনুযায়ী।

| Module | দায়িত্ব | Blueprint releases | MVP proposal |
|---|---|---|---|
| **C3** Identity, roles and access | Account, roles, school membership, session ও guardian access | R1, R2, R5 | 10 feature row |

### 03 Student Records and Admissions

Student ও guardian profile, admission এবং placement history-এর মূল record এখানে।

| Module | দায়িত্ব | Blueprint releases | MVP proposal |
|---|---|---|---|
| **A7** Admissions and enrolment | Quick registration এবং পরে inquiry, offer ও enrollment | R1, R2 | 1 feature row |
| **A8** Student records | Student record, household, documents, import ও history | R1, R2 | 7 feature row |
| **A13** Promotion, year rollover and leaving | Re-enrollment, promotion, rollover, leaving ও clearance | R2, R3 | পুরো module backlog |

### 04 Academics and Attendance

Class, routine, teaching group ও attendance এখানে। Class list Student Records-এর placement থেকে আসবে।

| Module | দায়িত্ব | Blueprint releases | MVP proposal |
|---|---|---|---|
| **A1** Academic management | School structure, year, term, grade, department ও policies | R1, R2 | 5 feature row |
| **A2** Class and section management | Class, section, class list, placement view ও capacity | R1, R2 | 4 feature row |
| **A3** Subject and course management | Subject list, teaching hours, electives ও credits | R1, R2, R3 | 2 feature row |
| **A4** Curriculum and lesson planning | Curriculum, scheme, lesson plan ও coverage | R1, R2, R3, R4, R5 | 2 feature row |
| **A5** Schedule management | Calendar, timetable, clashes, change ও substitute | R1, R2, R4 | 6 feature row |
| **A6** Teacher management | Teacher records, allocation, workload, availability ও leave | R1, R2, R4 | 4 feature row |
| **A9** Attendance and pickup | Attendance, offline correction, absence alerts ও pickup | R1, R2, R4 | 6 feature row |
| **A14** Early years | Early years observation, narrative report ও daily care | R1, R2 | পুরো module backlog |

### 05 Exams and Results

Exam records, calculation, approved result snapshot ও public verification এখানে।

| Module | দায়িত্ব | Blueprint releases | MVP proposal |
|---|---|---|---|
| **A10** Assessment and gradebook | Assessment plan, gradebook, policy ও marks calculation | R1, R2, R3 | 6 feature row |
| **A11** Exam management | Exam setup, routine, seats, duty, paper ও marks | R1, R2, R3, R4, R5 | 6 feature row |
| **A12** Results and report cards | Result review, publish, correction, report card ও transcripts | R1, R2, R3 | 5 feature row |

### 06 Desk Learn

Independent course, learners, content, progress ও course marks এখানে থাকবে।

| Module | দায়িত্ব | Blueprint releases | MVP proposal |
|---|---|---|---|
| **B1** Learning management | Learning Admin workspace, course settings ও categories | R1, R2, R3 | 2 feature row |
| **B2** Course catalogue and courses | Independent courses, catalogue, class link ও course states | R1, R2, R3 | 4 feature row |
| **B3** Instructors and course roles | Course instructor, assistant, observer ও permissions | R1, R3 | 3 feature row |
| **B4** Enrolment and learners | Course enrollments, periods, cohorts ও outside learners | R1, R2 | 2 feature row |
| **B5** Learning content | Learning materials, save offline, accessible media ও video | R1, R2, R3 | 2 feature row |
| **B6** Assignments | Assignment, submission, feedback, rubric ও extension | R1, R2, R3 | 2 feature row |
| **B7** Quizzes, question bank and online exams | Quiz, attempts, question bank ও online exam | R1, R2, R3, R5 | 4 feature row |
| **B8** Activities | Course announcement এবং পরে forum, activities ও compatibility | R1, R2, R3, R4 | 1 feature row |
| **B9** Live classes | Live class scheduling, attendance ও recordings | R2, R3 | পুরো module backlog |
| **B10** Progress, completion and certificates | Completion, progress, competencies, badges ও certificates | R1, R3, R4 | 1 feature row |
| **B11** Course gradebook | Course gradebook ও school ফলাফলে selected marks | R1, R2, R3 | 2 feature row |
| **B12** Collaboration | Course calendar, instructor office hours ও booking | R1, R2 | 1 feature row |
| **B14** Learning reports | Participation, completion, marks ও learning analytics | R1, R2, R3 | 1 feature row |

### 07 Fees and Accounts

School receivables ও course-fee books আলাদা। Platform subscription-এর owner C2।

| Module | দায়িত্ব | Blueprint releases | MVP proposal |
|---|---|---|---|
| **A16** School fees and collection | School fees, invoice, collections, refunds ও reconciliation | R1, R2 | 9 feature row |
| **A17** Accounting, budget and payroll | Receivables, accounting, budget ও payroll | R1, R2, R4, R5 | 1 feature row |
| **B13** Course fees and e-learning billing | Course pricing, invoices, receipts, access ও refunds | R1, R2, R3, R5 | 6 feature row |

### 08 Apps Communication and Reports

Apps, notification ও reporting অন্য modules-এর contract থেকে data নেবে।

| Module | দায়িত্ব | Blueprint releases | MVP proposal |
|---|---|---|---|
| **A27** Helpdesk and service requests | School helpdesk, requests, complaints ও restricted escalation | R2, R3 | পুরো module backlog |
| **C4** Communication | In-app notice, push, SMS, email ও guardian messaging | R1, R2, R3 | 7 feature row |
| **C5** The two apps and their workspaces | Student and Guardian app, Administration app ও workspaces | R1, R2, R3, R4, R5 | 12 feature row |
| **C6** Reporting and analytics | Scoped reports, exports, analytics ও report builder | R1, R2, R3, R4, R5 | 2 feature row |

### 09 School Operations

Operational modules নির্দিষ্ট role, policy ও tenant scope-এর মধ্যে কাজ করবে।

| Module | দায়িত্ব | Blueprint releases | MVP proposal |
|---|---|---|---|
| **A15** Behaviour, discipline and student support | Behaviour, support plan, safeguarding ও follow-up | R2, R3 | পুরো module backlog |
| **A18** HR and staff appraisal | Staff contracts, recruitment, appraisal ও private HR records | R4 | পুরো module backlog |
| **A19** Library | Library catalogue, loans, fines ও clearance | R4 | পুরো module backlog |
| **A20** Transport | Routes, student handover, transport attendance ও tracking | R4, R5 | পুরো module backlog |
| **A21** Clinic and wellbeing | Restricted clinic records, visits, consent ও emergency summary | R4 | পুরো module backlog |
| **A23** Front office, facilities and boarding | Visitors, gate pass, rooms, facility ও boarding | R4, R5 | পুরো module backlog |
| **A24** Clubs, sports, trips and events | Clubs, trips, sport, attendance ও event consent | R3, R4 | পুরো module backlog |
| **A25** Canteen, shop and prepaid balances | Meals, prepaid balance, spending limits ও shop | R4, R5 | পুরো module backlog |
| **A26** Alumni | Alumni opt-in, records ও events | R5 | পুরো module backlog |

### 10 Extensions and Advanced Features

External provider, optional feature ও future plugin-এর আলাদা interface থাকবে।

| Module | দায়িত্ব | Blueprint releases | MVP proposal |
|---|---|---|---|
| **A22** Inventory, procurement and assets | Stock, procurement, suppliers ও assets | R5 | পুরো module backlog |
| **A28** School website | School public site, content approval ও site builder | R2, R5 | পুরো module backlog |
| **B15** Moodle: connected mode and migration | Moodle mapping, sync, reconciliation ও course migration | R2, R3 | পুরো module backlog |
| **C7** AI assistance | Optional AI, human review ও usage controls | R5 | পুরো module backlog |
| **C8** Integrations and API | App API, events, payment/SMS providers ও external integrations | R1, R2, R3, R4, R5 | 4 feature row |

## Data owner ও module link

| Data | Authoritative owner | কীভাবে অন্য কাজ ব্যবহার করবে |
|---|---|---|
| School profile, product ও plan | Platform C1 এবং C2 | Identity membership ও প্রত্যেক product request-এ access check |
| Account, membership ও guardian rights | Identity C3 | School role grant ও child link দেখে app, file, report ও notice authorization |
| Student ও placement history | Student Records A8 | A2 class list ও teaching group-এ effective dates ধরে roster দেখাবে। Registrar placement change করবেন। |
| Subject, teacher assignment ও class session | Academics A3, A5 ও A6 | Attendance-এ current roster, Results-এ teacher scope, Desk Learn-এ optional class linkage |
| Attendance records | Attendance A9 | Notice delivery event, guardian day view এবং published attendance reports |
| School marks ও official result | Assessment ও Results A10–A12 | Subject Teacher submit করেন, Academic Lead review করেন, Principal approve করেন। School Admin approved snapshot থেকে report card release করেন। Correction-এ পুরোনো version থাকে। |
| Course content, enrollment ও marks | Desk Learn B2–B7 এবং B11 | Class link on থাকলে শুধু school-selected activity marks Results-এ যাবে। Independent course-এর নিজস্ব gradebook থাকবে। |
| School invoice ও course invoice | A16 এবং B13 | Shared payment integration ব্যবহার করবে। পৃথক invoice series, ledger ও closing থাকবে। |
| School subscription | Platform C1 ও C2 | Product usage ও billing state থেকে entitled actions। Student-এর official result access agreed policy অনুসরণ করবে। |
| Files, reports ও notifications | C9, C6 ও C4 | Tenant ও role scope যাচাই করে docs, exports এবং messages পৌঁছাবে। Sources-এ record authority থাকবে। |

## Integration rules

- Module অন্য module-এর data ব্যবহার করলে agreed service interface, export বা event দিয়ে নেবে। Data বদলাবে owner module।
- Event-এ tenant, record ID, event ID ও version থাকবে। Relevant permission ও tenant context verify হবে। Retry হলে একই কাজ দ্বিতীয়বার apply হওয়ার ঝুঁকি test হবে।
- Clinic, safeguarding ও appraisal-এর মতো sensitive data restricted scope-এ থাকবে। সাধারণ summary-তে policy অনুযায়ী দরকারি তথ্যটুকু যাবে।
- Changes-এর আগে module owner, API shape, permissions এবং data migration impact লিখতে হবে। Module আর feature scope [release ও branch plan](02-mvp-release-and-branch-plan.md)-এর সঙ্গে মিলবে।

## Future plugins

Optional Library বা Transport-এর মতো module, SMS/payments-এর মতো provider এবং Moodle-এর মতো external system আলাদা integration type। প্রতিটির tenant activation, role permissions, data contract, dependency, version, quotas, audit ও retry rules সংজ্ঞায়িত থাকবে। Module deactivate হলে existing record কীভাবে থাকবে এবং access কীভাবে বন্ধ হবে, সেটি rollout-এর আগে agree হবে। Dedicated deployment এবং marketplace যথাক্রমে তাদের release scope অনুযায়ী আসবে।

Source coverage Blueprint পৃষ্ঠা ১২–১৫, ২৩–২৬, ৩৫–৩৭, ৪৬, ৭৫–১০১।
