# System 3 · Student Records and Admissions — UI/UX design plan

**অবস্থা:** Draft, আপনার review বাকি। **তারিখ:** ২৬ নভেম্বর ২০২৬। **Planning checkout:** `main` @ `2f0b1e9`, শুধু docs। **Scope:** A7 quick registration (ADM-021) ও A8 Student records। **সঙ্গের নথি:** [Implementation plan](IMPLEMENTATION_PLAN.md)। **Review round 1:** ২৬ নভেম্বর ২০২৬-এর review report অনুযায়ী সংশোধিত। পরিবর্তনের তালিকা শেষের “পরিবর্তনের রেকর্ড”-এ। **সিদ্ধান্ত:** ২৬ নভেম্বর ২০২৬-এ আপনি সব সুপারিশ মেনেছেন (অংশ ১৭)।

## ১. সংক্ষেপে

Registrar একটি CSV থেকে student আনবেন, ভুল row ঠিক করবেন, duplicate দেখে সিদ্ধান্ত দেবেন, confirm করবেন, আর দরকার হলে পুরো import undo করবেন। একজন student-এর জন্য একই কাজ quick registration form দিয়ে হবে। এই plan সেই screen, flow ও state ঠিক করে।

তিনটি সীমা পুরো plan-এ একই থাকবে।

- **A8** student record, household contact ও placement history লেখে।
- **C3** guardian account–child access grant লেখে। A8 শুধু তার status দেখায়।
- **A2** class reference দেয় এবং তারিখ ধরে roster দেখায়। SIS-003 ও CLS-001 tracker-এ A2-এর feature, তাই এখানে এগুলো dependency।

Checkout-এ কোনো code বা screen নেই। তাই existing screen-এর audit pending, আর নিচের সব layout, component ও token **Proposed**।

২৬ নভেম্বর ২০২৬-এ আপনি সব open decision-এ সুপারিশ মেনেছেন। সিদ্ধান্তগুলো অংশ ১৭-এ, আর plan সেভাবে আপডেট হয়েছে।

## ২. Evidence

### ২.১ যা দেখা গেছে

