# System 4 · Academics and Attendance · implementation plan

**অবস্থা · review report অনুযায়ী সংশোধিত খসড়া · ২৬ নভেম্বর ২০২৬।** A1/A2 slice 0 প্রথম deliverable। Students merge candidate-এ তার class ও roster contract যাচাই হবে। এই খসড়ায় revision ও commit-এর ক্রম, session retry এবং guardian dispatch-এর প্রত্যাশিত ফল লেখা আছে। সংশ্লিষ্ট owner-এর সম্মতি ও বাস্তব test pending। [Design plan](UI_UX_DESIGN_PLAN.md) · [CTO handoff](../../integration/CTO-HANDOFF.md) · [Dependencies](../../integration/DEPENDENCIES.md)

## ১. প্রমাণ, সিদ্ধান্ত ও কাজ শুরুর শর্ত

| ধরন | এখন যা জানা আছে |
|---|---|
| দেখা প্রমাণ | এই planning checkout-এর HEAD `main` @ `2f0b1e9`। Doc 02–08, tracker, তিন integration নথি ও System 1–3-এর সর্বশেষ ছয়টি plan আছে। স্থানীয় **Desk-SchoolERP-Technical-Blueprint-v2-team.pdf**-এ inventory পৃ. ৭৮–৮০, ৮২, ৮৪–৮৫, ownership পৃ. ১২–১৬, approval পৃ. ৩১, sync পৃ. ৩৫–৩৯ ও acceptance পৃ. ৫৬–৫৯, ১০৬, ১২৪ মিলেছে। |
| Scope | Tracker-এ A1/A2/A3/A4/A5/A6/A9-এর যথাক্রমে ৫/৪/২/২/৬/৪/৬ row। §৫-এর ২৩টি **MVP candidate** ও ৬টি **MVP partial**-এর Blueprint release **R1**। সবগুলির implementation status **Needs repository verification**। |
| আপনার সিদ্ধান্ত | OD-13-এর merge ক্রম **Platform → Identity 1a → A1/A2 slice 0 → Students**। System 5 Exams and Results ও System 6 Desk Learn-এর পূর্ণ planning-এর আগে slice 0 design contract review। CX-01 অনুযায়ী Platform PLT-006/007/013-এর ওপর নির্ভরশীল Identity 1b custody/audit G1 staging-এর আগে লাগবে। |
| অনুমান ও এই খসড়ার সিদ্ধান্ত | D-01 অনুযায়ী একটি pilot campus ও native Desk Learn। প্রথম demo upper-primary ধরে সাজানো। Whole-section required-subject group, school/year অনুযায়ী একই transaction-এ roster যাচাই এবং dispatch attempt-এর সীমা review-এর জন্য ধরা হয়েছে। Backend দেখা গেলে প্রচলিত stack ও integration boundary ব্যবহার হবে। |
| Pending evidence | Backend checkout, integration base branch ও commit, app/screen, design token, endpoint/OpenAPI implementation, data schema/migration, fixture file/path/version, CI ও build/test command এখনো যাচাই হয়নি। Fixture-এর লিখিত contract পাওয়া গেছে। PR, test run, deployment বা client acceptance evidence বাকি। |

**Coding gate।** Plan অনুমোদনের পরে আপনি backend প্রস্তুত নিশ্চিত করে স্পষ্টভাবে কাজ শুরু করতে বললে প্রথমে যাচাইকৃত integration base থেকে একটি dedicated feature branch আপনার কাছে চাইতে হবে। আপনি assigned branch নিশ্চিত করে তাতে coding-এর অনুমতি দিলে base ও branch head মেলানো হবে। তারপর আলাদা worktree বা checkout-এ কাজ শুরু হবে। Doc 02-এর `feat/academics/offline-attendance` প্রথম school-day কাজের পরিকল্পিত নাম। Slice 0-এর assigned branch ও base আপনি নিশ্চিত করবেন। [Release ও branch plan](../../02-mvp-release-and-branch-plan.md)

**Stack ও source path।** Blueprint পৃ. ৯–১১, ১৫ ও ৩৫-এ Go modular backend, PostgreSQL/RLS, transaction-সহ outbox, PostgreSQL job queue, versioned REST/JSON ও OpenAPI 3.1, generated Dart/TypeScript client, Next.js/React/TypeScript web, Flutter apps ও encrypted SQLite operation log আছে। Redis স্বল্পমেয়াদি cache। এগুলি Blueprint-এর baseline। T00-তে বাস্তব project-এর manifest, module, tenancy/auth guard, migration convention, queue, design system, test ও build যাচাই করে path ও implementer ঠিক হবে। এই checkout-এ planning নথিগুলির path যাচাই করা গেছে। নিচে backtick-এ `<backend>`, `<contracts>`, `<web>`, `<mobile>`, `<tests>` ও তাদের child path সব **প্রস্তাবিত**। [Platform implementation](../01-platform/IMPLEMENTATION_PLAN.md) · [Identity implementation](../02-identity/IMPLEMENTATION_PLAN.md) · [Students implementation](../03-student-records/IMPLEMENTATION_PLAN.md)

<a id="slice-0"></a>

## ২. প্রথম deliverable · A1/A2 slice 0

