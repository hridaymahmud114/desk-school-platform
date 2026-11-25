# QA pilot ও client acceptance

Acceptance-এর evidence হবে বাস্তবসম্মত data দিয়ে complete workflow, expected result ও verified outcome। Planning snapshot-এ test status pending। এখানে acceptance conditions, test IDs এবং release gates রাখা হয়েছে, যাতে backend ready হলে QA একই plan থেকে শুরু করতে পারে।

## Test setup

দুটি demo school-এ একই ধরনের sample class, course ও invoice থাকবে। Users হবে Operator, School Admin, Registrar, Teacher, Academic Lead, Principal, Finance, Learning Admin, Instructor, Student ও Guardian। একটি guardian account-এর একাধিক linked child ও আলাদা rights থাকবে। Independent course, linked course, school fees ও course fees-এর data আলাদা ধরা হবে।

Staging-এ synthetic data, real phones, slow connection profile এবং provider sandbox ব্যবহার হবে। Pilot data client-approved access ও retention মেনে আসবে। School grading sample, receipts, period closing ও published report examples আগে agree হবে।

## Foundation ও R1 tests

| Test | Pass condition |
|---|---|
| **V01 Tenant isolation** | Two-school test-এ screen, API, file, search, report ও export access প্রত্যেক school-এর scope-এ থাকে। |
| **V02 Permissions ও custody** | Matrix row ধরে allowed actions মেলে। Guardian-এর তিন child-এর rights আলাদা। Restriction সঙ্গে সঙ্গে view, notices ও lists-এ apply হয়। Learning role ও school role আলাদা। |
| **V03 Sign-in** | Invite, email/phone login, verification, reset, session revoke, expiry ও recovery work করে। |
| **V04 Approval ও audit** | Approver requester থেকে আলাদা। Sensitive action audit হয়। Policy version, effective date ও approved calculation trace করা যায়। |
| **V05 Result accuracy** | School sample-এর marks, zero, missing, absent ও exempt ধরে calculation মেলে। Exam marks একবারই result-এ আসে। |
| **V06 Report cards** | English ও Amharic এবং দুই calendar ঠিক থাকে। Early Years report narrative থাকে। Verification code কাজ করে। Correction নতুন version দেয়। |
| **V07 Offline attendance** | Offline register reconnect-এ একবার যায়। Stale roster clear response পায়। History ও corrections থাকে। ৪০ জনের class ১ মিনিটের মধ্যে mark হয়। |
| **V08 Notices** | প্রতি event ও permitted guardian-এ একটি notice। Push/SMS fallback, correction notice, emergency channels ও safe preview test। |
| **V09 Fees** | Part/overpayment, bank, cash, refund, reversal ও late/repeated confirmation-এ bills, receipts ও book মেলে। Course ও school fees আলাদা থাকে। Closing-এর সব difference explained। |
| **V10 Coursework ও quiz** | Independent course, enrollment, quiz, dropped network, single gradebook entry ও saved content offline খুলে। |
| **V11 Setup ও import** | ১,০০০ students-এর dry run, duplicate review, undo ও reconciled totals। Section change-এর history ও current permissions মেলে। Routine clash, workload ও cover notification check। |
| **V12 Load ও fairness** | Launch peak-এর তিন গুণ ৬০ মিনিট এবং ২৪ ঘণ্টার soak evidence। এক school-এর bulk job অন্য school-এর notice delay করে কি না check। |
| **V13 Recovery** | Backup isolated environment-এ restore। একটি school আলাদা করে student, result ও fees data verify। Recovery time ও data loss measure। |
| **V14 Security** | Agreed security checklist, dependency review ও authorized external test। Launch-এর সময় open high/critical findings-এর resolution evidence। |
| **V15 Privacy ও support** | Purpose অনুযায়ী child consent, audit ও retention checks। Platform support-এর অনুমোদিত, time-limited access ও log মেলে। |
| **V16 Accessibility ও language** | Core flows-এর WCAG 2.2 AA review। Amharic native review, English copy এবং দুই calendar/PDF samples approve। |
| **V17 Device ও network** | Low-cost Android, older iPhone ও pilot school network-এ time/data budget অনুযায়ী daily flow work করে। |
| **V18 Products ও plans** | Entitlement toggle-এ menus, data access ও actions update। Desk Learn আবার চালু করলে existing courses ফিরবে। Restricted school export ও agreed access ধরে রাখবে। |
| **V19 Release ও operations** | Verified build staging থেকে production-এ যাওয়ার plan, rollback rehearsal, alerts ও incident drill-এর evidence। |
| **V20 Pilot term** | Live attendance, coursework, exams, একটি result cycle ও একটি fees cycle। Published result সঠিক, fees হিসাব মেলে এবং critical issue resolve। |

