# QA pilot ও client acceptance

Acceptance-এর evidence হবে বাস্তবসম্মত data দিয়ে complete workflow, expected result ও verified outcome। Planning snapshot-এ test status pending। এখানে acceptance conditions, test IDs এবং release gates রাখা হয়েছে, যাতে backend ready হলে QA একই plan থেকে শুরু করতে পারে।

## Test setup

দুটি demo school-এ একই ধরনের sample class, course ও invoice থাকবে। Users হবে Operator, Owner, School Admin, Registrar, Teacher, Academic Lead, Principal, Finance, Learning Admin, Instructor, Student ও Guardian। একটি guardian account-এর একাধিক linked child ও আলাদা rights থাকবে। Independent course, linked course, school fees ও course fees-এর data আলাদা ধরা হবে।

Staging-এ synthetic data, real phones, slow connection profile এবং provider sandbox ব্যবহার হবে। Pilot data client-approved access ও retention মেনে আসবে। School grading sample, receipts, period closing ও published report examples আগে agree হবে।

## Foundation ও R1 tests

| Test | Pass condition |
|---|---|
| **V01 Tenant isolation** | Two-school test-এ screen, API, file, search, report ও export access প্রত্যেক school-এর scope-এ থাকে। |
| **V02 Permissions ও custody** | [Role matrix](04-roles-and-permissions.md)-এর প্রতি row-তে allowed ও blocked action মেলে। Guardian-এর তিন child-এর rights আলাদা। Custody change সঙ্গে সঙ্গে screen, notice ও pickup list-এ চলে। Learning Admin ও outside instructor school record খুলতে পারেন না। School Admin আলাদা learning role পেলে তবেই course খুলতে পারেন। |
| **V03 Sign-in** | Invite, one-time code, password, second factor, device revoke ও recovery যাচাই হবে। Reused/expired code এবং নতুন মালিকের recycled phone number দিয়ে আগের account বা linked child-এ access মিলবে না। |
| **V04 Approval ও audit** | নিজের request নিজে approve করা যায় না। Sensitive action-এর audit event অক্ষত থাকে। Grading/fee policy আগের term-এর data-তে simulate করে বদলানো result/bill-এর report রাখতে হবে। অন্য approver, effective date ও আগের হিসাব আবার চালিয়ে মেলার প্রমাণ থাকবে। Grading policy term খুললে lock হবে। |
| **V05 Result accuracy** | প্রতি launch grading policy-এর school-approved golden files-এ zero, missing, absent ও exempt-সহ সব result শেষ digit পর্যন্ত মেলে। আবার চালালেও একই result। Exam setup, timetable, seating ও invigilation দেখাতে হবে। Room, teacher বা pupil clash আটকে যাবে। Exam marks একবারই term result-এ আসবে। |
| **V06 Report cards** | Principal-এর approval-এর পর School Admin পুরো grade-এর card release করেন। Exam marks ঠিক আসে। English ও Amharic, Ethiopian ও Gregorian dates এবং code verify হয়। Pre-KG staging sample-এ rubric ও narrative থাকে, numeric mark entry/display বন্ধ। Correction approve হলে নতুন version, পুরোনো file ও family notice থাকে। |
| **V07 Offline attendance** | Offline register reconnect-এ একবার যায়। Stale roster-এর entry আটকে যায়। Correction-এ আগের entry থাকে। ৪০ জনের class ১ মিনিটের কম সময়ে mark হয়। |
| **V08 Notices** | প্রতি event ও permitted guardian-এ একটি notice। Push/SMS fallback, correction notice, emergency channels ও safe preview test। |
| **V09 Fees** | Signature, provider verification ও amount/currency/reference মেলার আগে posting বা receipt হয় না। Forged, invalid বা mismatch case-এ কোনো posting হয় না। Repeat/late confirmation-এ duplicate হয় না। Part/overpayment, bank claim, cash, refund ও reversal-এর bill/receipt/ledger মেলে। দুই fee book ও receipt series আলাদা থেকে একই দিনের closing মেলে। Credit transfer এবং unpaid fee-র course-access rules [fee policy](06-school-policies-and-integrations.md) অনুযায়ী test হবে। অমীমাংসিত difference থাকলে day close হয় না। |
| **V10 Coursework ও quiz** | Independent course-এ instructor assign ও key দিয়ে enrollment হয়। পুরো class একই মিনিটে timed quiz শুরু করে ধীর network-এ। Connection কেটে ফিরলেও সব উত্তর থাকে। Marks শুধু course gradebook-এ একবার যায়। Saved content offline খোলে। |
| **V11 Setup ও import** | ১,০০০ students-এর dry run, duplicate review, undo ও reconciled totals। Section change-এর effective date থেকে marks, attendance ও access ঠিক। Curriculum, allocation ও workload সেট করে overloaded teacher-এর timetable আটকে যায়। Cover দিলে substitute ও class দুজনেই notice পায়। |
| **V12 Load ও fairness** | Foundation-এ confirm করা নিচের objectives তিন গুণ launch peak-এ ৬০ মিনিট ধরে মেলে এবং ২৪ ঘণ্টার soak report থাকে। এক school-এর bulk send-এ অন্য school-এর attendance notice দেরি হয় না। |
| **V13 Recovery** | Isolated restore থেকে একটি school extract করে result, student ও financial records recovery point-এর সঙ্গে মেলে। Critical data loss সর্বোচ্চ ১৫ মিনিট ও recovery সর্বোচ্চ ৪ ঘণ্টার target-এর সঙ্গে measured result তুলনা হবে। |
| **V14 Security** | API ও web-এর জন্য নির্ধারিত OWASP ASVS, দুই mobile app-এর জন্য OWASP MASVS checklist pass। ব্যবহৃত version evidence-এ থাকবে। Dependency ও image scans clean। API, web, mobile, uploads ও payment callbacks নিয়ে external security test শেষে G1-এর আগে open critical বা high finding-এর সংখ্যা শূন্য। |
| **V15 Privacy ও support** | Purpose অনুযায়ী child consent, audit ও retention checks। Platform support-এর অনুমোদিত, time-limited access ও log মেলে। |
| **V16 Accessibility ও language** | Core flows-এর WCAG 2.2 AA review। Amharic native review, English copy এবং দুই calendar/PDF samples approve। |
| **V17 Device ও network** | Low-cost Android, older iPhone ও pilot school network-এ time/data budget অনুযায়ী daily flow work করে। |
| **V18 Products ও plans** | Effective date অনুযায়ী plan ও entitlement বদলায়। Desk Learn বন্ধে তার menu, screen ও API বন্ধ হয়। আবার চালু করলে আগের courses ফিরে আসে। Restricted school-এর সব data থাকে, read-only ও export চলে। Reactivation-এ service ফেরে। |
| **V19 Release ও operations** | Staging-এ যাচাই করা একই signed build approved promotion-এ production-এ গেছে, তার build ID ও deployment record মেলে। Rollback-এ data loss শূন্য। Controlled fault-এ alert পৌঁছায় ও incident drill-এর record থাকে। |
| **V20 Pilot term** | Live attendance, coursework, exams, একটি result cycle ও একটি fees cycle। Published result সঠিক, fees হিসাব মেলে এবং critical issue resolve। |