**Review-এর সীমানা।** T01–T02-তে class target ও dated roster-এর ন্যূনতম A1/A2 interface। T03-তে Students-এর merge candidate-এ real A8 দিয়ে ওই interface যাচাই। T04 থেকে বাকি setup, A3–A6 ও A9। SIS-003 tracker-এ A2 feature। Placement ও তার history-র একমাত্র writer A8। Blueprint পৃ. ১২–১৬, ৭৮। [Design §৩](UI_UX_DESIGN_PLAN.md#slice-0) · [Interfaces](../../integration/INTERFACES.md#student-walkthrough) · [Students T5, T9 ও T12](../03-student-records/IMPLEMENTATION_PLAN.md)

### ২.১ Class ও roster contract

| কাজ ও caller | Contract-এর অর্থ ও ফল |
|---|---|
| A8 → A1/A2 class validation | Verified school context, campus, year, grade, section ও প্রযোজ্য subject group। School-এর civil effective date লাগবে। Parent chain ও date validity মেলে। বৈধ হলে stable IDs ও reference version। Blank বা invalid date, ভুল parent বা changed reference-এ safe field error/conflict। Scope-এর বাইরের target-এ not_found। Preview-এর পরে A8 confirm-এ আবার version মেলে। |
| Registrar → A8 placement | Validated refs, stable student ID, effective date, expected version ও key। A8 placement ID/version, origin ও dated history লেখে। একই body ও key-তে একই ফল। First placement-এ same-year অন্য section থাকলে A8 conflict দেয়। ঐ year-এর বাইরে reject, ঐ year-এর past date-এ Students OD-05-এর warning। |
| Students, A9, Results ও অনুমোদিত linked Learn → A2 roster | Context-scoped section ID বা teaching group ID এবং `asOf` civil date। Response-এ school/campus/year, target ID, date, roster version ও stable student IDs। UI channel প্রয়োজনীয় label পায় scope অনুযায়ী। একটি pupil একবার। Label/number দিয়ে join নয়। Denied target, invalid date, source unavailable ও valid empty roster-এর ফল ভিন্ন। |
| A1/A2 group definition → A8 placement | A1/A2-তে stable group ID, parent section, subject reference ও কার্যকর interval। Whole-section required subject group-এ A8-এর ঐ section-এর কার্যকর placements-ই members। Explicit pupil-to-group assignment যেখানে চালু হবে, তার dates ও history A8 লেখে। Group ID তৈরি বা allocation দেওয়া দিয়ে A2-তে দ্বিতীয় pupil list লেখা হয় না। |
| A8 → A2 `placement.changed` | A8 নিজের transaction-এ change ও outbox লেখে। A2-কে affected old/new target এবং effective date range মেলানোর জন্য scope ও A8 source revision জানতে হবে। Replay বা out-of-order event current version পাল্টায় না। Event cache refresh করতে পারে। Authoritative read-এর জন্য delivery-র অপেক্ষা লাগে না। |

**তারিখ ও roster version।** Interval-এর প্রথম দিন কার্যকর, শেষ সীমা পরের interval-এর প্রথম দিন হলে তা বাদ। Date year-এর সীমায় থাকে। A2-এর opaque token school/campus/year, section বা group, civil date এবং ঐ তারিখের membership ও target eligibility-র authoritative revision চেনে। পরিবর্তন commit হলে প্রভাবিত date range-এর revision এগোয়। Future move-তে পুরোনো placement-এর A8 record version বদলালেও move-এর আগের roster token স্থির। নাম বা sort order বদল, event replay, denied undo এবং অন্য target/date token পাল্টায় না। Eligibility বা প্রযোজ্য membership বদলালে token বদলায় বা target অকার্যকর হয়। একই roster পরে ফিরে এলেও আগের token পুনরায় বৈধ হয় না। A2 list pages/cursor একটি snapshot ধরে পড়ে। A8 ও A2 owner T01-এ একই নমুনায় version-এর এই ফল মিলিয়ে নেবেন।

| CX-07-এর নমুনা · case আলাদা করে চালান | Dated roster ও token-এর ফল |
|---|---|
| A/উত্তর/Grade 6/নীল, `r17`/`A-017`, stable pupil ID `sA`। D প্রথম placement date। | D−1 ও D একই year-এ। D−1-এ ০ এবং আগের token। D-তে ১ এবং নতুন token। Retry-তে একই A8 placement ID। School B-এর একই label/number-এ আলাদা IDs ও নিজের successful control। |
| M থেকে নীল → সবুজ · SIS-009 future case | M−1-এ নীল ১ ও সবুজ ০, দুই token আগের মতো। M-তে নীল ০ ও সবুজ ১, উভয় আক্রান্ত token বাতিল। A8 পুরোনো interval শেষ করে নতুন placement ID খোলে। একই student ID ও পুরোনো history। T02-তে fixture, G1-এ বাস্তব move। |
| D থেকে কার্যকর placement-এর clean undo · PLT-014 | D−1-এ শূন্যই থাকে। D-তে ১ থেকে ০ এবং নতুন token। A8 একই placement ID-তে নতুন void revision ও `placement.changed` লেখে। C3 শুধু ফেরানো row-র batch-specific link/request ID, origin ও expected version ধরে `ended` বা `cancelled` করে (CX-12)। |
| পরের placement, approved edit বা attendance-এ undo আটকে যায় | D-তে আগের ১ ও একই token। A8 ফল Conflict। Downstream check অনুপলব্ধ হলে Waiting — retry। দুই ক্ষেত্রেই void, event ও link reversal শূন্য। |

<a id="roster-consistency"></a>

**A8 undo ও A9 submit-এর একই সিদ্ধান্তবিন্দু।** Review-এর জন্য পদ্ধতি হল একই school/year-এ transaction শেষ হওয়া পর্যন্ত একটি shared/exclusive lock। A8 placement/group membership confirm, move বা undo, আর eligibility বদলানো A1/A2 reference ও A5/A6 session/allocation command-এ exclusive lock। A9 নতুন register জমায় shared lock নিয়ে ঐ slot-এর uniqueness-ও transaction-এ চালায়। Locks-এর ক্রম Platform → C3 → school/year → সংশ্লিষ্ট domain record। একাধিক year লাগলে ID-র ক্রমে school/year lock। CX-11-এর C3 lock order আগে থেকেই মানতে হবে। C3-এর প্রয়োজনীয় link lock আগে নেবে। A8-এর final TD-07 check pass হলে তবেই একই transaction-এ batch link withdrawal command চলবে। প্রত্যেক module নিজের table লেখে। Lock পেয়ে নতুন করে revision ও TD-07 check পড়তে হবে। Lock পাওয়ার আগে পড়া snapshot দিয়ে সিদ্ধান্ত হবে না। Lock timeout, unavailable source বা failed transaction-এ কোনো register/void/notice fact commit হবে না। Outbox delay দিয়েও gate পাশ হয় না। Blueprint পৃ. ১৫, ৩৫–৩৬। [Platform T5](../01-platform/IMPLEMENTATION_PLAN.md) · [Students T12 ও TD-07](../03-student-records/IMPLEMENTATION_PLAN.md)

| দুই transaction-এর পরীক্ষা | Expected ফল |
|---|---|
| A8 move/undo আগে commit করে | A9 তারপর একই দিনের পুরোনো token পরীক্ষা করে stale পায়। নতুন register, receipt ও attendance fact শূন্য। আগে কোনো receipt commit হয়ে থাকলে §৩.২-এর retry rule চলবে। |
| A9 নতুন register আগে commit করে | A9 accepted snapshot-এ used placement reference ও original mark রাখে। A8 undo final check-এ authoritative A9 downstream reference দেখে Conflict দেয়। Void ও link reversal শূন্য। অনুমোদিত পরের move হলে আগের accepted register তার snapshot-এই থাকে। |
| A8 check-এ A9-এর উত্তর পাওয়া যায় না বা commit-এর আগে fault হয় | A8 Waiting — retry দেয়। Atomic পথ rollback হলে কোনো side effect থাকে না। A9-ও authoritative revision না পেলে retry-যোগ্য ফল দেয়। দুটো ক্রমেই delayed/replayed event ফল বদলায় না। |

T01-এ এই outcomes ও affected ranges পর্যালোচনা হবে। T00-তে প্রকৃত locking/transaction interface মিলবে। অন্য পদ্ধতি নিলে একই দুই commit-order, fault ও lock contention-এর বাস্তব প্রমাণ লাগবে। TD-07-এর A9 port live enable-এর আগে A8-এ register হবে। CX-14-এর link transaction Identity/Students-এর সিদ্ধান্ত। Durable outbound পথ বাছলে, final TD-07 check থেকে C3 link withdrawal ও A8 void শেষ হওয়া পর্যন্ত একই pupil/placement-এর নতুন attendance আটকানোর সমতুল্য ব্যবস্থা লাগবে। তখন worker crash ও retry-তেও link withdrawal-এর পর ভুল Conflict হয় কি না পরীক্ষা হবে। Real A8+A9 proof T10-এ, slice 0 proof T02-তে test double দিয়ে। C3 student status event পড়ে না (CX-15)। `student.withdrawn` A13/R2।

<a id="fixture-and-students"></a>

### ২.২ Class data, shared fixture ও Students-এর merge candidate

T01-এ Academics ও Students test data mapping review করবেন। T02-তে A1/A2-এর নিজস্ব validation ও persistence ব্যবহার করে synthetic A ও B-তে Campus → Year → Grade → Section এবং একটি whole-section required-subject group seed হবে। T01-এর shared sample catalog scoped subject reference দেয়। T02-তে available A3 owner interface, অথবা সেই port-এ synthetic catalog দিয়ে reference মেলাবেন। A1/A2-তে setup service থাকলে সেটি, না থাকলে একই domain writer ডাকা test harness দিয়ে class/group records seed হবে। উৎপাদনের UI T04-এ। Students A1/A2 record/ID তৈরি করবে না। T03-তে seed করা real class IDs ব্যবহার হবে। T02 group case-এ section D থেকে valid, group G থেকে valid, যেখানে G > D। D-তে section-এ `sA` আছে, group date invalid। G-তে assigned teacher group-এ `sA` পান। Same-school অন্য group, অন্য campus ও B-র same-label group ID deny। Explicit elective, stream ও language-set management CLS-003 R1 backlog।

| প্রমাণের ধাপ | কী চালাবেন ও কী দাবি করা যাবে |
|---|---|
| T01 · shared contract | Platform T1-এর **একটিই versioned CX-07 fixture**। ১২টি Platform decision row, প্রতি school-এর নিজস্ব successful control, একই নীল/`A-017`, Students `r17`, Identity Fixture A/B ও unlinked sibling। Platform, Identity ও Students যে revision review করেন, তার path/version/hash T01-এ লিখতে হবে। Academics একই fixture ব্যবহার করবে। File এখন pending। |
| T02 · A1/A2 deliverable | Real Platform ও Identity 1a-র সঙ্গে real A1/A2। A8-এর dated source port-এ shared-fixture test double দিয়ে allow/deny, section/group, move/void ও token results। T02 pass হলে A1/A2 slice 0 merge-এর evidence তৈরি হয়। |
| T03 · Students দিকে A1/A2 integration | Merged A1/A2 ও real A8 candidate-এ T9 import এবং T5 আলাদা first placement দুই পথ। Import row-তেই date-সহ placement হতে পারে। C3 link ব্যর্থ হলে ঐ placement ও roster থাকে, access Failed থাকে, link পরে একই key-তে retry। পৃথক T5 path-এ unplaced pupil-এর target যাচাই, repeat ও D−1/D মিল। G-তে seeded group-এর roster-ও real A8 section placement থেকে মেলে। দুই school-এ নিজ নিজ allowed control, foreign ID replay, eligible batch undo এবং A8-এর later-use conflict মেলান। A9 attendance-জনিত real conflict T10-এ। |
| Students-এর পুরো merge evidence | T03 শুধু A1/A2 অংশ পূরণ করে। Real C1/C3-এর two-school run, X08-1000 rehearsal ও T18 checks-ও Students §১৫-এ লাগবে। Fixture version, service build/commit, ফল ও reviewer রেকর্ডে থাকবে। X08-1000 পৃথক volume fixture। |

**Consumer।** Students import/placement-এর আগে class validate করে, পরে profile বা import result-এ dated read নেয়। A9 published session ও effective teacher/cover দিয়ে একই date/token ব্যবহার করে। Results ঐ দিনের group, pupil IDs ও A6 subject allocation দেখে marks scope মেলায়। Desk Learn-এ দুই product চালু ও explicit class link থাকলে একই roster/allocation দিয়ে linked enrolment মেলে। Independent course-এর নিজের enrolment। School assignment থেকে Learn instructor grant আসে না। [System 3 §১৫](../03-student-records/IMPLEMENTATION_PLAN.md) · [Data ও permission contract](../../integration/INTERFACES.md)

## ৩. API, data ও access

নিচের contract operations-এর নাম review-এর জন্য। `POST /v1/attendance/registers`, `GET /v1/sync/changes?cursor=…` ও guardian child day view Blueprint পৃ. ৩৫-এ API-র উদাহরণ। বাস্তব route/schema pending। পৃ. ১২–১৬ ও ৩৫–৩৯ এবং [integration interfaces](../../integration/INTERFACES.md) অর্থ ও ownership ঠিক করে।

| Owner ও operation | কোন তথ্য থাকবে, কে ব্যবহার করবে |
|---|---|
| A1/A2 structure ও roster | School/campus Platform থেকে। A1 year, term, grade/band, A2 section/room/reference ও dated read। Stable ID, school, parent ref, valid interval, revision ও actor রাখবে। ব্যবহৃত structure পরিবর্তনে ইতিহাস অক্ষত। A8 rows তার interface দিয়েই পড়া হবে। |
| A3/A4 subject, unit ও lesson | Subject-এ code/name/type/department/language। Grade↔subject assignment ও curriculum unit school/year scoped। Lesson-এ class/group, author teacher, topic/unit, basic content, restricted notes, material reference ও revision। File-এ C9 access check। |
| A6 teaching allocation | Staff profile ও identity membership আলাদা reference। Teacher + subject + section/group, class/co-teacher role, start/end dates। নতুন বরাদ্দে `allocation.changed`। Exams/linked Learn effective mapping পড়ে। বরাদ্দ নিজে C3 grant বা Desk Learn instructor role তৈরি করে না। |
| A5 timetable publish/cover | Calendar, bell period, room, group ও allocation দিয়ে timetable revision তৈরি হয়। A5 প্রতি period occurrence-এর স্থায়ী session ID দেয়। Publish ও cover-এ একই session-এর প্রযোজ্য revision বদলায়। Clash checks, change event ও C4 audience fact থাকে। §৩.২-এ slot ও retry-র নিয়ম। |
| A9 register submit/read | `school, campus, civilDate, kind, section/group, slotId, rosterVersion, timetableRevision, assignmentRevision, policyVersion, captureInstant, marks[studentId,status]`। Daily ও period-এর slot আলাদা। Server ID/receipt/revision, capture ও acceptance time, scoped snapshot record। Invalid/missing/duplicate pupil বা stale নতুন attempt-এ পুরো submit ফেরত। |
| A9 correction request/decision | Register/mark ID, expected revision, আগে/পরে status, reason, maker, decision, checker ও সময়। Teacher অনুরোধ করেন। Cut-off-এর পরে আলাদা School Admin অনুমোদন করেন। Policy/approval decision ও C3 authorization মেলানোর পরে A9 fact লেখে। Refused request বা নতুন revision-এর ওপর পুরোনো approval ফল বদলায় না। |
| A9 → C4/C5/C6 | `attendance.recorded` ও `attendance.corrected`-এ প্রয়োজনীয় school, stable pupil/session ref, mark revision ও stable event ID। A9 mark ও correction রাখে। C4 logical notice ও provider receipt রাখে। C5 authorized day view, C6 scoped report পড়ে। A8-এর household contact C4-এর recipient source নয়। |

<a id="scope-checks"></a>

### ৩.১ Server-side scope checks

C1 verified school/product/lifecycle context দেয়। C3 session, membership, role/action, campus/grade/group ও প্রযোজ্য child right মেলায়। Domain owner record থেকে target বের করে state/assignment/approval মেলায়। Actual catalogue key T00/T01-এ Identity-র সঙ্গে নেবেন। নিচে যে action ও target-এর ফল চাই তা লেখা আছে। একই client parameter দিয়ে school বা child scope বদলায় না। [Identity authorization §৬](../02-identity/IMPLEMENTATION_PLAN.md) · [Role matrix](../../04-roles-and-permissions.md)

| Actor | অনুমোদিত action ও target | একই actor-এর denied action ও target |
|---|---|---|
| School Admin | নিজের campus-এর academic setup, publish/cover, scoped report ও অন্য ব্যক্তির closed-register correction decision | অন্য campus/school-এর section বা report `not_found`। নিজের correction বা in-scope grant ছাড়া action `forbidden`। |
| Registrar | Assigned campus-এর target validate, dated roster read, A8 first placement/undo ও অনুমোদিত field-limited export | একই school-এর অন্য campus `not_found`। নিজের class-এ timetable publish বা attendance mark `forbidden`। |
| Assigned teacher/co-teacher | Effective assignment-এ class/group roster, ঐ session-এ register submit/read ও granted correction request | Unassigned section/group `not_found`। Scoped class-এ approval বা অন্য role-এর staff action `forbidden`। |
| Substitute | A5-এর active cover session এবং C3 bounded grant দুটো মিললে ঐ slot-এ roster/read/submit | ভুল period, withdrawn cover বা expired grant-এ scope deny। A5 ও C3-এর যে কোনো একটির অনুমতি হারালেই একই slot-এ queued new submit এবং receipt read-এর access পুনরায় যাচাই। |
| Pupil | IAM-001 অনুযায়ী verified account-এ নিজের published routine এবং policy-তে থাকা self attendance | অন্য pupil বা class roster `not_found`। D-16/Identity OD-10 অনুযায়ী নিজের phone/email ছাড়া pilot-এর পথ guardian-এর child view। |
| Guardian | Current linked child-এর granted routine/attendance read। Attendance notice-এর জন্য একই child-এর current attendance right | Unlinked sibling, contact-only, wrong child right বা custody deny-তে `not_found`। Data বা dispatch শূন্য। General notice preference attendance right যোগ করে না। |
| Learn-only role | নিজ product-এর অনুমোদিত Learn action | School Management entitlement বন্ধ হলে C1 product result। School module চালু থাকলেও Learn-only membership দিয়ে roster খুললে C3 deny। |

V01/V02-তে A ও B উভয়ের own-school allow → একই request-এর foreign ID replay → own-school allow। Wrong campus/class/child, group, cover expiry/revoke ও revoked session আলাদা case। একই স্কুলে in-scope staff action deny-তে `forbidden`। Out-of-scope ও অস্তিত্বহীন target-এর `not_found` status/body/headers/redirect একই। Count, cursor, roster diff, file metadata ও domain write শূন্য। UI/API, search, export generate ও download/collect, file link ও sync-এ একই ফল। Context switch/child switch-এ আগের data ও late response বাদ। Sensitive guardian detail online fail/resume-এ ঢাকা (Identity OD-18)। Worker scope verified enqueue context ও current grant থেকে নেয়। Dispatch-এ প্রতি attempt-এ C3 লাগে (§৬.২)।

<a id="session-retry"></a>

### ৩.২ স্থায়ী session ও একই command-এর receipt

Daily slot = school + section + civil date + daily kind। Period slot = school + A5-এর stable session occurrence ID। A5-তে occurrence নির্দিষ্ট section/group ও school date-এ থাকে। একই occurrence republish, room/time edit বা cover/replacement-এ ID একই, revision বদলায়। সত্যিই নতুন lesson-এর আলাদা ID, cancelled slot-এর history অক্ষত। একই দিনের একই class/period আবার publish করার সময় পুরোনো occurrence match না হলে slot mapping ঠিক করে publish করতে হবে। এভাবে timetable/assignment/roster version নতুন হলেও দ্বিতীয় register-এর slot তৈরি হয় না।

১. **Receipt দেখার আগে scope।** Active verified context, C1 এবং C3 দিয়ে target/receipt read যাচাই। Key school, original submitting membership ও operation-এ scoped। Fingerprint-এ slot, revisions ও body। অন্য context/actor দিয়ে আগের queued command replay হয় না।

২. **গৃহীত command।** একই scope/key ও body-র committed receipt থাকলে সেই receipt ফেরে। এর মধ্যে roster, timetable, assignment revision বা policy বদলালেও পুরোনো command আবার execute হয় না। বর্তমান access নেই হলে safe deny। Same key দিয়ে ভিন্ন body conflict। Server receipt, stable slot ID ও accepted revision দিয়েই app ফল দেখায়।

৩. **নতুন command।** গৃহীত receipt না থাকলে current submit grant ও A5/A6 effective assignment/cover মেলান। §২.১-এর guard ও slot lock নেওয়ার পরে একই key-এর receipt আবার দেখুন, কারণ অন্য request ইতিমধ্যে commit করে থাকতে পারে। Matching receipt পেলে §৩.২-এর আগের ফলই ফেরে। অন্য key-তে slot আগে accepted হলে অনুমোদিত summary-সহ already submitted, এরপর correction flow। Slot খালি হলে current roster, timetable/assignment, policy revision ও term state মেলান। Stale-এ write/fact শূন্য। Current version/diff শুধু অনুমোদিত caller পান। Register snapshot, mark, receipt ও outbox একই transaction-এ। Unique slot ও scoped receipt key-তে concurrent submit-এ এক write। Versioned API error-এ problem details।

৪. **Test।** একটি period submit করে acknowledgement থামান। তখন same session republish, roster change এবং cover assign করুন। একই authorized actor-এর replay-তে মূল receipt। Cover হারানো actor-এ deny। নতুন authorized substitute ও দ্বিতীয় device একই slot-এ দ্বিতীয় register পায় না। আলাদা run-এ এখনও খালি slot-এর stale version reject, বর্তমান revision দিয়ে নতুন submit একবার সফল। Daily ও period slot আলাদা থাকে।

Server school civil date ও UTC instants রাখে। `captureInstant` audit-এর জন্য। Event envelope-এ event ID/type/version, school ও record ID/version, occurredAt এবং প্রযোজ্য product/effective date। Outbox replay-তে subscriber event ID দিয়ে duplicate আটকায়। Version gap-এ owner-এর বর্তমান revision মেলায়। Additive API change পুরোনো client-এর সঙ্গে চলে। Breaking change new version ও deprecation window পায়। [API ও event delivery](../../integration/INTERFACES.md) · Blueprint পৃ. ১৬, ৩৫–৩৯।

<a id="attendance-policy"></a>

### ৩.৩ Attendance policy, cut-off ও V04

Blueprint পৃ. ৩০–৩১ অনুযায়ী প্রতি grade band-এর status ও correction cut-off-সহ policy School Admin দ্বিতীয় approver নিয়ে ছাড়েন। Date-effective approved version থেকেই phone ও server rule নেয়। Register-এ প্রযোজ্য policy version ও মূল mark থাকে। QA নিচের case-গুলি school-approved sample policy দিয়ে চালাবে। V04-তে policy release, approval ও আগের ফল পুনরায় মেলানোর evidence যাবে।

| Test | প্রত্যাশিত ফল |
|---|---|
| Cut-off-এর আগে valid প্রথম submit | Assigned teacher permitted status দিয়ে draft ঠিক করে স্বাভাবিক পথে submit করেন। Invalid status বা open slot না থাকলে accept হয় না। জমা mark বদলালে A9 versioned correction flow ও audit। |
| Cut-off-এর পরে প্রথম offline draft | শুধু capture time আগের হলেই accept হবে না। School-এর অনুমোদিত late-first-submit নিয়ম থাকলে current access, slot, versions ও term state মিলিয়ে স্বাভাবিক submit। Sample-এ দেরিতে submit নিষিদ্ধ থাকলে write/fact শূন্য এবং teacher-কে office-এ পাঠায়। Office দেখে গৃহীত register আছে কি না। থাকলে correction flow, না থাকলে school-এর missing-register procedure। |
| Cut-off থেকে closed register correction | Teacher reason ও expected revision দেন। আলাদা, authorized School Admin approve/reject করেন। একই ব্যক্তি দ্বিতীয় role-এ reject, wrong campus deny, পুরোনো revision conflict। Pending/rejected-এ confirmed mark একই। Approve-এ original ও নতুন revision/audit থাকে। Term locked হলে normal submit/edit বন্ধ। |
| Approved policy version বদল | পরের কার্যকর দিন নতুন status/cut-off ব্যবহার হয়। আগের accepted register পুরোনো policy version ও snapshot-এ আবার মিলবে। Approver, effective date, reason, audit এবং before/after results সংরক্ষিত। |

## ৪. কাজের ক্রম

| Task | নির্ভরতা ও কাজ | ফল ও review gate |
|---|---|---|
| T00 · backend যাচাই | §১-এর coding gate-এ branch ও base মিলে গেলে actual repo, stack, app, RLS, port, queue, test, migration ও base build যাচাই। | Base commit ও build, verified existing path অথবা verified parent + নতুন file, owner ও command লিখিত হবে। অমিল হলে path ও design-এর প্রভাব আপনার review-তে। |
| T01 · contract | A1/A2, A8, Platform ও C3 owner এখনই seed mapping, class/group scope, revision ও দুই commit order review করবেন। T00-র পরে fixture path/version ও integration test মেলাবেন। | Design-এর ownership ও expected results মিললে System 5/6-এর পূর্ণ planning। Actual interface run T02/T03-এ। |
| T02 · slice 0 | T01, real Platform ও Identity 1a। A1/A2 writer দিয়ে synthetic reference, এরপর class validation, dated section/group read, A8 source port, token ও scope। | Allow/deny ও date/version test slice 0 merge-এর প্রমাণ। A8 test double শুধু interface-এর ফল যাচাই করে। |
| T03 · Students-এর A1/A2 integration | T02 merged। A8 candidate-এর T9 import-এ placement ও independent link retry, T5 আলাদা first placement এবং T12 undo-তে real reference/roster। | দুই school, repeat, date, eligible undo ও conflict-এর evidence Students §১৫-এর A1/A2 শর্ত পূরণ করে। Full merge-তে X08-1000, T18 ও অন্য checks-ও লাগে। CX-14 T-00-তে সংশ্লিষ্ট owner মিলাবেন। |
| T04 · academic setup ও class detail | T02–03। Year, grade, room, history, protected edits, guided progress। Subject/teacher/routine ধাপের readiness সংশ্লিষ্ট owner-এর saved state থেকে আসে। | Office user pilot school setup করেন ও ব্যবহৃত class-এর আগের তারিখ খুলতে পারেন। পূর্ণ progress T08 পরে মেলে। |
| T05 · subjects | T04। Catalogue ও required grade assignment। | Campus/grade অনুযায়ী একই subject picker allocation ও lesson-এ খোলে। |
| T06 · teachers | T04–05 ও C3 membership। Directory/profile, class/co-teacher ও subject/class allocation, কার্যকর dates। | সঠিক teacher ও co-teacher-এর scope। বরাদ্দ বদলালে পুরোনো day history থাকে। |
| T07 · curriculum ও lesson | T05–06, C9 file access। Pilot unit ও assigned teacher-এর basic plan/material। | নিজের class-এ lesson খোলে, অন্য class-এ deny। R1 পূর্ণ fields আলাদা। |
| T08 · calendar, timetable ও cover | T04–06, C3/IAM-010 bounded grant, C4 audience contract। Calendar/bell → clash → effective publish → cover। | একই occurrence-এর stable slot ও নতুন revisions। Substitute ও class notice এবং real grant-এর evidence producer delivery-র পরে। |
| T09 · class overview | T02, T06, T08। Class teacher-এর section card-এ A9 (T10/T14), Results ও C4 (T13) read ports। | Scoped attendance, marks due ও notices। Producer অপ্রাপ্য হলে unavailable। পূর্ণ check-এ তিন producer-এর real data মেলে। |
| T10 · A9 register | T03, T06, T08 ও school policy। Stable daily/period slot, current permissions, scoped receipt, mark, outbox ও A8 TD-07 port। | দুই commit order, old receipt replay, republish/cover ও দ্বিতীয় device পরীক্ষা। A8+A9 atomic check প্রমাণ হলে live enable review। |
| T11 · phone ও offline sync | T10, C5 teaching mode (APP-008) ও encrypted store। Durable queue, reconnect, stale review। | Restart, receipt, cover offline ও reconnect-এর আগে grant expiry/revoke, V07 ৪০ pupil-এর timing। V17 device evidence §৮-এ। |
| T12 · correction | T10–11, C9 audit/approval ও C3 checker। Effective policy, cut-off, reason, pending/approve/reject ও term lock। | Immutable original, আলাদা approver, policy-revision ও audit checks (§৩.৩)। |
| T13 · guardian notice | T10, T12, C3/C4/C5। §৬.২-এ attempt start, current child right, sent/unknown, suppression, correction ও fallback-এর যৌথ contract। | ওই contract-এ Identity ও Apps সম্মতি, তারপর দুই commit order ও logical notice counts। C8 provider sandbox ও live SMS evidence পৃথক। |
| T14 · registers ও reports | T10, T12 ও C6 scope। Date/class/status filters, drill-down ও অনুমোদিত export। | Confirmed ও corrected totals register-level source-এ মেলে। |
| T15 · rollout ও handover | T04–T14 এবং §৭-এর release dependency ও remainder evidence। | G0, G1 staging ও live V20, পূর্ণ R1 evidence এবং G2 proof নিজ নিজ gate-এ। V19 containment/rollback drill ও reviewer record §৮। |

<a id="feature-trace"></a>

## ৫. Feature থেকে design, task, path ও acceptance

প্রতি row-তে design requirement [UI plan](UI_UX_DESIGN_PLAN.md)-এর একই Feature ID। § চিহ্ন তার section। **সব path প্রস্তাবিত**। যেখানে একাধিক task আছে, প্রথমটিতে producer ও পরেরটিতে integration check হবে। Test ID হল প্রত্যাশিত evidence। কোনো run সম্পন্ন হয়নি। সব row R1, ★ চিহ্নিত ছয়টি MVP partial, বাকি ২৩টি MVP candidate।

| Feature · PDF পৃ. | Design ও dependency | Task · path | প্রত্যাশিত ফল ও acceptance check |
|---|---|---|---|
| SET-001 · ৭৮ | §৪ guided progress · A1–A6 readiness | T04/T08 · `<web>/academics/setup` | সংরক্ষিত setup-ই ready দেখাবে। Subject বাদে incomplete, subject ও শেষ প্রয়োজনীয় ধাপ পূরণ করে আবার খুললে complete (V11)। |
| SET-002 · ৭৮ | §৩–৪ year/term · C1 campus | T02/T04 · `<backend>/academics/years` | Valid year/term reference। Year-এর বাইরে term reject, আগের term ও dates ফিরে আসে (V11)। |
| SET-003 · ৭৮ | §৩–৪ grade/band · school/year | T02/T04 · `<backend>/academics/grades` | School-এর দেওয়া band/grade নাম save। Rename-এ pupil-এর পুরোনো grade ID একই (V11)। |
| SET-011 · ৭৮ | §৪ protected history · A8 references, C3 | T02/T04 · `<backend>/academics/structure` | ব্যবহৃত section মুছে পুরোনো roster বদলানো যায় না। Unauthorized edit denied, dated versions পুনরায় পড়া যায় (V02/V11)। |
| PLT-003 ★ · ৭৮ | §৩/৭ এক-campus tree · C1/C3 | T02/T04 · `<backend>/academics/structure` | বৈধ department/grade/section এক school-এ খোলে। একই label-এ B-র parent পাঠালে create/read বন্ধ (V01)। |
| SIS-003 · ৭৮ | §৩ first placement ও history · A8 writer | T02/T03 · `<contracts>/academics/class-reference` | T9 import row ও T5 আলাদা command-এর valid target, D−1/D ও repeat মেলে। Link retry-তে placement version একই। Wrong year-এ ০ write (V11)। |
| CLS-001 · ৭৮ | §৩ section/group dated roster · A8 port | T02/T03/T10 · `<backend>/academics/rosters` | Effective section/group, wrong scope deny, move boundary, void/version ও replay মেলে। A9 আগে accepted হলে undo conflict (V11/V01/V02)। |
| CLS-002 · ৭৮ | §৪ class overview · A6/A9, Results/C4 | T09/T10/T13/T14 · `<web>/academics/classes` | Assigned teacher-এর attendance total A9 register/report থেকে, marks due real Results থেকে ও notices C4 থেকে মেলে। অন্য class deny, source unavailable-এ ০ দেখায় না (V07/V11)। |
| SET-004 ★ · ৭৮ | §৩/৭ section/room · validated grade, campus | T02/T04 · `<backend>/academics/sections` | Room/section একই campus/year-এ saved। ভুল parent reject, A8 ও timetable একই section ID পায় (V11)। |
| SUB-001 · ৭৯ | §৪ catalogue · department ও C3 | T05 · `<backend>/academics/subjects` | Sample-এ code, নাম, type, department, language পুনরায় পড়ে মেলে। অন্য school-এর ID দিয়ে detail পাওয়া যায় না (V11/V01)। |
| SET-005 ★ · ৭৯ | §৪/৭ required subjects · SUB-001, grade | T05 · `<backend>/academics/grade-subjects` | দুই pilot grade-এ নিজ নিজ required subject। ভুল grade allocation দিয়ে timetable তৈরি বন্ধ (V11)। |
| CUR-001 ★ · ৭৯ | §৪/৭ pilot map · grade/subject | T07 · `<backend>/academics/curriculum` | দুটি subject-এর unit পৃথক। একটি grade-এর unit অন্য grade-এর map-এ আসে না (V11)। |
| CUR-003 ★ · ৭৯ | §৪/৭ basic lesson · curriculum, TCH-002, C9 | T07 · `<backend>/academics/lessons` | Assigned teacher-এর lesson/material আবার খোলে। ভুল class-এ edit, private notes ও file read denied (V11/V02)। |
| SET-006 · ৭৯ | §৪ school calendar · campus ও year | T08 · `<backend>/academics/calendar` | Holiday, event, closure ও exam interval একই school date-এ দুই calendar-এ মেলে। অন্য campus filter scope মানে (V11)। |
| SET-007 · ৭৯ | §৪ bell/period · SET-006 | T08 · `<backend>/academics/periods` | বৈধ period থেকে slot পাওয়া যায়। শেষ সময় শুরুর আগে বা overlap হলে publish বন্ধ (V11)। |
| ACA-001 · ৮০ | §৪ clash builder · bell, subject, room, allocation | T08 · `<backend>/academics/timetable` | Teacher/room/class clash প্রত্যেকটি publish আটকে দেয়। Conflict সরালে একটি version publish। Parallel publish-এও একই নিয়ম (V11)। |
| ACA-002 · ৮০ | §২/৪ routine · published session, C3/C5 | T08 · `<mobile>/academics/timetable` | Teacher, pupil ও guardian-এর নিজের published day মেলে। Unlinked child বা draft denied (V07/V11/V02)। |
| ACA-004 · ৮০ | §৪ effective publish ও stable session · draft, C4 | T08/T10/T13 · `<backend>/academics/timetable` | Start date-এর দুপাশে ঠিক version। Same-occurrence republish-এ slot ID একই ও দ্বিতীয় register নেই। Retry-তে duplicate change notice শূন্য (V11/V08)। |
| ACA-005 · ৮০ | §২/৪ cover · A5 session, C3 time bound, C4 | T08/T10/T11/T13 · `<backend>/academics/cover` | Substitute ও class notice পায়। Active cover-এ offline mark ও একবার reconnect submit। অন্য period, reconnect-এর আগে expiry/revoke বা grant/session mismatch-এ ০ write (V07/V11/V02/V08)। |
| HRM-001 · ৮০ | §৪ staff directory · school/campus, C3 | T06 · `<backend>/academics/staff` | অনুমোদিত campus-এর তালিকা ও profile মেলে। অন্য campus/staff fields family search-এ নেই (V11/V02)। |
| TCH-002 · ৮০ | §৪ allocation · staff, subject, class | T06/T08 · `<backend>/academics/allocations` | Date M-তে নতুন teacher/group পায়, M−1 history থাকে। M থেকে পুরোনো grant দিয়ে edit denied (V11/V02)। |
| TCH-003 · ৮০ | §২/৪ class teacher/co-teacher · section, C3 | T06/T10 · `<backend>/academics/allocations` | একই section-এ দুই দায়িত্ব আলাদা দেখা যায়। Granted co-teacher submit করতে পারেন, দুই device এক session-এ দ্বিতীয় register বানাতে পারে না (V11/V07)। |
| TCH-001 ★ · ৮০ | §৪/৭ pilot profile · HRM-001, C3 | T06 · `<web>/academics/teachers` | নির্বাচিত teacher-এর pilot profile ও assigned classes পুনরায় দেখা যায়। Cross-school profile বা private document প্রকাশ শূন্য (V11/V02)। |
| ATT-001 · ৮২ | §৫ daily/period · roster, stable slot, policy | T10 · `<backend>/attendance/registers` | Daily ও period আলাদা, republish/cover-এ একই period-এ এক register। Stale/wrong-class/duplicate pupil-এ ০ write (V07, live V20)। |
| ATT-002 · ৮২ | §৫ দ্রুত mark · A2 roster, C5 phone | T11 · `<mobile>/attendance/roll-call` | Teacher bulk present, exception ও undo ব্যবহার করে **৪০ pupil-এর attendance ১ মিনিটের কমে** সঠিকভাবে নেবেন। বাস্তব ফোনে সময় ও pupil-ভিত্তিক ফল রাখা হবে (V07, PDF পৃ. ৫৬)। V17-এ device evidence আলাদা। |
| ATT-003 · ৮২ | §৫.১ durable queue · T10, encrypted store | T11 · `<mobile>/attendance/sync` | Restart, stale-এ ০ mark/event, updated roster review, হারানো acknowledgement-এর পরে একই authorized key-তে মূল receipt। বদলানো access-এ deny (V07/X-01/V17)। |
| ATT-004 · ৮২ | §৫ history ও approval · C3/C9, A9, C4 | T12/T13 · `<backend>/attendance/corrections` | Reason, আলাদা checker, original+revised version ও approved policy history। Prior sent notice-এর জন্য current permitted guardian-কে correction। Receipt pending হলে sent rule §৬.২ (V04/V07/V08)। |
| ATT-005 · ৮২ | §৫ attendance notice · A9/C3/C4/C5 | T13 · `<contracts>/events/attendance` | Absent/late event×guardian-এ একটি logical notice। Final live right check, C3 outage wait, obsolete fact suppression ও provider unknown reconciliation §৬.২-এ মেলে (V08/V02)। |
| ATT-008 · ৮২ | §৫ report · A9, C3/C6 | T14 · `<backend>/attendance/reports` | Class/date ধরে total accepted marks-এর সঙ্গে মেলে, approved correction একবার গণনা। Pending/empty আলাদা। unauthorized report/export-এ row শূন্য (V07/V01)। |

<a id="offline-and-notice"></a>

## ৬. Offline, correction ও notice একসঙ্গে পরীক্ষা

### ৬.১ Stale draft থেকে অনুমোদিত correction

Phone-এ actor, school, stable slot, roster/timetable/assignment ও policy revisions, capture time, draft marks এবং command key encrypted log-এ থাকে। Restart-এ queue order ও data থাকে। Reconnect-এ §৩.২-এর ক্রম। C3/assignment revoke হলে পুরোনো actor-এর নতুন submit আটকে যায়। Current scope নেই এমন account-এ draft, diff বা receipt খুলবে না।

| একই period-এ test-এর ধাপ | Register, event ও notice-এর প্রত্যাশিত ফল |
|---|---|
| R version-এ ক absent, খ present। A8-এ ঐ দিনের খ অন্য roster-এ যায়। পুরোনো R প্রথমবার পাঠানো হয়। | Local draft ১। গৃহীত register, A9 fact ও C4 notice প্রত্যেকটি ০। নতুন roster-এ খ বাদ। বহাল pupil stable ID-তে review-তে যায়, নতুন pupil unmarked। |
| Teacher সংশোধিত body ও current version নতুন key-তে পাঠান। উত্তর হারায়। একই authorized key/body আবার পাঠান। | ১ accepted register, ১ original receipt, ক-এর ১ `attendance.recorded` fact। Retry-তে নতুন effect ০। এই run-এ দুজন eligible guardian প্রথম notice পান, ২ logical notice। |
| Teacher ক-এর absent → late করার কারণ দেন। আলাদা School Admin approve করেন। | Pending-এ original status। Approve-এ একই register-এ ১ revised mark ও ১ `attendance.corrected` fact। Original mark/audit থাকে। Replayed decision-এ বাড়তি fact ০। |
| Correction dispatch শুরু হওয়ার আগে দ্বিতীয় guardian-এর right প্রত্যাহার কার্যকর। | দ্বিতীয় guardian-এর correction suppressed, provider attempt ০। প্রথম guardian-এর ১ সংশোধনী। মোট ১ register, ২ A9 facts, ৩ পাঠানো logical notice। Fallback একই notice-এর attempt। যদি right প্রথম notice-এর আগেই সরে, দ্বিতীয় guardian-এর দুই dispatch-ই শূন্য এবং পাঠানো logical notice মোট ২। |

এখানে ক-ই একমাত্র notice-যোগ্য pupil। Fallback, rejected approval, wrong child ও no-access review-এর জন্য পৃথক run। §৩.২-এ republish/cover-এর পর receipt-এর test, §৩.৩-এ cut-off ও policy version-এর test। Phone-এ class/child বদলালে পুরোনো totals তৎক্ষণাৎ সরে, late response বাদ যায়, authorized stale review-তে focus heading-এ যায়। প্রথম confirmed receipt-এ screen reader একবার ঘোষণা করে। [UI §৫–৬](UI_UX_DESIGN_PLAN.md)

<a id="dispatch-contract"></a>

### ৬.২ C3/C4 dispatch contract · review দরকার

A9 একই transaction-এ pupil-নির্দিষ্ট attendance/correction fact-এর stable event ID রাখে। এক register-এ siblings থাকলেও fact IDs আলাদা। C4 এক event + guardian-এ এক logical notice রাখে। তার নিচে in-app প্রকাশ, push ও SMS attempt/receipt। Current attendance right থেকে recipient মেলে। Preference, language, quiet hours ও fallback policy channel চালায়। Safe preview-তে reason বা sensitive detail থাকে না। C5-এর child detail online right check-এ খোলে।

**Attempt শুরুর মুহূর্ত।** C4 worker একটি নির্দিষ্ট attempt claim করে। Provider call-এর ঠিক আগের শেষ ধাপে authoritative C3 check ঐ attempt-এ allow ও link version বাঁধে। OD-19/SC-09 অনুযায়ী এই সফল final check-এই dispatch শুরু। Enqueue, প্রথম audience lookup ও delayed rights event দিয়ে attempt শুরু করা যায় না। Worker ঐ call-এর আগে কাজটি আর queue-তে ফেরায় না। Retry, fallback বা worker restart-এ নতুন check লাগে। C3 সাড়া না দিলে attempt waiting, provider call ০। Current deny-তে suppressed। C3 effective commit, final check ও provider outcome-এর order log হবে, notice content ছাড়া। [Identity T-09 ও SC-09](../02-identity/IMPLEMENTATION_PLAN.md)

| পরিস্থিতি | C4-এর প্রত্যাশিত কাজ |
|---|---|
| C3 effective commit → claimed attempt-এর final check | Restricted guardian-এ deny, নতুন provider call শূন্য। Open online child detail-এ access সরে। Event দেরি করিয়েও একই ফল। |
| ঐ attempt-এর final allow → effective commit → ওই provider call | SC-09-এর আগে শুরু হওয়া attempt হিসেবে গণনা ও log। Provider গ্রহণ করলে “sent before effective” হিসেবে সেই ফল থাকবে। নতুন retry বা fallback-এ আবার check, restriction থাকলে call শূন্য। অন্য eligible guardian-এর notice চলবে। |
| Prior notice in-app-এ প্রকাশিত কিংবা provider accepted, delivery receipt pending/confirmed | Original `sent` ধরে correction event-এ এক logical notice eligible। `delivered` পৌঁছানোর প্রমাণ। Correction attempt-এ নতুন করে right মেলে। |
| Prior provider attempt `unknown`, in-app-এ প্রকাশ হয়নি | Stable attempt reference-এ provider status মেলে। Accepted প্রমাণ হলে বর্তমান right দেখে correction। নিশ্চিত unsent হলে পরের row-এর নিয়ম। Outcome unknown থাকলে নতুন send/fallback ও prior-sent-নির্ভর correction অপেক্ষায় থাকে। Ledger-এ unknown দেখা যায়। |
| Original কেবল queued, অথবা কোনো channel-এ পাঠানো যায়নি এবং A9-এর নতুন approved mark এসেছে | C4 latest A9 revision দেখে obsolete original বন্ধ করে। নতুন status absent/late হিসেবে notice-যোগ্য হলে latest fact থেকে একটি স্বাভাবিক update। অন্য status-এ notice শূন্য। ইতিমধ্যে sent থাকলে correction rule চলে। |

Correction এলে C4 সর্বশেষ A9 revision মেলায়। Original attempt শুরু হয়ে ফল আসেনি থাকলে সেটিও unknown ধরে। Original এখনো queue-তে থাকলে latest revision-এর নিয়ম চলবে। একই channel-এর stable attempt key দিয়ে concurrent retry-তেও সর্বোচ্চ একটি provider acceptance-এর প্রমাণ রাখতে হবে। Logical notice, প্রতি channel-এ acceptance ও actual delivery আলাদা ফল। Push outcome অজানা থাকলে SMS fallback-এর আগে reconcile হবে। Scoped in-app publish-এও C3 current right দেখবে। C3/C4 owner final check ও handoff-এর বাস্তব সীমা, C8 provider reconcile এবং late/out-of-order event-এ ফল T13-এ মেলাবেন। Blueprint পৃ. ৩১, ৩৮–৩৯, ৫৬। [C3 §৬](../02-identity/IMPLEMENTATION_PLAN.md) · [X-06 ও V08](../../07-qa-pilot-and-acceptance.md)

<a id="release-evidence"></a>

## ৭. Pilot, পূর্ণ R1 ও পরের release-এর evidence

### ৭.১ ছয়টি partial-এর R1 remainder

সব ছয়টি row-এর Blueprint release **R1**। প্রতিটির pilot এবং R1 remainder status **Pending**। নিচে pilot task-এর পরে যে R1 কাজ লাগবে তার owner ও pass condition আছে। T15-এ QA প্রতিটির tracker case `G1/<Feature ID>`-তে build, reviewer ও result link দেবেন। Pilot ফল `MVP slice evidence`, পূর্ণ অংশের ফল `R1 remainder evidence`-এ যাবে। দুটো মিললে পুরো feature-এর sign-off। [Feature tracker](../../tracker/features.csv) · [Scope rules](../../01-requirements-and-feature-tracker.md)

| Partial · PDF পৃ. | Pilot run | পূর্ণ R1 কাজ, owner ও প্রমাণ |
|---|---|---|
| PLT-003 · ৭৮ | T02/T04 · এক campus-এর department, grade ও section valid। দুই school isolation। | T04-এর পরে Academics + Platform-এর অনুমোদিত group/school/multi-campus tree। V01-এ valid hierarchy, ভুল parent deny ও parent বদলের আগের date-এর history। |
| SET-004 · ৭৮ | T02/T04 · class/section/room দিয়ে real reference ও routine। | T04/T08-এর পরে Academics বাকি homeroom, house ও room/capacity mapping যাচাই করবে। V11-এ class/room reference ঠিক চলে। Over-capacity warning CLS-004-এর আলাদা R1 case। |
| SET-005 · ৭৯ | T05 · pilot grade/class-এর required subjects। | T05-এর পরে Academics A2/A3 elective ও stream cases-এ grade/group-ভিত্তিক assignment মিলাবে। V11-এ subject picker, dated roster ও timetable agree। CLS-003-এর বাকি management R1, pupil choice/approval SUB-004 R2। |
| CUR-001 · ৭৯ | T07 · pilot grade/subject/unit। | T07-এর পরে A4 ও school academic lead পূর্ণ topic, objectives, fields ও instructional hours মেলাবেন। V11-এ school sample-র সঙ্গে saved map মেলে। |
| CUR-003 · ৭৯ | T07 · assigned teacher/class-এর basic lesson ও material ref। | T07-এর পরে A4 ও C9 full topic linkage, resources ও teacher notes মেলাবে। V11/V02-তে authorized teacher ও wrong-class/file deny। CUR-006 review আলাদা R1 feature। |
| TCH-001 · ৮০ | T06 · pilot profile ও effective assignments। | T06-এর পরে A6, C3 ও C9 qualification, eligible subject, grade band, employment dates এবং restricted docs যাচাই করবে। V11/V02-তে scoped read ও history। |

### ৭.২ G1 ও G2-র অন্য owner-এর কাজ

এই ফলগুলি T15-এর release evidence-এ যাবে। MVP feature trace §৫-এ ২৯টিই আছে। নিচের backlog ও gate-এর Blueprint release বদলায় না। যৌথ run-এর জন্য QA producer/consumer build বা commit, policy/fixture version, date boundary-র actual ফল ও sign-off owner রাখবেন। সংশ্লিষ্ট owner-দের build ও QA/client sign-off এখনও pending। [QA release gates](../../07-qa-pilot-and-acceptance.md) · [Dependencies](../../integration/DEPENDENCIES.md)

| Gate | আগে কী deliver হবে ও কোন owner | QA-তে কী ফল লাগবে |
|---|---|---|
| G1 · SIS-009 section change · R1 backlog, পৃ. ৭৮ | Students/A8-এর বাস্তব date-effective section move ও history → A2 roster ও A5/A6 effective assignment → A9 attendance, Results marks এবং linked Learn enrolment যেখানে link চালু → C3/C5 current access। Academics/Students contract ও producer builds মেলাবেন। | E2E-07/V11-তে M−1 ও M-তে roster, marks entry scope, attendance ও access মেলে। পুরোনো accepted register/marks history ঠিক। Students T12-এর later-placement fixture শুধু পুরোনো undo conflict-এর নমুনা। Real move ও consumer run-এর evidence এই gate-এ আলাদা। পৃ. ৫৭–৫৮, ১২৪। |
| G1 · পূর্ণ V11 workload/cover · R1 backlog inputs | A6 TCH-004 ও প্রযোজ্য SUB-002/ACA-003/TCH-005 inputs → A5 schedule guard → C3 bounded cover ও C4 delivery। T08/T13-এর pilot clash/cover-এর পর পূর্ণ run। | Curriculum, allocation ও approved workload limit থেকে overloaded teacher-এর invalid timetable আটকে যায়। Valid cover-এ substitute ও class notice পায়। V11, পৃ. ৫৭, ১০৬। |
| G1 · A14 GRD-008/GRD-021 · R1 backlog, পৃ. ৮৪ | Early Years/A14 Pre-KG observation/rubric/narrative দেন → school approver → Results report issue ও C9 rendered file → Apps/C5 ও C3 authorized guardian view। QA sample চালাবে, pilot school ও আপনি KG coverage নিশ্চিত করবেন। | E2E-02/V06-এ approved narrative development report, numeric entry ও display শূন্য। Upper-primary demo হলেও Pre-KG staging লাগে। পূর্ণ V06-তে Results ও Apps-এর build-এ bilingual full-grade English/Amharic, Ethiopian/Gregorian date, verification code, approved correction-এ নতুন reissue version ও পুরোনো history। পৃ. ৫৬, ৫৮, ১২৪। |
| G1 · custody screen ও notice · V02/X-06 প্রাসঙ্গিক অংশ | Platform PLT-006/007/013 → Identity 1b ও live custody-র IAM-005 → C4/C5 final-dispatch ও screen run। | কার্যকর restriction-এ screen/reopen এবং পরের dispatch বন্ধ। §৬.২-এর দুই order ও provider outcome test। QA এই evidence record করে ও pickup-list অংশ Pending দেখায়। পূর্ণ X-06 gate disposition QA ও client owner মেলাবেন। |
| G2 · SIS-009 full campus transfer · label R1, পৃ. ৭৮ | G1-এর section flow-এর পরে Students/A8-এর বাস্তব transfer ও প্রয়োজনীয় year-cycle work → Academics stable refs, C3 campus scope এবং Results/A9/Learn/Apps consumers। | E2E-07/V22-তে দুই campus-এর date boundary, নতুন section/enrolment/access, পুরোনো pupil ID ও অক্ষত history। G1 section-change run-এর থেকে পৃথক test/build/sign-off। পৃ. ৫৮, ১২৪। |
| G2 · X-06 pickup-list evidence | A9 ATT-010 gate flow, C3 current custody rights, A8 approved pickup contact inputs। ATT-011 arrival/departure যেখানে প্রযোজ্য। | Controlled rollout-এ restriction কার্যকর হওয়ার পর staff pickup list ও gate check-এ আগের authority পাওয়া যায় না। G1 screen/notice evidence মেলালে QA পূর্ণ X-06 review করতে পারবেন (Identity SC-09/E-04, পৃ. ৫৯, ৮২)। |

**অন্যান্য release।** ATT-006/007 **R1 backlog**। A8 SIS-002-এর pickup contacts বাকি **R1**, access grant C3 দেয়। A9 ATT-009/010/011/013 ও A14 EYS-001/002/003, APP-013, EYS-005 **R2**। TCH-010 pool **R2**। ATT-012/014 scan/device এবং FAC-002 gate pass **R4**। ACA-011 automatic timetable **R4**। পৃ. ৮০–৮৫, ৮৮। নতুন proposal কেবল আপনার অনুরোধে `S4-PR-nn`।

## ৮. Acceptance, rollout ও rollback

| Evidence ও উৎস পৃষ্ঠা | Run ও pass condition | Owner ও কখন |
|---|---|---|
| V01/V02 · পৃ. ৩২, ৫৬ | §৩.১-এর প্রতিটি actor-এর allowed/denied action। দুই school-এর before/after controls, দুই দিকে foreign ID replay, wrong target-এর not_found ও staff in-scope forbidden। UI/API, search, file, report/export ও sync-এ কোনো leak নেই। | Platform/C3 ও Academics T02 থেকে। G1 custody-র জন্য Identity 1b, IAM-005, PLT-006/007/013 ও real C4/C5। পূর্ণ pickup-list X-06 §৭.২। |
| V11 · পৃ. ৫৭, ১০৬ | §২-এর dates, tokens ও void, §৩.২-এর stable session, publish, clash ও cover। Full V11-তে workload reject ও cover recipient proof। | A1/A2 + Students T02–03, A5/A6 ও C3/C4 T08/T13। A8 owner X-08 ১,০০০ pupil run। G1-এর real move ও workload §৭.২। |
| V07/X-01 · পৃ. ৩৮–৩৯, ৫৬, ৫৮ | §৬.১ offline → restart → stale reject → reviewed retry → acknowledgement হারিয়েও এক register। §৩.২ republish ও cover expiry/revoke। Teacher **৪০ pupil-এর attendance ১ মিনিটের কমে** সঠিকভাবে mark করেন (V07, পৃ. ৫৬)। Timing হল ready register থেকে সর্বশেষ সঠিক mark review পর্যন্ত। | A9/C5 ও QA T10–11। Expected/actual marks, সময়, receipt, fact ও notice counts। Submit latency V12-তে। |
| V04/V07 · পৃ. ১৬, ৩০–৩১, ৫৬ | §৩.৩-এর school sample-এ before/after cut-off, late first, separate maker/checker, wrong campus, stale revision, reject, term lock ও historical policy replay। | T12, School Admin, C3 ও C9 policy/approval/audit। Real approved sample এবং engine pending। |
| V08/E2E-05 · পৃ. ৩৮–৩৯, ৫৬, ১২৪ | §৬.১-এর exact counts ও §৬.২-এর দুই commit order, C3 unavailable, sent বনাম delivered/unknown, stale queued facts, duplicate/reordered event, safe preview, SMS-only ও fallback। | A9 fact T10/T12। C3/C4/C5 + C8 T13। Provider acceptance/receipt, sandbox ও live evidence পৃথক। |
| V06/E2E-02 ও E2E-07 · পৃ. ৫৬–৫৮, ১২৪ | §৭.২-এর Pre-KG narrative ও পূর্ণ report evidence। G1 section change এবং G2 campus transfer দুই build/test record। | Early Years/Results/Apps, Students/Academics/Identity/Learn ও QA। |
| V16/V17 · পৃ. ৫৭ | English/Amharic native review, দুই calendar-এ একই school day, keyboard/screen reader, focus, text scaling ও contrast। Low-cost Android ও older iPhone-এ school network এবং mobile data দিয়ে offline, restart, reconnect, time ও data budget যাচাই। | Apps/QA, DVL-003 design, pilot school ও language lead। Reference devices, budgets ও field run pending। |
| V19/V20 · পৃ. ৫৫, ৫৭ | Approved promotion-এ staging-এর একই signed build। Controlled fault-এ alert, containment drill ও rollback-এ data loss শূন্য, safe replay। V20-তে পুরো live term-এ attendance/corrections মেলে। | Release/on-call/QA V19, pilot school V20। G1 staging এবং live evidence আলাদা। |

**সময়ের প্রমাণ।** School-day staging run-এ capture, server acceptance, notice queue ও in-app delivery-র সময় রাখুন। Blueprint পৃ. ৪৪-এর V12 baseline-এ register submit-এর p95 ১ সেকেন্ডের কম, event থেকে in-app notice ১ মিনিটের মধ্যে। Morning absence/late notice পৃ. ৮২ অনুযায়ী একই সকালের run-এ C4 মেলাবে। Offline থাকার সময়, provider অপেক্ষা ও teacher-এর V07 marking time আলাদা করে দেখালে কোথায় দেরি হয়েছে ধরা যাবে।

**Rollout।** Synthetic দুই school-এ T02/T03। G0/F2-তে real-phone offline spike, F3-তে real class import থেকে register/sync। Staging-এ C3, A8, A5/A6, A9 ও C4/C5 দিয়ে এক pupil-এর পুরো journey। Live A9-এর আগে §২.১-এর TD-07/atomic proof এবং §৭.২-এর custody prerequisites মেলে। Approved campus/class-এ enable, ব্যবহারকারী training ও backup/reconciliation। প্রথম দিনের register school-এর approved record-এর সঙ্গে মিলবে। পুরো V20 school calendar-এ। Provider, KG, device ও অন্যান্য gate evidence নিজ নিজ run-এ আসে। [School inputs](../../06-school-policies-and-integrations.md) · [Launch ও handover](../../08-migration-launch-and-handover.md)

<a id="rollback-drill"></a>

**Incident containment ও rollback।** Wrong-school read, register loss বা wrong-recipient alert পেলে release-এর on-call incident owner scope নেন। Cross-school exposure-তে Platform/Identity ও affected API/App owner মিলে affected read routes, search, sync এবং report/export download থামান। Generating exports, signed file links ও shared/edge cache-ও বন্ধ বা invalidate হবে। Scope নিশ্চিত না হলে affected endpoint সব tenant-এ সাময়িক বন্ধ রাখুন। Connected client-এর cached view recheck-এ ঢাকতে হবে। Offline device-এর key/cache revoke পরের connection-এ কার্যকর হবে এবং আগে exposure হয়ে থাকলে incident owner device follow-up রাখবেন। ভুল write/dispatch এবং affected jobs থামান। Build, queues, original receipts ও audit রেখে evidence সংগ্রহ করুন।

Compatible signed build-এ rollback-এর আগে expand → migrate → contract compatibility যাচাই। Pending device commands ও outbox অক্ষত থাকবে। A8 থেকে roster read পুনর্গঠন করুন। Accepted register/mark revisions, keys, receipts, audit এবং C4 ledger মেলান। যেগুলি commit হয়নি সেগুলি current auth ও slot check দিয়ে replay করুন। Provider outcome unknown থাকলে receipt reconcile হবে। আগে পৌঁছানো notice সংশোধনের জন্য approved correction flow। Restore লাগলে isolated backup থেকে যাচাই করে committed changes মেলান। [Doc 08 · Cutover ও rollback](../../08-migration-launch-and-handover.md)

**V19 drill।** Staging signed build ও approved promotion-এর একই ID/signature রেকর্ড করুন। Controlled isolation ও queue fault-এ alert যথাসময়ে on-call-এ পৌঁছেছে কি, কে কখন incident নিলেন ও কোন read/file/export/cache path বন্ধ হল লিখুন। দুই school-এর authorized controls ও cross-school deny দিয়ে reopen যাচাই। Pending queue, accepted marks ও C4 attempts before/after মিলিয়ে zero lost writes ও zero duplicate effects-এর ফল দিন। QA ও release owner build, drill log, actual reconciliation এবং safe replay sign-off যুক্ত করবেন। DVL-013/014/015-এ evidence থাকবে। Blueprint পৃ. ৫৭, ৬০।

**Evidence record।** প্রতি run-এ Feature ID, design section, test ID, fixture revision বা real service versions, commit/build, school/role/device, input ও expected, actual counts/versions/timing, evidence link, reviewer ও status লাগবে। Fixture, merged service, staging device, live pilot, R1 remainder ও G2-এর ফল পৃথক থাকবে। DVL-003-এ screen/token review, DVL-006-এ API/event contract, DVL-012-এ A8 import reconciliation, DVL-013/014-এ release ও test evidence, DVL-015-এ runbook handover। আজকের deliverable দুটি plan ও review prompt।

## ৯. আপনার কাছে ও অন্য owner-এর কাছে যা বাকি

| প্রশ্ন বা তথ্য | কে দেবেন | কোন কাজ অপেক্ষা করবে |
|---|---|---|
| Pilot campus-এ কোন grade/KG, কী attendance status, late first submit/cut-off ও dated calendar? | আপনি ও school academic lead | Approved policy sample, V04/V06/V07 ও live V20। Slice 0 contract review এখনই করা যায়। |
| Backend, integration base, verified fixture revision, schema/port ও app shell/token কোথায়? | আপনি, Platform, Identity, Students ও Apps | T00-তে verified path/build, T02–03-তে real run ও DVL-003 review। |
| Version ranges, scoped section/group seed এবং A8 undo/A9 submit guard মেলে কি? | Academics, Students ও Platform/C3 | §২.১–২.২ দিয়ে slice 0 review এখনই। T00-তে বাস্তব transaction path ও CX-14। T10 live-এর আগে দুই ক্রমের evidence। |
| Cover grant ও C3/C4 attempt boundary, C8 provider reconcile, pilot device/network এবং native Amharic কে প্রমাণ করবেন? | Identity, Apps/C4, C8, QA ও pilot school | §৬.২-তে expected results আছে। Owner সম্মতি, real V08/V16/V17 ও rollout evidence বাকি। |

§৭.২-এর producer build ও QA/client gate sign-off তাদের delivery-র পরে বসবে। Coding gate §১-এ আছে।

[System ও data map](../../03-system-module-and-data-map.md) · [Roles](../../04-roles-and-permissions.md) · [Workflows](../../05-workflows-and-ui-ux.md) · [Acceptance](../../07-qa-pilot-and-acceptance.md)