## R2 থেকে R5 tests

| Test | Release | Pass condition |
|---|---|---|
| **V21 Admissions** | R2 | Website application থেকে review, offer, pay, enrollment, account ও course join। |
| **V22 Year rollover ও transfer** | R2 | Old year locked, history preserved, new year open, campus transfer ও leaving verification। |
| **V23 Moodle sync** | R2 default | Student/course/assessment mapping stable। Repeat sync duplicate করে না। Conflict, resync ও agreed update timing test। D-01 পাল্টালে timing update। |
| **V24 Parity** | R3 | Published Moodle parity matrix-এর প্রতিটি agreed row demo course-এ test। H5P, SCORM ও LTI evidence। R4 Database activity তার release-এ link হবে। |
| **V25 Appraisal privacy** | R4 | Appraisal release, acknowledgement ও response। Assigned HR ও management access এবং unrelated accounts-এর scope isolation। |
| **V26 Subscription expiry** | R2 | Date অনুযায়ী expiry, grace ও restriction, notices, preserved data ও export। |
| **G5 feature evidence** | R5 | Accounting, payroll, builders, partner ও AI deliverables-এর feature-specific flows এবং agreed benefit। AI-তে approval, consent, opt-in ও human review evidence। |

## Client scenarios ও extra checks

| Acceptance stage | Client scenarios |
|---|---|
| **G1 full** | E2E-02 Early Years, E2E-03 results, E2E-05 absence, E2E-06 partial payment, E2E-08 school isolation, E2E-12 emergency notice, E2E-13 restore এবং E2E-14 guardian rights |
| **G1 partial ও G2 full** | E2E-07 R1 section change এবং R2 full campus transfer। E2E-09 R1 operator-applied states এবং R2 expiry by rule। |
| **G2** | E2E-01 admissions এবং E2E-04 Moodle sync |
| **G3 ও G4** | E2E-11 learning parity এবং E2E-10 confidential staff appraisal |

Blueprint-এর extra checks **X-01–X-10** যথাক্রমে offline retry ও stale roster (V07), payment duplicate (V09), unique bank reference (V09), quiz network drop (V10), full-grade Amharic report (V06), custody restriction (V02), cross-school fairness (V12), ১,০০০ student import (V11), basic phone SMS/payment (V08/V09), grading policy approval ও lock (V04/V05)।

## Release gates ও evidence

| Gate | কী approve হবে |
|---|---|
| **G0 Foundation** | দুই demo school, ছোট school-day flow, pilot policy sample, permissions, offline, PDF এবং sandbox payment baseline। |
| **G1 Daily School** | R1 staging-এ V01–V19 এবং real pilot term V20। প্রথম result স্কুলের agreed manual calculation-এর সঙ্গে মিলবে। |
| **G2 Admissions ও Year Cycle** | V21–V23, V26 এবং remaining R2 paths। |
| **G3 Desk Learn Depth** | V24 parity pack ও agreed learning coverage। |
| **G4 Operations** | V25 এবং module-by-module UAT, privacy ও role checks। |
| **G5 Scale ও Intelligence** | Agreed R5 feature acceptance ও evaluated benefit। |

প্রতিটি test result-এ `Test ID | Requirement/Feature ID | Build বা PR | Tenant ও role | Expected | Actual | Evidence link | Status | Reviewer` থাকবে। Repeat test হলে আগের result history-তে থাকবে। Delayed provider, store বা pilot milestone-এর impact [launch guide](08-migration-launch-and-handover.md) ও tracker-এ বসবে।

Source coverage Blueprint পৃষ্ঠা ৪০–৪৫, ৫৩–৫৯, ১০৬, ১২৪–১২৫।