### V12-এর সময় ও service targets

| যে কাজ মাপা হবে | Blueprint baseline (পৃ. ৪৪) |
|---|---|
| Server read, quiz answer save, register submit | প্রতিটির ৯৫তম percentile (p95) যথাক্রমে ৮০০ ms-এর কম, ৫০০ ms-এর কম এবং ১ সেকেন্ডের কম |
| Family home usable | Reference phone ও network-এ ৩ সেকেন্ডের মধ্যে |
| Event থেকে in-app notice | ১ মিনিটের মধ্যে |
| এক grade-এর report cards | School-এর সঙ্গে Foundation-এ ঠিক করা সময়ের মধ্যে |
| Moodle marks staging-এ দেখা | ৫ মিনিটের মধ্যে। V23-এ connected release-এর sync দিয়েও মাপা হবে। |
| Core portal ও API availability | মাসে ৯৯.৫%, agreed maintenance বাদে। Monthly service report-এ আলাদা করে মাপা হবে। |

## R2 থেকে R5 tests

| Test | Release | Pass condition |
|---|---|---|
| **V21 Admissions** | R2 | Website application থেকে review, offer, pay, enrollment, account ও course join। |
| **V22 Year rollover ও transfer** | R2 | Old year locked, history preserved, new year open, campus transfer ও leaving verification। |
| **V23 Moodle sync** | R2 default | Student/course/assessment mapping stable। Repeat sync duplicate করে না। Conflict, resync ও agreed update timing test। D-01 পাল্টালে timing update। |
| **V24 Parity** | R3 | Published Moodle parity matrix-এর প্রতিটি agreed row demo course-এ test। H5P, SCORM ও LTI evidence। R4 Database activity তার release-এ link হবে। |
| **V25 Appraisal privacy** | R4 | Staff appraisal-এর সঙ্গে একমত না হয়েও acknowledgement দিয়ে আলাদা লিখিত response দিতে পারেন। Assigned reviewer ছাড়া unrelated staff, pupil ও guardian কোনো route-এ appraisal দেখতে পারেন না। |
| **V26 Subscription expiry** | R2 | Date অনুযায়ী expiry, grace ও restriction, notices, preserved data ও export। |
| **G5 feature evidence** | R5 | Accounting, payroll, builders, partner ও AI deliverables-এর feature-specific flows এবং agreed benefit। AI-তে approval, consent, opt-in ও human review evidence। |

## Client scenarios ও extra checks

