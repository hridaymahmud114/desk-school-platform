# System 5 · Exams and Results · UI/UX ও Information Architecture

**Combined review অনুযায়ী সংশোধিত খসড়া · ২৬ নভেম্বর ২০২৬।** Exam Officer paper সাজান, Subject Teacher নিজের group-এর marks জমা দেন, Academic Lead review করেন, Principal approve করেন এবং School Admin approved report card release করেন। আপনার অনুমোদনের জন্য school policy ও shared menu/action-এর সিদ্ধান্ত §৮-এ আছে। [Implementation draft ও Feature ID trace](IMPLEMENTATION_PLAN.md#feature-trace)

## ১. পরিধি, প্রমাণ ও সিদ্ধান্ত

Planning checkout `main` @ `3466e19`-এ Docs 02–08, tracker এবং সংশোধিত System 4 plan দুটি দেখা হয়েছে। স্থানীয় **Desk-SchoolERP-Technical-Blueprint-v2-team.pdf**, ২২ নভেম্বর ২০২৬-এর ছাপা পৃ. ১৯, ৩০–৩১, ৩৪, ৫৬–৫৯, ৮২–৮৪, ১০৬ এবং প্রয়োজনীয় contract পৃষ্ঠা সরাসরি মিলিয়ে দেখা হয়েছে। চালু screen, route, component/token, API, data model ও integration test-এর repository evidence **Pending**। Tracker-এ ১৭টির implementation status **Needs repository verification**। [CTO handoff · বর্তমান অবস্থা](../../integration/CTO-HANDOFF.md) · [Feature tracker](../../tracker/features.csv)

| Module ও Blueprint | R1 · MVP candidate | R1 · MVP partial |
|---|---|---|
| A10 Assessment and gradebook · পৃ. ৮২ | GRD-001, GRD-002, GRD-003, GRD-004, GRD-009, GRD-012 | — |
| A11 Exam management · পৃ. ৮৩ | EXM-001, EXM-002, EXM-005, EXM-007, EXM-011 | EXM-008 |
| A12 Results and report cards · পৃ. ৮৪ | GRD-005, GRD-006 | GRD-007, GRD-010, APP-012 |

- **আপনার সিদ্ধান্ত ও লিখিত baseline।** এখন planning ও review। OD-13 অনুযায়ী Platform → Identity 1a → A1/A2 slice 0 → Students। D-01-এর baseline এক campus ও native Desk Learn। D-10 অনুযায়ী school-এর approved policy, D-12 অনুযায়ী English/Amharic ও দুই calendar, D-19 অনুযায়ী দুই product-এর পৃথক entitlement। [Dependencies · Merge ও demo-র ক্রম](../../integration/DEPENDENCIES.md) · [Doc 06 · Blueprint decision references](../../06-school-policies-and-integrations.md) · [Blueprint পৃ. ১৮–১৯, ৩০–৩১, ১০৬]
- **Design-এর অনুমান।** Grade 6, Term 1, Mathematics এবং নীল/সবুজ section দিয়ে walkthrough। Paper-এর school civil day দিয়ে roster read এই fixture-এর নিয়ম। APP-003 family results ও APP-017 offline behaviour C5-এর R1 dependency। System 5-এর ১৭টি row-এর scope tracker অনুযায়ী। [Feature tracker](../../tracker/features.csv) · [Blueprint §17 ও C5, পৃ. ৩৮–৩৯, ৯৯]

<a id="navigation"></a>

## ২. Navigation ও অনুমতি

Doc 05-এর **Administration** workspace-এই staff flow এবং **Student and Guardian** workspace-এই family flow থাকবে। **Proposed** লেখা path-এর placement C5 ও সংশ্লিষ্ট workspace owner-এর সঙ্গে মিলবে। নিচের tree-তে user তাঁর current scope-এ যে entry পান সেটাই দেখানো হয়েছে। [Doc 03 · A10–A12 ও Data owner](../../03-system-module-and-data-map.md) · [Doc 04 · Role matrix](../../04-roles-and-permissions.md) · [Doc 05 · MVP ও R1 flows](../../05-workflows-and-ui-ux.md) · [Blueprint পৃ. ১৯, ৩৪]

```text
Administration
├─ Academics → Exams                                      [Proposed]
│  ├─ Exam setup → Exam session → Papers
│  └─ Timetable → Paper detail
│     ├─ Candidates
│     ├─ Paper attendance
│     └─ On-screen mark sheet
├─ Teaching → Gradebook                                  [Proposed]
│  └─ Term → Class / teaching group → Subject
│     ├─ Assessment plan ও marks
│     └─ Calculation, submit ও correction request
├─ Academics → Review queue                              [Proposed]
│  └─ Grade → Submitted subject → Return / Forward
├─ Approvals → Results                                   [Proposed]
│  └─ Grade approval / Published correction approval
├─ Results → Report cards                                [Proposed]
│  └─ Approved grade → Generate → Release → Issue history
└─ Academic Setup → Grading policies                      [Proposed]
   └─ Grade band → Versions → Simulation → Approval

Student and Guardian
└─ নিজের workspace / Child switch → Results               [Proposed]
   └─ Published term result → Report card

Public document verification
└─ Printed code/QR থেকে অথবা code entry → Verification result
```

| Menu → screen | কে কী করতে পারেন |
|---|---|
| Exams → Exam setup, Timetable, Candidates | Exam Officer assigned campus/term/exam-এ session ও paper তৈরি করেন, clash ঠিক করেন, dated candidates যাচাই করে সময়সূচি confirm করেন। Paper detail-এ operational তথ্য তাঁর scope অনুযায়ী আসে। Seat plan ও invigilator panel পূর্ণ R1 dependency (§৭)। |
| Exams → Paper attendance | Paper-এর দেওয়া scope-এ C3-granted recorder present, absent বা exempt দেন। Pilot-এ assigned Subject Teacher-কে এই action দেওয়ার mapping review-তে। Exam Officer-এর exam grant থেকে marks edit আসে না। |
| Exams → On-screen mark sheet → Gradebook | Subject Teacher current A6 subject/group allocation ও C3 action পেলে নিজের paper-এর mark save করেন। Gradebook-এ scoped assessment plan, classroom/exam items, eligible Learn items, calculation ও submit দেখেন। Returned subject ঠিক করেন এবং released result-এর correction চান। |
| Academic Setup → Grading policies | Academic Lead তাঁর grade band-এ draft ও simulation review প্রস্তুত করেন। School-এর অনুমোদিত অন্য checker policy approve করেন। Assessment plan edit ও policy release-এর নির্দিষ্ট action mapping school ও C3 owner-এর সঙ্গে মেলাতে হবে। |
| Academics → Review queue | Academic Lead assigned grade band-এ submitted subject, missing count, outlier ও calculation দেখেন। কারণসহ return বা Principal-এর কাছে forward করেন। |
| Approvals → Results | Principal scope-এর মধ্যে grade snapshot এবং published correction-এর evidence দেখেন। Approve বা reject করেন। নিজের request approve করা যায় না। |
| Results → Report cards | School Admin assigned campus-এ approved batch generate, preview, release, retry ও issue history দেখেন। Marks edit action নেই। |
| নিজের workspace / Child switch → Results | Sign-in করা pupil নিজের permitted result দেখেন। Guardian C3-তে যে child-এর result right পান, তার issued term result ও report card দেখেন। Draft, returned, approved কিন্তু unreleased, staff notes ও sibling-এর অননুমোদিত ফল staff scope-এ থাকে। |
| Public document verification → Code result | Report-card code দিয়ে যে কেউ অনুমোদিত চারটি field দেখতে পারেন (§৬)। Receipt-এর issuer Fees। Public page app shell ও child switch থেকে আলাদা। |

**Web ও phone action review।** C5 owner web teaching/office ও phone teaching/leadership entry-এর menu এবং deep link দেখাবেন। C3 owner একই target ও action-এ direct link, দ্বিতীয় role, expired grant ও বদলানো active context চালাবেন। নিজের group-এ Subject Teacher save/submit করতে পারেন। Exam Officer-এর marks edit এবং School Admin-এর marks change সংশ্লিষ্ট role grant-এ deny। দ্বিতীয় role থাকলে কেবল ঐ action-এর বর্তমান scoped grant কার্যকর, maker-checker একই account-এও চলে। Paper attendance recorder, assessment-plan editor, policy checker এবং historical paper reassignment-এর grant matrix **Proposed**, school ও C3 approval §৮-এ। [Doc 04 · Role matrix](../../04-roles-and-permissions.md) · [Blueprint পৃ. ১৯, ৩২, ৩৪, ৯৯ · APP-003/007/008/009/010]

**Scope দেখা।** Staff header-এ school, campus, School Management, active role ও year/term। List ও details-এ grade, section/group এবং subject দেখান, paper-এ school civil date-ও থাকবে। লেখা শুরু হলে context স্থির দেখান। School/role বদলালে draft ছাড়ার বা অনুমোদিত context-এ ফিরে যাওয়ার সুযোগ, তারপর আগের content ও cache সাফ করে নতুন data আনুন। Family header-এ selected child ও term। Child switch বা app resume-এ current rights যাচাই চলাকালে আগের result ও file preview ঢেকে রাখুন। Staff ও family route, action ও download-এ C1 entitlement/C3 permission server যাচাই করবে। Public surface-এ issued code ও §৬-এর field allowlist দিয়ে access নির্ধারণ হবে। [Interfaces · Owner ও consumer-এর চুক্তি](../../integration/INTERFACES.md) · [Blueprint পৃ. ৩২, ৩৪, ৯৭–৯৯]

<a id="exam-and-roster"></a>

## ৩. Exam setup, timetable ও dated candidates

**Setup ও timetable · EXM-001/002।** Setup form-এ exam type (unit test, mid-term, final, mock), term-এর session, class/group ও subject। Timetable-এ date, শুরু, duration, room ও সংশ্লিষ্ট teacher। Desktop-এ calendar-এর পাশে unscheduled papers ও clash list। Phone ও keyboard-এর জন্য তারিখভিত্তিক list এবং field দিয়ে slot edit। Clash row থেকে দুই slot-এর অনুমোদিত detail খুলবে। Overlap-এ pupil, teacher ও room আলাদা দেখান। অন্য campus বা group-এর detail-এ grant না থাকলে সংশ্লিষ্ট resource conflict আছে এটুকু দেখান। Save বা timetable confirm-এর সময় server আবার clash এবং source version মিলিয়ে দেখে। [Doc 05 · Exam ও result](../../05-workflows-and-ui-ux.md) · [Blueprint পৃ. ৮৩, ১০৬]

**Candidates · EXM-005।** Validate করা class/group-এর পাশে **paper day**, roster-এর civil **as-of date**, A2 roster version, candidate revision এবং eligibility policy দেখান। Confirmed selection-এ কে কখন confirm করেছেন আলাদা দেখান। List-এ অনুমোদিত নাম, stable pupil ID-তে বাঁধা row, অন্তর্ভুক্ত/বাদ ও অনুমোদিত কারণ। Source reply না এলে candidate count অজানা দেখান। একই pupil এক paper-এ একবার আসে। Staff evidence-এ exam/paper, class/group/subject refs ও header-এর selection তথ্য থাকে। Eligibility ও freeze-এর সীমা আপনি school-এর সঙ্গে ঠিক করবেন (§৮)। [Blueprint A11, পৃ. ৮৩, ১০৬] · [System 4 Design §৩ · A1/A2 slice 0](../04-academics-attendance/UI_UX_DESIGN_PLAN.md#slice-0) · [System 4 Implementation §২.১ ও §৪ · T02/T03](../04-academics-attendance/IMPLEMENTATION_PLAN.md)

**Confirmed paper বদলালে · EXM-002/005।** Exam Officer paper date/time বা candidate list বদলালে আগে draft revision খুলবে। Existing confirmed slot ও proposed details পাশাপাশি থাকবে। Confirm-এর আগে updated roster ও pupil, teacher এবং room clash একসঙ্গে recheck হয়। নতুন candidate-এর অন্য paper overlap করলে নতুন revision confirm হবে না। Exam Officer scoped conflict link থেকে কারণ মেটান। Candidate list বদলানো বা paper reschedule-এর পরে নতুন revision confirm হলে mark sheet-এ last checked version দেখায়, পুরোনো draft refresh করতে হয়। আগের accepted attendance ও marks তাদের original revision-এ থাকে। [Implementation §৪.১](IMPLEMENTATION_PLAN.md#roster-consistency) · [Blueprint পৃ. ৮৩, ১০৬]

| তথ্য ও owner | System 5-এ কী কাজে লাগে |
|---|---|
| **A8 · placement writes ও history** · SIS-003, SIS-009, PLT-014 | Registrar-এর section move, group membership, undo এবং effective interval A8 লেখে। A8 `placement.changed`-এ affected range জানায়। Results ইতিহাসের জন্য source reference ধরে রাখে এবং placement edit-এর পথ A8-এ থাকে। |
| **A1/A2 · class validate ও dated roster** · SET-002/004, CLS-001 | School/campus/year/grade/section/group/subject chain এবং প্রযোজ্য তারিখ মেলে। A2 authorized section/group ও `asOf` ধরে effective, void হয়নি এমন A8 placement থেকে stable pupil IDs এবং opaque version দেয়। নাম, row number বা student number দিয়ে match হয় না। |
| **A6 · effective allocation** · TCH-002/003 | Paper এবং gradebook-এর subject/group চিহ্নিত করে। ঐ paper-এর date-এ group/subject বৈধ ছিল কি দেখা হয়। প্রতিটি save, attendance, submit ও correction request-এর সময় actor-এর **বর্তমান** allocation বা ঐ historical paper-এর জন্য অনুমোদিত দায়িত্ব এবং C3 action/scope আবার মিলতে হবে। পুরোনো assignment দিয়ে আজকের edit খুলবে না। |

System 4 Design §৩-এর **Consumer-এর চাহিদা** এবং §৪-এর **Allocation TCH-002/003**, System 4 Implementation §২.১-এর dated token ও CX-07, §২.২-এর fixture/consumer এবং §৪-এর T06 allocation এই contract-এর ভিত্তি। Dates-এ membership বদলালে সংশ্লিষ্ট দিনের token বদলায়। Future move-এর আগে যে দিন ছিল, সেই দিনের token থাকে। [System 4 Design](../04-academics-attendance/UI_UX_DESIGN_PLAN.md) · [System 4 Implementation](../04-academics-attendance/IMPLEMENTATION_PLAN.md) · [Interfaces](../../integration/INTERFACES.md) · [Blueprint পৃ. ৭৮, ৮০, ৮৩]

<a id="marks-and-gradebook"></a>

## ৪. Paper attendance, mark sheet ও gradebook

**Paper attendance · EXM-007।** Paper detail-এর candidate list-এ present, absent বা exempt দিন। Paper date, candidate version এবং last saved status header-এ। Absent বা exempt হলে mark sheet-এ সেই state দেখা যায়। Present হলে numeric mark না দেওয়া পর্যন্ত missing। Entered mark থাকা অবস্থায় absent/exempt প্রস্তাবে পাশে old mark, proposed state ও কারণ দেখান। Grant থাকা user review শেষ না করা পর্যন্ত row-টি unresolved এবং submit আটকে থাকবে। Resolved হলে আগের numeric revision history-তে থাকবে, নতুন active state থেকেই হিসাব হবে। Published result-এর effect বদলাতে §৫-এর correction লাগে। A11 paper attendance এবং A9 daily/period register নিজ নিজ record। [Blueprint A9/A11, পৃ. ১৯, ৮২–৮৩] · [Doc 03 · A9/A11](../../03-system-module-and-data-map.md)

**Mark sheet ও gradebook · EXM-008/011, GRD-001/002/003/004/009/012।** Paper grid-এ pupil, attendance/state, mark, maximum এবং row error। Gradebook-এ term/group/subject, assessment type, category ও weight, maximum marks, source item এবং policy version। Exam item paper-এর row খুলবে। Classroom item একই gradebook-এ থাকে। School দুই product ও class/subject link চালু করলে school-selected Learn activity source label সহ আসে। Pending link-এ classroom ও exam flow চলবে। Drawer-এ pupil-এর source mark, weights, rounding, pending reason ও policy version। Submit preview-তে missing ও invalid rows, outlier-এর কারণ, এবং policy অনুযায়ী কোনটি blocker তা স্পষ্ট। Academic Lead একই calculation evidence দেখেন। [Doc 06 · School policy pack](../../06-school-policies-and-integrations.md) · [Interfaces · Course ও official marks](../../integration/INTERFACES.md) · [Blueprint পৃ. ১৮–১৯, ৩০–৩১, ৮২–৮৩, ৯৪, ৯৭, ১০৬ · PRD-007/LRN-115]

| State | Mark sheet ও calculation-এ ফল |
|---|---|
| Entered, যেমন `0` বা `27` | যে value দেওয়া হয়েছে তা দেখান। Entered zero ওজনসহ শূন্য। Audit-এ entry ও source থাকে। |
| Missing | “Not entered”, pending count-এ থাকবে। Blueprint অনুযায়ী required missing row submit আটকে দেয়, যতক্ষণ না mark বা অনুমোদিত অন্য state দেওয়া হয়। Preview-তে pending দেখান। |
| Absent | “Absent”। Approved policy অনুযায়ী exclusion, contribution zero বা re-assessment pending। Contribution zero হলেও attendance state Absent থাকে। |
| Exempt | “Exempt”। বাদ যায় এবং school-approved নিয়মে প্রযোজ্য weights পুনর্বণ্টিত হয়। |
| Ungraded | “Not graded”। Policy-তে প্রযোজ্য item থাকলে staff দেখতে পান, মোটে যোগ হয় না। |

**Policy screen · GRD-001/004, PLT-011।** Grade band/year, scale, pass mark, assessment category, weights, rounding ও pending/absent/exempt treatment পড়া যায়। Draft-এর পাশে prior-term simulation-এর aggregate ও scoped pupil diffs। আলাদা checker approve করলে effective date দেখায়, term খুললে সেই version lock। পুরোনো term খুললে তার নিজস্ব rules ও হিসাবই দেখা যায়। [Doc 06 · Policy release](../../06-school-policies-and-integrations.md) · [Blueprint পৃ. ১৮–১৯, ৩০–৩১, ৫৯, ১০১]

**Offline marks draft · GRD-003, EXM-008, APP-017।** C5 phone teaching mode-এ আগে খোলা অনুমোদিত item-এর marks device-এ draft করা যায়। Header-এ “Saved on this device”, “Waiting to sync”, “Saved to school” অথবা “Needs review”, সঙ্গে শেষ server confirmation। সংযোগ ফিরলে একটি batch যায়। App restart-এর পরেও draft ও pending status থাকে। Server current A6/C3, item ও roster/candidate revision দেখে। Item বা তালিকা বদলালে draft device-এ অক্ষত, status “Needs review”। Current grant থাকলে changed rows ও নিজের draft পাশাপাশি দেখে শিক্ষক পুনরায় batch পাঠাতে পারেন। Grant বা session শেষ হলে scope detail ও draft review বন্ধ এবং server-এ draft commit হয় না। Subject review-এর জন্য submit, approval ও publication online confirmation নেয়। [Implementation · Offline capture](IMPLEMENTATION_PLAN.md#offline-marks) · [Blueprint §17 ও §5.6.3, পৃ. ৩৮–৩৯, ৯৯, ১১২]

<a id="approval-and-correction"></a>

## ৫. Review, approval, release ও section বদলের উদাহরণ

**Approval · GRD-005।** Teacher save করে preflight দেখে subject submit করেন। Academic Lead-এর queue-তে grade/class filter, subject, submit সময় ও exception count। Row খুললে calculation ও source revision, পাশে return/forward action। Return-এ comment লাগে, revision নতুন করে submit হয়। Principal-এর detail-এ grade-এর subject coverage, forwarded evidence ও snapshot revision-এর নিচে approve/reject। Approval record-এ maker, checker, সিদ্ধান্ত, সময় ও version থাকে। C9 maker-checker ও audit প্রয়োগ করে এবং C3 action/state আবার মেলায়। School Admin-এর batch detail-এ উপরে eligible/ready/failed counts ও issue version, নীচে pupil rows, template এবং language/calendar preview। Generate দিয়ে file প্রস্তুত, Release দিয়ে family view-তে issued status। [Doc 04 · Role matrix](../../04-roles-and-permissions.md) · [Doc 05 · Exam ও result](../../05-workflows-and-ui-ux.md) · [Blueprint পৃ. ১৯, ৩১–৩৪, ৮৪, ১০১ · PLT-006/007/013/020]

**একই pupil · EXM-005/007/011, GRD-005/006/007/010, SIS-009।** School A/উত্তর/Grade 6/Term 1/Mathematics। Synthetic `sA` নীল group-এ দিন P-তে paper দেন। A2 `asOf=P`, version `R17`। দিন M, P-এর পরে, Registrar A8-এ নীল → সবুজ effective move দেন। P-তে পুরোনো নীল candidate এবং token `R17` থাকে। M থেকে সবুজ roster-এ `sA` একই ID-তে আসে। সবুজের teacher পুরোনো নীল paper-এ নিজে থেকে edit পান না। নীল paper-এর বর্তমান authorized teacher P-এর marks নিয়ে submit করেন। Academic Lead ও Principal approved snapshot-এ ঐ source দেখেন। একই pupil-এর card issue হয় একবার। এই fixture-এ card-এর frozen class label নীল, কোন date-এ final card class নিতে হবে তা school ঠিক করবে। [System 4 Design §৩ · SIS-009](../04-academics-attendance/UI_UX_DESIGN_PLAN.md#slice-0) · [System 4 Implementation §২.১ ও §৭.২](../04-academics-attendance/IMPLEMENTATION_PLAN.md) · [Blueprint পৃ. ১৯, ৫৭–৫৮, ১২৪]

**Stale branch।** আলাদা run-এ A8/A2 P-এর membership বা target eligibility সংশোধন করলে `R17` stale। Pending draft save বা submit-এ conflict, নতুন accepted mark, successful receipt বা result event শূন্য। Grant বর্তমান থাকলে teacher affected rows preview করেন এবং Exam Officer dated selection review ও তিন clash recheck করেন। Unchanged eligible row-তে stable ID ধরে draft দেখান। Removed বা newly eligible row explicit review-তে যাবে। সংশোধিত body নতুন key ও reviewed revision-এ পাঠান। Allocation শেষ হলে edit, submit এবং roster diff deny, historical paper reassignment-এর জন্য অনুমোদিত office action লাগে। আগে accepted mark ও published source অক্ষত থাকবে। [Implementation §৪.১](IMPLEMENTATION_PLAN.md#roster-consistency)

**দুই section-এর marks।** আলাদা fixture-এ M-এর পরে `sA` সবুজ group-এ Mathematics-এর আরও একটি বৈধ classroom mark পান। নীল ও সবুজের teachers নিজ নিজ items submit করেন। Authorized Academic Lead source group, applicable placement dates, weights ও duplicate/conflict status দেখে pupil-এর এক subject total review করেন। Principal এক term result approve করেন। এক current card-এর calculation-এ দুই group-এর eligible marks দেখা যায়। কোন categories/items মিলবে এবং কোন class label যাবে, school approve করবে। Rule বা source mapping unresolved থাকলে ঐ pupil-এর result release আটকে থাকবে। [Implementation §১ ও §৪.৩](IMPLEMENTATION_PLAN.md#source-identity) · [System 4 Implementation §৭.২ · SIS-009](../04-academics-attendance/IMPLEMENTATION_PLAN.md) · [Blueprint পৃ. ১৮–১৯, ৮৩, ১০৬]

**Published correction · GRD-006।** Teacher published version, proposed change এবং কারণ দিয়ে request করেন। Principal পুরোনো ও proposed হিসাব, impact ও source history দেখে approve বা reject করেন। Approval-এ নতুন version-এর content স্থির হয়। School Admin সেই version generate ও release করলে family view নতুন card দেখায়। পুরোনো PDF ও issue history authorized staff-এর কাছে থাকে। Active per-child rights ও channel policy মেনে C4 update পাঠায়। Notice preview-তে marks, grade বা PDF থাকে না। Denied বা rejected request-এ issued version আগের মতোই দেখা যায়। [Doc 05 · Exam ও result](../../05-workflows-and-ui-ux.md) · [Interfaces · Official marks ও notice](../../integration/INTERFACES.md) · [Blueprint পৃ. ১৯, ৩১, ৫৬, ৮৪]

<a id="published-and-public"></a>

## ৬. Published results, verification ও screen behaviour

**Family screen · APP-003।** Child ও term header-এর নিচে released subject results, issue date ও current card version। Child switch, offline এবং resume-এ current online C3 check-এর আগে marks, card thumbnail ও app-switcher preview ঢেকে দিন। Offline-এ last checked time ও “Connect to see”। C9 link দেওয়ার সময় এবং দেওয়া link redeem করার সময় per-child grant ও custody আবার মেলায়। আগে পাওয়া link পরে revoke হলে download deny। Pupil-এর নিজের login Identity policy অনুযায়ী। [Doc 04 · Roles](../../04-roles-and-permissions.md) · [Identity Design · OD-10, OD-18/19](../02-identity/UI_UX_DESIGN_PLAN.md) · [Identity Implementation · SC-15](../02-identity/IMPLEMENTATION_PLAN.md) · [Blueprint পৃ. ১৯, ৩২, ৩৪, ৫৬, ৯৯]

**Public verification · APP-012, GRD-010।** Report-card code/QR থেকে আলাদা public surface। সফল report-card response-এ শুধু **নাম, class, term, issue date**। Marks, pupil ID, guardian detail, PDF download, roster ও search-এর dataset staff/family scope-এ। Invalid ও unavailable code-এ একই সাধারণ message। পুরোনো issued code correction-এর পর কীভাবে চলবে তা §৮-এ review-এর জন্য আছে। Receipt-এর তথ্য ও public limits Fees-এর review-তে। Certificate ও প্রতিটি document type-এর agreed limits পূর্ণ R1-এ প্রমাণ হবে (§৭)। [Blueprint §08 পৃ. ১৯, A12 পৃ. ৮৪] · [Doc 07 · V06, V09, V16](../../07-qa-pilot-and-acceptance.md)

| State | Screen-এ কী দেখাবেন |
|---|---|
| Loading | Context label ও skeleton/accessible status। Roster, approval ও batch count আসা পর্যন্ত commit action বন্ধ। |
| Empty | বৈধ date-এ candidate নেই, review-এর জন্য কিছু জমা হয়নি, অথবা family-তে published result নেই। Staff-কে অনুমোদিত next action দিন। Family copy থেকে draft আছে কি বোঝা যাবে না। |
| Denied | অন্য school/campus/group/child বা revoked link-এ সাধারণ “পাওয়া যায়নি”। একই permitted record-এ নিষিদ্ধ action-এ “এই কাজের অনুমতি নেই”। আগের data ও file preview সরান। |
| Stale | Record বা roster version পরিবর্তন হয়েছে। অনুমোদিত হলে recheck, scoped diff ও unresolved rows দেখান। Rights বদলালে diff ফেরত দেওয়ার আগে access যাচাই করুন। |
| Offline | Teacher-এর local draft ও server-confirmed status আলাদা। Family result/preview online check পর্যন্ত ঢাকা। Approval ও release-এ reconnect action। |
| Error | Field-এ error ও focus, request/server error-এ retry এবং আগের acknowledgement দেখার পথ। Batch-এ failed ও ready row আলাদা, retry একই job/version ধরে। Unknown outcome-এ status check। |
| Success | Saved বা submitted revision, approval decision, released/failed count ও issue version। Download-এর জন্য release confirmation, notice delivery status আলাদা। |

**Reusable UI ও accessibility।** C5 shell, context switcher, child switch, date picker, filter, data grid, status label, field error, confirmation, approval history ও job progress ব্যবহার করার জায়গা আছে। Implementation ও token evidence **Pending**। DVL-003-এ real component/token path, contrast, focus ring, type scale, spacing, status states ও Ethiopic font support মেলান। Desktop-এ dense grid ও frozen headings। Narrow screen-এ pupil row cards এবং সম্পূর্ণ label সহ এক row edit। Keyboard navigation, visible focus, status text ও screen-reader announcement দিন। Colour ছাড়াও state চেনা যায়, grid error summary থেকে field-এ যাওয়া যায়, বড় text ও long Amharic label-এ action হারায় না। School civil date একই থাকে, English/Amharic এবং Gregorian/Ethiopian rendering যাচাই হবে। C9 PDF-তে Ethiopic fonts embed করে, file ও app-এর accessible alternatives QA মেলাবে। [Doc 05 · UI states ও accessibility](../../05-workflows-and-ui-ux.md) · [Blueprint পৃ. ১৯, ৫৭, ৫৯, ১০১ · PLT-015/016/018/020] · [DVL-003](../../tracker/deliverables.csv)

<a id="release-scope"></a>

## ৭. Partial-এর pilot, পূর্ণ R1 ও G1 dependency

| R1 · MVP partial | Pilot deliverable | R1 remainder এবং owner |
|---|---|---|
| EXM-008 · পৃ. ৮৩ | On-screen paper mark sheet, states, validation ও scoped save। C5/APP-017 দিয়ে offline draft ও safe sync। | Exams: printed sheet ও validated spreadsheet import, errors, totals ও retry। GRD-011-এর A10 spreadsheet backlog তার নিজস্ব row। |
| GRD-007 · পৃ. ৮৪ | Selected pilot grades-এর approved cards। প্রথম review fixture এক grade। | Results + C9 + school: পূর্ণ Grades 1–12 template/content coverage ও QA samples। GRD-017 comments/conduct/attendance summary তার নিজস্ব R1 backlog। |
| GRD-010 · পৃ. ৮৪ | Pilot batch generate ও release, report-card code। | Results + C9 + QA: পূর্ণ school batch, retry/reconciliation, agreed throughput ও release evidence। |
| APP-012 · পৃ. ১৯, ৮৪ | Results/C9 report card verify। Fees/C9-এর FIN-010 issued receipt verify একই pilot-এর proof। V09-এ payment verification-এর আগে receipt issue হয় না। | Document issuers + public surface owner: certificate verification ও প্রতিটি type-এর agreed public field limits। Certificate issuer/delivery availability আলাদা evidence, YRC-007 leaving/transfer certificate-এর Blueprint label R2 থাকে। |

**পূর্ণ G1 V05/V06।** EXM-003 seating/capacity ও EXM-004 invigilation/workload/duty notices দুটিই **R1 backlog**। Pilot timetable-এর room/teacher/pupil clash tests-এর পরে পূর্ণ exam setup → seating → invigilation → marks once → card run-এ এদের delivery evidence যোগ হবে (Blueprint পৃ. ৮৩, ১০৬)। A14-এর **GRD-008/GRD-021, R1 backlog**, Early Years-এর Pre-KG observation, rubric ও narrative V06/E2E-02 staging sample-এ লাগে। একই approval ও family rights চলবে, numeric mark entry ও display শূন্য (পৃ. ১৯, ৫৬, ৮৪, ১২৪)। SIS-009-এর real dated section move G1 E2E-07 dependency, campus transfer G2 proof। [Doc 07 · Gates](../../07-qa-pilot-and-acceptance.md) · [Implementation · Evidence](IMPLEMENTATION_PLAN.md#acceptance)

## ৮. আপনার review-তে যে সিদ্ধান্তগুলি খোলা

| কার সিদ্ধান্ত বা evidence | কী দরকার |
|---|---|
| আপনি ও school academic lead | Pilot campus/grade/term ও KG coverage, native Learn baseline, grading pack, prior-term data, golden files ও independent policy checker। Candidate eligibility/as-of/freeze/reopen, attendance recorder, historical paper-এর দায়িত্ব, cross-group calculation এবং card class attribution। |
| Academics, Students, Identity ও Results | বাস্তব A1/A2 reference/dated version, A6 grant ও paper assignment, A8 move ও downstream undo guard, C3 current-action ও child-right enforcement। |
| আপনি, Apps/C5, Identity/C3, Platform/C9, Fees ও certificate issuer | Web/phone menu ও action matrix, assessment-plan editor, offline draft recovery, templates/calendars, type-specific public field limits ও superseded card code। Batch-এ একটি card ব্যর্থ হলে grade একসঙ্গে প্রস্তুত হওয়ার পরে release হবে, নাকি ready cards আগে release হবে, তা ঠিক করুন। Fixture-এ whole-grade visibility ধরা হয়েছে। |
| Client language lead, QA ও release owner | Native Amharic copy/font proof, calendar sample, real device, signed backend/app builds, V01/V02, approval, rollout ও live gate evidence। |

নতুন feature proposal আপনি চাইলে `S5-PR-nn` দিয়ে নথিভুক্ত হবে। [Implementation · Task order](IMPLEMENTATION_PLAN.md#task-order) · [Doc 06 · Client inputs](../../06-school-policies-and-integrations.md#client-inputs)