| ID | Finding | উৎস |
|---|---|---|
| O-01 | Checkout-এ ১৮টি tracked file আছে, ১৪টি `.md` ও ৪টি `.csv`। Source code, screen, route, component, API, test বা design file নেই। | `git ls-files`, `main@2f0b1e9` |
| O-02 | A7, A8 ও A13-এর সব feature row-এর status `Needs repository verification`। | [features.csv](../../tracker/features.csv) row ৭০–১০৭, ১৬৭–১৭৫ |
| O-03 | Registrar-এর flow এক লাইনে লেখা আছে। CSV preview, duplicate review, confirm, guardian link, effective date দিয়ে class assign। ফল হিসেবে history, class list, guardian view, import audit ও undo। Screen-level design নেই। | [Doc 05](../../05-workflows-and-ui-ux.md) |
| O-04 | Office-এর bulk import web-এ হবে। ভাষা English ও Amharic, calendar Ethiopian ও Gregorian। Keyboard, text scale, contrast ও screen reader QA-তে থাকবে। | Doc 05, V16 |
| O-05 | Registrar-এর scope assigned school বা campus। কাজ registration, import, household, guardian rights, placement ও field-limited export। Marks, ledger ও health detail বন্ধ। Custody restriction-এ document ও দ্বিতীয় approver লাগে। | [Doc 04](../../04-roles-and-permissions.md) |
| O-06 | Preview-তে persistent student, placement বা grant তৈরি হয় না। Internal ID ও school-এর student number আলাদা। Contact থাকা আর access থাকা আলাদা। A8 একমাত্র placement writer। Undo-তে conflict review হয়। | [INTERFACES](../../integration/INTERFACES.md#student-walkthrough) |
| O-07 | SIS-002 `MVP partial`। Pilot অংশ guardian, sibling ও emergency contact। বাকি R1 অংশ pickup-contact coverage। | features.csv row ৯৪ |
| O-08 | Core UI design system (DVL-003)-এর owner Product and design, status Planned। Office workspace (APP-009) C5-এর feature, `MVP partial`। | [deliverables.csv](../../tracker/deliverables.csv), features.csv row ৪৮৫ |
| O-09 | V11 ও X-08-এ ১,০০০ pupil-এর dry run, duplicate review, undo ও totals মেলাতে হবে। | [Doc 07](../../07-qa-pilot-and-acceptance.md) |

### ২.২ নথিভুক্ত journey-তে যে ফাঁক পাওয়া গেছে

| ID | ফাঁক | এই plan-এ কোথায় ধরা হয়েছে |
|---|---|---|
| G-01 | Doc 05-এর flow-তে ভুল row ঠিক করার ধাপ নেই। | F1 ধাপ ৩, S05 |
| G-02 | Guardian link “করেন” বলা আছে, কিন্তু pending অবস্থা Registrar কোথায় দেখবেন তা নেই। | F3, S08 |
| G-03 | Undo “থাকে” বলা আছে। কে করবেন, কতদিন পর্যন্ত, আর কী থেকে যাবে তা নেই। | F6, OD-06 |
| G-04 | Placement date খালি বা ভুল হলে কী হবে তা নেই। | OD-05 |
| G-05 | Quick registration-এর field ও ধাপ লেখা নেই। | F2, S11 |
| G-06 | Registrar roster কোথায় যাচাই করবেন তা নেই। | F5, S13 |
| G-07 | CSV-র তারিখ কোন calendar-এ তা নেই। | S03-এ calendar বাছাই |
| G-08 | ১,০০০ guardian-এর access request একসাথে যাবে নাকি একজন একজন করে, তা IAM-002-এর pilot সীমার সঙ্গে মেলানো নেই। | OD-07 |

### ২.৩ যে evidence pending

- Existing screen, navigation, component library ও design token। Checkout-এ নেই।
- চালু app-এ role ও journey-র আচরণ। Checkout-এ নেই।
- Blueprint PDF এখন Git Bash-এর `pdftotext -layout` দিয়ে পড়া যায় (২৬ নভেম্বর যাচাই)। Scope-এর দাবিতে Blueprint-এর পাতা অংশ ৩-এ দেওয়া। প্রথম draft-এর কিছু তথ্য planning docs-এর page reference থেকে নেওয়া হয়েছিল।
- Pilot school-এর আসল CSV sample ও numbering rule হাতে নেই। Doc 06-এর [client input](../../06-school-policies-and-integrations.md#client-inputs) তালিকায় “Pilot policies ও rehearsal data” এখনো Pending। CSV sample ও numbering rule ওই input-এর সঙ্গে আসবে, এটি আমাদের অনুমান।
- Registrar-দের নিয়ে prototype feedback। DVL-003-এ হবে।

### ২.৪ Assumptions

| ID | যা ধরে নেওয়া হয়েছে | কোন সিদ্ধান্তে বদলাতে পারে |
|---|---|---|
| A-01 | Registrar-এর screen System 3 বানাবে। Administration web app-এর shell, navigation ও workspace switcher System 8-এর (APP-009)। চূড়ান্ত token DVL-003-এর। | সিদ্ধান্ত: OD-09 |
| A-02 | Product-এর UI copy English ও Amharic। এই নথির wireframe-এ English sample copy আছে। | — |
| A-03 | MVP-তে import ও undo শুধু Registrar করেন। School Admin import-এর audit দেখতে পারেন। চূড়ান্ত action–role তালিকা C3-এর IAM-014 matrix-এ থাকবে। | C3 review |
| A-04 | প্রথম branch-এ file format শুধু CSV (UTF-8), template সহ। Excel (.xlsx) পরের ছোট branch-এ, R1-এর ভেতরে। | সিদ্ধান্ত: OD-08 |
| A-05 | Import-এ fees, opening balance, course enrolment, marks, photo বা document আসে না। Opening balance Fees-এর আলাদা কাজ (Doc 08)। Import বা quick registration কোনো pupil account বা pupil activation code-ও তৈরি করে না। Pupil-এর sign-in System 2-এর কাজ: IAM-001, আর D-16 অনুযায়ী upper primary থেকে pupil account, ছোটরা guardian ও class card দিয়ে (Blueprint পৃ. ৬৪, ৯৮)। | Owner-এর নিয়ম: শুধু Blueprint v2-এর ভেতরের কাজ |
| A-06 | CSV-র student number-ই school-এর student number হিসেবে থাকবে। | সিদ্ধান্ত: OD-01 |
| A-07 | Guardian-এর পরিচয় নিশ্চিত না হলেও student ও placement confirm হবে। শুধু access link pending থাকবে। | সিদ্ধান্ত: OD-03 |
| A-08 | Placement date খালি থাকলে row Error হয়, যদি না Registrar mapping-এ সব খালি row-এর জন্য একটা তারিখ বেছে দেন। চলতি academic year-এর আগের তারিখ সতর্কবার্তা সহ চলে। Year শুরুর আগের তারিখ চলে না। | সিদ্ধান্ত: OD-05 |
| A-09 | একই year-এ আগে থেকে placed student-এর জন্য অন্য section এলে সেটি section change। সেটি SIS-009 (R1 backlog), এই import তা করে না। | — |
| A-10 | Confirm-এর পর batch বন্ধ হয়। বাদ পড়া row নতুন upload-এ আসে। আগে import হওয়া row student number দিয়ে চেনা যায়। File checksum শুধু “এই file আগে এসেছে” সতর্কবার্তার জন্য। | সিদ্ধান্ত: TD-01 |
| A-11 | Undo হওয়া student-এর record মুছে যায় না, “import undone” অবস্থায় থাকে, number-ও ওই record-এর সঙ্গেই থাকে। পরে একই number আবার import হলে row-এর ফল Reactivated: একই internal ID ও number, নতুন row-এর মান নতুন version হিসেবে, আর নতুন placement, contact ও access request নতুন batch-এর origin নিয়ে। ভিন্ন child হলে Registrar number-টি পুরোনো record থেকে ছেড়ে নতুন record-কে দিতে পারেন, audit সহ। Reactivation পুরোনো guardian link ফেরায় না, নতুন request লাগে। Undo হওয়া student সাধারণ read, search ও roster-এ পাওয়া যায় না (not-found)। শুধু import history ও re-import-এর মিলানোয় দেখা যায় (CX-15)। | সিদ্ধান্ত: OD-01, OD-06, TD-01 |
| A-12 | Custody restriction ও per-child rights বদলানোর screen System 2-এর। এখানে শুধু status দেখা যায়। | সিদ্ধান্ত: OD-04 (দ্বিতীয় approver Principal) |
| A-13 | Student number পুরো school-এ unique। Campus scope-এর Registrar-এর row-এর number অন্য campus-এর কোনো student-এর হলে Registrar শুধু দেখেন “এই number আপনার scope-এ পাওয়া যাচ্ছে না”। নাম, field, campus বা count দেখানো হয় না, link করাও যায় না। একই child হলে সেটি campus transfer (SIS-009, R1 backlog), এই import তা করে না। | সিদ্ধান্ত: OD-11 |
| A-14 | Row atomic। Student, contact ও placement-এর কোনো অংশ লেখা না গেলে ওই row-এর কিছুই লেখা হয় না। Guardian access request row লেখার পরে আলাদা যায়, তার ব্যর্থতা row ফিরিয়ে দেয় না। | — |
| A-15 | Sibling মানে একই school-এ enrolled আরেকজন student। Registrar S11 বা S12-এ link করেন, একই guardian contact থাকলে প্রস্তাব আসে। School-এ নেই এমন sibling pilot-এ রাখা হয় না। প্রথম branch-এর import sibling link আনে না (Doc 08-এর import তালিকাতেও sibling নেই)। | সিদ্ধান্ত: OD-12 |

## ৩. Scope ও release label

Tracker-এর label অপরিবর্তিত রাখা হয়েছে। “এই plan-এ” কলাম শুধু design-এর অগ্রাধিকার দেখায়। Blueprint v2-এর পাতা: A2-এর SIS-003, CLS-001 ও SIS-009 পৃ. ৭৮; A7 পৃ. ৮০–৮১ (ADM-021 পৃ. ৮০); A8 পৃ. ৮১–৮২ (PLT-014 পৃ. ৮১); A13 পৃ. ৮৪; C3-এর IAM পৃ. ৯৮। Pilot শুরু হয় imported pupil ও quick registration দিয়ে, পূর্ণ admissions Release 2-এ (D-15, পৃ. ৬৪)।

| Feature ID | Module | Tracker label | এই plan-এ |
|---|---|---|---|
| ADM-021 | A7 | R1 · MVP candidate | P0 |
| SIS-001 | A8 | R1 · MVP candidate | P0 |
| SIS-002 | A8 | R1 · MVP partial | Pilot অংশ P0। Pickup অংশ R1 remainder, শুধু সীমা লেখা আছে (অংশ ১৫) |
| PLT-014 | A8 | R1 · MVP candidate | P0 |
| SIS-007 | A8 | R1 · MVP candidate | Versioning-এর নিয়ম P0-র সঙ্গে। History view P1 |
| SIS-010 | A8 | R1 · MVP candidate | P1। প্রথম branch-এ শুধু navigation-এর জন্য ছোট list। পূর্ণ search ও filter SIS-010-এর নিজের branch-এ। |
| SIS-016 | A8 | R1 · MVP candidate | P1। `students.export` C3-এর T-01 catalogue-এ থাকে। প্রথম ব্যবহার ও V01 export evidence SIS-016-এর branch-এ। |
| SIS-006 | A8 | R1 · MVP candidate | P2। Flag লেখার নিয়ম OD-10-এ ঠিক হয়েছে |
| SIS-003, CLS-001 | A2 | R1 · MVP candidate | A2 dependency |
| IAM-003, IAM-004, IAM-007, IAM-014 | C3 | R1 · MVP candidate | C3 dependency |
| SIS-004, SIS-008 | A8 | R1 backlog | নেই |
| SIS-009, CLS-003, CLS-004 | A2 | R1 backlog | নেই |
| SIS-005, SIS-011–015 | A8 | R2 backlog | নেই। দুই existing record merge (SIS-012) এখানে হয় না |
| ADM-001–020, ADM-022, ADM-023 | A7 | R2 backlog। ADM-001–020-এ baseline decision D-15 | নেই |
| YRC-001–003, 005–007, 009, 010 | A13 | R2 backlog | নেই |
| YRC-008 | A13 | R3 backlog | নেই |

### Design requirements

Implementation plan-এর প্রতিটি task এই ID ধরে link করবে।

| ID | Requirement | Feature ID |
|---|---|---|
| DR-01 | School, campus, product ও year পর্দায় স্পষ্ট থাকে। File বা form দিয়ে school বদলায় না। | PLT-002, PRD-001, PLT-005 (dependency) |
| DR-02 | Registrar-এর campus scope অনুযায়ী allowed ও blocked state। | IAM-003, IAM-014 (dependency) |
| DR-03 | CSV upload ও column mapping। Source row ID ও mapping version থাকে। | PLT-014 |
| DR-04 | Dry-run preview। স্থায়ী write শূন্য, চার ভাগে count ও প্রতি row-এর কারণ। | PLT-014 |
| DR-05 | Row correction, class value mapping ও invalid section। | PLT-014, SIS-003 (dependency) |
| DR-06 | Duplicate review। | PLT-014, SIS-001 |
| DR-07 | Confirmation, repeat-safe result ও partial failure। | PLT-014, SIS-001, SIS-007 |
| DR-08 | Household contact ও guardian access আলাদা। Pending status দেখা যায়। | SIS-002, IAM-004 (dependency) |
| DR-09 | Effective date সহ প্রথম placement। | SIS-003 (dependency) |
| DR-10 | তারিখ ধরে roster যাচাই। | CLS-001 (dependency) |
| DR-11 | Undo, তার আগে impact preview, conflict review ও মেলানো totals। | PLT-014 |
| DR-12 | Quick registration। | ADM-021 |
| DR-13 | Student profile ও record history। | SIS-001, SIS-007 |
| DR-14 | Scope-এর ভেতরে search ও filter। প্রথম branch-এ শুধু ছোট list (S01)। | SIS-010 |
| DR-15 | Field-level permission সহ list ও export। | SIS-016 |
| DR-16 | Teacher-এর জন্য need-to-know flag, detail ছাড়া। | SIS-006 |
| DR-17 | Responsive, accessibility, ভাষা ও দুই calendar। | PLT-015, PLT-016, PLT-018 (dependency) |
| DR-18 | Loading, empty, error ও success state। | সব |
| DR-19 | Import history-তে কে, কখন, কী করেছেন তা দেখা যায়। | PLT-014, PLT-006 (dependency) |

## ৪. একটি CSV row থেকে প্রথম class roster

উদাহরণ INTERFACES-এর Proposed fixture থেকে নেওয়া। School A-র উত্তর campus-এর Registrar row `r17` আনছেন। তাতে student number `A-017`, guardian contact, Grade 6, section নীল ও class শুরুর তারিখ আছে।

1. Registrar School A-র membership নিয়ে School Management workspace-এ ঢোকেন। Server session থেকে school নেয়, C1-এ entitlement দেখে, C3-তে Registrar-এর action ও campus scope দেখে।
2. Registrar CSV upload করে column map করেন।
3. Preview-তে A8 duplicate, অজানা section ও ভুল guardian reference ধরে। এই ধাপে কিছুই স্থায়ীভাবে লেখা হয় না।
4. Confirm-এ A8 নতুন student তৈরি করে, অথবা Registrar-এর অনুমোদিত existing record-এর সঙ্গে row মেলায়। Household contact রাখে। Placement history-তে student ID, class reference ও effective date লেখে।
5. Registrar-এর অনুমোদিত action-এ C3 guardian link ও rights লেখে, পরিচয় মেলার পর। তার আগে status pending, access বন্ধ। A8 student ID ও household reference দেয় (Identity brief)। TD-04-এর সুপারিশকৃত পথে (Identity-র T-00-এর শর্তে, CX-14) A8-এর row transaction-এর ভেতরেই C3-এর command চলে। যেকোনো পথেই C3 ব্যর্থ হলে student ও placement থাকে, শুধু access Failed হয়, আর পরে idempotent retry হয়।
6. A2 placement history পড়ে roster বানায়। শুরুর আগের দিন `A-017` নেই, শুরুর দিন একবার আছে। Teacher assignment মিললে A9 register-এ নাম আসে।

| Data | কে লেখে | কে পড়ে |
|---|---|---|
| **Student ID** · SIS-001, PLT-014 | Confirm-এ A8। স্থায়ী internal ID আর school-এর পুনর্ব্যবহার অযোগ্য student number আলাদা। School B-তে একই number থাকলেও সেটি অন্য record। | Registrar নিজের scope-এ। A2 roster-এ internal ID। A9, Results ও Fees অনুমোদিত reference। |
| **Household contact** · SIS-002 | A8, Registrar-এর দেওয়া guardian, sibling ও emergency contact। | Registrar নিজের scope-এ। Link করার সময় C3 student ও household reference পায়। |
| **Guardian grant** · IAM-004, IAM-007 | C3, পরিচয় মেলার পর Registrar-এর অনুমোদিত action-এ। Custody restriction-এ document ও দ্বিতীয় approver লাগে। | A8 শুধু link status। App, notice, report, file, Attendance, Results ও Fees প্রতি child ও action-এ C3-কে জিজ্ঞেস করে। |
| **Dated placement** · SIS-003, CLS-001 | A8, Registrar-এর দেওয়া effective date ও A1/A2-এর যাচাই করা class reference দিয়ে। | A2 তারিখ ধরে roster বানায়। A9 সেই roster থেকে register নেয়। A2 বা A9 placement লেখে না। |

## ৫. Role ও access থেকে screen state

| Actor | Action | Scope | Approval | ফল | Screen state |
|---|---|---|---|---|---|
| Registrar, School A উত্তর campus | Import preview, confirm, undo, quick registration, placement | নিজের campus | লাগে না | Allow | পুরো flow |
| Registrar, School A দক্ষিণ campus | উত্তর campus-এ import বা student খোলা | Scope-এর বাইরে | — | Deny | নিরাপদ বার্তা। অন্য campus-এর নাম বা count দেখায় না |
| Registrar, School A উত্তর campus | Import row-এর number দক্ষিণ campus-এর কোনো student-এর | Scope-এর বাইরে | — | Link deny (A-13) | “Number not available in your scope”। নাম, field, campus বা count দেখায় না। Skip, অথবা ভিন্ন child হলে নতুন number |
| Registrar, পুরো school-এর scope | অন্য campus-এর existing student-এর সঙ্গে row মেলানো | নিজের scope | — | Allow, যদি এই year-এ ওই student-এর কোনো placement না থাকে। থাকলে deny, কারণ campus বা section বদল SIS-009 | S06-এর সাধারণ compare |
| Registrar | Custody restriction বদল | নিজের campus | Document ও দ্বিতীয় approver | C3-এর screen-এ | এখানে শুধু status chip ও C3-র link |
| School Admin | Import-এর audit দেখা | নিজের school | — | Read (A-03) | Import history, action button ছাড়া |
| Subject বা Homeroom Teacher | Assigned group-এর student-এর নাম ও flag | Assigned group | — | সীমিত read (SIS-006, P2) | Flag দেখায়, detail নয় |
| Learning Admin | Student record খোলা | — | — | Deny | Menu-তে Students নেই |
| যেকোনো role, School Management বন্ধ school | Import | — | — | Deny | “এই school-এ School Management চালু নেই” |
| মেয়াদোত্তীর্ণ session বা প্রত্যাহৃত role | যেকোনো write | — | — | Deny | আবার sign-in। Draft batch server-এ থাকে |
| Platform Support | Student record | অনুমোদিত support session | School consent | C9-এর নিয়মে (PLT-008) | এই plan-এ আলাদা screen নেই |

Menu লুকানো যথেষ্ট নয়। প্রতিটি সিদ্ধান্ত server-এ হয়, আর confirm ও undo-র মুহূর্তে আবার দেখা হয়।

## ৬. Information architecture

```
Administration app (web) · School Management workspace · Office
├─ Students                      S01  list ও search
│  ├─ New student                S11  quick registration (ADM-021)
│  └─ Student profile            S12  Overview · Household · Placement · History
│                                S13  placement panel ও roster check
└─ Imports                       S09  import history
   ├─ New import (wizard)        S02 Upload → S03 Map columns → S04 Preview and fix
   │                             → S06 Review duplicates → S07 Confirm
   └─ Batch detail               S08 Result · Guardian access · Roster check
                                 S10 Undo
বাইরের link:  Classes → roster (A2-এর screen)   ·   Guardian access and custody (C3-এর screen)
```

Shell, top bar ও workspace switcher System 8-এর (A-01)। প্রতিটি Students screen-এর উপরে context bar থাকে: school · campus · product · academic year।

## ৭. Flows

### F1 · CSV import (PLT-014, SIS-001, SIS-002)

1. **Upload (S02).** Registrar file বাছেন বা টেনে আনেন। Template download করা যায়। File গ্রহণ হলে একটি draft batch তৈরি হয়। একই file আগে import হয়ে থাকলে সতর্কবার্তা আসে, কোন batch-এ ও কবে তা সহ।
2. **Map columns (S03).** প্রতিটি source column একটি field-এর সঙ্গে মেলে। Registrar বলে দেন তারিখের column কোন calendar-এ। Grade ও section-এর আলাদা আলাদা মান A2-এর section-এর সঙ্গে একবার করে মেলানো হয় (class value mapping)। Mapping-এর version থাকে।
3. **Preview and fix (S04, S05).** Server dry run চালায়। প্রতিটি row চার অবস্থার একটিতে পড়ে: Ready, Needs review, Error বা Excluded। Registrar Error row ঠিক করেন, বাদ দেন, অথবা class value mapping বদলে একসাথে অনেক row ঠিক করেন।
4. **Review duplicates (S06).** প্রতিটি flagged row-তে Registrar তিনটির একটি সিদ্ধান্ত দেন। প্রতিটি সিদ্ধান্তের পাশে লেখা থাকে কী যোগ হবে আর কী বদলাবে না (নিচের table)।
5. **Confirm (S07).** Summary প্রতিটি সিদ্ধান্তের ফল আলাদা count-এ দেখায়। কোন row কেন import হবে না, তা কারণ ধরে ভাগ করা থাকে। Import করার মতো row শূন্য হলে Confirm button বন্ধ থাকে।
6. **Result (S08).** প্রতি row-এর ফল, guardian access-এর অবস্থা, roster যাচাই ও undo। Page ছেড়ে গেলেও confirm server-এ চলতে থাকে, আর একই key দিয়ে আবার শুরু করা যায়। Confirm background job-এ চলে (TD-03)।

**সিদ্ধান্তের ফল**

| সিদ্ধান্ত | Student profile | Placement | Household contact | Guardian access request |
|---|---|---|---|---|
| Same student — use the existing record | বদলায় না | এই year-এ কোনো placement না থাকলে যোগ হয়। একই section ও তারিখের placement আগে থাকলে কিছু বদলায় না। অন্য section-এ থাকলে row import হয় না (Already placed elsewhere) | File-এর যে contact এই student-এর সঙ্গে আগে link নেই, শুধু সেটি যোগ হয় | শুধু নতুন যোগ হওয়া guardian contact-এর জন্য |
| Different student — create a new record | নতুন student। Number একই হলে ভিন্ন number দিতে হয় | নতুন placement | File-এর contact | প্রতিটি valid guardian contact-এর জন্য |
| Skip this row | — | — | — | — |

Row atomic (A-14)। সিদ্ধান্ত না দেওয়া row import হয় না।

**বিশেষ ক্ষেত্র**

- আগের কোনো batch-এ import হওয়া এবং এখনো কার্যকর row: label “Already imported in batch #12”, Skip আগে থেকে বাছা। Registrar “use existing” বাছলে ওপরের নিয়মে চলে, তাই সাধারণত কিছুই বদলায় না (No change)।
- আগে import হয়ে undo হওয়া student-এর number: label “Imported in batch #12 and undone”। Registrar compare দেখে তিনটির একটি বাছেন: “Same child — reactivate”, “Different child — release the number and create a new record”, অথবা Skip। নিয়ম A-11-এ (TD-01)।
- Scope-এর বাইরের number: A-13-এর নিয়ম (OD-11)। সিদ্ধান্ত শুধু Skip, অথবা ভিন্ন child হলে নতুন number।
- এই year-এ অন্য section-এ আগে থেকে placed student: row import হয় না, শুধু Skip। Section বদল SIS-009।

**Row-এর অবস্থা (Proposed নাম)**

| পর্যায় | অবস্থা | মানে |
|---|---|---|
| Preview | Ready | Confirm করলে import হবে |
| Preview | Needs review | Registrar-এর সিদ্ধান্ত লাগবে। কারণ হতে পারে same student number, same name and date of birth, already imported, imported and undone, number outside your scope, already placed elsewhere this year, guardian reference broken |
| Preview | Error | ঠিক না করলে import হবে না। অজানা section, ভুল বা খালি তারিখ, দরকারি field নেই, file-এর ভেতরে একই number দুইবার |
| Preview | Excluded | Registrar নিজে বাদ দিয়েছেন |
| Confirm-এর পর | Created · Reactivated · Linked to existing · Not imported · Failed | Linked to existing-এর ভেতরে No change আলাদা count। Not imported-এর কারণ: No decision, Excluded by you, Skipped, Already placed elsewhere, Number not available in your scope। Failed হলে কারণ ও Retry |
| Undo-র পর | Reversed · Kept (existing) · Conflict · Not imported | Conflict হলে Registrar দেখে সিদ্ধান্ত নেন। Undo চলার সময় কোনো row “Waiting — retry” অবস্থায় থাকতে পারে, কারণ সহ (C3-এ link শেষ করা যায়নি, বা অন্য module-এর check পাওয়া যায়নি)। তখনো ওই row-এর কিছুই বদলায়নি। এমন row শেষ না হওয়া পর্যন্ত চূড়ান্ত total হয় না |

**Batch-এর অবস্থা (Proposed নাম)।** Draft → Previewed → Confirming → Completed বা Completed with issues → Undoing → Undone বা Partly undone। চলার সময় feature flag বা product বন্ধ হলে, school-এর অবস্থা বদলালে, Registrar-এর access বদলালে, বা permission matrix-এ Registrar role × ওই action-এর ঘর বদলালে batch হয় Blocked — access changed। এটি নিজে থেকে আবার চলে না। একই বা অন্য অনুমোদিত Registrar নতুন confirm দিয়ে বাকি row চালান (আপনার সিদ্ধান্ত, System 1-এর OD-7-এর সঙ্গে মিলিয়ে)।

### F2 · Quick registration (ADM-021)

1. Registrar S11 খোলেন। তিন ভাগের একটি form: Student, Household (guardian ও emergency contact, আর একই school-এর sibling-এর link), Class placement।
2. Save-এ একই validation ও একই duplicate check চলে। Flagged হলে S06-এর compare এই form-এর ভেতরেই খোলে।
3. সফল হলে profile (S12) খোলে। সেখানে পরের কাজ দেখা যায়: guardian access request ও roster যাচাই।

Quick registration আলাদা student store বানায় না। এটি A8-এর একই command ডাকে।

### F3 · Guardian access pending (SIS-002, dependency IAM-004)

1. Contact save হলে card-এ লেখা থাকে “Contact saved. App access not given yet.”
2. তারপর access-এর অবস্থা দুই ভাগে দেখা যায় (নিচের table)। প্রতি child-এর status আলাদা।
3. Custody restriction এই flow থেকে দেওয়া বা বদলানো যায় না। এর জন্য document ও Principal-এর approval লাগে (school-এর Approvals policy-তে, OD-04)। Screen-টি C3-এর (System 2)।
4. Batch detail-এর Guardian access tab-এ status ধরে count ও তালিকা থাকে।

**Access-এর অবস্থা (নাম Proposed)**

| কোথা থেকে | Status | মানে | Registrar কী করতে পারেন |
|---|---|---|---|
| A8-এর request | Not requested | Valid guardian contact নেই, বা request পাঠানো হয়নি | Contact যোগ বা ঠিক করলে request যায় (OD-07) |
| A8-এর request | Failed | পৌঁছায়নি বা C3 নেয়নি, কারণ সহ | Retry |
| C3-এর link | Pending | Link আছে, পরিচয় যাচাই বাকি, access বন্ধ | C3-এর screen-এ যাওয়ার link |
| C3-এর link | Active | Access দেওয়া হয়েছে | — |
| C3-এর link | Restricted | Custody restriction কার্যকর | C3-এর screen-এ দেখা |
| C3-এর link | Ended | Link শেষ, বা pending request বাতিল, যেমন undo বা relationship শেষে (C3-এর ফল `ended` বা `cancelled`) | — |

A8-এর request-এর দুটি আর C3-এর link-এর চারটি, মোট ছয়টি status। C3-এর চারটি System 2-এর draft-এর `pending`, `active`, `restricted`, `ended`-এর সঙ্গে মেলানো। A8 নিজে কোনো link status বানায় না। সুপারিশকৃত পথে C3-এর command row-এর transaction-এর ভেতরে চলে (TD-04), তাই আলাদা “Sending” অবস্থা নেই। পথটি Identity-র T-00-এর শর্তে (CX-14)। বিকল্প পথ (durable outbound record + idempotent command) বাছা হলে “Sending” যোগ হবে।

### F4 · প্রথম placement (dependency SIS-003)

1. Registrar class picker থেকে campus, year, grade ও section বাছেন। Picker-এ শুধু A2-এর valid target আসে।
2. Effective date দেন। দুই calendar পাশাপাশি দেখা যায়।
3. Save-এ A8 placement history লেখে। Target বাতিল হয়ে থাকলে save আটকে যায় ও কারণ দেখায়।

একই year-এ আগে থেকে placed student-এর section বদল এই flow-তে হয় না (A-09)।

### F5 · তারিখ ধরে roster যাচাই (dependency CLS-001)

Batch detail ও student profile-এ একটি read-only panel থাকে। Registrar একটি তারিখ দিলে A2 ওই দিনের roster দেয়। Panel-এ লেখা থাকে “Source: Academics (A2)”। এখান থেকে placement বদলানো যায় না। A2-এর interface তৈরি না থাকলে panel বলে “Roster check is waiting for Academics”।

### F6 · Undo (PLT-014)

1. Registrar import history থেকে batch খুলে Undo import বাছেন।
2. **Impact preview (S10)** পাঁচ ভাগে দেখায়, আর তাদের যোগফল মোট row। Will be reversed: এই batch-এর তৈরি student, placement, contact ও access link। এর মধ্যে কতগুলো access ইতিমধ্যে Active, তা আলাদা সংখ্যায়। Existing students kept: student থাকে, কিন্তু এই batch তাদের যে placement, contact ও access link যোগ করেছিল সেগুলো ফেরে। Needs your review (Conflict): batch-এর পরে placement বা অনুমোদিত edit হয়েছে, অথবা অন্য module-এ record আছে, যেমন attendance। এই row-এর কিছুই বদলায় না, access-ও না। Not part of this import। Total।
3. Registrar কারণ লিখে confirm করেন।
4. Undo চলার সময় প্রতিটি row আবার যাচাই হয়। Preview-এর পরে বদলে যাওয়া row Conflict হয়ে যায়, কারণ লেখা থাকে “changed since the preview”।
5. A8 নিজের batch-এর পরিবর্তন ফেরায়। C3 শুধু সেই access link-গুলো শেষ করে, যেগুলো এই batch যোগ করেছিল এবং এখন ফেরানো হচ্ছে: Will be reversed row-এর link, আর Existing students kept row-এ এই batch-এর যোগ করা link। Conflict row-এর link ও অন্য batch-এর link অক্ষত থাকে। A2 roster আবার মিলিয়ে নেয়। প্রতিটি row-এ A8-এর reversal আর C3-এর link শেষ করা একসঙ্গে হয়: হয় দুটোই, নয় কোনোটাই নয় (TD-04)। বিকল্প পথে (CX-14) আগে C3-এ link শেষ হয়, তারপর A8-এর reversal।
6. কোনো row-এ C3-এ link শেষ করা না গেলে, বা অন্য module-এর check পাওয়া না গেলে, row দেখায় “Waiting — retry”, কারণ সহ: “Access withdrawal pending” (C3-এ link শেষ হয়নি; নামটি System 2-এর CX-15-এর) অথবা “Check unavailable”। ওই row-এর কিছুই বদলায় না। এমন row থাকলে batch “Partly undone”, আর চূড়ান্ত total দেখানো হয় না।
7. Result-এ Reversed, Kept (existing), Conflict ও Not imported-এর count থাকে, আর যোগফল মোট row-এর সমান হয়।

Conflict row-এর কিছুই নিজে থেকে বদলায় না। Registrar প্রতিটি দেখে ঠিক করেন কী করবেন। Undo করতে পারেন ওই campus-এর import অধিকার থাকা Registrar, কারণ লিখে, confirm-এর পরে ৩০ দিন পর্যন্ত (OD-06)। এরপর Undo বন্ধ হয়, কারণ সহ, আর ভুল ঠিক হয় student ধরে, profile থেকে। Raw CSV ও staged row ৩০ দিন শেষে মোছা হয়। Origin reference, batch-এর হিসাব ও audit থাকে (TD-02)।

## ৮. Screen layout

Wireframe-এর সংখ্যাগুলো implementation plan-এর Proposed ১,০০০-row fixture থেকে নেওয়া। নাম ও তারিখ synthetic।

### S04 · Preview and fix

```
Import students · Step 3 of 5 · Preview            School A · North campus · School Management · 2026–27
+----------------------------------------------------------------------------------------------+
| 1,000 rows     [v] Ready 940     [!] Needs review 35     [x] Error 25     [-] Excluded 0      |
| Your draft is saved. No students, placements or access are created until you confirm.        |
+----------------------------------------------------------------------------------------------+
Filter: (All) (Ready) (Needs review) (Error) (Excluded)          Find row or student no. [        ]

Row    Student no.  Name             Grade · Section   Starts       Guardian    Status
r17    A-017        Test Pupil 17    6 · Blue          2026-09-14   1 contact   [v] Ready
r981   A-981        Test Pupil 981   6 · Blu           2026-09-14   1 contact   [x] Section not found      [Fix]
r941   A-2001       Test Pupil 941   5 · Green         2026-09-14   1 contact   [!] Same student number    [Review]
r991   A-991        Test Pupil 991   5 · Green         (empty)      1 contact   [x] Start date missing     [Fix]

[Back to mapping]                                             [Review duplicates (30)]   [Continue]
```

Count bar নিজেই filter। প্রতিটি status-এ icon ও লেখা দুটোই থাকে। Row-এর কারণ এক লাইনের সহজ বাক্য।

### S05 · Row correction drawer

```
Fix row r981                                                        [Close]
Problem: Section "Blu" was not found in Grade 6, North campus, 2026–27.

Section      [ Blue            v ]    From your file: "Blu"
[ ] Apply to all 6 rows with "Blu" in Grade 6

[Exclude this row]                              [Save and next problem]
```

File-এর মূল মান সবসময় দেখা যায়। কে, কখন বদলেছেন তা রাখা হয়।

### S06 · Duplicate review

```
Review possible duplicates · 3 of 30         Filter: (All 30) (Same number 20) (Same name and birth date 10)
[Previous]  [Next unresolved]                                     Left to decide: 28          Saved
+-- From your file · row r941 -------------+   +-- Already in School A ---------------------+
| Student no.    A-2001                    |   | Student no.    A-2001             = same   |
| Name           Test Pupil 941            |   | Name           Test Pupil 941     = same   |
| Date of birth  2014-03-02                |   | Date of birth  2014-03-20         ≠ differs|
| Guardian       +251 9•• ••• •41          |   | Guardian       none on record             |
| Class          5 · Green from 2026-09-14 |   | Class now      none this year              |
+------------------------------------------+   +--------------------------------------------+
Why this is flagged: student number A-2001 is already used in School A. A matching number does not
prove it is the same child. Check the details before you decide.

( ) Same student — use the existing record
      Profile stays as it is. Adds: placement 5 · Green from 2026-09-14, 1 guardian contact, 1 access request.
( ) Different student — create a new record
      Needs a new student number, because A-2001 is taken.
( ) Skip this row
      Nothing is imported for this row.

[Save decision and next unresolved]
```

```
Example 2 · row r961
Why this is flagged: same name and date of birth as a student already in School A, but a different
student number (your file A-961, school A-2021). Check the details before you decide.
The three choices are the same. "Use the existing record" keeps A-2021 and does not use A-961.
```

```
Example 3 · another file, North campus Registrar (targeted scenario, not in the 1,000-row fixture)
Why this is flagged: student number A-2015 is already used in School A, outside your campus.
No details can be shown. If this is a different child, give a new number. If it is the same child,
this is a campus transfer, which is not part of this import.
( ) Different student — create a new record with a new number
( ) Skip this row
```

নিয়মগুলো:

- Duplicate খোঁজা হয় শুধু একই school-এর ভেতরে। অন্য school-এর একই number কখনো দেখানো বা মেলানো হয় না।
- একই number মানে number collision, পরিচয়ের প্রমাণ নয়। নাম ও জন্মতারিখ মিললে সম্ভাব্য একই child। দুই ক্ষেত্রেই পরিচয়ের সিদ্ধান্ত Registrar-এর (OD-02)।
- কেন flag হয়েছে তা সবসময় এক বাক্যে লেখা থাকে।
- প্রতিটি সিদ্ধান্তের নিচে লেখা থাকে কী যোগ হবে আর কী বদলাবে না (F1-এর “সিদ্ধান্তের ফল”)।
- যে field মেলে না সেটি চিহ্ন ও লেখা দিয়ে আলাদা করা হয়, শুধু রং দিয়ে নয়।
- একই number থাকলে “create a new record” বাছতে নতুন number দিতে হয়।
- Scope-এর বাইরের candidate-এর নাম, field, campus বা count দেখানো হয় না। Server শুধু Registrar-এর scope-এর candidate ID গ্রহণ করে (A-13, OD-11)।
- আগের batch-এ import হওয়া row-এ Skip আগে থেকে বাছা থাকে। Undo হওয়া student-এর number হলে তিনটি সিদ্ধান্ত: একই child হলে Reactivate, ভিন্ন child হলে number ছেড়ে নতুন record (TD-01), অথবা Skip।
- সিদ্ধান্ত সঙ্গে সঙ্গে save হয়, পাশে Saving, Saved বা Not saved — try again দেখায়। Page বন্ধ করে ফিরলেও থাকে। Confirm-এর আগে বদলানো যায়। Actor ও সময় রাখা হয়।
- “Next unresolved” সরাসরি পরের সিদ্ধান্তহীন row-এ নেয়। কারণ ধরে filter করা যায়। “Left to decide” সংখ্যা সঙ্গে সঙ্গে বদলায়।
- Screen reader পড়ে: row, কেন flag, কোন field মেলে না, বাছা সিদ্ধান্ত, আর save হয়েছে কি না।
- দুই existing record merge করার কোনো action এখানে নেই। সেটি SIS-012 (R2)।
- Registrar-এর অনুমতি নেই এমন field (যেমন health detail) দুই পাশের কোনোটিতেই আসে না।
- সিদ্ধান্ত সবসময় row ধরে। একসাথে অনেক row-এ একই সিদ্ধান্ত দেওয়ার কোনো option নেই, কারণ প্রতিটি row-এর পরিচয়ের প্রমাণ আলাদা।

### S07 · Confirm

```
Confirm import · Step 5 of 5
You are importing into:  School A · North campus · School Management · 2026–27

Rows in file                                1,000
  New students to create                      970
  Rows linked to existing students             25   adds 25 placements and 25 guardian contacts · no change 0
  Rows to reactivate                            0
  Rows not imported                             5   excluded by you 5 · skipped 0 · no decision 0
                                                    · already placed elsewhere 0 · outside your scope 0
First placements to record                    995   start 2026-09-14
Guardian access requests                      990   Access stays pending until identity is checked.
Rows without a valid guardian contact           5   no access request; add a contact later

Not part of this import: fees, course enrolments, marks, sibling links.
If you leave this page, the import keeps running and you can come back to it.
[Back]                                                                  [Confirm import]
```

Confirm চলার সময় progress দেখা যায় এবং page ছেড়ে গেলেও কাজ চলতে থাকে। Page ছেড়ে যাওয়া নিরাপদ।

### S08 · Batch result ও guardian access

```
Import #14 · Completed with issues                        by Registrar R. · 2026-09-10 10:42
  [v] Created 970   [v] Reactivated 0   [v] Linked to existing 25   [-] Not imported 5   [x] Failed 0

Tabs:  (Rows)  (Guardian access)  (Roster check)  (History)

Guardian access   Not requested 5   Failed 12   Pending 978   Active 0   Restricted 0   Ended 0
                  [Retry all failed]
Row    Student          Contact            Access status
r17    Test Pupil 17    +251 9•• ••• •17   [~] Pending — identity not checked yet
r31    Test Pupil 31    +251 9•• ••• •31   [x] Failed — request not delivered            [Retry]
r996   Test Pupil 996   —                  [-] Not requested — no valid guardian contact

Contact saved does not mean app access. Access is managed in Guardian access (C3).
                                                                       [Undo import]
```

একই confirm একই key দিয়ে আবার পাঠালে মূল receipt ফেরে। এই page সবসময় live অবস্থা দেখায়।

### S10 · Undo impact preview

```
Undo import #14                  Preview made 2026-09-25 10:02 · every row is checked again when you undo
Undo available until 2026-10-10 (30 days after confirm)
Will be reversed            968 rows   968 students · 968 placements · 963 access links (0 already active)
Existing students kept       25 rows   students stay · this import's 25 placements, 25 contacts and
                                       25 access links are reversed
Needs your review             2 rows   nothing in these rows changes, including access
     r212  Test Pupil 212   placed in 6 · Green from 2026-09-21, after this import
     r540  Test Pupil 540   name corrected on 2026-09-18, after this import
Not part of this import       5 rows   left out at confirm
Total                     1,000 rows
Placements: 993 reversed · 2 kept          Access: 988 cancelled · 0 ended · 2 kept

Reason for undo  [                                              ]   (required)
[Cancel]                                                            [Undo import]
```

Undo-র পরে result একই ভাগে count দেখায়। কোনো row “Waiting — retry” থাকলে batch “Partly undone” দেখায়, সঙ্গে Retry, আর চূড়ান্ত total তখনো আসে না। r212-এর মতো পরের placement প্রথম branch-এ শুধু test fixture দিয়ে তৈরি হয়, কারণ section বদল SIS-009 (R1 backlog)। SIS-009 এলে এটি বাস্তব ঘটনা হবে। Fixture-এ সব link তখনো pending ছিল, তাই C3-এর ফল `cancelled`। Active link থাকলে ফল `ended`।

### S11 · Quick registration

```
New student                                   School A · North campus · School Management · 2026–27
1 Student          Student no. [        ]  Legal name [            ]  Preferred name [          ]
                   Date of birth [ Gregorian | Ethiopian ]   Gender [   v ]
2 Household        [+ Add contact]   Role ( Guardian / Emergency )   Name   Phone
                   Sibling in this school  [ Search student by number or name ]   suggested from the same guardian
3 Class placement  Grade [  v ]  Section [  v ]  Starts on [ Gregorian | Ethiopian ]

[Cancel]                                                                    [Save student]
```

কোন field বাধ্যতামূলক তা pilot school-এর numbering rule ও policy পেলে চূড়ান্ত হবে। Student record-এর পূর্ণ field তালিকা requirement 5.1.1 থেকে আসবে।

### S13 · Roster check

```
Roster check · Grade 6 · Blue · North campus                  Source: Academics (A2) · read only
Date [ 2026-09-13 ]      0 students     A-017 is not on this list
Date [ 2026-09-14 ]     32 students     A-017 is on this list once
```

### বাকি screen

| Screen | কী থাকে |
|---|---|
| S01 Students | Scope-এর ভেতরের student list, search ও filter (SIS-010)। উপরে New student ও Import students। প্রথম branch-এ শুধু navigation-এর জন্য ছোট list। পূর্ণ search ও filter SIS-010-এর branch-এ। |
| S02 Upload | File button ও drop area, template link। শুধু CSV (UTF-8), সীমা ৫,০০০ row ও ১০ MB। Excel পরের branch-এ (OD-08)। |
| S03 Map columns | Source column, তার তিনটি sample মান, target field। বাধ্যতামূলক field চিহ্নিত। তারিখের calendar বাছাই। Class value mapping। যাদের তারিখ নেই তাদের জন্য একটা তারিখ বাছা যায় (ঐচ্ছিক, OD-05), preview-তে চিহ্নিত থাকে। |
| S09 Import history | Batch, কে, কখন, অবস্থা, count। Batch খুললে S08। প্রতিটি batch-এ actor, সময়, action (preview, confirm, retry, undo) ও undo-র কারণ। |
| S12 Student profile | Overview, Household (contact card, access status, Siblings in this school ও link করার সুযোগ), Placement (history ও S13), History (record version, SIS-007)। |

## ৯. Reusable components

| ID | Component | কোথায় |
|---|---|---|
| CMP-01 | Context bar: school · campus · product · year | সব screen |
| CMP-02 | Step indicator | S02–S07 |
| CMP-03 | File upload, button ও drag-drop দুটোই | S02 |
| CMP-04 | Column mapper | S03 |
| CMP-05 | Class value mapper | S03, S05 |
| CMP-06 | Summary count bar, filter হিসেবেও কাজ করে | S04, S08, S10 |
| CMP-07 | Row table, page ধরে load হয় | S01, S04, S08, S09 |
| CMP-08 | Status chip, icon ও লেখা সহ | সব |
| CMP-09 | Row correction drawer | S05 |
| CMP-10 | Side-by-side compare | S06, undo conflict |
| CMP-11 | Decision group, প্রতিটি পছন্দের পাশে তার ফল লেখা | S06, S10 |
| CMP-12 | Dual-calendar date field, হাতে লেখা ও picker দুটোই | S03, S11, S13 |
| CMP-13 | Class reference picker, A2-এর data | S05, S11, S13 |
| CMP-14 | Household contact card, CMP-22-এর access status সহ | S08, S11, S12 |
| CMP-15 | Confirmation summary | S07, S10 |
| CMP-16 | Progress ও result banner | S04, S07, S08, S10 |
| CMP-17 | Dated roster panel, read-only | S08, S12, S13 |
| CMP-18 | History timeline | S09, S12 |
| CMP-19 | Safe denied state | সব |
| CMP-20 | Empty state, inline error ও toast | সব |
| CMP-21 | Review queue navigator: next unresolved, কারণ ধরে filter, বাকি সংখ্যা, save-এর অবস্থা | S06 |
| CMP-22 | Access status chip set: A8 request ও C3 link-এর ছয়টি status, প্রতিটির action সহ | S08, S12 |

Shell-এর সাধারণ component (button, input, dialog, table-এর ভিত্তি) DVL-003 থেকে আসবে। উপরের তালিকা Students-এর নিজের composition।

## ১০. Design tokens (Proposed)

চূড়ান্ত মান DVL-003 ঠিক করবে। এখানে শুধু এই flow-এর দরকারি semantic token ও placeholder মান আছে। প্রতিটি রঙের contrast V16-এ মাপা হবে।

| Token | কাজ | Placeholder |
|---|---|---|
| `color.surface.base`, `color.surface.raised` | পাতা ও card | `#FFFFFF`, `#F8FAFC` |
| `color.text.primary`, `color.text.secondary` | লেখা | `#0F172A`, `#475569` |
| `color.border.default` | শুধু সাজানোর divider, control চেনার জন্য নয় | `#CBD5E1` (white-এ 1.5:1) |
| `color.border.control` | Input, checkbox, radio ও অন্য control-এর সীমা | `#64748B` (white-এ 4.8:1, `surface.raised`-এ 4.5:1) |
| `color.action.primary` | প্রধান button | `#1D4ED8` |
| `color.status.ready` | Ready, Created, Active | `#15803D` |
| `color.status.review` | Needs review, Pending, Conflict | `#92400E` |
| `color.status.error` | Error, Failed | `#B91C1C` |
| `color.status.neutral` | Excluded, Not imported, Reversed | `#475569` |
| `color.focus.ring` | Keyboard focus | `#1D4ED8`, ২px রেখা ও ২px ফাঁক |
| `space.1`–`space.7` | ফাঁক | ৪, ৮, ১২, ১৬, ২৪, ৩২, ৪৮ px |
| `font.size.xs`–`xl` | লেখার মাপ | ১২, ১৪, ১৬, ২০, ২৪ px। Body ১৬ px |
| `font.lineHeight.body` | লাইনের উচ্চতা | ১.৫, Amharic-এ ১.৬ |
| `font.family.base` | Font | System UI, আর Ethiopic অক্ষরের জন্য Ethiopic-সমর্থিত font |
| `radius.sm`, `radius.md` | কোণ | ৪, ৮ px |
| `size.target.min` | Click বা touch target | ২৪×২৪ px ন্যূনতম, touch-এ ৪৪×৪৪ px |
| `motion.fast`, `motion.base` | Transition | ১৫০, ২৫০ ms। Reduced motion চাইলে বন্ধ |
| `breakpoint.md`, `lg`, `xl` | Layout | ৬০০, ১০২৪, ১৪৪০ px |

নিয়ম: status কখনো শুধু রং দিয়ে বোঝানো হবে না। Table-এর সংখ্যা সমান-প্রস্থের অঙ্কে থাকবে যাতে count মিলিয়ে পড়া সহজ হয়।

Placeholder রঙের contrast white-এ মাপা: text.primary 17.9:1, text.secondary 7.6:1, action.primary ও focus ring 6.7:1, status.ready 5.0:1, status.review 7.1:1, status.error 6.5:1, border.control 4.8:1। Text-এ অন্তত 4.5:1, আর control-এর সীমা, focus ও status icon-এ অন্তত 3:1 লাগবে। DVL-003 চূড়ান্ত রং দিলে আবার মাপা হবে।

## ১১. Responsive আচরণ

Doc 05 অনুযায়ী bulk import office-এর web কাজ। তাই import wizard desktop-first।

| প্রস্থ | আচরণ |
|---|---|
| ১০২৪ px ও বেশি | পুরো wizard। S06-এ দুই card পাশাপাশি। S05 ডান পাশের drawer। |
| ৬০০–১০২৩ px | পুরো wizard চলে। Table-এ কম জরুরি column ভাঁজ হয়ে row-এর ভেতরে যায়। S06-এ দুই card পাশাপাশি থাকে, লেখা ভাঙে। S05 পুরো পর্দার sheet। |
| ৬০০ px-এর কম | Batch-এর অবস্থা, result, student search, profile ও quick registration (এক column)। S06-এ card উপর-নিচে, প্রতি field-এ “same” বা “differs” লেখা। Upload, mapping ও bulk fix-এ বলা হয় বড় পর্দায় খুলতে, তবে draft হারায় না। Batch page থেকে “Review duplicates (n)” দিয়ে S06-এ যাওয়া যায়, “Back to batch” দিয়ে ফেরা যায়। Confirm ও undo এই প্রস্থে করা যায় না (Proposed): summary দেখায় আর বড় পর্দায় খুলতে বলে। Draft হারায় না। |

Phone-এ wizard সীমিত রাখা একটি Proposed সিদ্ধান্ত। Text ২০০% বড় করলেও কাজ চলবে। Table ছাড়া বাকি content ৩২০ px প্রস্থে আড়াআড়ি scroll ছাড়া পড়া যাবে।

## ১২. Accessibility

লক্ষ্য WCAG 2.2 AA (PLT-018, V16)।

- পুরো import, duplicate review, undo ও quick registration শুধু keyboard দিয়ে করা যায়। Focus-এর ক্রম পর্দার ক্রমের সঙ্গে মেলে।
- Step বদলালে focus নতুন step-এর শিরোনামে যায়। Drawer খুললে focus ভেতরে যায়, বন্ধ হলে আগের জায়গায় ফেরে।
- File upload-এ drag-drop-এর পাশে সাধারণ button থাকে।
- Table-এ সঠিক header থাকে। Row-এর status লেখা হিসেবে পড়া যায়।
- Dry run, confirm ও undo-র progress এবং count বদল screen reader-কে জানানো হয়, কাজে বাধা না দিয়ে।
- Error summary থেকে সরাসরি ভুল field-এ যাওয়া যায়। প্রতিটি ভুলের বার্তায় কী ঠিক করতে হবে তা লেখা থাকে।
- তারিখ হাতে লেখা যায়। দুই calendar-এর মান পাশাপাশি পড়া যায়।
- Amharic লেখায় সঠিক language চিহ্ন থাকে, যাতে screen reader ঠিক উচ্চারণ করে।
- কোনো সময়সীমায় কাজ হারায় না। Draft batch ও duplicate-এর সিদ্ধান্ত server-এ থাকে।
- Phone number-এর মতো তথ্য দরকারের বেশি দেখানো হয় না। Shared device-এ কম দেখানোর নিয়ম requirement 5.1.C থেকে।
- Text-এর contrast অন্তত 4.5:1। Control-এর সীমা, focus ring ও status icon-এর অন্তত 3:1 (§১০)।
- S06-এ screen reader সিদ্ধান্তের save-এর অবস্থাও পড়ে।
- DVL-003-এর prototype test-এ একজন Registrar শুধু keyboard দিয়ে, আরেকজন screen reader দিয়ে ৩০টি duplicate row শেষ করেন। কোথায় আটকান আর কতক্ষণ লাগে তা লেখা হয়। সময়ের কোনো target দাবি করা হয় না।

## ১৩. Loading, empty, error ও success state

| Screen | Loading | Empty | Error | Success |
|---|---|---|---|---|
| S01 Students | Table-এর কাঠামো | “No students in your scope yet.” সঙ্গে New student ও Import students | List আনা যায়নি, Retry | — |
| S02 Upload | File পড়া হচ্ছে, progress | — | ভুল format, খালি file, header নেই, সীমার বেশি (৫,০০০ row বা ১০ MB)। প্রতিটিতে কী করতে হবে লেখা | Draft batch তৈরি, পরের ধাপে |
| S03 Map columns | Sample মান আসছে | — | বাধ্যতামূলক field map হয়নি। A2 থেকে section আনা যায়নি | Mapping save |
| S04 Preview | Dry run চলছে, কত row শেষ হয়েছে | সব row Ready হলে “No problems found.” | Dry run থেমে গেছে, আবার চালান। Draft থাকে | Count ও row |
| S05 Row correction | Row আসছে। Saving | — | মান এখনো ভুল। Row অন্য কেউ বদলেছেন, নতুন মান দেখান। Save হয়নি, আবার চেষ্টা করুন | Saved। আর ভুল না থাকলে row Ready। আরেকটি ভুল থাকলে Error থাকে বাকি কারণ সহ। পরের সমস্যায় যায় |
| S06 Duplicate review | Existing record আসছে। Saving | “No possible duplicates.” | Existing record আনা যায়নি, Retry। Not saved — try again, সিদ্ধান্ত হারায় না | Saved, পরের সিদ্ধান্তহীন row |
| S07 Confirm | Confirm চলছে, progress। Page ছাড়া নিরাপদ | Import করার মতো row নেই: Confirm বন্ধ, কারণ সহ | Access বদলে গেছে। Section বদলে গেছে, আবার preview করুন। একই confirm আগে হয়ে গেছে, আগের ফল দেখান। Batch আগেই confirm হয়েছে, অন্য key দিয়েও: বর্তমান অবস্থা দেখান | S08-এ যায় |
| S08 Result | Result আসছে | Guardian tab-এ “No access requests in this import.” | কিছু row বা request Failed, কারণ ও Retry। Blocked — access changed: কারণ, আর নতুন confirm দিয়ে বাকি row চালানোর পথ। Job চালানো নিরাপদ না হলে (`job_integrity`): “This import could not run safely. Nothing was saved. Start again.” (System 1-এর Draft v3-এর copy) | Count মেলে, পরের কাজ দেখায় |
| S09 Import history | Import তালিকা আসছে | “No imports yet.” সঙ্গে Import students | তালিকা আনা যায়নি, Retry | — |
| S10 Undo | Impact হিসাব হচ্ছে। Undo চলছে | “Nothing left to undo.” | Preview পুরোনো হয়ে গেছে, আবার হিসাব করুন। কিছু row Conflict। Waiting — retry: কারণ ও Retry, batch Partly undone। ৩০ দিন পেরিয়ে গেলে Undo বন্ধ, কারণ ও profile থেকে ঠিক করার পথ সহ | Reversed, Kept (existing), Conflict ও Not imported-এর count, যোগফল মোট row |
| S11 Quick registration | Save হচ্ছে, button বন্ধ | — | Field-এর ভুল। সম্ভাব্য duplicate। Section বাতিল | Profile খোলে, পরের কাজ দেখায় |
| S12 Student profile | Profile আসছে | Household-এ এখনো কোনো contact নেই, সঙ্গে Add contact | Student পাওয়া যায়নি বা আপনার scope-এর বাইরে (একই বার্তা)। Save হয়নি, আবার চেষ্টা করুন | Saved, নতুন version History-তে |
| S13 Roster check | Roster আসছে | “No students on this date.” | Academics সাময়িকভাবে সাড়া দিচ্ছে না: Retry। A2-এর interface এখনো নেই: “Roster check is waiting for Academics.” | তারিখ ধরে তালিকা |
| সব | — | — | Permission বা product বন্ধ: নিরাপদ বার্তা, অন্য school বা campus-এর কিছু প্রকাশ পায় না। School read-only হলে read চলে, কিন্তু নতুন import, registration ও undo বন্ধ, কারণ সহ। School suspended হলে নিরাপদ বার্তা। শেষ দুটির code System 1-এর draft-এর প্রস্তাব, এখনো সিদ্ধান্ত নয় (implementation plan অংশ ৫) | — |

## ১৪. অগ্রাধিকার ও acceptance criteria

P0 হলো প্রথম branch-এর flow। P1 ও P2 A8-এর বাকি MVP candidate। মাপার সংখ্যাগুলো count বা pass/fail। সময়ের target (যেমন ১,০০০ row-এর preview কত সেকেন্ডে) কোনো source-এ নেই, তাই Foundation-এ ঠিক হওয়া পর্যন্ত pending।

### P0

**P0-1 · Upload, mapping ও dry-run preview** (PLT-014 · DR-03, DR-04)
Registrar কিছু save হওয়ার আগেই দেখেন কোন row যাবে আর কোনটি আটকাবে।
- Preview শেষে নতুন student, placement ও access grant-এর সংখ্যা ০ বাড়ে।
- Ready + Needs review + Error + Excluded = file-এর মোট row।
- প্রতিটি Error ও Needs review row-তে অন্তত একটি কারণ ও পরের কাজ লেখা থাকে।
- File বা form-এ অন্য school-এর ID দিলেও target school বদলায় না।
- ১,০০০ row-এর file-এ preview শেষ হয়, আর page refresh-এর পরে একই count ফেরে।
- ৫,০০০ row বা ১০ MB-এর বেশি file reject হয়, কারণ সহ।

**P0-2 · Row correction ও invalid section** (PLT-014, dependency SIS-003 · DR-05)
পুরো file আবার না বানিয়ে ভুল ঠিক করা যায়।
- অজানা section-এর row কখনো Ready হয় না।
- একটি class value mapping দিলে একই ভুল মানের সব row একবারে ঠিক হয় ও count বদলায়।
- ঠিক করা row-তে file-এর মূল মান, কে ও কখন বদলেছেন তা দেখা যায়।
- Section picker-এ শুধু ওই school, campus ও year-এর valid section আসে।
- Correction-এর পরে row-এ আরেকটি ভুল থাকলে row Error থাকে, বাকি কারণগুলো সহ।

**P0-3 · Duplicate review** (PLT-014, SIS-001 · DR-06)
একই student দুইবার তৈরি হয় না, আর ভুল করে দুই আলাদা student মিলেও যায় না।
- School B-তে একই number থাকলে School A-র import-এ কোনো flag আসে না।
- Flag-এর দুই কারণ আলাদা লেখায় আসে: number collision, আর same name and date of birth। কোনোটিই নিজে থেকে “same student” ধরে না।
- প্রতিটি সিদ্ধান্তের নিচে profile, placement, contact ও access-এর ফল save-এর আগেই দেখা যায়।
- Flagged row-তে সিদ্ধান্ত না দিলে সেটি import হয় না।
- “Use the existing record” বাছলে existing profile-এর field অপরিবর্তিত থাকে।
- Scope-এর বাইরের candidate-এর নাম, field, campus বা count দেখা যায় না, আর তার সঙ্গে link করা যায় না।
- প্রতিটি সিদ্ধান্তে actor ও সময় থাকে, save-এর অবস্থা দেখা যায়, আর confirm-এর আগে বদলানো যায়।
- দুই existing record merge করার কোনো action নেই।
- আগের batch-এ import হওয়া row আলাদা label পায় ও Skip আগে থেকে বাছা থাকে। Undo হওয়া student-এর number হলে সিদ্ধান্ত তিনটি: Reactivate, number ছেড়ে নতুন record, বা Skip।
- শুধু keyboard দিয়ে, আর screen reader দিয়ে, একটি row-এর সিদ্ধান্ত দিয়ে পরের সিদ্ধান্তহীন row-তে যাওয়া যায়।

**P0-4 · Confirmation ও result** (PLT-014, SIS-001, SIS-007 · DR-07, DR-19)
Registrar জানেন ঠিক কী তৈরি হয়েছে, আর একই কাজ দুইবার হয় না।
- Result-এর প্রতিটি সংখ্যা confirm-এর আগের summary-র সমান, অথবা প্রতিটি পার্থক্যের row কারণ সহ দেখায় (যেমন Failed, বা preview-এর পরে বাতিল হওয়া section)।
- একই confirm দুইবার গেলে student, placement ও access request-এর সংখ্যা বাড়ে না, মূল receipt ফেরে। Confirm হয়ে যাওয়া batch অন্য key দিয়েও আবার confirm হয় না।
- আগে import হয়ে এখনো কার্যকর row আবার এলে কিছু বদলায় না (No change)।
- কিছু row ব্যর্থ হলে বাকি row-এর ফল থাকে। ব্যর্থ row-তে কারণ ও Retry থাকে। Retry-তে duplicate হয় না।
- Created + Reactivated + Linked to existing + Not imported + Failed = মোট row। Not imported কারণ ধরে ভাগ করা থাকে।
- Page ছেড়ে গেলেও confirm চলে, ফিরে এসে অবস্থা দেখা যায়।

**P0-5 · Household contact ও guardian access pending** (SIS-002 pilot অংশ, dependency IAM-004 · DR-08)
Registrar বুঝতে পারেন কার contact আছে আর কে সত্যিই access পেয়েছেন।
- Guardian ও emergency contact save হয় ও দেখা যায়। Sibling হিসেবে একই school-এর enrolled student link করা যায় (A-15)। Pickup role pilot-এর UI-তে নেই।
- Access-এর ছয়টি status (F3) প্রতিটি ঠিক জায়গায় দেখা যায়, প্রতিটির পাশে সঠিক action থাকে।
- C3 Active না জানানো পর্যন্ত কোনো chip Active দেখায় না। A8 নিজে কোনো link status বানায় না।
- তিন child-এর guardian-এ প্রতি child-এর status আলাদা দেখায়।
- Access request ব্যর্থ হলে student ও placement অপরিবর্তিত থাকে। Retry-তে দ্বিতীয় request হয় না।
- Custody restriction এই screen থেকে বদলানো যায় না।

**P0-6 · প্রথম placement ও roster যাচাই** (dependency SIS-003, CLS-001 · DR-09, DR-10)
Student ঠিক দিন থেকে ঠিক class-এ দেখা যায়।
- Effective date ছাড়া placement save হয় না।
- শুরুর আগের দিনের roster-এ student ০ বার, শুরুর দিনে ১ বার।
- Roster panel read-only, আর সেখান থেকে placement বদলানো যায় না।
- তারিখ দুই calendar-এ পাশাপাশি দেখা যায়।
- Year শুরুর আগের তারিখ save হয় না। চলতি year-এর আগের তারিখে সতর্কবার্তা দেখায়।
- Mapping-এ বেছে দেওয়া তারিখ শুধু খালি row-এ বসে, আর preview-তে চিহ্নিত থাকে।

**P0-7 · Undo** (PLT-014 · DR-11, DR-19)
ভুল import নিরাপদে ফেরানো যায়, অন্য কাজ নষ্ট না করে।
- Undo-র আগে impact preview পাঁচ ভাগ দেখায়: Will be reversed, Existing students kept, Needs your review, Not part of this import, Total। Total = মোট row।
- ইতিমধ্যে Active access কতগুলো শেষ হবে, তা আলাদা সংখ্যায় দেখায়।
- Batch-এর আগে থেকে থাকা student কখনো সরে না। এই batch তাদের যা যোগ করেছিল, শুধু সেটুকু ফেরে।
- Batch-এর পরে placement, অনুমোদিত edit বা অন্য module-এর record থাকা row-এর কিছুই বদলায় না, access-ও না। Row conflict review-তে যায়।
- Undo চলার সময় প্রতিটি row আবার যাচাই হয়। Preview-এর পরে বদলে যাওয়া row Conflict হয়।
- “Waiting — retry” row-এর কিছুই বদলায় না, Retry সহ দেখা যায়। এগুলো শেষ না হওয়া পর্যন্ত চূড়ান্ত total দেখায় না।
- প্রতিটি row-এ A8-এর reversal ও C3-এর link শেষ করা একসঙ্গে হয়, আধাআধি নয়।
- Confirm-এর ৩০ দিন পরে Undo বন্ধ হয়, কারণ সহ।
- Reversed + Kept (existing) + Conflict + Not imported = মোট row।
- Import history-তে undo-র actor, সময় ও কারণ দেখা যায় (DR-19)।

**P0-8 · Quick registration** (ADM-021 · DR-12)
একজন student-এর জন্য file বানাতে হয় না।
- Import-এর একই validation ও duplicate check চলে।
- Save দুইবার চাপলেও একটিই student তৈরি হয়।
- Save-এর পরে profile-এ student number থাকে ও পরের কাজ দেখায়।

**P0-9 · Access ও context state** (dependency IAM-003, PRD-001, PLT-002 · DR-01, DR-02)
ভুল school বা campus-এ কাজ হওয়ার সুযোগ থাকে না।
- অন্য campus-এর Registrar, Learning Admin, মেয়াদোত্তীর্ণ session ও School Management বন্ধ school, এই চারটিতেই write বন্ধ।
- চারটির কোনোটিতেই অন্য school বা campus-এর নাম, row বা count প্রকাশ পায় না।
- প্রতিটি import screen-এ school, campus, product ও year দেখা যায়।

**P0-10 · Accessibility ও responsive acceptance** (dependency PLT-018 · DR-17, DR-18)
সব Registrar, যেকোনো device ও assistive technology নিয়ে, একই কাজ শেষ করতে পারেন।
- F1, F2 ও F6 শুধু keyboard দিয়ে শেষ করা যায়। প্রতিটি control-এ focus দেখা যায়।
- একটি desktop screen reader দিয়ে S04, S06, S07 ও S10-এর count, কারণ ও save-এর অবস্থা শোনা যায়।
- 320 CSS px প্রস্থে table ছাড়া content আড়াআড়ি scroll ছাড়া পড়া যায়। Text 200% করলে কিছু হারায় না।
- Text-এর contrast অন্তত 4.5:1। Control-এর সীমা, focus ও status icon-এর অন্তত 3:1।
- Click target অন্তত 24×24 CSS px, touch-এ 44×44।
- §১৩-এর প্রতিটি state-এ পৌঁছানো যায় এবং বার্তা পড়া যায়।

### P1

**P1-1 · Student profile ও record history** (SIS-001, SIS-007 · DR-13)
কে কী বদলেছেন তা পরে দেখা যায়।
- প্রতিটি edit-এ আগের মান, নতুন মান, actor ও সময় থাকে। পুরোনো version মুছে যায় না।
- History-তেও field-level permission মানা হয়।

**P1-2 · Search ও filter** (SIS-010 · DR-14)
Registrar দ্রুত সঠিক student খুঁজে পান।
- Student number, নাম, grade, section ও status দিয়ে খোঁজা যায়।
- Scope-এর বাইরের student ফলে আসে না, মোট count-এও গোনা হয় না।
- প্রথম branch-এ S01 শুধু navigation-এর ছোট list। এই criteria SIS-010-এর branch-এ মাপা হবে।

**P1-3 · List ও export** (SIS-016 · DR-15)
Office দরকারি তালিকা নিতে পারে, অনুমতির বাইরের তথ্য ছাড়া।
- Export-এ শুধু ওই role-এর permitted field থাকে।
- প্রতিটি export-এর audit entry থাকে।
- দুই school-এ একই export চালালে প্রত্যেকে শুধু নিজের data পায় (V01)।

### P2

**P2-1 · Need-to-know flag** (SIS-006 · DR-16)
Teacher জানেন কোন student-এর দিকে খেয়াল রাখতে হবে, বিস্তারিত না জেনেই।
- Assigned teacher flag দেখেন, detail দেখেন না।
- Assigned নয় এমন teacher flag-ও দেখেন না।
- Principal সিদ্ধান্ত দেন, Registrar শুধু flag-এর ধরন লেখেন, detail নয় (OD-10)।
- Flag-এ detail লেখার কোনো জায়গা নেই। কে অনুরোধ করেছিলেন ও কবে, তা record-এ থাকে।

## ১৫. SIS-002 · pilot contact বনাম R1 pickup coverage

| | Pilot অংশ (MVP slice) | বাকি R1 অংশ |
|---|---|---|
| কী | Guardian ও emergency contact, আর sibling হিসেবে একই school-এর enrolled student-এর link (A-15) | Pickup contact-এর coverage |
| Screen | S11 ও S12-এর Household, import-এর contact column | এই plan-এ কোনো screen নেই |
| Contact role | Guardian, Emergency। Sibling আলাদা link, contact নয় | Pickup যোগ হবে |
| Data | Role-এর তালিকা পরে বাড়ানো যায় এমনভাবে রাখা হবে, যাতে pickup যোগ করতে পুরোনো data বদলাতে না হয় | Pickup-এর নিজস্ব field ও নিয়ম |
| Evidence | Tracker-এর `MVP slice evidence` | Tracker-এর `R1 remainder evidence` |
| Test | V11, V02-এর guardian অংশ | V02 ও X-06-এর pickup list অংশ |

Custody restriction (IAM-004) pilot-এই প্রযোজ্য, আর সেটি C3-এর কাজ। Gate-এ pickup code যাচাই ATT-010 (R2), সেটি এই plan-এর বাইরে। Pilot অংশের evidence দিয়ে পুরো SIS-002 Verified করা যাবে না।

Sibling-এর অর্থ OD-12-এ চূড়ান্ত হবে।

## ১৬. Proposals

এই plan-এ Blueprint v2-এর বাইরের কোনো proposal নেই। আগের PR-01 থেকে PR-05 owner-এর সিদ্ধান্তে বাদ গেছে (২৬ নভেম্বর ২০২৬), কারণ কোনোটিই Blueprint v2-এ নেই। কোনো screen, flow বা acceptance criterion এগুলোর ওপর নির্ভর করত না। নতুন proposal আসবে শুধু owner চাইলে, আর তার ID হবে S3-PR-nn।

## ১৭. সিদ্ধান্ত

২৬ নভেম্বর ২০২৬-এ আপনি সব সুপারিশ মেনেছেন। নিচে প্রতিটি সিদ্ধান্ত। শেষ কলামে লেখা আছে কোথায় অন্য system-এর কাজ বা সম্মতি বাকি।

| ID | প্রশ্ন | সিদ্ধান্ত | প্রভাবিত কাজ | বাকি নির্ভরতা |
|---|---|---|---|---|
| OD-01 | CSV-র পুরোনো student number, নাকি নতুন? | School-এর পুরোনো number-ই থাকবে। Pilot-এ নতুন student-এর number Registrar নিজে লেখেন, system শুধু school-এ unique কি না দেখে | S03, S06, S11, P0-3, P0-4 | — |
| OD-02 | কোন তথ্যে duplicate ধরা হবে? | একই school-এ একই number হলে number collision। নাম ও জন্মতারিখ (TD-05-এর নিয়মে গুছিয়ে) মিললে সম্ভাব্য একই child। শুধু phone মিললে flag নয়, fuzzy মেলানো নেই। সিদ্ধান্ত Registrar-এর | S06, S11, P0-3 | — |
| OD-03 | Guardian-এর পরিচয় অনিশ্চিত হলে confirm হবে? | Student ও placement confirm হয়। Guardian link pending, access বন্ধ | F3, S07, S08, S11, P0-5 | — |
| OD-04 | Custody restriction-এর দ্বিতীয় approver কে? | Principal, school-এর Approvals policy-তে (Owner policy approve করেন) | F3, S08 ও S12-এর copy | System 2-এর plan-এ বসানো |
| OD-05 | তারিখ খালি থাকলে, বা আগের তারিখ? | System নিজে তারিখ বসায় না। Mapping-এ Registrar চাইলে সব খালি row-এর জন্য একটা তারিখ বাছেন, preview-তে চিহ্নিত। চলতি academic year-এর আগের তারিখ সতর্কবার্তা সহ চলে। Year শুরুর আগের তারিখ নয় | S03, S04, S05, S11, S13, P0-1, P0-6 | — |
| OD-06 | Undo কে, কতদিন, Active access? | ওই campus-এর import অধিকার থাকা Registrar, কারণ লিখে। Confirm-এর পরে ৩০ দিন, তারপর profile থেকে ঠিক করা। Active access-ও শেষ হয়, preview-তে আলাদা সংখ্যায় | F6, S10, P0-7 | — |
| OD-07 | Access request একসাথে, নাকি একজন একজন? | Confirm-এ সব valid guardian-এর pending link একসাথে। Invite বা activation আলাদা, পরিচয় যাচাইয়ের পরে, C3-এর IAM-002 নিয়মে | S07, S08, P0-5 | Pilot batch invite (System 2-এর PR-06, CX-13): project owner গৃহীত, client product owner-এর অনুমোদন বাকি। এটি C3-এর আলাদা ধাপ। A8 শুধু pending link-এর তালিকা ও filter (batch, class, campus) দেয়। Import কোনো invite পাঠায় না |
| OD-08 | CSV না Excel? | প্রথম branch-এ শুধু CSV (UTF-8), template সহ। Excel (.xlsx) ছোট আলাদা branch-এ, R1-এর ভেতরে, PLT-014 Verified হওয়ার আগে | S02 | — |
| OD-09 | Registrar-এর screen কে বানাবে? | Students-এর screen System 3, shell System 8 (APP-009), token ও component DVL-003 | পুরো UI | — |
| OD-10 | SIS-006-এর flag কে লেখেন? | Principal সিদ্ধান্ত দেন, Registrar শুধু flag-এর ধরন লেখেন, detail নয়। অনুরোধকারী record-এ থাকে | P2-1 | — |
| OD-11 | অন্য campus-এর student-এর number এলে? | Detail দেখানো হয় না, link করা যায় না। Skip, অথবা ভিন্ন child হলে নতুন number। একই child হলে campus transfer (SIS-009) | §৫, S06, P0-3 | — |
| OD-12 | Sibling মানে কী? | একই school-এর enrolled student-এর link। School-এ নেই এমন sibling রাখা হয় না, import sibling আনে না। কারণ FIN-002-এর sibling pricing-এ enrolled sibling লাগে | S11, S12, §১৫, P0-5 | — |
| OD-13 | Students merge-এর আগে real A1/A2 কে দেবে? | System 1–3-এর cross-check-এর পরেই System 4-এর একজন planner ছোট A1/A2 slice 0 plan করবেন, পাশাপাশি তৈরি হবে। Merge-এর ক্রম Platform → Identity 1a → A1/A2 slice 0 → Students। Students কোনো stub বানায় না, শুধু test-এর fake | §১৮, P0-6-এর real evidence | Slice 0-এর plan ও তৈরি বাকি |
| System 1-এর OD-7 | Flag বন্ধ করে আবার চালু করলে চলতি import? | নিজে থেকে চলে না। Blocked — access changed, নতুন confirm দিয়ে বাকি row | F1-এর batch অবস্থা, S08 | System 1-এর plan-এ OD-7 বন্ধ করা |

## ১৮. অন্য system-এর ওপর নির্ভরতা

| কার কাছ থেকে | কী লাগবে | না থাকলে screen-এ কী দেখাবে |
|---|---|---|
| C1, C2, C9 (System 1) | Session-এর school context, entitlement, isolation, audit (PLT-006), file service (PLT-013), দুই calendar (PLT-015), দুই ভাষা (PLT-016)। System 1-এর queued-job guard-এর flag-এর নিয়ম আপনি মেনেছেন (Blocked, নতুন confirm)। Draft v3-এর চাওয়াগুলোও এই plan-এর সঙ্গে মেলে: নতুন job type, lease ও claim token, chunk checkpoint, আর actor-এর access মাঝে বন্ধ হয়ে ফিরলেও job Blocked। Chunk size CTO-র cross-check-এ খোলা। বিস্তারিত implementation plan-এর অংশ ৫-এ | Product বন্ধ হলে নিরাপদ denied state |
| C3 (System 2) | Registrar-এর authorize সিদ্ধান্ত, guardian link command ও status, IAM-014 matrix-এ Students-এর action। Link status (`pending`, `active`, `restricted`, `ended`), আর undo-তে নির্দিষ্ট link ID ধরে link শেষ করা। Custody restriction-এর দ্বিতীয় approver Principal (OD-04)। Undo-তে প্রত্যাহারের ফল চারটি: `ended`, `cancelled`, `conflict`, `not_found` (CX-12)। MVP-তে কোনো student চলে গেলে Registrar C3-এর screen-এ হাতে guardian link শেষ করেন (CX-04)। | Access chip “Not available yet” |
| A1, A2 (System 4) | Valid class reference ও তারিখ ধরে roster। Students merge-এর আগে real class reference ও roster লাগবে। OD-13-এর সিদ্ধান্ত: System 4-এর একজন planner A1/A2 slice 0 plan করবেন, পাশাপাশি তৈরি হবে। Students কোনো stub বানায় না। | Picker খালি ও কারণ। Roster panel “waiting for Academics” |
| C5 (System 8) | Office workspace-এর shell ও navigation (APP-009) | — |
| DVL-003 | চূড়ান্ত token ও সাধারণ component | এই নথির Proposed token |

## পরিবর্তনের রেকর্ড

**Review round 1 · ২৬ নভেম্বর ২০২৬।** Review report-এর findings যাচাই করে এই পরিবর্তনগুলো হয়েছে:

- Duplicate review: number collision ও same name/birth date আলাদা, দ্বিতীয় ও scope-এর বাইরের উদাহরণ, সিদ্ধান্তের ফলের table, queue ও save-এর অবস্থা (F1, S06, P0-3)।
- Already imported, imported-and-undone, already placed ও scope-এর বাইরের row-এর নিয়ম (F1, A-10, A-11, A-13, A-14)।
- Access-এর সাতটি status, C3-এর চারটি link status-এর সঙ্গে মিলিয়ে (F3, S08, P0-5)।
- Undo preview-এ পাঁচ ভাগ ও মোট row, kept access, undo চলার সময় আবার যাচাই, Access withdrawal pending ও Waiting for check (F6, S10, P0-7)।
- S04-এর copy, S07-এর breakdown, P0-4-এ পার্থক্য কারণ সহ মেলানোর শর্ত।
- State table-এ S05, S06, S09, S10, S12 ও S13-এর বাকি state। Phone-এ S06-এ যাওয়া ও ফেরা।
- Control border token, contrast-এর মাপ ও P0-10।
- Saved mapping (আগের P2-2) proposal PR-05-এ সরানো।
- Sibling-এর নিয়ম (A-15), আর নতুন প্রশ্ন OD-11, OD-12 ও OD-13।

**আপনার সিদ্ধান্তের পরে · ২৬ নভেম্বর ২০২৬।** আপনি সব সুপারিশ মেনেছেন। এই পরিবর্তনগুলো হয়েছে:

- অংশ ১৭ এখন সিদ্ধান্তের তালিকা। Assumption-গুলোর পাশে সিদ্ধান্তের ID।
- Duplicate ও re-import: undo হওয়া number-এ তিনটি সিদ্ধান্ত, ভিন্ন child হলে number ছাড়া (TD-01)।
- তারিখ: mapping-এ খালি row-এর জন্য একটা তারিখ, আগের তারিখে সতর্কবার্তা, year শুরুর আগের তারিখ নয় (OD-05)।
- Undo: ৩০ দিনের সীমা, একটাই অপেক্ষার অবস্থা “Waiting — retry”, প্রতিটি row-এ reversal ও link শেষ করা একসঙ্গে (OD-06, TD-04)।
- Access status ছয়টি, “Sending” বাদ (TD-04)।
- Flag বা access বদলালে “Blocked — access changed”, নিজে থেকে আবার চলে না।
- File: শুধু CSV, ৫,০০০ row ও ১০ MB-এর সীমা, ৩০ দিনে raw file মোছা (OD-08, TD-02)।
- Custody approver Principal (OD-04), flag লেখার নিয়ম (OD-10), A1/A2 slice 0 (OD-13)।

**CTO-র cross-check-এর পরে · ২৬ নভেম্বর ২০২৬।** System 2-এর owner-গৃহীত cross-system item-এর সঙ্গে মেলাতে:

- TD-04-এর পথ সুপারিশ, Identity-র T-00-এর শর্তে (CX-14)। বিকল্প: durable outbound record + idempotent command।
- Undo-তে প্রত্যাহারের ফল `ended` বা `cancelled` (CX-12)। S10-এ pending link-এর ফল `cancelled` 988।
- OD-07: pilot batch invite System 2-এর PR-06 (CX-13), C3-এর আলাদা ধাপ।
- MVP-তে Registrar হাতে link শেষ করেন (CX-04)।
- Blocked-এর কারণে permission matrix-এর ঘর বদলও যোগ হয়েছে। `students.export` T-01 catalogue-এ, ব্যবহার SIS-016-এ।
- CX-15: প্রথম branch-এ C3 `student.changed` পড়ে না। Undo-র অপেক্ষার কারণের নাম “Access withdrawal pending”। Reactivation পুরোনো link ফেরায় না। Undo হওয়া student সাধারণ read-এ not-found (A-11)।

**Owner-এর নিয়মের পরে · ২৬ নভেম্বর ২০২৬ (CTO-র মাধ্যমে)।** নিয়ম: শুধু Blueprint v2 PDF-এর ভেতরের কাজ, আর scope-এর দাবিতে Blueprint-এর পাতা।
- System 2-এর PR-05 (pupil activation code) বাদ গেছে। এই plan-এ ওই ধরনের কিছু ছিল না। A-05-এ স্পষ্ট লেখা হলো: import বা quick registration কোনো pupil account বা activation code তৈরি করে না (IAM-001, D-16, Blueprint পৃ. ৬৪, ৯৮)।
- এই plan-এর PR-05 আলাদা জিনিস (saved column mapping), pupil activation code নয়। PR-01–PR-05 সবই Blueprint-এর বাইরের proposal; এগুলো রাখা হবে কি না, আপনার উত্তরের অপেক্ষায়।
- অংশ ৩-এ Blueprint-এর পাতা যোগ হয়েছে। Blueprint PDF এখন `pdftotext -layout` দিয়ে পড়া যায় (অংশ ২.৩)।

**Proposal বাদের পরে · ২৬ নভেম্বর ২০২৬ (owner-এর সিদ্ধান্ত, CTO-র মাধ্যমে)।**
- PR-01 থেকে PR-05 বাদ, কারণ কোনোটিই Blueprint v2-এ নেই। অংশ ১৬-এ এখন শুধু সেই কথা লেখা।
- S06-এর নিয়মে PR-02-এর উল্লেখ সরানো: সিদ্ধান্ত সবসময় row ধরে, একসাথে অনেক row-এ নয়।
- কোনো screen, flow বা acceptance criterion এই proposal-গুলোর ওপর নির্ভর করত না, তাই আর কিছু কমাতে হয়নি।
- অন্য system-এর proposal-এর সঙ্গে গোলমাল এড়াতে PR-06-কে “System 2-এর PR-06” লেখা হয়েছে। এই plan-এ নতুন proposal আসবে শুধু owner চাইলে, ID S3-PR-nn।

**Student count ও event বাদের পরে · ২৬ নভেম্বর ২০২৬ (CTO-র মাধ্যমে)।**
- System 1-এর revert panel owner বাদ দিয়েছেন। তার জন্য চাওয়া student count interface-ও বাদ (অংশ ১৮)।
- System 2 CX-15-এর reconciliation alert বাদ দিয়েছে। C3 student-এর কোনো status event পড়ে না।

## উৎস

[Doc 02](../../02-mvp-release-and-branch-plan.md) · [Doc 03](../../03-system-module-and-data-map.md) · [Doc 04](../../04-roles-and-permissions.md) · [Doc 05](../../05-workflows-and-ui-ux.md) · [Doc 06](../../06-school-policies-and-integrations.md) · [Doc 07](../../07-qa-pilot-and-acceptance.md) · [Doc 08](../../08-migration-launch-and-handover.md) · [INTERFACES](../../integration/INTERFACES.md) · [System 3 brief](../../integration/SYSTEM-AGENT-BRIEFS.md) · [features.csv](../../tracker/features.csv) · [deliverables.csv](../../tracker/deliverables.csv)
