# Workflows ও UI UX

Desk School-এ দুটি app থাকবে। **Student and Guardian**-এ student ও guardian-এর নিজ নিজ view থাকবে। **Administration**-এ teacher, office ও leadership-এর workspace থাকবে। একই account ব্যবহার করলে active school, product ও role পরিষ্কার দেখা যাবে। Operator console platform staff-এর আলাদা view।

## MVP ও R1 flows

| Flow | Screen ও কাজ | যে result verify হবে |
|---|---|---|
| **School onboarding** | Operator school ও campus setup করেন, product enable করেন, plan দেন, School Admin invite করেন। Admin year, term, subject, section ও staff সেট করেন। | Ready status-এ school-এর users নিজের product ও role অনুযায়ী menu পান। Disabled product-এর access বন্ধ থাকে। |
| **Student intake ও placement** | Registrar CSV preview করেন, duplicate review করেন, import confirm করেন, guardian link করেন ও effective date দিয়ে class assign করেন। | History রাখা হয়, class list update হয়, guardian নিজ সন্তানের view পান, import audit ও undo থাকে। |
| **Teacher day ও attendance** | Teacher routine থেকে class খুলে roster দেখেন, present/absent/late mark করেন, offline entry save করেন, sync status দেখেন। Correction-এর ক্ষেত্রে reason দেন। | Connection ফিরলে একবারই submit। Stale list হলে refresh দরকার দেখায়। Correction-এ আগের entry থাকে। |
| **Guardian child view** | Guardian সন্তান বদলে today, attendance, notice, homework, result ও fees দেখেন। Message অথবা approved absence report পাঠাতে পারেন। | Permission প্রতি child-এ আলাদা। Push বা SMS preview-তে sensitive details সীমিত। |
| **Exam ও result** | Exam Officer schedule ও candidates setup করেন। Teacher marks entry করেন। Academic Lead review করেন। Principal approve ও publish করেন। Guardian report card দেখেন। | Zero, missing, absent ও exempt আলাদা। Recalculation মেলে। Publish-এর পরে correction নতুন version হয়। Public verification-এ সীমিত তথ্য। |
| **Desk Learn course** | Learning Admin course করেন, instructor ও learners যোগ করেন। Instructor content, assignment ও quiz publish করেন। Student lesson, grade ও progress দেখেন। | Independent course চালু থাকে। Offline saved content পড়া যায়। Quiz start ও submit online। মাঝের drop-এ উত্তর রেখে পরে sync হয়। |
| **Class linked course** | School link on করে subject ও course map করে। Approved activities select করে। | Class roster ও selected marks একবার sync হয়। Official school result review ও approval একই নিয়মে চলে। |
| **School ও course fees** | Finance price এবং bill তৈরি করে। Guardian pay করেন বা bank slip দেন। Cashier cash receipt দেন। Finance verification, refund ও closing করেন। | Pending, confirmed এবং refunded status পরিষ্কার। School/course-এর bill ও book আলাদা। Repeated notification দ্বিতীয় receipt করে না। |
| **Operations ও leadership view** | Principal approvals, attendance summary ও emergency notices দেখেন। Office role-specific reports, import result ও billing exceptions দেখেন। | Assigned school ও campus-এর data আসে। Export permission একই। |

## UI states

| State | পর্দায় কী থাকবে |
|---|---|
| **Offline** | শেষ update-এর সময়, saved content এবং submit queue। Teacher-এর কাছে **ফোনে save হয়েছে** ও **পাঠানো বাকি** আলাদা দেখা যাবে। |
| **Stale roster** | Class list update দরকার দেখাবে। Draft history রেখে current list নিয়ে নতুন করে review করা যাবে। |
| **Pending payment** | **যাচাই চলছে** দেখাবে। Confirmation বা review দেওয়ার পর balance ও receipt update হবে। |
| **Result under review** | Staff draft/review/published status বুঝবেন। Guardian approved publication দেখবেন। |
| **Permission বা subscription state** | Allowed action, safe message এবং valid navigation। অন্য school, child বা product-এর details প্রকাশ পাবে না। |
| **Empty ও error states** | কেন list খালি এবং next action কী, সেটা সহজভাবে দেখাবে। Sync retry ও error হলে current entry safe থাকবে। |
| **Notification preferences** | Channel, language, child ও quiet hours। Emergency notice-এর separate rules applied হবে। |

## Device language ও accessibility

Core flows **Android, iOS ও web**-এ agreed role scope অনুযায়ী কাজ করবে। Teacher-এর routine ও attendance phone-first। Office-এর bulk import, billing ও report কাজ web-এ। School branding sign-in-এর পরে দেখা যাবে। English ও Amharic এবং Ethiopian ও Gregorian calendar-এর conversions একই rules অনুসরণ করবে। App, public verification page, alerts ও PDF-এর copy, keyboard use, text scale, contrast ও screen reader flow QA review-তে থাকবে।

দুটি demo school দিয়ে user journey review হবে। প্রতিটি screen-এর owner একই mapping-এ feature ID, allowed role, input, success/error state, offline behaviour ও QA case রাখবেন। Detailed acceptance [QA plan](07-qa-pilot-and-acceptance.md)-এ থাকবে।

## Later workflows

R2-এ school site থেকে inquiry → application → review → offer → payment → enrollment, year rollover, consent, care ও Moodle connection। R3-এ advanced learning, certificates, parity ও helpdesk। R4-এ HR, Library, Transport, Clinic ও other operational views। R5-এ accounting, stock, payroll, site/report builders, partners ও approved AI views। প্রতিটি later screen তার feature release ও permission matrix-এর সঙ্গে যুক্ত হবে।

Source coverage Blueprint পৃষ্ঠা ৪–৫, ১৬–২২, ৩৮–৩৯, ৫১–৫৫, ৭৭–১০১।