| Acceptance stage | Client scenarios |
|---|---|
| **G1 full** | E2E-02 Early Years, E2E-03 results, E2E-05 absence, E2E-06 partial payment, E2E-08 school isolation, E2E-12 emergency notice, E2E-13 restore এবং E2E-14 guardian rights |
| **G1 partial ও G2 full** | E2E-07 R1 section change এবং R2 full campus transfer। E2E-09 R1 operator-applied states এবং R2 expiry by rule। |
| **G2** | E2E-01 admissions এবং E2E-04 Moodle sync |
| **G3 ও G4** | E2E-11 learning parity এবং E2E-10 confidential staff appraisal |

Blueprint-এর অতিরিক্ত X-scenarios (পৃ. ৫৮–৫৯) এই table-এ track হবে। Test চালানোর পরে evidence ঘরে build, expected/actual ফল ও reviewer-এর link বসবে। `DVL-014` এই table ও test results একসঙ্গে ধরে।

| ID | Test | যে প্রমাণে pass হবে | Status | Evidence |
|---|---|---|---|---|
| X-01 | V07 | Offline register একবার sync, stale roster ফিরিয়ে দেওয়া হয় | Pending | — |
| X-02 | V09 | Forged/invalid, repeat ও late confirmation-এ অননুমোদিত বা দ্বিতীয় posting/receipt শূন্য | Pending | — |
| X-03 | V09 | Finance bank claim statement দেখে approve করেন, একই reference দ্বিতীয়বার ব্যবহার আটকে যায় | Pending | — |
| X-04 | V10 | পুরো class একই মিনিটে timed quiz শুরু করে, ধীর network ও disconnect-এ উত্তর হারায় না | Pending | — |
| X-05 | V06 | পুরো grade-এর Amharic report-এ Ethiopian dates ও verification code মেলে | Pending | — |
| X-06 | V02 | Custody restriction সঙ্গে সঙ্গে screen, notice ও pickup list-এ চলে | Pending | — |
| X-07 | V12 | এক school-এর bulk send অন্য school-এর attendance notice দেরি করায় না | Pending | — |
| X-08 | V11 | ১,০০০ pupil import-এ dry run, duplicate review, undo ও totals মেলে | Pending | — |
| X-09 | V08, V09 | Basic phone-এ guardian SMS পান এবং reference দিয়ে payment করেন | Pending | — |
| X-10 | V04, V05 | আগের term-এ grading policy simulate হয়, বদলানো results review করে দ্বিতীয় ব্যক্তি approve করেন, policy lock হয় | Pending | — |

## Release gates ও evidence

| Gate | কী approve হবে |
|---|---|
| **G0 Foundation** | F1-এ pilot discovery, baseline decisions, threat model ও matrix drafts। F2-তে real phone-এ offline register, ধীর network-এ quiz spike, Amharic PDF ও Ethiopian dates, forged/repeated sandbox confirmation, D-05 identity comparison ও staging pipeline। F3-তে দুই school আলাদা রেখে class import, offline register sync, quiz তৈরি ও দেওয়া, real policy-তে result calculate ও approve, card issue, invoice, verified online payment ও bank claim reconcile। প্রতিটির evidence লাগবে। |
| **G1 Daily School** | R1 staging-এ V01–V19 এবং real pilot term-এ V20 pass। প্রথম cycle-এ school-এর manual হিসাব parallel-এ চলে, পার্থক্য ব্যাখ্যা করে তারপর result release হয়। ভুল published result, unexplained fee difference ও unresolved critical incident শূন্য। |
| **G2 Admissions ও Year Cycle** | V21–V23, V26 ও remaining R2 flows। A2-তে Early Years care ও support cases আলাদা demo হবে। যেমন guardian consent সহ media (EYS-002), restricted safeguarding case ও access log (BHV-007)। V22 দিয়ে rollover, transfer ও leaving মাপা হবে। |
| **G3 Desk Learn Depth** | V24 parity pack ও agreed learning coverage। |
| **G4 Operations** | V25 এবং module-by-module UAT, privacy ও role checks। |
| **G5 Scale ও Intelligence** | Feature ID ধরে module demo ও agreed benefit। যেমন `G5/INV-001`-এ stock issue ও transfer-এর পর দুই location-এর balance মেলে। `G5/AIX-004`-এ school opt-in, logged use ও human decision দেখাতে হবে। |

প্রতিটি test result-এ `Test ID | Requirement/Feature ID | Build বা PR | Tenant ও role | Expected | Actual | Evidence link | Status | Reviewer` থাকবে। Repeat test হলে আগের result history-তে থাকবে। Delayed provider, store বা pilot milestone-এর impact [launch guide](08-migration-launch-and-handover.md) ও tracker-এ বসবে।

Source coverage Blueprint পৃষ্ঠা ৪০–৪৫, ৫৩–৫৯, ১০৬, ১২৪–১২৫।
