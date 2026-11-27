# System 4 · Academics and Attendance · UI ও UX design plan

**আপনার review-এর জন্য সংশোধিত খসড়া · ২৬ নভেম্বর ২০২৬।** Registrar একজন pupil-কে class-এ বসানোর আগে A1/A2 দিয়ে class ও কার্যকর তারিখ যাচাই করেন। Teacher ঐ দিনের তালিকা থেকে attendance নেন। পরে mark সংশোধন হলে প্রযোজ্য guardian খবর পান। [A1/A2 slice 0](#slice-0) · [২৯টি Feature ID-র কাজ ও acceptance check](IMPLEMENTATION_PLAN.md#feature-trace)

## ১. কী দেখা গেছে, কী সিদ্ধান্ত হয়েছে

| ধরন | এই design-এর ভিত্তি |
|---|---|
| দেখা প্রমাণ | Planning checkout `main` @ `2f0b1e9`-এ Docs 02–08, tracker, integration handoff ও System 1–3-এর সর্বশেষ design/implementation plan আছে। স্থানীয় **Desk-SchoolERP-Technical-Blueprint-v2-team.pdf** (২২ নভেম্বর ২০২৬)-এর ছাপা পৃষ্ঠা মিলিয়ে নিচে দেওয়া হয়েছে। ২৯টি row-ই School Management-এর Blueprint **R1**। Tracker-এ ২৩টি **MVP candidate**, ৬টি **MVP partial**, implementation status **Needs repository verification**। |
| আপনার সিদ্ধান্ত ও লিখিত baseline | OD-13 অনুযায়ী Platform → Identity 1a → A1/A2 slice 0 → Students। System 5 ও 6-এর পূর্ণ planning-এর আগে §৩-এর contract design review হবে। D-01-এর pilot একটি campus-এ, native Desk Learn সহ। School ও Learn-এর entitlement এবং teacher ও instructor-এর grant আলাদা। |
| Design-এ ধরা অনুমান | প্রথম demo upper-primary ধরে আঁকা। নীল একটি নমুনা section। Slice 0-তে whole-section required subject group-এর নিয়ম review-এর জন্য ধরা হয়েছে। Layout, field, UI library, route ও API-র নাম বাস্তব backend এবং app পেলে মিলবে। |
| খোলা সিদ্ধান্ত ও প্রমাণ | আপনি school-এর class list দিয়ে pilot-এ KG আছে কি না নিশ্চিত করবেন। School academic lead grade band অনুযায়ী অনুমোদিত status, daily/period register-এর নিয়ম ও cut-off দেবেন (Blueprint পৃ. ৩০–৩১)। Class/group ও roster contract-এ Students/Academics এবং notice dispatch-এ Identity/Apps-এর সম্মতি বাকি। Approved calendar, native Amharic copy, device ও network evidence-ও বাকি। |

**Screen ও journey audit।** Doc 05-এ Administration app-এ setup ও teacher day এবং Student and Guardian app-এ child switch-এর journey আছে। Students-এর design F5/S13-এ profile ও import result থেকে A2-র dated roster read-only panel খোলে। Identity OD-18/19-এ family detail ও dispatch-এর নিয়ম আছে। Platform-এর design-এ active context, entitlement ও denied state আছে। এগুলি planning evidence। Runnable screen, screenshot, navigation tree, component/token source, actual backend, API এবং schema এই checkout-এ **Pending**। Backend এলে একই কাজ staff web, teacher phone ও family app-এ চালিয়ে path, build, screenshot ও অমিল নথিভুক্ত হবে। [Workflows](../../05-workflows-and-ui-ux.md) · [Platform design](../01-platform/UI_UX_DESIGN_PLAN.md) · [Identity design](../02-identity/UI_UX_DESIGN_PLAN.md) · [Students design](../03-student-records/UI_UX_DESIGN_PLAN.md)

## ২. কার কোন পর্দা

Administration web-এ **Academic Setup**, **Classes and Subjects**, **Teachers**, **Timetable**, **Attendance**। Teacher-এর Administration app খুলবে **আজকের ক্লাস** দিয়ে। Student and Guardian app-এ pupil-এর নিজের দিন অথবা guardian-এর নির্বাচিত সন্তানের দিন দেখাবে। প্রতিটি পর্দায় active school, প্রযোজ্য campus, ভূমিকা ও তারিখ থাকবে। School বদলালে আগের search, খোলা detail, cache ও draft অন্য context-এ যাবে না। Blueprint পৃ. ৪–৫, ২৫–২৬, ৩১–৩২, ৩৮–৩৯। [Role matrix](../../04-roles-and-permissions.md)

| কে | শুরু থেকে ফল | সীমা যেভাবে বোঝা যাবে |
|---|---|---|
| School Admin | নিজের campus-এর setup সম্পূর্ণ করেন, class teacher/subject allocation দেন, routine প্রকাশ ও cover দেন। Attendance-তে submitted register, report ও correction queue দেখেন। | Server-granted campus/action-এ কাজ। Closed register-এ teacher-এর কারণ ও পুরোনো/নতুন mark দেখে অন্য ব্যক্তি হিসেবে approve বা reject করেন। নিজের request নিজে approve করা বন্ধ। |
| Registrar | Students-এ placement preview → A1/A2 target যাচাই → A8 confirm → date বেছে Classes-এ ফল মেলান। | Assigned campus-এর read ও field-limited export যেখানে grant আছে। Placement বদলানো বা undo করতে A8 import/history-তে ফেরেন। Roster-এ mark বা timetable publish করার action নেই। |
| Assigned teacher | Class teacher নিজের section-এর attendance, marks due ও notices দেখেন। Subject teacher নিজের group-এর lesson বানান, published session থেকে register নেন ও correction চান। Co-teacher নিজের বরাদ্দ কাজ পান। | Class overview-তে খুলতে পারা তথ্য C3 scope ধরে আসে। অন্য subject-এর unsubmitted marks, staff profile-এর গোপন তথ্য বা অন্য class-এর roster দেখা যায় না। Co-teacher একই session-এ দ্বিতীয় register খুলতে পারেন না। |
| Substitute | Cover notice → তারিখ, period, class ও মেয়াদ → ঐ session-এর register। সক্রিয় cover-এ online roster নিয়ে offline mark করতে পারেন। | A5-এর current session/cover ও C3-র সময়বাঁধা grant দুটোই লাগবে। Reconnect-এর আগে cover শেষ বা revoke হলে সেই queue submit হবে না। পাশের period, অন্য class ও মূল teacher-এর private notes তাঁকে দেখানো হয় না। |
| Pupil | নিজের published routine-এ subject, room ও পরিবর্তন দেখেন। School policy যেখানে দেয়, নিজের নিশ্চিত attendance-ও দেখতে পান। | D-16 ও IAM-001 অনুযায়ী upper-primary pupil নিজের যাচাইকৃত phone/email দিয়ে sign in করেন। তা ছাড়া pilot-এ guardian-এর মাধ্যমে খবর পান। ছোটদের IAM-011 class-card sign-in R2। অন্য pupil-এর তালিকা ও draft routine staff scope-এ থাকে। |
| Guardian | Child switch → অনুমোদিত সন্তানের published routine, নিশ্চিত attendance বা notice → correction-এর পরে নতুন status। | C3 প্রতি child ও কাজের right মেলায়। Attendance detail ও attendance notice-এর জন্য ঐ সন্তানের বর্তমান attendance right লাগে। Channel preference delivery-তে প্রযোজ্য। Unlinked sibling, contact-only adult ও restricted guardian detail পান না। Sensitive detail খোলা বা app resume-এ online check লাগে। Check সম্পন্ন হওয়ার আগ পর্যন্ত last update time ছাড়া আগের sensitive detail দেখা যায় না। |

**Denied screen।** একই school-এর ভুল campus/class, অন্য school বা unlinked child খুললে “পাওয়া যায়নি” দেখান। নাম, count, roster diff বা পুরোনো screen-এর data থাকবে না। অনুমোদিত class খোলা থাকলেও, যেমন Registrar-এর ক্ষেত্রে timetable publish, নিষিদ্ধ staff action-এ “এই কাজের অনুমতি নেই” দেখান। নিজের অনুমোদিত কাজ দিয়ে শুরু ও শেষ করে এই দুই ফল পরীক্ষা করুন। [C3 actions ও V01/V02-এর প্রত্যাশিত ফল](IMPLEMENTATION_PLAN.md#scope-checks) · [Identity D-16 ও OD-10](../02-identity/UI_UX_DESIGN_PLAN.md)

<a id="slice-0"></a>

## ৩. A1/A2 slice 0 · আলাদা review-এর জন্য

**আপনি যেভাবে review করবেন।** Academic Setup → Students-এর placement panel → dated roster ধরে দেখুন, কোন class বৈধ, কোন দিন কোন pupil আছে এবং কার হাতে write থাকে। ন্যূনতম slice-এ year/grade/section চেনা, date যাচাই ও roster read থাকবে। Setup wizard-এর বাকি ধাপ, class overview, teacher day বা attendance delivery-র জন্য এই review অপেক্ষা করবে না। ভিত্তি SIS-003, CLS-001, SET-002, SET-003, SET-004, SET-011, PLT-003। Blueprint পৃ. ১২–১৬, ৭৮। [Interfaces](../../integration/INTERFACES.md) · [OD-13 ও merge order](../../integration/DEPENDENCIES.md) · [Students T5 ও T12](../03-student-records/IMPLEMENTATION_PLAN.md)

**পর্দা।** Admin school/campus-এর নিচে year → grade → section দেখতে পান। Section detail-এ নাম, স্থায়ী reference, প্রযোজ্য room, কখন থেকে কার্যকর এবং কোন year-এর তা থাকে। একই নামের section বাছতে label-এর পাশে campus, year ও grade থাকে। Registrar-এর A8 form-এ date ও class বাছলে effective date-এ বৈধ target-এর summary আসে। ভুল date বা অচল target হলে সংশ্লিষ্ট field-এ কারণ ও পুনরায় বাছার উপায় আসে। অন্য school-এর ID দিলে নাম বা existence বোঝা যায় না।

| Interface ও owner | Input ও যাচাই | Reviewer যে ফল দেখবেন |
|---|---|---|
| Class reference · A1/A2 | Context-এর school ধরে campus → year → grade → section-এর parent মিলবে। School-এর civil date year-এর সীমায় এবং target ঐ দিন কার্যকর হতে হবে। Group দেওয়া থাকলে সেটির section ও date-ও মেলে। | Stable IDs ও reference version। Preview-এর পর reference বদলালে A8 confirm-এ আবার যাচাই হয়। অন্য scope-এর ID-তে safe not_found। অনুমোদিত target-এ ভুল date হলে date field-এ সমস্যা দেখায়। |
| Placement ও group membership · A8 | Registrar valid section, pupil ID, effective date ও expected version দিয়ে A8-এ confirm করেন। Year শুরুর আগের date ফেরত যায়। চলতি year-এর অতীত date-এ Students OD-05-এর সতর্কতা আসে। | A8 placement ID/version দেয়। একই confirm-এ একই ফল। Undo/void ও history A8-এ। আলাদা pupil বা group membership পরিবর্তনও A8-এর history-তেই লেখা হয়। |
| Dated roster · A2 | Authorized section বা teaching group + `asOf` civil date। A2 তারই বৈধ reference দেখে A8-এর ঐ দিনের কার্যকর, void হয়নি এমন placement পড়ে। Whole-section group-এ group-এর কার্যকর তারিখে ঐ section-এর pupils আসে। | Header-এ class, campus, date, count ও last checked। প্রতি পূর্ণ তালিকায় একটি roster version, প্রতি pupil-এ stable ID ও অনুমোদিত label। “এই দিনে কোনো placement নেই” এবং “তালিকা যাচাই করা যায়নি” আলাদা। |
| পরিবর্তনের খবর · A8 → A2 | A8 commit-এ `placement.changed` দেয়। A2 পুরোনো ও নতুন target-এর প্রভাবিত তারিখ refresh করে। নতুন read ও register submit-এ authoritative version মেলে। | Membership বদলের দিনগুলি পুরোনো version প্রত্যাখ্যান করে। অন্য দিনের version একই থাকে। খোলা draft-এ refresh দেখায়। Replay-তে দ্বিতীয় পরিবর্তন হয় না। |

**প্রথম class ও group-এর data।** পূর্ণ Setup screen-এর আগে T02-তে Academics-এর নিজস্ব setup/fixture path দিয়ে School A ও B-এর year, grade, section এবং সাধারণ required-subject group তৈরি হবে। A1/A2 group ID ও তার কার্যকর সময় দেয়। Whole-section group-এর member A8-এর dated section placement থেকে আসে। Elective, stream ও language-set management CLS-003-এর R1 backlog। [বাস্তব reference ও shared fixture](IMPLEMENTATION_PLAN.md#fixture-and-students)

**একটি row দিয়ে walkthrough।** Platform T1-এর shared fixture-এ School A/উত্তর/Grade 6/নীল ও School B-তে একই label ও student number `A-017` আছে। School A-র row `r17`-এর স্থায়ী student ID এখানে `sA`। D প্রথম placement-এর তারিখ, M section বদলের তারিখ। D−1 ও M−1 একই academic year-এর দিন। Move ও undo আলাদা run।

| পরীক্ষা | তালিকা, version ও history |
|---|---|
| SIS-003 · A8 D থেকে প্রথম placement লেখে | D−1-এ নীলে `sA` নেই, D-তে একবার। শুধু কার্যকর interval-এর version বদলায়। একই confirm retry-তে একই A8 placement ID ও ফল। School B-এর `A-017` আলাদা ID। |
| SIS-009 · M থেকে নীল → সবুজ | M−1-এ শুধু নীল, M-তে শুধু সবুজ। Student ID একই। দুই section ও প্রযোজ্য group-এর M থেকে প্রভাবিত version বদলায়। M−1-এর version একই। Slice 0-তে move fixture, বাস্তব move G1। |
| PLT-014 · attendance-এর আগে import undo গৃহীত | আগে D-তে `sA` ছিল। Undo-র পর D−1 ও পুনর্গঠিত D-তে নেই। A8-এর একই placement ID-তে void version ও event থাকে। D-র পুরোনো roster token-এ নতুন register আটকে যায়। C3 ফেরানো row-তে ঐ batch-এর link-ই শেষ করে। |
| PLT-014/ATT-001 · attendance আগে গৃহীত | Undo-তে A8 “Conflict” দেখায়। D-তে `sA` ও আগের roster version থাকে। A9-এর accepted register ও C3 link অক্ষত। Downstream check পাওয়া না গেলে “Waiting — retry” দেখায়, reversal হয় না। |
| CLS-001 · group আলাদা করে পড়া | A2-র required-subject group শুরু হওয়ার দিন থেকে assigned teacher-কে অনুমোদিত pupils দেখায়। তার আগে section roster-এ pupil থাকলেও group-টি কার্যকর নয়। Subject teacher শুধু বরাদ্দ group পান। একই label-এর B-র group, পাশের campus বা নিজের অনির্ধারিত group-এ not_found। |

**Consumer-এর চাহিদা।** Students আগে target validate করে, পরে profile/import result থেকে একই dated roster পড়ে। Exams/Results সংশ্লিষ্ট দিনে বৈধ class/group, stable pupil/subject IDs ও teacher allocation নিয়ে marks scope মেলায়। পুরোনো marks সেই সময়ের group-এই থাকে। Desk Learn-এ school স্পষ্টভাবে class link চালু করলে একই roster ও allocation থেকে linked enrolment মেলে। Independent course-এর নিজের enrolment ও instructor থাকে। Teacher assignment নিজে থেকে learning permission দেয় না। A9 date ও version নিয়ে session register খোলে। প্রত্যেক read-এ C1 entitlement ও C3 action/scope লাগে। §৩-এর review-তে Students, Academics, Exams, Learn, Identity ও Apps নিজেদের ব্যবহারটি মেলাবেন। ছাত্র বসানোর দ্বিতীয় writer নেই। [Data ownership](../../03-system-module-and-data-map.md) · [Interfaces](../../integration/INTERFACES.md)

## ৪. Academic Setup থেকে published session

নিচের Feature ID-ই design requirement-এর চাবি। প্রতিটি row-র একই ID implementation plan §৫-এ dependency, task, path ও test-এর সঙ্গে মেলে। **আংশিক** চিহ্নিত ছয়টির পূর্ণ R1 সীমা §৭-এ আছে।

| Module ও PDF পৃ. | Feature ID | পর্দা ও ব্যবহারকারীর ফল |
|---|---|---|
| A1 · ৭৮ | SET-001 | Setup-এর বাঁয়ে ধাপ, ডানে নির্বাচিত কাজ। Year, grade, section, subject, teacher ও routine-এর কোন তথ্য বাকি এবং কে পূরণ করতে পারবেন দেখায়। আবার খুললে সত্যিকারের saved progress আসে। |
| A1 · ৭৮ | PLT-003 · আংশিক | Structure tree-তে pilot school, একটি campus, দরকারি department, grade ও section। Platform-এর school/campus reference ব্যবহার হয়। ভুল parent বাছলে save আটকে যায়। |
| A1 · ৭৮ | SET-002 | Year ও term-এর শুরু/শেষ একই calendar view-তে। Year-এর বাইরে term হলে field error। আগের year বাছলে সেই year-এর structure খোলে। |
| A1 · ৭৮ | SET-003 | Grade ও band-এর নাম school নিজে দেয়। Rename-এর আগে ও পরে একই grade ID থাকে। Filter-এ band ও grade দুটোই দেখা যায়। |
| A1 · ৭৮ | SET-011 | ব্যবহৃত section বা grade বদলানোর সময় কার্যকর তারিখ ও আগের version দেখা যায়। পুরোনো roster নষ্ট করে এমন delete বন্ধ। history-তে কে, কখন কী বদলেছেন থাকে। |
| A2 · ৭৮ | SET-004 · আংশিক | Section detail-এ class, section, room ও উপলব্ধ capacity। Pilot section/room mapping দিয়ে register ও routine একই section চেনে। |
| A2 · ৭৮ | SIS-003 | Students থেকে target/date যাচাই, A8-এ placement, তারপর history-র নির্দিষ্ট তারিখ খুলে ফল মিলিয়ে দেখা যায় (§৩)। |
| A2 · ৭৮ | CLS-001 | Classes-এ section ও subject group-এর আলাদা filter। A8 placements থেকে একই দিনের তালিকা খোলে। Date বদলালে count, row ও version একসঙ্গে বদলায়। |
| A2 · ৭৮ | CLS-002 | Class overview-তে class teacher, A9-এর attendance summary, Results-এর marks due ও C4-এর notices। প্রতিটি card-এর নিজস্ব source ও scope। Source সাড়া না দিলে “তথ্য পাওয়া যায়নি”। শূন্য দেখাতে producer-এর নিশ্চিত ফল লাগে। |
| A3 · ৭৯ | SUB-001 | Subjects table-এ code, নাম, core/elective/co-curricular type, department ও শিক্ষার ভাষা। Row খুললে ওই subject-এর detail দেখা যায়। Elective choice-এর release §৭-এ আছে। |
| A3 · ৭৯ | SET-005 · আংশিক | Grade-এর পাশে pilot-এর required subjects বাছা ও saved allocation দেখা। Class detail এবং timetable picker একই assignment দেখায়। |
| A4 · ৭৯ | CUR-001 · আংশিক | Grade → subject → unit ধরে pilot curriculum map। Pilot subject-এ unit না থাকলে অনুমোদিত staff প্রথম unit যোগ করতে পারেন। |
| A4 · ৭৯ | CUR-003 · আংশিক | Assigned teacher class, subject ও unit/topic-এর পাশে basic lesson ও material reference রাখেন। Staff-only notes ও material access আলাদা। ভুল group থেকে edit বা restricted file খোলা বন্ধ। |
| A6 · ৮০ | HRM-001 | Teachers directory-তে campus/subject filter, নাম ও অনুমোদিত profile fields। Directory read staff grant অনুসারে। pupil বা guardian route থেকে খোলে না। |
| A6 · ৮০ | TCH-001 · আংশিক | Pilot teacher profile থেকে identity/staff reference, নিজের campus ও assignments দেখা। Sensitive documents-এর link বা qualification যাচাই pilot readiness-এর শর্ত নয়। |
| A6 · ৮০ | TCH-002 | Allocation form-এ teacher + subject + class/group + কার্যকর সময়। Preview-তে পরের session কার এবং আগের interval কোনটি তা বোঝা যায়। |
| A6 · ৮০ | TCH-003 | Section detail-এ class teacher ও co-teacher-এর আলাদা দায়িত্ব ও মেয়াদ। কোন action পাওয়া যাবে C3 ও সেই assignment নির্ধারণ করে। |
| A5 · ৭৯ | SET-006 | School calendar-এ ছুটি, event, closure ও exam period। তারিখ খুললে কোন campus ও দিন প্রযোজ্য বোঝা যায়। |
| A5 · ৭৯ | SET-007 | Bell schedule-এ period-এর ক্রম, শুরু ও শেষ। Overlap-এর পাশে সময় দেখায়। saved schedule থেকে session slot পাওয়া যায়। |
| A5 · ৮০ | ACA-001 | Web timetable grid-এর পাশে unscheduled class ও clash list। Teacher, room বা class-এর একই slot মেলেনি দেখালে সংশ্লিষ্ট দুটো lesson খোলে। সমাধান করে publish করা যায়। Keyboard-এ list থেকেও slot সরানো যায়। |
| A5 · ৮০ | ACA-002 | Teacher নিজের routine, pupil নিজের এবং guardian অনুমোদিত child-এর routine দেখেন। Phone-এ দিন ও period অনুযায়ী list, room, subject ও teacher দেখা যায়। Draft শুধু অনুমোদিত staff দেখেন। |
| A5 · ৮০ | ACA-004 | Draft ও published version-এর শুরু ও বদলানো slot পাশাপাশি। Publish-এর পর কার্যকর দিন থেকে নতুন routine। একই session-এ room বা সময় বদলালে session আগেরটাই থাকে। সংশ্লিষ্ট audience C4 থেকে খবর পায়। আগের দিনের published history দেখা যায়। |
| A5 · ৮০ | ACA-005 | Admin অসুস্থ বা অনুপস্থিত teacher-এর session খুলে substitute ও সময় বাছেন। একই session-এ বর্তমান teacher/cover আর মেয়াদ দেখা যায়। Substitute ও class নোটিশ পায়। Cover শেষে বা revoke হলে offline draft জমা দিতে গিয়ে office-এ যোগাযোগের বার্তা আসে। |

## ৫. A9 · teacher-এর ফোন থেকে guardian-এর notice

Academic Setup-এ বৈধ class → A8 placement থেকে A2 roster → A5 published session ও A6 assigned teacher/cover → A9 offline register → reconnect → প্রয়োজনে teacher-এর correction ও School Admin-এর সিদ্ধান্ত → C4 dispatch → অনুমোদিত guardian-এর child view। Blueprint পৃ. ১৩, ১৬, ৩১, ৩৮–৩৯, ৫৬, ৮২, ১২৪।

| Feature ID · পৃ. ৮২ | পর্দার আচরণ | কী দেখে কাজ শেষ বুঝবেন |
|---|---|---|
| ATT-001 | Today থেকে daily অথবা period register। Header-এ class, subject, দিন, প্রযোজ্য period, roster version ও policy। Row-তে school-এর status যেমন present, absent, late, excused বা left early। শুরুতে unmarked। | Submit-এর আগে marked/unmarked ও totals মেলে। Daily ও period ফল আলাদা। Republish বা cover বদলালেও একই slot-এ একটি register। |
| ATT-002 | বড় touch control-এ বাকি pupil-দের একসঙ্গে present করে exception আলাদা করা যায়। Undo ও review-তে ভুল bulk mark পাল্টানো যায়। | **V07**-এ শিক্ষক ৪০ pupil-এর class **১ মিনিটের কমে** সঠিকভাবে mark করেন। QA timing ও expected marks দুটোই রাখবে। Blueprint পৃ. ৫৬। |
| ATT-003 | Offline-এ mark ফোনে save হয়। Last sync ও “ফোনে সেভ হয়েছে · পাঠানো বাকি” আলাদা দেখা যায়। App restart-এ নিজের draft ফেরে। | উত্তর না এলে ফল অজানা দেখায়। একই command retry-তে আগে গৃহীত হলে মূল receipt ফেরে, বর্তমান access মেলার শর্তে। গৃহীত না হলে stale roster বা বদলানো session/cover-এর নির্দিষ্ট বার্তা। |
| ATT-004 | জমা register-এ মূল mark, proposed mark, কারণ ও history একসঙ্গে। Cut-off-এর পর teacher request দেন ও আলাদা School Admin সিদ্ধান্ত দেন। Pending বা rejected হলে নিশ্চিত পুরোনো mark-ই দেখায়। Term close-এ register locked। | Approve হলে original ও নতুন version, requester, approver, সময় থাকে। আগের notice in-app-এ প্রকাশিত বা provider গ্রহণ করে থাকলে বর্তমান অনুমোদিত guardian সংশোধনী পান। Delivery receipt পেতে দেরি হলেও correction চলে। Provider গ্রহণ করেছেন কি না অজানা হলে C4 তা আগে যাচাই করে। |
| ATT-005 | A9 থেকে absence/late fact এলে C4 ঐ সন্তানের বর্তমান attendance right মেলায়। Staff-এর scoped view-তে queued, provider গ্রহণ করেছে, পৌঁছেছে, যাচাই চলছে বা blocked আলাদা দেখা যায়। Guardian neutral preview থেকে sign-in করে detail খোলেন। | প্রতি event/guardian-এ একটি logical notice। Retry ও SMS fallback সেই notice-এর delivery attempt। মেয়াদ শেষ right-এ নতুন dispatch হয় না। C3 সাড়া না দিলে পাঠানো অপেক্ষায় থাকে। |
| ATT-008 | Register history ও reports-এ campus, class/group, civil date range, daily/period ও status filter। Summary থেকে অনুমতিসাপেক্ষে সেই register/correction খোলে। | Counts accepted register-এর effective mark থেকে মেলে। Pending ও missing register স্পষ্ট থাকে। Export-এও একই filters ও field scope। |

### ৫.১ একটি stale register review করে পাঠানো

১. Teacher প্রকাশিত period session-এর R version-এ offline-এ mark করেন। Pupil **ক** absent এবং **খ** present। পরে A8-এ ঐ দিনের খ-এর membership বদলায়। Teacher প্রথমবার পাঠাতে গেলে server R প্রত্যাখ্যান করে। তখনও কোনো register বা notice তৈরি হয়নি।

২. Teacher “বর্তমান তালিকা দেখুন” চাপলে review panel-এর heading-এ focus যায়। বহাল pupil-এর mark stable ID দিয়ে মেলে। খ-এর mark draft থেকে বাদ। নতুন pupil unmarked থাকে। বদলানো নাম, count বা diff শুধু বর্তমান access থাকলে দেখায়। Cover শেষ বা revoke হলে “জমা দেওয়া যায়নি · স্কুল অফিসে যোগাযোগ করুন” দেখায় এবং roster detail ঢাকে।

৩. পর্যালোচনার পর নতুন roster ও body-তে নতুন command পাঠান। Server গ্রহণের পর উত্তর হারালে “নিশ্চিতকরণ আসেনি” দেখায়। একই command আবার পাঠালে বর্তমান access যাচাই করে মূল receipt আসে। এর মধ্যে routine বা roster বদলালেও receipt সেই মূল submission-এর। Screen reader একবার “জমা হয়েছে” ঘোষণা করে। অন্য teacher একই session আগে জমা দিয়ে থাকলে অনুমোদিত register ও correction-এর পথ খুলবে।

৪. ক absent-এর notice দুই অনুমোদিত guardian পান। Teacher পরে ক-কে late করতে কারণ দেন। আলাদা School Admin approve করলে original mark ও correction history দুটোই থাকে। Correction পাঠানোর ঠিক আগেই দ্বিতীয় guardian-এর right শেষ হলে প্রথম guardian সংশোধনী পান, দ্বিতীয় জনের attempt বন্ধ থাকে। ঐ run-এ এক register, দুই A9 attendance fact ও মোট তিনটি পাঠানো logical notice। Push/SMS-এ কারণ থাকে না। [Stale, retry ও dispatch test](IMPLEMENTATION_PLAN.md#offline-and-notice) · [Identity OD-18/19](../02-identity/UI_UX_DESIGN_PLAN.md)

৫. School-এর policy অনুযায়ী cut-off পেরিয়ে প্রথম draft পাঠানোর সুযোগ বন্ধ থাকলে staff screen office-এর পথ দেখায়। Office দেখে ঐ slot-এ গৃহীত register আছে কি না ও school-এর অনুমোদিত নিয়ম কী। গৃহীত mark বদলাতে স্বাভাবিক correction flow লাগে। [Cut-off ও policy পরীক্ষা](IMPLEMENTATION_PLAN.md#attendance-policy)

## ৬. Component, device ও state

System 8-এর app shell, school/role/child switcher ও notification detail-এর সঙ্গে এই পর্দাগুলি বসবে। DVL-003-এর component ও token source **Pending**। দরকারি shared components হল scoped class picker, dual-calendar date field, date/version header, roster row, mark control, timetable slot ও তার list view, sync banner, correction history এবং inline error summary। Saved, pending, stale, denied ও locked state-এ icon-এর সঙ্গে লেখা থাকবে। Actual palette, contrast pair, spacing, type scale, focus ring ও touch target পাওয়া গেলে DVL-003-এর মান ব্যবহার হবে। [Students design §৯–১০](../03-student-records/UI_UX_DESIGN_PLAN.md) · [Workflows](../../05-workflows-and-ui-ux.md)

| State | ব্যবহারকারী কী দেখেন | তাঁর পরের কাজ |
|---|---|---|
| Loading | বাছা class/date দৃশ্যমান। Class বা child বদলালে আগের totals ও details সঙ্গে সঙ্গে সরবে। আগের বাছাইয়ের দেরিতে আসা response ফেলে দেবে। | Data এলে focus নিজের জায়গায় থাকবে। |
| Empty | “এই দিনে কোনো placement নেই”, “আজ আপনার ক্লাস নেই” অথবা “এখনও register জমা হয়নি”। | Date বদলান, নিজের routine দেখুন, অথবা grant থাকলে Students/setup খুলুন। |
| Denied বা session শেষ | অন্য school/class-এর label ছাড়া access message। Product restricted হলে Platform-এর allowed read/export policy দেখায়। | অনুমোদিত workspace-এ ফেরা বা নতুন sign-in। Revoked session-এর draft দিয়ে submit বন্ধ। |
| Offline | Teacher-এর last update, saved draft, পাঠানো বাকি count। Guardian-এর sensitive detail ঢাকা, online ফিরলে নতুন check। | অনুমোদিত teacher mark চালাতে পারেন। Publish, approve ও report export-এর জন্য সংযোগ লাগে। |
| Stale | বদলানো roster/session, প্রযোজ্য date ও সর্বশেষ যাচাই। চলতি access যাচাই হলে draft review-তে থাকে। | Refresh চাপলে review heading-এ focus। Error summary ও নতুন unmarked row screen reader-এ বোঝা যাবে। |
| Error | Field-এর পাশে নির্দিষ্ট কারণ। Server/source পাওয়া না গেলে retry। ফল অজানা হলে “নিশ্চিতকরণ আসেনি”। | একই command retry বা ভুল field ঠিক করা। অনুমোদন বা delivery সম্পন্ন দেখাবে কেবল receipt এলে। |
| Success | Setup save-এ কার্যকর তারিখ, publish-এ version, attendance-এ original server receipt, correction-এ decision ও history। | Queue-এর register প্রথমবার গৃহীত হলে status একবার announce হবে। Focus সরবে না। Notice-এর delivery status C4 থেকে আলাদা আসে। |

Office-এর setup ও timetable wide web-এ পাশাপাশি panel, ছোট screen-এ এক ধাপে এক panel। Teacher-এর ফোনে roster search-এর নিচে এক column ও দৃশ্যমান submit summary। বড় লেখা, portrait/landscape এবং zoom-এ controls বা pupil label ঢাকা যাবে না। Timetable-এর table header, list বিকল্প, keyboard navigation, visible focus, focus ফেরত, screen reader-এ pupil + status, error ও sync announcement পরীক্ষা হবে। রঙ ছাড়াও clash ও অনুপস্থিতি বোঝা যাবে। English/Amharic, Ethiopic font, Ethiopian/Gregorian calendar-এ একই school date থাকে। V16-এ WCAG 2.2 AA ও native copy review, V17-এ low-cost Android, পুরোনো iPhone ও pilot network-এ register চালাতে হবে। Blueprint পৃ. ৩৫, ৫৬–৫৭। [QA ও device evidence](../../07-qa-pilot-and-acceptance.md)

## ৭. Pilot ও পরের release-এর সীমানা

ছয়টি partial-এর Blueprint label **R1**। Tracker-এর একটি ঘরে pilot-এর ফল, অন্যটিতে বাকি R1-এর ফল যাবে। আজ দুটোই Pending। [Feature tracker](../../tracker/features.csv)

| Feature · PDF পৃ. | Pilot-এ যা দেখাবেন | পূর্ণ R1-এ আলাদা করে যা মেলাবেন |
|---|---|---|
| PLT-003 · ৭৮ | এক school, একটি campus, প্রয়োজনীয় department, grade ও section | Group থেকে school ও একাধিক campus-সহ অনুমোদিত hierarchy, parent ও isolation-এর বাকি case। |
| SET-004 · ৭৮ | Pilot class/section ও room setup | বাকি homeroom, house, room/capacity mapping। Over-capacity warning CLS-004 আলাদা R1 backlog। |
| SET-005 · ৭৯ | Pilot grade/class-এ required subject assignment | Elective ও stream অনুযায়ী assignment। Pupil choice ও approval SUB-004 R2। |
| CUR-001 · ৭৯ | Pilot grade/subject-এর unit map | Topic, পূর্ণ objective, প্রয়োজনীয় সব field ও instructional hours। |
| CUR-003 · ৭৯ | Teacher/class-এর basic lesson ও material reference | সম্পূর্ণ topic link, resources, teacher notes ও field/access case। Department approval CUR-006 আলাদা R1 backlog। |
| TCH-001 · ৮০ | Teacher profile ও pilot assignments | Qualification, কোন subject পড়াতে পারেন, grade band, employment dates ও scoped documents-এর পূর্ণ case। |

| Tracker-এ অবস্থান | Gate বা অন্য owner-এর প্রমাণ |
|---|---|
| SIS-009 · A2, **R1 backlog**, পৃ. ৭৮ | **G1/E2E-07/V11**-তে A8-এর বাস্তব section change চালিয়ে কার্যকর দিনের আগে ও পরে roster, marks, attendance ও access মিলবে। **G2/E2E-07/V22**-তে campus transfer, enrolment/access ও history মেলে। দুই gate-এর owner ও run [implementation §৭](IMPLEMENTATION_PLAN.md#release-evidence)-এ। Blueprint পৃ. ৫৭–৫৮, ১২৪। |
| A14 · GRD-008 ও GRD-021 **R1 backlog**, পৃ. ৮৪ | **G1/V06/E2E-02**-এর staging sample-এ Pre-KG observation, rubric level ও narrative → approval → guardian development report। Numeric entry ও display থাকে না। A14, Results, Apps ও QA sample চালাবেন। Pilot-এ KG থাকলে আপনি বাস্তব class coverage নিশ্চিত করবেন। পৃ. ৫৬, ৫৮, ১২৪। |
| A14 care ও A9 pickup | EYS-001 পৃ. ৮৪, আর EYS-002, EYS-003, APP-013, EYS-005 পৃ. ৮৫, সবই **R2 backlog**। SIS-002-এর বাকি pickup contacts **R1** (পৃ. ৮১)। ATT-010 pickup code/gate check এবং ATT-011 arrival/departure **R2**। ATT-012 QR/ID ও ATT-014 biometric/RFID **R4** (পৃ. ৮২), FAC-002 gate pass **R4** (পৃ. ৮৮)। G1-এ custody screen ও delivery যাচাই হবে। পূর্ণ X-06-এর pickup-list অংশ ATT-010 rollout পর্যন্ত pending। |
| অন্যান্য সীমানা | ATT-006 guardian advance absence ও ATT-007 threshold follow-up **R1 backlog**। ATT-009 leave ও ATT-013 event register **R2** (পৃ. ৮২)। ACA-003 availability, TCH-004 workload ও TCH-005 availability **R1 backlog**। V11-এর পূর্ণ overload test সংশ্লিষ্ট R1 কাজের সঙ্গে। TCH-010 substitute pool **R2**, ACA-011 automatic timetable **R4**, পৃ. ৮০। |

## ৮. আপনার review-এর সিদ্ধান্ত

আপনি §৩-এ একই roster থেকে placement, move, void ও register জমার ফল যাচাই করতে পারেন। §৫.১-এ একবারের receipt, approved correction ও guardian dispatch দেখুন। School-এর policy, KG coverage ও cross-system contract-এ সংশ্লিষ্ট owner-এর সম্মতি এখনও বাকি। নতুন feature proposal আপনার অনুরোধে `S4-PR-nn` পাবে। [Implementation review ও coding gate](IMPLEMENTATION_PLAN.md)

Scope ও gate-এর উৎস [MVP release plan](../../02-mvp-release-and-branch-plan.md) · [Data map](../../03-system-module-and-data-map.md) · [QA plan](../../07-qa-pilot-and-acceptance.md) · [Launch ও handover](../../08-migration-launch-and-handover.md) · [CTO handoff](../../integration/CTO-HANDOFF.md)
