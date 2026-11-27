# System 3 · Student Records and Admissions — Implementation plan

**অবস্থা: Pending owner review।** এটি draft। Coding-এর অনুমতি দেওয়া হয়নি। Review round 1-এর findings অনুযায়ী সংশোধিত (শেষে “পরিবর্তনের রেকর্ড”)। **তারিখ:** ২৬ নভেম্বর ২০২৬। **Planning checkout:** `main` @ `2f0b1e9` (same as `origin/main`), docs only। **Proposed first branch:** `feat/students/registration-import`। **Integration base: Pending** — backend/app checkout এই workspace-এ নেই, তাই তার branch বা commit কোনোটিই verified নয়। **সঙ্গের নথি:** [Design plan](UI_UX_DESIGN_PLAN.md)। **সিদ্ধান্ত:** ২৬ নভেম্বর ২০২৬-এ আপনি সব সুপারিশ মেনেছেন (অংশ ১৭)।

## ১. সংক্ষেপে

- প্রথম branch Registrar-এর import ও quick registration দেয়: ADM-021, SIS-001, SIS-002 (শুধু pilot অংশ) ও PLT-014।
- তিনটি সীমা সব জায়গায় একই থাকবে।
  - **A8** student, household contact ও placement history লেখে।
  - **C3** guardian account–child access grant লেখে। A8 শুধু request reference ও last known status রাখে।
  - **A2** class reference দেয় এবং তারিখ ধরে roster পড়ে। SIS-003 ও CLS-001 tracker-এ A2-এর feature, তাই এখানে এগুলো dependency।
- এই checkout-এ কোনো code নেই। তাই নিচের প্রতিটি path, route, table ও name **Proposed**। Task T0 সেগুলোকে verified করবে।
- Requirement link: 5.1.1 (SIS-001), 5.1.2 (SIS-002, IAM-004), 5.1.3 (SIS-003, CLS-001), 5.1.5 (PLT-014, SIS-016), 5.1.W (ADM-021) ও 18.1.5 (PLT-014, DVL-012)।
- Open decision-এর সব সিদ্ধান্ত হয়েছে (২৬ নভেম্বর ২০২৬, অংশ ১৭)। বাকি শুধু অন্য system-এর কাজ বা সম্মতি: TD-04-এর পথ (CX-14, Identity-র T-00-এর পরে Identity ও Students owner, তারপর আপনি), OD-13-এর A1/A2 slice 0 (System 4), System 1-এর chunk size, আর pilot batch invite (System 2-এর PR-06)-এ client product owner-এর অনুমোদন (CX-13)।

## ২. যা যাচাই হয়েছে, যা pending

**Verified**

- Checkout-এ ১৮টি tracked file আছে: ১৪টি `.md` ও ৪টি `.csv`।
- Branch `main`, HEAD `2f0b1e9` = `origin/main`।
- Source code, route, component, API, test, schema বা config নেই।
- `integration/CTO-HANDOFF.md`-এর “বর্তমান অবস্থা” table-এ এখনো commit `febdc8f` লেখা, কিন্তু ২৬ নভেম্বরের দায়িত্ব বদলের log-এ `2f0b1e9` লেখা হয়েছে। Summary-র লাইনটি Integration CTO মিলিয়ে নেবেন।
- System 2-এর plan (`system-plans/02-identity/IMPLEMENTATION_PLAN.md`)-এ C3-এর guardian link status `pending`, `active`, `restricted`, `ended`। Undo-তে link বা request ID ধরে প্রত্যাহার সেখানে owner গৃহীত (CX-12, এই plan-এর TD-08)।

**Pending**

- Backend/app checkout ও তার integration branch।
- Stack ও framework।
- Build ও test command।
- Schema/migration tool।
- Auth middleware, এবং session কীভাবে school context দেয়।
- আগে থেকে থাকা student, guardian বা class table/module।
- Background job বা queue।
- Outbox/event mechanism।
- File service।
- Web framework, component library, i18n ও calendar utility।
- CI।
- (এখন যাচাই হয়েছে) Blueprint PDF Git Bash-এর `pdftotext -layout` দিয়ে পড়া যায়। এই plan-এ Blueprint-এর যে পাতা উল্লেখ আছে (পৃ. ১৫, ২৩, ৩৫–৩৬, ৬৪, ৯৮), সেগুলো PDF থেকে মেলানো। Scope-এর পাতা design plan-এর অংশ ৩-এ।

## ৩. Stack ও conventions

Existing stack এখনো জানা নেই। তাই এই plan কোনো language, framework বা database ধরে নেয় না। T0-তে stack যাচাই হলে তার convention-ই মানা হবে। ততক্ষণ এই plan শুধু [INTERFACES](../../integration/INTERFACES.md)-এ আগে থেকে লেখা convention মানে (Blueprint পৃ. ৩৫–৩৬, INTERFACES-এ যেভাবে উদ্ধৃত)।

- Versioned API। State বদলানো request-এ `Idempotency-Key` থাকে। একই body দিলে আগের result ফেরে। অন্য body দিলে reject হয়।
- Record version পুরনো হলে conflict ফেরে, সঙ্গে বর্তমান state। Error problem-details shape-এ আসে।
- Date হলো school-এর civil date। Instant হলো UTC।
- Owner-এর data change ও তার outbox event একই transaction-এ থাকে। Delivery at least once। Subscriber event ID দেখে repeat বাদ দেয়।
- Proposed event envelope field: `eventId, eventType, eventVersion, schoolId, recordId, recordVersion, occurredAt`।
- Event-এ শুধু ID ও fact থাকে। শিশুর নাম, custody document বা raw CSV থাকে না।
- অন্য module-এর data ID ধরে owner-এর interface বা event দিয়ে নেওয়া হয়। তার table কখনো সরাসরি পড়া বা join করা হয় না।

## ৪. Paths

**Verified path: নেই।**

নিচের সব path **Proposed**। Angle bracket-এর placeholder T0-তে ঠিক হবে, আর তখন শেষ কলামে আসল path বসবে।

| Proposed path | এখানে কী থাকে | Verified path |
|---|---|---|
| `<backend>/students/` | A8 domain: student, household contact, placement, versioning | Pending (T0) |
| `<backend>/students/import/` | batch, staged row, preview, confirm, undo | Pending (T0) |
| `<backend>/students/registration/` | A7 quick registration entry, যা A8 command ডাকে | Pending (T0) |
| `<backend>/students/ports/` | বাইরের দিকের interface: C1 entitlement, C3 authorise/guardian link, A1/A2 class reference ও roster | Pending (T0) |
| `<contracts>/students/` | API ও event-এর example। Shared fixture Platform T1-এর file-এ (CX-07) | Pending (T0) |
| `<web>/office/students/` | Registrar screen S01–S13 | Pending (T0) |
| `<tests>/students/` | unit, integration, contract ও E2E test | Pending (T0) |
| `<tests>/fixtures/students/x08-1000/` | ১,০০০ row-এর synthetic fixture | Pending (T0) |

## ৫. C1, C3 ও A2 dependency

সব contract **Proposed**।

| কার কাছ থেকে | কী লাগবে (Proposed contract) | Feature ID | Merge না হওয়া পর্যন্ত |
|---|---|---|---|
| C1/C2/C9 (System 1) | `SchoolContext` ও entitlement query: verified session → school, actor ও scope। School + product + module/action + time → effective entitlement। সঙ্গে audit (PLT-006) ও file service (PLT-013)। | PRD-001, PLT-005, PLT-001, PLT-002 | port-এর পেছনে fake, shared fixture দিয়ে |
| C3 (System 2) | `Authorize`: actor/membership + product + action + target school/campus/student → allow/deny ও approval state। Guardian link request: student ID + household contact reference + source batch/row reference + per-operation key → request reference, link status ও version। Link `pending` অবস্থায়, কোনো right ছাড়া শুরু হয়। সুপারিশকৃত পথ: request A8-এর row transaction-এর ভেতরে C3-এর command হিসেবে চলে, savepoint সহ। এটি Identity-র T-00-এর শর্তে (CX-14, Identity ও Students owner-এর কাছে খোলা)। বিকল্প: durable outbound record + (batch, row) key-এর idempotent command (TD-04)। Undo-তে প্রত্যাহার: নির্দিষ্ট link ID + origin + expected version + per-operation key → প্রতিটি item-এর ফল: `ended` (active link শেষ), `cancelled` (pending request বাতিল), `conflict` (version মেলেনি), `not_found` (CX-12)। Batch reference ধরে প্রত্যাহার নয় (TD-08)। Custody restriction-এর দ্বিতীয় approver Principal, school-এর Approvals policy-তে (OD-04)। Link status পড়া ও `guardian_right.changed` event। Students C3-কে বলবে IAM-014 matrix-এ এই Proposed action-গুলো রাখতে: `students.import.preview`, `students.import.confirm`, `students.import.undo`, `students.register`, `students.placement.create`, `students.read`, `students.household.edit`। `students.export` C3-এর T-01 catalogue-এ থাকে, তবে প্রথম ব্যবহার SIS-016-এর branch-এ। Pilot batch invite (System 2-এর PR-06, CX-13; client product owner-এর অনুমোদন বাকি) C3-এর আলাদা ধাপ। A8 শুধু batch, class ও campus ধরে pending link-এর তালিকা দেয়। Import নিজে কোনো invite পাঠায় না। | IAM-003, IAM-004, IAM-007, IAM-014 | fake |
| A1/A2 (System 4) | Class reference যাচাই: school/campus/year/grade/section + effective date → valid target ও reference version। তারিখ ধরে roster: section + date → student ID ও roster version। A2 `placement.changed` consume করে। A8 একমাত্র placement writer। Students merge-এর আগে real A1/A2 দিয়ে run লাগবে (DEPENDENCIES)। কে দেবে ও কখন, তা OD-13। সিদ্ধান্ত (OD-13): System 4-এর একজন planner ছোট A1/A2 slice 0 plan করবেন, পাশাপাশি তৈরি হবে। Port-এর পেছনের fake শুধু test-এর জন্য; Students A1/A2-এর কোনো stub বা implementation বানাবে না। | SIS-003, CLS-001, SET-002, SET-004 | fake |
| অন্য module (A9, Results, Fees, Learn, C4) | Downstream-reference check: student ID + তারিখ → batch-এর পরে ওই module-এ এই student-এর record আছে কি না | প্রতিটি consumer-এর নিজের feature | প্রথম branch-এ কোনো consumer live নেই, তাই registry খালি। প্রতিটি consumer live হওয়ার আগে নিজের check যোগ করবে, এটি release gate (TD-07)। |

Merge order: Platform → Identity → Students ([DEPENDENCIES.md](../../integration/DEPENDENCIES.md))। Fake থেকে পাওয়া evidence ও আসল integration run-এর evidence আলাদা করে রাখা হয়।

System 2-এর draft-এ `student.withdrawn` event চাওয়া আছে (তাদের CX-04)। Leaving A13-এর কাজ (R2), তাই প্রথম branch এই event দেয় না। MVP-তে কোনো student চলে গেলে Registrar C3-এর screen-এ হাতে guardian link শেষ করেন।

### Cross-system বিষয়: System 1-এর queued-job guard

Integration CTO জানিয়েছেন, System 1-এর Draft v2 ও v3 (`system-plans/01-platform/IMPLEMENTATION_PLAN.md`, T8, §৭.৩, §৯.১ ও §১৯; design plan §২০) A8-এর import job-এর জন্য একটি guard contract প্রস্তাব করেছে। Flag বা access বদলালে কী হবে, তা আপনি ২৬ নভেম্বর ঠিক করেছেন (System 1-এর নিয়ম)। Chunk size CTO-র cross-check-এ খোলা। Student count interface বাদ গেছে, কারণ তার একমাত্র ব্যবহার System 1-এর revert panel owner বাদ দিয়েছেন। Merge-এর আগে দুই plan মেলাতে হবে।

| বিষয় | System 1-এর প্রস্তাব | এই plan | মিল বা অমিল |
|---|---|---|---|
| Chunk ও transaction | প্রতিটি import chunk A8-এর নিজের write transaction। শুরুতে Platform-এর guard আর C3-এর `AuthorizeInTx` lock সহ চলে। Chunk “শুরু হয়” যখন lock ধরে | প্রতিটি row atomic, নিজের transaction-এ (T9) | মেলে। C3-এর command-ও row-এর transaction-এ চলে (TD-04)। Chunk-এর ভেতরে প্রতি row-এ savepoint, তাই এক row ব্যর্থ হলে বাকি row থাকে। থামার জায়গা চলতি chunk-এর শেষে। Chunk size Platform owner-এর সঙ্গে Pending |
| Confirm-এর পরে access বা flag বদলালে | Product, module বা school lifecycle বদলালে (বন্ধ করে আবার চালু করলেও) chunk `access_changed` দিয়ে refuse হয়, job blocked। কিছু নিজে থেকে আবার চলে না (তাদের OD-5) | Blocked — access changed। নিজে থেকে আবার চলে না, নতুন confirm (T9, অংশ ১৫)। Permission matrix-এ role × action-এর ঘর বদলালেও একই। | **মিলেছে।** আপনি ২৬ নভেম্বর System 1-এর নিয়ম মেনেছেন। System 1-এর plan-এ OD-7 বন্ধ করা বাকি |
| আবার চালানো | Job row-এর guard field (school, product, module, actor, campus, batch, confirmedAt, key) পরে বদলায় না | অনুমোদিত আরেকজন Registrar আবার শুরু করতে পারেন | মেলে, যদি “আবার শুরু” মানে বাকি row-এর জন্য নতুন confirm, নতুন job ও নতুন guard field সহ। Undo-ও একই guard-এর অধীনে চলে, তাই product বন্ধ থাকলে undo-ও চলে না |
| Session শেষ হলে | শুধু session-এর মেয়াদ শেষ হলে job থামে না। Account lock বা membership suspend হলে পরের chunk-এ থামে | Background কাজের প্রতিটি অংশে access check | মেলে |
| C3-এ link request (TD-04) | C3-এর check chunk-এর transaction-এর ভেতরে চলে | Row-এর transaction-এর ভেতরে C3-এর command, savepoint সহ (T10) | মেলে, তবে TD-04 এখনো সুপারিশ: Identity-র T-00-এর শর্তে (CX-14)। বিকল্প: durable outbound record + (batch, row) key-এর idempotent command |
| Problem code | `product_off`, `module_off` (404); `school_read_only`, `school_suspended` (403, তাদের OD-3); job-এ `access_changed`, `job_integrity` | নিরাপদ denied state | A8 ও UI এই code চিনবে। Read-only school-এ read চলে, write বন্ধ (design অংশ ১৩-এর “সব” row) |
| Decision cache | P0-তে নেই। প্রতিটি request ও chunk database থেকে পড়ে | A8 কোনো decision cache রাখে না | মেলে |
| Job type (v3) | Guard-এর অধীনের import job নতুন job type পায় (যেমন `students.import.v2`), যা পুরোনো worker চেনে না। Rollback-এর পরের পুরোনো binary এই job claim করে না | Confirm ও undo background job (TD-03) | মেলে। Confirm-এর job `students.import.v2`, undo-র জন্য আলাদা type। নাম T0-তে Platform-এর job framework দেখে চূড়ান্ত (T9, T12) |
| Lease, claim token ও checkpoint (v3) | Claim-এ lease ও claim token। প্রতিটি chunk token মেলায় (fencing), না মিললে rollback। Chunk-এর checkpoint write-এর একই transaction-এ | Confirm-এর অগ্রগতি durable, থামলে বাকি row থেকে চলে (T9, S-06) | মেলে। A8 Platform-এর helper ব্যবহার করে। নতুন acceptance: lease শেষে reclaim হলে পুরোনো worker-এর দেরিতে আসা chunk কিছু লেখে না (T9) |
| প্রতি chunk-এ C3-এর লেখা (v3) | একই transaction-এ, অথবা (batch, row) key-এর idempotent command | Row-এর transaction-এ, savepoint সহ। Retry-তে একই per-operation key (TD-04) | মেলে। Per-operation key = (batch, row)। কোন পথ, তা CX-14-এ Identity-র T-00-এর পরে ঠিক হবে |
| `job_integrity` বার্তা (v3) | Registrar দেখেন “This import could not run safely. Nothing was saved. Start again.” | নিরাপদ error state | মেলে। Design অংশ ১৩-এর S08 row-এ বসানো হয়েছে |
| Actor-এর access মাঝে বন্ধ হয়ে ফিরলে (v3) | Confirm-এর পরে actor revoke বা suspend হয়ে থাকলে, পরে ফিরলেও job blocked `access_changed` | Blocked — access changed (T9) | মেলে। এই case-ও T9-এর acceptance-এ যোগ হয়েছে |
| Protected download (v3) | প্রতিটি download-এর মুহূর্তে বর্তমান permission ও entitlement আবার দেখা হয় | প্রথম branch-এ কোনো download নেই | পরে লাগবে SIS-016-এর export-এর branch-এ (Blueprint পৃ. ৮১) |

যে অংশ এর ওপর নির্ভর করে: T4, T9, T10, T12, T14 ও অংশ ১৫-এর rollback।

## ৬. API (Proposed, শুধু যেখানে দরকার)

School ID কখনো path, body বা CSV থেকে আসে না। সেটা verified session থেকে আসে।

- প্রতিটি state-changing request (POST, PUT, PATCH, DELETE)-এ `Idempotency-Key`। একই key ও body-তে আগের ফল, ভিন্ন body-তে reject।
- বিদ্যমান record বদলালে expected version লাগে (batch revision বা row version)। পুরোনো version হলে conflict, সঙ্গে caller-এর scope-এর বর্তমান অবস্থা।
- Confirm একটি নির্দিষ্ট batch revision ধরে চলে (mapping version, correction ও সিদ্ধান্ত মিলিয়ে)। Confirm হয়ে যাওয়া batch অন্য key দিয়েও আবার confirm হয় না, বর্তমান অবস্থা সহ conflict ফেরে।
- Replay-তে জমা ফল ফেরানোর আগে caller-এর বর্তমান read scope আবার দেখা হয়। Confirm-এর replay মূল receipt ফেরায়, সঙ্গে live অবস্থার link।

| কাজ | Proposed route | Note | C3 action |
|---|---|---|---|
| Draft batch তৈরি | `POST /v1/students/imports` | `Idempotency-Key`। File file service দিয়ে যায়। School ও campus session থেকে। শুধু CSV (UTF-8), সর্বোচ্চ ৫,০০০ row ও ১০ MB (OD-08, TD-02) | `students.import.preview` |
| Mapping সংরক্ষণ | `PUT /v1/students/imports/{batchId}/mapping` | `Idempotency-Key`, expected batch revision। Mapping version রাখা হয়। খালি তারিখের জন্য একটা তারিখ বাছা যায় (ঐচ্ছিক, OD-05) | `students.import.preview` |
| Dry run | `POST /v1/students/imports/{batchId}/preview` | `Idempotency-Key`। কোনো persistent student, placement বা grant লেখে না | `students.import.preview` |
| Row-এর তালিকা | `GET /v1/students/imports/{batchId}/rows` | status দিয়ে filter, paged | `students.import.preview` |
| Row ঠিক করা বা বাদ দেওয়া | `PATCH /v1/students/imports/{batchId}/rows/{rowId}` | `Idempotency-Key`, expected row version | `students.import.preview` |
| Duplicate সিদ্ধান্ত | `PUT /v1/students/imports/{batchId}/rows/{rowId}/decision` | `Idempotency-Key`, expected row version। Scope-এর বাইরের candidate ID নেওয়া হয় না। Undo হওয়া number-এ: reactivate, number ছেড়ে নতুন record, বা skip (TD-01) | `students.import.preview` |
| Confirm | `POST /v1/students/imports/{batchId}/confirm` | `Idempotency-Key`, expected batch revision | `students.import.confirm` |
| Batch-এর ফল | `GET /v1/students/imports/{batchId}` | count ও reconciliation | `students.import.preview` |
| Failed row retry | `POST /v1/students/imports/{batchId}/retry` | `Idempotency-Key` | `students.import.confirm` |
| Undo preview | `POST /v1/students/imports/{batchId}/undo-preview` | `Idempotency-Key`। কোনো domain write নেই। Confirm-এর ৩০ দিন পরে reject | `students.import.undo` |
| Undo | `POST /v1/students/imports/{batchId}/undo` | `Idempotency-Key`, expected batch revision, কারণ দিতে হবে। Confirm-এর ৩০ দিনের মধ্যে (OD-06) | `students.import.undo` |
| Import history | `GET /v1/students/imports` | — | `students.import.preview` |
| Quick registration | `POST /v1/students` | `Idempotency-Key` | `students.register` |
| Student পড়া | `GET /v1/students/{studentId}` | field-level permission | `students.read` |
| প্রথম placement | `POST /v1/students/{studentId}/placements` | `Idempotency-Key`, expected version | `students.placement.create` |
| Contact যোগ | `POST /v1/students/{studentId}/contacts` | `Idempotency-Key`। নতুন guardian contact-এ access request যায় (OD-07) | `students.household.edit` |
| Sibling link | `POST /v1/students/{studentId}/sibling-links` | `Idempotency-Key`। একই school-এর enrolled student (OD-12) | `students.household.edit` |
| Sibling link সরানো | `DELETE /v1/students/{studentId}/sibling-links/{linkId}` | `Idempotency-Key`, expected version | `students.household.edit` |

**অন্যের owned, এখানে শুধু consume:** C1 entitlement query। C3 authorise, guardian link command, status ও withdraw। A2 class reference validation ও তারিখ ধরে roster।

## ৭. Schema (Proposed, শুধু যেখানে দরকার)

Students-এর নিজের নতুন record শুধু এগুলো: student; student version history; household contact; role সহ student–contact link; placement; import batch; import batch row; sibling link (একই school-এর দুই student); C3 request-এর A8-দিকের অবস্থা: প্রতি row-এ link বা request ID, শেষ জানা status ও version (`guardian_right.changed` থেকে হালনাগাদ), আর per-operation key। Undo এই version-ই expected version হিসেবে পাঠায় (CX-12); batch revision; undo-র row progress (pending, reversed, kept, conflict, waiting — retry)। Platform আগে থেকে outbox দিলে সেটাই reuse হবে।

Students এগুলো **তৈরি করে না:** access grant (C3), roster (A2), class reference (A1/A2), আর pupil account বা pupil activation code (C3-এর কাজ: IAM-001 ও D-16, Blueprint পৃ. ৬৪, ৯৮)।

নিয়ম:

- Internal ID হলো time-ordered UUID (INTERFACES-এ যেভাবে উদ্ধৃত)।
- (school, student number) unique। Number একজন আসল student-এর, অন্য student-কে দেওয়া হয় না। ব্যতিক্রম একটাই: undo হওয়া record ভিন্ন child প্রমাণিত হলে Registrar number ছাড়তে পারেন, audit সহ (TD-01)।
- Tenant boundary database-এ enforce হয় Platform-এর mechanism দিয়ে (PLT-002)।
- প্রতিটি record-এ created/updated metadata ও version থাকে (requirement 14.2.6, SIS-007)।
- Student, contact link ও placement প্রতিটিতে একটি origin reference থাকে (batch row বা একক command)। তাতে undo বুঝতে পারে batch কী তৈরি করেছিল।
- Contact role-এর তালিকা বাড়তে পারে। তাই R1 remainder-এ pickup যোগ করলে পুরনো data বদলাতে হবে না।
- Student profile-এর সঠিক field তালিকা আসবে requirement 5.1.1 ও pilot school-এর নিয়ম থেকে, T0-র পর।
- Student-এর status: active → import undone → reactivated। Reactivation-এ একই internal ID ও number, নতুন version। Reactivation পুরোনো guardian link ফেরায় না, নতুন batch-এর origin-এ নতুন request যায় (CX-15)।
- “Import undone” student সাধারণ read, search ও roster-এ not-found দেয়, অস্তিত্বহীন ID-র মতোই (A8-এর নিজের state check, CX-15)। শুধু import history ও re-import-এর মিলানোয় দেখা যায়।
- Raw CSV ও staged row confirm-এর ৩০ দিন পরে মোছা হয় (TD-02)। Origin reference, mapping version, batch-এর হিসাব ও audit থাকে।

## ৮. Auth

কোনো নতুন role নেই। প্রতিটি Students command ও query-তে check-এর ক্রম:

1. C1 entitlement
2. C3 authorise
3. A8-এর নিজের state rule

প্রতিটি route কোন action চায় তা §৬-এর table-এ। Confirm ও undo শুরুতে, background কাজের প্রতিটি নতুন অংশ শুরুর আগে, আর replay-তে ফল ফেরানোর আগে আবার check হয়। চলার সময় Registrar-এর access, flag, product বা school-এর অবস্থা বদলালে, বা confirmedAt-এর পরে permission matrix-এ Registrar role × ওই action-এর ঘর বদলালে কাজ চলতি row (বা chunk) শেষ করে থামে, batch হয় “Blocked — access changed”। আগে লেখা row যেমন আছে থাকে। নিজে থেকে আবার চলে না। একই বা অন্য অনুমোদিত Registrar নতুন confirm দিয়ে বাকি row চালান। Denied response অন্য school বা campus সম্পর্কে কিছু জানায় না। School বদলালে আগের school-এর Students data cache থেকে আসে না, API ও web দুই জায়গাতেই। C3-এর কাছে অনুরোধ: ৭টি Proposed action IAM-014 matrix-এ রাখা (System 2-এর CX-03-এর উত্তর `students.household.edit`)।

## ৯. Events

`student.enrolled` ও `placement.changed` (নাম Blueprint পৃ. ১৩-এর Students-এর event তালিকা থেকে; shape **Proposed**)। Undo-র ফল অন্য module জানে `placement.changed` দিয়ে, কারণ void হওয়া placement roster থেকে সরে যায়। Student-এর নিজের status বদলের (import undone, reactivated, number ছাড়া) জন্য আলাদা কোনো event নেই: Blueprint-এর তালিকায় এমন event নেই, আর কোনো module সেটি পড়ত না। Confirm ও undo-তে owner-এর write আর event একই transaction-এ outbox-এ যায়। Envelope-এ প্রযোজ্য হলে product ও effective date থাকে। Consumer event পেয়ে নিজের বর্তমান scope আবার দেখে। A8 `guardian_right.changed` event ID দিয়ে repeat বাদ দেয়, আর পুরোনো record version-এর event উপেক্ষা করে। History কখনো মোছা হয় না। `student.withdrawn` A13-এর (R2), প্রথম branch-এ নেই। Undo-তে guardian access শেষ হয় শুধু CX-12-এর command দিয়ে। C3 student-এর কোনো status event পড়ে না (System 2-এর CX-15; reconciliation alert-ও বাদ গেছে)।

## ১০. Tasks (dependency order)

প্রতিটি task-এ আছে: Design link, Feature ID, Paths, পরিবর্তন, Acceptance, Verification, নির্ভরতা ও সিদ্ধান্ত। T0 শেষ না হওয়া পর্যন্ত প্রতিটি task-এর জন্য **Verified path: নেই**। এটা এখানে একবারই বলা হলো। প্রতি task-এ শুধু Proposed path লেখা আছে।

### T0 · Repository ও integration base যাচাই

(কোনো code change নেই; বাকি সবকিছুর gate)

- **Design link:** সব DR (precondition)।
- **Feature ID:** ADM-021, SIS-001, SIS-002, PLT-014।
- **Paths:** নেই। Output হলো অংশ ৪-এর table-এর “Verified path” কলাম পূরণ করা।
- **পরিবর্তন:** Backend/app checkout খুঁজে বের করা। Default ও integration branch এবং commit যাচাই। Build ও test command। Schema/migration tool। Auth middleware ও session থেকে school context। আগে থেকে থাকা student, guardian বা class table/module। Job/queue। Outbox/event। File service। Web framework, component library, i18n ও calendar utility। Test framework। CI। A8-র সঙ্গে মেলে এমন existing code থাকলে সেটা reuse হবে নাকি replace, তা লিখে রাখা।
- **Acceptance:** প্রতিটি placeholder path হয় আসল path-এ বদলায়, নয় "new" চিহ্নিত হয়। Integration base-এর branch ও commit এই plan-এ লেখা হয়। চারটি Feature ID-র tracker row evidence যতটুকু দেয় ততটুকুই update হয়। যেখানে existing code A8-এর কাজের সঙ্গে মেলে, প্রতিটির জন্য reuse নাকি নতুন, তা লেখা থাকে। Platform-এর outbox ও job infrastructure আছে কি না লেখা থাকে।
- **Verification:** আপনি বা Integration CTO base নিশ্চিত করবেন। ওই base-এ একটি clean build ও existing test-এর একটি run command ও result সহ record করা হয়।
- **নির্ভরতা:** আপনি backend/app checkout-এর access দেবেন।
- **সিদ্ধান্ত:** TD-03। খোলা: TD-06 (এই task-এই ঠিক হবে)।

### T1 · Contract ও shared fixture (C1, C3, A2, A8)

(শুধু example ও fake)

- **Design link:** DR-01, DR-02, DR-08, DR-09, DR-10।
- **Feature ID:** dependency PRD-001, PLT-005, IAM-003, IAM-004, SIS-003, CLS-001; convention INT-001, INT-002; deliverable DVL-006।
- **Proposed paths:** `<contracts>/students/`, Platform T1-এর shared fixture file (path T0-তে), `<backend>/students/ports/`।
- **পরিবর্তন:** INTERFACES-এর চারটি contract-এর জন্য Students-পক্ষের request/response sample লেখা ("তিন team আলাদা branch নেওয়ার আগে")। Platform T1-এর একটি versioned shared fixture file তিন system-এর জন্য (CX-07)। তাতে Students-এর row যোগ হয়: School A (School Management চালু), School B (একই section label "নীল", একই student number `A-017`), North-campus ও South-campus-এর একজন করে Registrar, একজন Learning Admin, একটি expired session ও row `r17`। সঙ্গে প্রতি school-এ একটি সফল control, আর Identity-র Fixture A/B (তিনটি linked সন্তান ও একটি unlinked চতুর্থ sibling)। Version বদলালে তিন owner review করেন। X08-1000 volume fixture আলাদা থাকে। C1, C3 ও A2-র জন্য fake সহ port, যাতে ওই তিনটি merge হওয়ার আগেই Students-এর কাজ শুরু হতে পারে। C3-এর link request, link ID ধরে প্রত্যাহার (TD-08) ও status-এর sample। C3-এর fake প্রত্যাহারের চারটি ফলই দেয়: `ended`, `cancelled`, `conflict`, `not_found`। Contract test চারটিই চালায়। Downstream-reference check-এর contract ও fake (TD-07)।
- **Acceptance:** Platform, Identity ও Academics owner প্রত্যেকে নিজের sample review করেন। Allowed/denied sample-এ একমত হতে হবে: ভুল school ID কিছুই দেখে না; ভুল product লিখতে পারে না; অন্য campus denied; অজানা section invalid; তারিখের আগে ও পরে roster; confirm আবার করলে একই placement। প্রত্যাহারের sample-এ conflict row-এর link থাকে না।
- **Verification:** Fake-এর বিরুদ্ধে contract test চলে, producer ও consumer একই fixture version-এ। Review sign-off-এর link থাকে।
- **নির্ভরতা:** T0।
- **সিদ্ধান্ত:** OD-04, OD-07, OD-13, TD-07, TD-08 (CX-12)। খোলা: TD-04 (CX-14)।

### T2 · Student record ও versioning (A8 core)

- **Design link:** DR-07, DR-13; P0-4, P1-1 (data দিক)।
- **Feature ID:** SIS-001, SIS-007।
- **Proposed paths:** `<backend>/students/`, student table-এর migration, `<tests>/students/`।
- **পরিবর্তন:** Time-ordered UUID internal ID ও school-scoped student number সহ student record। প্রতিটি change একটি নতুন version বানায়, সঙ্গে actor, সময় ও origin। "import undone"-এর জন্য একটি status (design assumption A-11)। এই task-এ domain ও storage তৈরি হয়। বাইরের কোনো route T4-এর guard ছাড়া খোলা হয় না। Status: active, import undone, reactivated। Reactivation-এ একই internal ID ও number, নতুন row-এর মান নতুন version। ভিন্ন child প্রমাণিত হলে undo হওয়া record থেকে number ছাড়ার command, audit সহ (TD-01)।
- **Acceptance:** School A ও School B-তে একই number দুটি আলাদা record দেয়। একই school-এ একই number দিয়ে দ্বিতীয় create conflict দিয়ে reject হয়। Edit নতুন version বানায় ও পুরনোটা পড়া যায়। School A-র session ID দিয়ে School B-র student পড়তে পারে না। Reactivation-এর পরে internal ID ও number একই, পুরোনো version পড়া যায়। Number ছাড়া হলে voided record-এ “number released” audit থাকে, আর নতুন record একই number পায়। Undo হওয়া student-কে ID দিয়ে চাইলে not-found আসে, অস্তিত্বহীন ID-র মতোই। Reactivation-এর পরে পুরোনো guardian link ফেরে না।
- **Verification:** দুই-school fixture-এ unit ও integration test; tenant isolation suite (V01 slice)।
- **নির্ভরতা:** T0, T1।
- **সিদ্ধান্ত:** OD-01, TD-01। খোলা: TD-06।

### T3 · Household contact (SIS-002-এর pilot অংশ)

- **Design link:** DR-08; P0-5।
- **Feature ID:** SIS-002 (শুধু MVP slice)।
- **Proposed paths:** `<backend>/students/` (household), migration, test; §৬-এর contact ও sibling-link route।
- **পরিবর্তন:** Guardian, sibling ও emergency contact। একটি contact একাধিক student-এর সঙ্গে link হতে পারে, প্রতি student-এর জন্য আলাদা role সহ। Sibling হলো একই school-এর দুই enrolled student-এর link। Registrar দেন, একই guardian contact থাকলে প্রস্তাব আসে (OD-12)। Import sibling link আনে না। Contact সংরক্ষণে কোনো access তৈরি হয় না।
- **Acceptance:** একই guardian contact-এর তিন সন্তান প্রত্যেকে নিজের link দেখায়। Contact সংরক্ষণে C3-র কাছে grant call হয় শূন্যটি। Pilot API pickup role নেয় না। Sibling link শুধু একই school-এর ও Registrar-এর scope-এর student-এর সঙ্গে হয়।
- **Verification:** Integration test। Evidence যায় শুধু tracker-এর `MVP slice evidence`-এ।
- **নির্ভরতা:** T2।
- **সিদ্ধান্ত:** OD-12। খোলা: TD-06।

### T4 · Access wiring: C1 entitlement ও C3 authorise

- **Design link:** DR-01, DR-02; P0-9।
- **Feature ID:** dependency PRD-001, PLT-005, PLT-001, PLT-002, IAM-003, IAM-014।
- **Proposed paths:** `<backend>/students/ports/`, প্রতিটি Students route-এ ব্যবহৃত একটি shared guard।
- **পরিবর্তন:** প্রতিটি Students command ও query-তে একটি guard। School শুধু verified session থেকে। School Management entitlement। Action, campus ও target সহ C3 authorise। Denied response কিছু ফাঁস করে না। Confirm ও undo-তে আবার check। §৬-এর route-action table অনুযায়ী guard। Replay ও background কাজের প্রতিটি অংশে আবার check।
- **Acceptance:** চারটি denied case (অন্য campus-এর Registrar, Learning Admin, expired session বা revoked role, School Management বন্ধ) সবই write আটকায় এবং অন্য school বা campus-এর কোনো নাম, row বা count দেখায় না। Body বা CSV-তে School B-র ID থাকলেও target বদলায় না। Access প্রত্যাহারের পরে একই key-এর replay deny হয়। School বদলালে আগের school-এর data cache থেকে আসে না।
- **Verification:** Permission test (V02 slice) ও isolation test (V01, E2E-08 slice)। আগে fake দিয়ে, তারপর Platform ও Identity merge হলে আসল দিয়ে। দুই evidence আলাদা রাখা হয়।
- **নির্ভরতা:** T1।
- **সিদ্ধান্ত:** OD-06।

### T5 · প্রথম placement (A8 writer) ও class reference যাচাই

- **Design link:** DR-09; F4; P0-6।
- **Feature ID:** dependency SIS-003 (A2 feature; A8 হলো writer)।
- **Proposed paths:** `<backend>/students/` (placement), `<backend>/students/ports/` (A2), migration, test।
- **পরিবর্তন:** Placement history record: student ID, class reference ID ও reference version, school-এর civil date হিসেবে effective date, version ও origin। লেখার আগে A1/A2 interface দিয়ে target যাচাই। একই command ও key একই placement দেয়। একই section ও একই তারিখের placement আগে থাকলে কিছু বদলায় না (No change)। একই year-এ অন্য section-এ placed থাকলে কিছু লেখা হয় না, ফল “Already placed elsewhere” (design A-09; section change SIS-009, R1 backlog)। `placement.changed` / `student.enrolled` একই transaction-এ outbox-এ যায়। তারিখের নিয়ম (OD-05): চলতি academic year-এর আগের তারিখ চলে, সতর্কবার্তা সহ। Year শুরুর আগের তারিখ reject।
- **Acceptance:** Effective date না থাকলে reject। অজানা বা invalid section কারণ সহ reject, কিছু লেখা হয় না। Repeat দিলে একটিই placement। পুরনো reference version হলে conflict। প্রতি change-এ একটি event। Year শুরুর আগের তারিখ reject। চলতি year-এর আগের তারিখ সতর্কবার্তা সহ save হয়।
- **Verification:** A2 fake দিয়ে integration test; A2-র সঙ্গে contract test।
- **নির্ভরতা:** T1, T2, T4।
- **সিদ্ধান্ত:** OD-05, OD-13।

### T6 · Import batch: upload, mapping ও staging

- **Design link:** DR-03; F1 ধাপ ১–২; S02, S03; P0-1।
- **Feature ID:** PLT-014।
- **Proposed paths:** `<backend>/students/import/`, batch ও staged row-এর migration, test।
- **পরিবর্তন:** Draft batch (session থেকে school ও campus, actor, file service PLT-013 দিয়ে file reference, file checksum, mapping version)। Staged row: source row ID ও raw value। Column mapping, সঙ্গে date column কোন calendar ব্যবহার করে (PLT-015) ও class value mapping। Staging tenant-scoped এবং এটা student store নয়। Checksum ধরে একই file-এর warning। `Idempotency-Key` সহ batch তৈরি। Checksum শুধু “এই file আগে এসেছে” সতর্কবার্তার জন্য। আগে import হওয়া row student number দিয়ে চেনা হয়। শুধু CSV (UTF-8), template সহ (OD-08)। সীমা ৫,০০০ row ও ১০ MB (TD-02)। Mapping-এ খালি তারিখের জন্য একটা তারিখ বাছা যায়, শুধু খালি row-এ বসে, preview-তে চিহ্নিত (OD-05)। Confirm-এর ৩০ দিন পরে raw CSV ও staged row মোছার job (TD-02)।
- **Acceptance:** Upload-এ 0 student, 0 placement ও 0 grant তৈরি হয়। ১,০০০ row-এর file ১,০০০ staged row দেয়, source row ID স্থির থাকে। Non-UTF-8, খালি, header ছাড়া বা limit-পার file কারণ সহ reject হয়। File School B থেকে পড়া যায় না। ৫,০০১ row বা ১০ MB-এর বেশি file reject, কারণ সহ। Mapping-এর তারিখ শুধু খালি row-এ বসে। ৩০ দিন পরে raw file ও staged row থাকে না, কিন্তু batch-এর হিসাব, mapping ও audit থাকে।
- **Verification:** Integration test; file-এর উপর isolation test; §১২-এর S-13।
- **নির্ভরতা:** T4।
- **সিদ্ধান্ত:** OD-05, OD-08, TD-02।

### T7 · Dry-run validation ও duplicate detection

- **Design link:** DR-04, DR-05, DR-06; S04; P0-1, P0-2, P0-3।
- **Feature ID:** PLT-014, SIS-001।
- **Proposed paths:** `<backend>/students/import/` (validation), test।
- **পরিবর্তন:** প্রতি row-এর check: required field; valid date (OD-05: year শুরুর আগের তারিখ Error; চলতি year-এর আগের তারিখ সতর্কবার্তা সহ Ready); A2-র মাধ্যমে class reference (value mapping ধরে); file-এর ভেতরে একই number দুবার; একই school-এ একই number (number collision); একই নাম ও জন্মতারিখ, ভিন্ন number (OD-02; TD-05-এর নিয়মে গুছিয়ে হুবহু মেলানো, phone একা flag করে না, fuzzy মেলানো নেই); আগের কার্যকর batch-এ import হওয়া (Skip আগে থেকে বাছা); import undone student-এর number (Reactivate বা Skip, TD-01); Registrar-এর scope-এর বাইরের number (details ছাড়া, OD-11); এই year-এ অন্য section-এ placed; guardian reference। প্রতি row একটি status ও কারণ পায়। Status ধরে count। কোনো persistent student, placement বা grant লেখা হয় না।
- **Acceptance:** Ready + Needs review + Error + Excluded = মোট row। Persistent write count 0। School B-র একই number flag হয় না। অজানা section কখনো Ready হয় না। আবার চালালে একই count।
- **Verification:** প্রতি rule-এর জন্য একটি unit test; X08-1000 fixture-এ integration run, প্রত্যাশা 940 / 35 / 25 / 0।
- **নির্ভরতা:** T2, T5, T6।
- **সিদ্ধান্ত:** OD-01, OD-02, OD-05, OD-11, TD-01, TD-05।

### T8 · Row correction ও duplicate decision

- **Design link:** DR-05, DR-06; S05, S06; P0-2, P0-3।
- **Feature ID:** PLT-014, SIS-001।
- **Proposed paths:** `<backend>/students/import/`, test।
- **পরিবর্তন:** Correction staged row-এ override হিসেবে জমা হয়, মূল value রাখা থাকে, সঙ্গে actor, সময় ও row version (পুরনো version হলে conflict)। Class value mapping বদলালে প্রভাবিত row আবার evaluate হয়। Exclude ও include। প্রতিটি flagged row-এর জন্য একটি সিদ্ধান্ত: use existing, create new (number একই হলে আলাদা number লাগবে) বা skip। সিদ্ধান্ত সঙ্গে সঙ্গে save হয় এবং confirm পর্যন্ত বদলানো যায়। দুটি existing record merge নেই (SIS-012, R2)। সিদ্ধান্তের বিকল্প row-এর ধরন অনুযায়ী (design F1-এর “সিদ্ধান্তের ফল”)। Scope-এর বাইরের candidate ID server নেয় না। Decision-এ expected row version। Undo হওয়া number-এর row-এ তিনটি বিকল্প: একই child হলে reactivate, ভিন্ন child হলে number ছেড়ে নতুন record, অথবা skip (TD-01)।
- **Acceptance:** একটি value mapping ওই value-র সব row ঠিক করে। মূল value ফিরে পড়া যায়। সিদ্ধান্ত reload-এর পরও থাকে। "Use existing" existing profile-এর কোনো field বদলায় না। Audit entry আছে। Scope-এর বাইরের candidate ID দিয়ে সিদ্ধান্ত পাঠালে reject হয়, কোনো detail ফাঁস হয় না।
- **Verification:** Integration test; audit check (PLT-006)।
- **নির্ভরতা:** T7।
- **সিদ্ধান্ত:** OD-01, OD-02, OD-11, TD-01।

### T9 · Confirm: repeat-safe write, row-level outcome ও partial failure

- **Design link:** DR-07, DR-19; S07, S08; P0-4।
- **Feature ID:** PLT-014, PLT-006, SIS-001, SIS-002, SIS-007।
- **Proposed paths:** `<backend>/students/import/` (confirm), test।
- **পরিবর্তন:**
  - Confirm একটি batch revision ধরে চলে। Confirm হয়ে যাওয়া batch অন্য key দিয়েও আবার চলে না।
  - Access, entitlement ও class reference আবার যাচাই হয়।
  - প্রতিটি row atomic: student, contact, placement ও outbox event একই transaction-এ। C3-এর link command-ও এই transaction-এ চলে, তবে savepoint-এর ভেতরে: C3 ব্যর্থ হলে শুধু link বাদ যায়, row থাকে, access Failed (TD-04, OD-03)। A8-এর কোনো অংশ ব্যর্থ হলে ওই row-এর কিছুই থাকে না।
  - Row outcome: Created, Reactivated, Linked to existing (ভেতরে No change আলাদা), Not imported (কারণ সহ), Failed (কারণ সহ)।
  - Confirm-এর অগ্রগতি durable। থেমে গেলে একই key দিয়ে আবার চালালে বাকি row থেকে চলে, শেষ হওয়া row আবার লেখা হয় না।
  - চলার সময় access, flag, product বা school-এর অবস্থা বদলালে, বা confirmedAt-এর পরে permission matrix-এ role × action-এর ঘর বদলালে চলতি row (বা chunk) শেষে থামে: Blocked — access changed। নিজে থেকে আবার চলে না, নতুন confirm দিয়ে বাকি row (System 1-এর OD-7-এর সঙ্গে মিলিয়ে, আপনার সিদ্ধান্ত)।
  - Replay মূল receipt ফেরায়, তার আগে scope আবার দেখে।
  - Summary আর result-এর প্রতিটি পার্থক্যের row কারণ সহ থাকে।
  - Confirm background job-এ চলে (TD-03), তাই page ছেড়ে গেলেও চলে। Job type `students.import.v2`, যা পুরোনো worker চেনে না। Claim-এ lease ও claim token, প্রতিটি chunk token মেলায়, আর chunk-এর checkpoint write-এর একই transaction-এ (System 1-এর T8, Platform-এর helper)।
  - Undo হওয়া number-এ “ভিন্ন child” বাছলে number ছাড়া হয় ও নতুন record Created (TD-01)।
  - Confirm-এর পর batch বন্ধ (design A-10)। Reconciliation total জমা থাকে।
- **Acceptance:** একই confirm দুবার: count অপরিবর্তিত, মূল receipt। একই key ভিন্ন body: reject। Confirm হয়ে যাওয়া batch নতুন key দিয়ে: conflict, count অপরিবর্তিত। কোনো row-এ student লেখার পরে fault দিলে ওই row-এর student, contact, placement, event ও C3 link কিছুই থাকে না। Worker ৪০০ row-এর পরে থামালে একই key দিয়ে বাকি ৬০০ হয়, প্রথম ৪০০ আবার লেখা হয় না। Access ৩০০ row-এর পরে প্রত্যাহার হলে ৩০০-তে থামে, আর revoked user-এর replay deny হয়। Batch Blocked — access changed। Flag আবার চালু হলেও নিজে থেকে চলে না। Created + Reactivated + Linked to existing + Not imported + Failed = মোট row। Unresolved row import হয় না। C3-এর command ব্যর্থ হলে row থাকে, access Failed। Lease শেষে job নতুন token-এ reclaim হলে পুরোনো worker-এর দেরিতে আসা chunk কিছু লেখে না। Confirm-এর পরে Registrar-এর access বন্ধ হয়ে আবার ফিরলেও job Blocked — access changed। Job payload guard field-এর সঙ্গে না মিললে (`job_integrity`) কিছু লেখা হয় না, আর Registrar দেখেন “This import could not run safely. Nothing was saved. Start again.”
- **Verification:** Fault injection সহ integration test; §১২-এর targeted scenario S-01 থেকে S-07, S-10 ও S-14; X08-1000 stage 4–6; System 1-এর §৯.১ variant d2 ও h1–h3-এর Students-দিকের run।
- **নির্ভরতা:** T3, T4, T5, T8।
- **সিদ্ধান্ত:** OD-03, OD-07, TD-01, TD-03, System 1-এর OD-7। খোলা: TD-04 (CX-14)।

### T10 · Guardian link request ও status (C3)

- **Design link:** DR-08; F3; S08; P0-5।
- **Feature ID:** SIS-002; dependency IAM-004, IAM-007।
- **Proposed paths:** `<backend>/students/ports/` (C3), `<backend>/students/import/`, test।
- **পরিবর্তন:** Guardian link command row-এর transaction-এর ভেতরে savepoint সহ চলে, per-operation key দিয়ে (TD-04, Blueprint পৃ. ১৫)। C3 ব্যর্থ হলে savepoint পর্যন্ত ফেরে: student ও placement থাকে, access Failed। Retry নতুন transaction-এ একই key দিয়ে command চালায়, তাই দ্বিতীয় link হয় না। A8 প্রতি row-এ শুধু link বা request ID, শেষ জানা status ও version রাখে, grant রাখে না। Version `guardian_right.changed` থেকে হালনাগাদ থাকে, আর undo সেটাই expected version হিসেবে পাঠায় (CX-12)। Status আসে C3-এর উত্তর বা `guardian_right.changed` থেকে; repeat event বাদ যায়, পুরোনো version উপেক্ষা হয়। Status-এর তালিকা design F3-এর ছয়টি। Batch-level “retry all failed”। Invite বা activation C3-এর কাজ (IAM-002)। এই পথ সুপারিশ, Identity-র T-00-এর শর্তে (CX-14)। তা না হলে durable outbound record + (batch, row) key-এর idempotent command, তখন “Sending” status যোগ হবে। System 2-এর PR-06 অনুমোদন পেলে C3-এর batch invite-এর জন্য A8 একটি read interface দেয়: batch, class বা campus ধরে student ও pending link-এর তালিকা, শুধু Registrar-এর scope-এ।
- **Acceptance:** C3 12 টি row-এ fail করলে 12 টি Failed হয় এবং student ও placement অপরিবর্তিত থাকে। Retry duplicate request ছাড়া সফল হয়। যে contact-এর পরিচয় নিশ্চিত নয় সে Pending, কখনো Active নয়। A8 store-এ কোনো grant নেই। Job কোনো row-এর মাঝে থেমে গেলে ওই row-এর student ও link দুটোই থাকে না, অথবা দুটোই থাকে। আবার চালালে link একবারই হয়। Import চলার সময় invite call 0।
- **Verification:** C3-র সঙ্গে contract test; fault injection; পরে V02 / E2E-14 slice-এর জন্য আসল C3 দিয়ে run।
- **নির্ভরতা:** T1, T9।
- **সিদ্ধান্ত:** OD-03, OD-04, OD-07। খোলা: TD-04-এর পথ (CX-14, Identity-র T-00-এর পরে)। Pilot batch invite (System 2-এর PR-06) C3-এর আলাদা ধাপ, client product owner-এর অনুমোদন বাকি (CX-13)।

### T11 · Roster যাচাই (A2 read)

- **Design link:** DR-10; F5; S13; P0-6।
- **Feature ID:** dependency CLS-001।
- **Proposed paths:** `<backend>/students/ports/` (A2), test।
- **পরিবর্তন:** A2-র তারিখ ধরে roster-এ read-only call। A8 কোনো roster রাখে না। Batch ও student-এর panel-এর জন্য data। A2 সাময়িকভাবে সাড়া না দিলে retry।
- **Acceptance:** শুরুর তারিখের আগের দিন student নেই। শুরুর তারিখে student ঠিক একবার আছে। A2 পাওয়া না গেলে panel অপেক্ষার state দেখায় এবং আর কিছু ভাঙে না।
- **Verification:** A2-র সঙ্গে contract test; আসল A2 integration run আলাদা record হয়।
- **নির্ভরতা:** T5 ও A2-র interface।
- **সিদ্ধান্ত:** OD-13। খোলা: slice 0 তৈরি হওয়া।

### T12 · Undo

- **Design link:** DR-11, DR-19; F6; S10; P0-7।
- **Feature ID:** PLT-014; dependency IAM-007 (link শেষ হলে access বন্ধ), PLT-006।
- **Proposed paths:** `<backend>/students/import/` (undo), test।
- **পরিবর্তন:** Undo preview কোনো domain write ছাড়া প্রতিটি row-কে Reversed, Kept (existing), Conflict বা Not imported-এ ভাগ করে। নিয়ম নয়টি:
  1. শুধু যার origin এই batch, সেটাই ফেরানোর যোগ্য।
  2. Batch-এর আগে থেকে থাকা student কখনো সরে না বা বদলায় না। এই batch তাকে যা যোগ করেছে (placement, contact, access link), শুধু সেটুকু ফেরে, যদি এরপর বদলে না থাকে।
  3. Row Conflict হয় যদি row-এর batch-তৈরি কোনো item-এর পরের version থাকে, student-এর পরের placement বা অনুমোদিত edit থাকে, অথবা downstream-reference check বলে batch-এর পরে অন্য module-এ এই student-এর record আছে (যেমন attendance)। Conflict row-এর কিছুই বদলায় না, access-ও না।
  4. Undo চলার সময় প্রতিটি row আবার যাচাই হয় (version ও downstream check)। Preview-এর পরে বদলে যাওয়া row Conflict হয়, কারণ “changed since the preview”। Downstream check পাওয়া না গেলে row “Waiting — retry”, কিছুই বদলায় না।
  5. Reversal নতুন version: student “import undone”, placement ও contact link void। কিছু hard-delete হয় না। Reversal, outbox event আর C3-এর link শেষ করার command একই transaction-এ, সব-বা-কিছুই-না (TD-04)। বিকল্প পথে (CX-14) আগে C3-এ link শেষ হয়, তারপর A8-এর reversal। Link শেষ না হওয়া পর্যন্ত ওই row-এর কিছুই বদলায় না (Waiting — retry)।
  6. C3-কে প্রত্যাহার পাঠানো হয় নির্দিষ্ট link ID, origin ও expected version দিয়ে, শুধু Reversed row-এর link আর Kept (existing) row-এ এই batch-এর যোগ করা link-এর জন্য। Batch reference ধরে কখনো নয়। Conflict row-এর link, অন্য batch-এর link, আর পরে right বা custody বদলানো link বাদ থাকে। পরে Active হওয়া link-ও শেষ হয়, preview-তে আলাদা সংখ্যায় দেখিয়ে (OD-06)। C3-এ কখনো পৌঁছায়নি এমন request (অর্থাৎ Failed row) শুধু A8-এ বাতিল হয়। C3-এ থাকা pending request C3 নিজে `cancelled` করে। C3-এর ফল `conflict` বা `not_found` হলে row Conflict, কিছু বদলায় না।
  7. C3 ব্যর্থ হলে ওই row-এর পুরো transaction ফেরে: row “Waiting — retry”, কারণ “Access withdrawal pending” (CX-15-এর নাম), কিছুই বদলায় না। পরে একই per-operation key-এ retry। Downstream check না পেলেও “Waiting — retry”। এমন row থাকলে batch “Partly undone”, চূড়ান্ত total তখনো নয়।
  8. Undo command repeat-safe (key ও batch revision)। শুরুতে, প্রতিটি অংশে আর replay-তে access আবার দেখা হয়। কারণ দিতে হয়। Undo-ও background job-এ চলে, আলাদা job type নিয়ে, confirm-এর মতো একই guard, lease ও claim token-এর অধীনে।
  9. Undo শুধু confirm-এর ৩০ দিনের মধ্যে, ওই campus-এর import অধিকার থাকা Registrar, কারণ লিখে। এরপর reject, কারণ সহ (OD-06)।
- **Acceptance:**
  - **U1** — Batch-এর আগে থেকে থাকা ও এক row-এ matched student undo-র পরে আছে, profile অপরিবর্তিত। এই batch তাকে যে placement, contact ও access link দিয়েছিল তা void, `ended` বা `cancelled`। ওই student-এর অন্য contact ও link অক্ষত।
  - **U2** — Batch-তৈরি student, পরে আরেকটি placement পেয়েছে: student, batch-এর placement, পরের placement ও row-এর access link সবই আগের মতো। Row Conflict।
  - **U3** — Batch-তৈরি student, পরে অনুমোদিত edit: একইভাবে সব অক্ষত, access সহ। Row Conflict।
  - **U4** — C3 ৭টি row-এ প্রত্যাহারে ব্যর্থ: ওই ৭টি “Waiting — retry”, তাদের student, placement ও link আগের মতো। Retry-তে row ফেরে ও প্রতিটি link একবারই `ended` বা `cancelled`। তারপর চূড়ান্ত total।
  - **U5** — Preview-এর পরে ৩টি row-এ অনুমোদিত edit: undo-তে ওই ৩টি Conflict।
  - **U6** — Downstream check-এর fake বলে attendance আছে: ওই row Conflict। Check পাওয়া না গেলে “Waiting — retry”, কিছু বদলায় না।
  - **U7** — Undo চলার সময় Registrar-এর access প্রত্যাহার: চলতি row শেষে থামে।
  - **U8** — Confirm-এর ৩১তম দিনে undo: reject, কারণ সহ, কিছু বদলায় না।
  - Reversed + Kept (existing) + Conflict + Not imported = মোট row। Undo আবার করলে একই result। শুরুর তারিখের roster-এ ফেরানো student নেই। C3-তে শুধু পাঠানো ID-গুলো `ended` বা `cancelled`।
  - Contract test-এ C3-এর চারটি ফলই আসে: `ended` ও `cancelled`-এ row ফেরে, `conflict` ও `not_found`-এ row Conflict।
  - **U2-র setup:** প্রথম-placement route (T5) একই year-এ দ্বিতীয় section আটকায়। তাই পরের placement একটি স্পষ্ট test fixture দিয়ে A8-এর domain layer-এ লেখা হয়, যা SIS-009-এর কাজ অনুকরণ করে। SIS-009-এর branch-এ আসল route দিয়ে এই test আবার চলবে।
- **Verification:** Fault injection সহ U1–U8 integration test; §১২-এর S-08, S-09 ও S-12; X08-1000 stage 8–10।
- **নির্ভরতা:** T9, T10, T11।
- **সিদ্ধান্ত:** OD-01, OD-03, OD-06, TD-01, TD-07, TD-08 (CX-12)। খোলা: TD-04 (CX-14)।

### T13 · Quick registration (ADM-021)

- **Design link:** DR-12; F2; S11; P0-8।
- **Feature ID:** ADM-021, SIS-001, SIS-002।
- **Proposed paths:** `<backend>/students/registration/`, test।
- **পরিবর্তন:** একক student-এর command, যা T2, T3, T5, T7-এর rule ও T10 reuse করে। `Idempotency-Key`। Flagged duplicate একই তিনটি সিদ্ধান্ত পায়। আলাদা কোনো student store নেই। Reactivated ও number ছাড়ার নিয়মও একই (TD-01)।
- **Acceptance:** Double submit একটি student বানায়। একই data import-এর মতোই একই duplicate result দেয়। Student শুরুর তারিখ থেকে roster-এ থাকে।
- **Verification:** Integration test; fixture-এ E2E।
- **নির্ভরতা:** T2, T3, T4, T5, T7, T10।
- **সিদ্ধান্ত:** OD-01, OD-02, OD-03, OD-05, OD-07, OD-11, OD-12। খোলা: TD-04 (CX-14)।

### T14 · Web UI: import wizard (S02–S08)

- **Design link:** DR-03 থেকে DR-08, DR-17, DR-18; CMP-01 থেকে CMP-16, CMP-19 থেকে CMP-22; P0-1 থেকে P0-5, P0-9, P0-10।
- **Feature ID:** PLT-014, SIS-001, SIS-002; dependency PLT-015, PLT-016, PLT-018।
- **Proposed paths:** `<web>/office/students/import/`।
- **পরিবর্তন:** Wizard, preview table, correction drawer, duplicate compare, confirm, result ও guardian access tab। Design plan-এর অংশ ১৩-র সব state সহ। System 8-এর shell ও DVL-003-এর token থাকলে সেগুলো ব্যবহার হয়, না থাকলে design plan-এর Proposed token। S08 এই task-এর; T15 শুধু সেখান থেকে link নেয়। Access-এর ছয়টি status। Phone-এ batch page থেকে S06-এ যাওয়া ও ফেরা, confirm বড় পর্দায় (design §১১)। School বদলালে client cache খালি। শুধু CSV, template ও ৫,০০০ row / ১০ MB-এর সীমা (OD-08, TD-02)।
- **Acceptance:** Design P0-1 থেকে P0-5 ও P0-9-এর on-screen criteria। শুধু keyboard দিয়ে একটি run। State table-এর প্রতিটি row-তে পৌঁছানো যায়।
- **Verification:** Component test; fixture-এ E2E; অংশ ১৬-র accessibility check।
- **নির্ভরতা:** T6–T10।
- **সিদ্ধান্ত:** OD-03, OD-04, OD-07, OD-08, OD-09, TD-03।

### T15 · Web UI: import history, undo ও roster panel (S09, S10, S13)

- **Design link:** DR-10, DR-11, DR-17, DR-18, DR-19; CMP-06 থেকে CMP-08, CMP-10, CMP-11, CMP-15 থেকে CMP-18, CMP-22; P0-6, P0-7, P0-10।
- **Feature ID:** PLT-014, PLT-006; dependency CLS-001।
- **Proposed paths:** `<web>/office/students/import/`, `<web>/office/students/roster-check/`।
- **পরিবর্তন:** History list (actor, সময়, action ও undo-র কারণ), undo-র impact preview ও result, “Waiting — retry”-এর retry, আর ৩০ দিন পরে Undo বন্ধের অবস্থা (OD-06), read-only roster panel। Batch detail (S08) T14-এর।
- **Acceptance:** Design P0-6 ও P0-7-এর criteria পর্দায়। Design §১৩-এর S09, S10 ও S13-এর প্রতিটি state-এ পৌঁছানো যায়। History-তে undo-র actor, সময় ও কারণ দেখা যায়।
- **Verification:** Component test; E2E।
- **নির্ভরতা:** T11, T12।
- **সিদ্ধান্ত:** OD-06, OD-09।

### T16 · Web UI: quick registration ও student profile (S11, S12, minimal S01)

- **Design link:** DR-12, DR-13, DR-17, DR-18; CMP-01, CMP-07, CMP-08, CMP-12 থেকে CMP-14, CMP-17, CMP-22; P0-5, P0-8, P0-10।
- **Feature ID:** ADM-021, SIS-001, SIS-002।
- **Proposed paths:** `<web>/office/students/`।
- **পরিবর্তন:** Registration form, Household ও Placement সহ profile, এবং শুধু navigation-এর জন্য একটি minimal student list। পূর্ণ search ও filter (SIS-010) পরের branch-এ। Household-এ contact যোগ ও sibling link (design A-15)। Profile-এর placement ও roster panel T5, T11 ও T15-এর component দিয়ে।
- **Acceptance:** Design P0-8-এর criteria পর্দায়। Design §১৩-এর S11 ও S12-এর প্রতিটি state-এ পৌঁছানো যায়।
- **Verification:** Component test; E2E।
- **নির্ভরতা:** T5, T11, T13, T15।
- **সিদ্ধান্ত:** OD-05, OD-09, OD-12।

### T17 · ১,০০০ pupil rehearsal ও reconciliation (V11, X-08)

- **Design link:** P0-1 থেকে P0-7; DR-03 থেকে DR-11।
- **Feature ID:** PLT-014, SIS-001, SIS-002; deliverable DVL-012, DVL-014।
- **Proposed paths:** `<tests>/fixtures/students/x08-1000/`, `<tests>/students/`-এর অধীনে একটি scripted run।
- **পরিবর্তন:** Synthetic file-এর generator এবং অংশ ১২-র দশটি stage-এর scripted run। §১২-এর targeted scenario fixture। DVL-012-এর evidence: source file reference, approved mapping version, active status মেলানো, migration owner (DVL-012-এর proposed owner Student Records and QA), accepted totals, review links, backup ও rollback point, মাপা timing (কোনো target দাবি ছাড়া)। ৩০ দিন পরে raw file মোছার পরেও DVL-012-এর evidence থাকে: total, mapping ও audit।
- **Acceptance:** প্রতি stage-এর total অংশ ১২-র table-এর সমান। School B-র count কখনো বদলায় না।
- **Verification:** Automated run। Evidence যায় DVL-012-এ (mapping, dry-run ও duplicate report, গৃহীত total, মাপা timing — কোনো target দাবি ছাড়া, rollback point) এবং DVL-014-এ।
- **নির্ভরতা:** T9–T12।
- **সিদ্ধান্ত:** TD-05।

### T18 · Isolation, permission, responsive ও accessibility check

- **Design link:** DR-01, DR-02, DR-17, DR-18, DR-19।
- **Feature ID:** ADM-021, SIS-001, SIS-002, PLT-014; dependency PLT-002, PLT-006, PLT-015, PLT-016, PLT-018, IAM-003, IAM-004, IAM-007।
- **Proposed paths:** `<tests>/students/` (T0-র পর ঠিক হবে)।
- **পরিবর্তন:** অংশ ১৬-র প্রতিটি check চালানো ও record করা। সঙ্গে: staging build-এ X08-1000 fixture দিয়ে Registrar-এর UI run (dry run → ৩০টি duplicate row keyboard ও screen reader দিয়ে → confirm → undo)। V01/E2E-08: দুই school-এই নিজের import সফল হওয়ার পরে অন্য school-এর API, file, search ও count deny, আর school বদলালে cache খালি। V02 slice: তিন child-এর আলাদা status (custody ও queue-তে থাকা notice System 2-এর SC-09-এ, Students fixture দেয়)। V04 slice: import, confirm ও undo-র audit history ও record version। এই slice-এ A8-এর নিজের কোনো approval ধাপ নেই, তাই self-approval check এখানে প্রযোজ্য নয়। V16: responsive ও assistive technology run, দুই calendar-এর date sample, native Amharic review (client language lead, Doc 06 input Pending)। এই slice-এ PDF নেই।
- **Acceptance:** প্রতিটি আবশ্যক check pass করে। কোনো আবশ্যক check fail করলে, evidence না থাকলে, বা critical বা high finding খোলা থাকলে sign-off হয় না।
- **Verification:** System Agent brief-এর evidence format-এ test result (test ID, Feature ID, base/PR, school/product/role, input, expected, actual, evidence link, status, reviewer)।
- **নির্ভরতা:** T14–T17।
- **সিদ্ধান্ত:** OD-09।

### T19 · Rollout প্রস্তুতি ও completion report

- **Design link:** DR-01 থেকে DR-19 (delivery)।
- **Feature ID:** ADM-021, SIS-001, SIS-002 (pilot অংশ), PLT-014।
- **Proposed paths:** নেই।
- **পরিবর্তন:** অংশ ১৫-র rollout ও rollback ধাপ আসল environment-এর জন্য লেখা; Integration CTO-র কাছে completion report। অংশ ১৫-র merge gate পূরণের প্রমাণ।
- **Acceptance:** Report-এ প্রতিটি Feature ID-র শেষ ও বাকি কাজ evidence link সহ। SIS-002-এর pilot অংশ ও R1 remainder আলাদা। কোনো আবশ্যক evidence না থাকলে report complete ধরা হয় না।
- **Verification:** Integration CTO report-এর দাবি PR-এর diff, test result ও tracker-এর সঙ্গে মিলিয়ে দেখেন ([CTO handoff](../../integration/CTO-HANDOFF.md)-এর নিয়মে)।
- **নির্ভরতা:** T18।
- **সিদ্ধান্ত:** OD-13, TD-02। খোলা: TD-06।

## ১১. যে scenario-গুলো ঢাকতে হবে

| Scenario | প্রত্যাশিত ফল | Task |
|---|---|---|
| Preview-তে invalid section | Row Error, কখনো Ready নয়; value mapping বা row correction-এ ঠিক হয়; কিছু লেখা হয় না | T7, T8, T14 |
| Preview ও confirm-এর মাঝে section invalid হয়ে গেল | ওই row Failed ও কারণ সহ; বাকি row এগোয়; ওই row-এর জন্য নতুন preview লাগে | T5, T9 |
| Confirm repeat, একই key ও body | আগের result ফেরে; count বদলায় না | T9 |
| Confirm repeat, একই key কিন্তু body বদলানো | Reject | T9 |
| একই file আবার upload | Warning। কার্যকর batch-এর row-এ Skip আগে থেকে বাছা। Undo হওয়া batch-এর row-এ Reactivate বা Skip। Registrar না বাছলে নতুন student নেই | T6, T7, T9 |
| Partial failure: কিছু row-এ A8-র write fail | ওই row Failed; বাকিগুলো থাকে; retry duplicate বানায় না | T9 |
| Partial failure: confirm মাঝপথে থেমে যায় | একই key দিয়ে আবার চালালে বাকি row হয়; শেষ হওয়া row আবার লেখা হয় না | T9 |
| Partial failure: মাঝপথে C3-র grant fail | Student ও placement থাকে; access Failed; retry | T10 |
| Existing student সহ undo (U1) | Student থাকে, অপরিবর্তিত | T12 |
| পরের placement সহ undo (U2) | Student ও দুটি placement-ই থাকে; row Conflict | T12 |
| পরের approved edit সহ undo (U3) | থাকে; Conflict | T12 |
| Undo repeat | একই result | T12 |
| ১,০০০ pupil | প্রতি stage-এ total মেলে | T17 |
| Confirm-এ সিদ্ধান্তহীন duplicate | ওই row Not imported (No decision) | T8, T9 |
| আগের কার্যকর batch-এর row, use existing | No change: নতুন placement, contact বা request নেই | T7, T9 |
| Undo হওয়া batch-এর row আবার | Reactivated, একই internal ID ও number | T2, T9 |
| এই year-এ অন্য section-এ placed | Not imported (Already placed elsewhere), কিছু লেখা হয় না | T5, T9 |
| Scope-এর বাইরের number | Details ছাড়া Needs review, link করা যায় না | T7, T8 |
| Student লেখার পরে, commit-এর আগে fault | ওই row-এর কিছুই থাকে না, event বা C3 request-ও না | T9 |
| Confirm চলাকালীন access প্রত্যাহার | চলতি row (বা chunk) শেষে থামে, Blocked — access changed। Revoked user-এর replay deny | T4, T9 |
| Confirm হয়ে যাওয়া batch, নতুন key | Conflict, count অপরিবর্তিত | T9 |
| Undo-তে C3-এর প্রত্যাহার ব্যর্থ | ওই row-এর কিছুই বদলায় না (Waiting — retry)। Retry-তে row ফেরে, link একবারই `ended` বা `cancelled` | T12 |
| Undo preview-এর পরে edit | ওই row Conflict | T12 |
| Confirm চলার সময় flag বন্ধ করে আবার চালু | Blocked — access changed। নিজে থেকে চলে না, নতুন confirm-এ বাকি row | T9 |
| Undo হওয়া number, ভিন্ন child | Number ছাড়া হয়, audit সহ। নতুন record Created | T2, T8, T9 |
| Year শুরুর আগের placement তারিখ | Error। চলতি year-এর আগের তারিখ সতর্কবার্তা সহ Ready | T5, T7 |
| ৫,০০০ row বা ১০ MB-এর বেশি file | Reject, কারণ সহ | T6 |
| Confirm-এর ৩০ দিন পরে undo | Reject, কারণ সহ, profile থেকে ঠিক করার পথ | T12, T15 |

## ১২. X08-1000 fixture ও reconciliation (Proposed)

এটি একটি **Proposed** synthetic fixture। এটি তিন system-এর shared fixture থেকে আলাদা (CX-07)। সব নাম synthetic। সংখ্যাগুলো এমনভাবে বাছা হয়েছে যাতে প্রতিটি total হাতে মেলানো যায়।

**শুরুর অবস্থা:** School A-তে 30 জন active student, number A-2001 থেকে A-2030। কারো 2026–27-এ placement নেই, কারো household contact নেই। 20 জন student number দিয়ে মিলবে, 10 জন নাম ও জন্মতারিখ দিয়ে। School B-তে 5 জন student, একজনের number `A-017`। প্রতিটি stage-এ School B-র student, placement, contact ও link অপরিবর্তিত থাকতে হবে। File-এর সব valid start date 2026-09-14, আর ঠিক করা 5টি তারিখও সেই দিন।

**১,০০০ row-এর file:**

| ধরন | Row | Row ও number |
|---|---|---|
| clean row | 940 | r1–r940, A-001–A-940 (r17 = A-017) |
| School A-র existing student-এর সঙ্গে একই student number | 20 | r941–r960, A-2001–A-2020 |
| School A-র existing student-এর সঙ্গে একই নাম ও জন্মতারিখ, ভিন্ন number | 10 | r961–r970, A-961–A-970, মেলে A-2021–A-2030-এর সঙ্গে |
| file-এর ভেতরে একই number-এর 5 জোড়া | 10 | r971–r980: A-971, A-973, A-975, A-977, A-979 প্রতিটি দুবার |
| অজানা section (2টি আলাদা ভুল value) | 10 | r981–r990: "Blu" 6টি, "Gren" 4টি |
| invalid বা খালি start date | 5 | r991–r995 |
| ভাঙা guardian reference | 5 | r996–r1000 |
| মোট | 1,000 | r1–r1000 |

Preview-তে Needs review 35 = 20 + 10 + 5 (দুই ধরনের duplicate ও ভাঙা guardian reference)। Error 25 = 10 + 10 + 5 (file-এর ভেতরের duplicate, অজানা section ও ভুল তারিখ)।

**Stage:**

| Stage | কাজ | প্রত্যাশিত ফল |
|---|---|---|
| 1. Dry run | — | Ready 940, Needs review 35, Error 25, Excluded 0। Persistent write 0। School A active student 30। |
| 2. Fix | 2টি class value mapping 10টি section row ঠিক করে; 5টি date ঠিক করা হয়; 5 জোড়ার প্রতিটি থেকে একটি row exclude; বাদ যায় r972, r974, r976, r978, r980 | Ready 960, Needs review 35, Error 0, Excluded 5। School A active student 30, domain write 0। |
| 3. Duplicate review | r941–r960 → use existing; r961–r965 → use existing (A-2021–A-2025); r966–r970 → create new; r996–r1000 → guardian contact ছাড়া import | 970 নতুন student, 25 existing-এর সঙ্গে linked, 5 বাদ। Needs review 0। r996–r1000-এ access request হবে না। |
| 4. Confirm, 12 row-এ C3 failure inject করে | — | Created 970, Reactivated 0, Linked to existing 25, Not imported 5, Failed 0। Placement 995। Access request: C3 নিয়েছে 978 (Pending, পরিচয় যাচাই বাকি), Failed 12, Not requested 5 (r996–r1000)। School A active student 1,000। |
| 5. Failed access request retry | — | C3 নিয়েছে 990, সবই Pending। এটি Active-এর সংখ্যা নয়, fixture-এ Active 0। Student এখনো 1,000। |
| 6. একই key দিয়ে confirm repeat | — | Scope আবার দেখে মূল receipt ফেরে (978 / 12)। Live অবস্থা 990 / 0। সব count অপরিবর্তিত। |
| 7. Roster check | — | সব সংশ্লিষ্ট section মিলিয়ে শুরুর তারিখে এই batch-এর 995 জন, আগের দিন 0। |
| 8. পরের পরিবর্তন | r212-এর পরের placement 6 · Green, 2026-09-21 থেকে (test fixture, SIS-009-এর অনুকরণ); r540-এর অনুমোদিত নাম-সংশোধন, 2026-09-18 | নতুন student 0। Placement history entry 996। |
| 9. Undo | — | Reversed 968, Kept (existing) 25, Conflict 2 (r212, r540), Not imported 5 — মোট 1,000। Placement: void 993 (968 + 25), batch-এর placement অক্ষত 2, পরের placement অক্ষত 1। Access: `cancelled` 988 (963 + 25, সবই তখনো pending), `ended` 0 (কোনো link Active ছিল না), অক্ষত 2 (r212, r540)। School A active student 32। শুরুর তারিখের roster-এ এই batch-এর 2 জন। School B অপরিবর্তিত। |
| 10. Undo repeat | — | অপরিবর্তিত। |

**দুটি identity যাচাই করতে হবে:**

- (a) প্রতিটি stage-এ row status-এর যোগফল 1,000।
- (b) School A active student = 30 + Created + Reactivated − Reversed।

**Targeted scenario fixture (Proposed)**

| ID | Setup | প্রত্যাশিত ফল |
|---|---|---|
| S-01 | ১০ row, একটি flagged row-এ সিদ্ধান্ত নেই | Created 9, Not imported 1 (No decision) |
| S-02 | Batch #14 কার্যকর অবস্থায় তার ১০টি row আবার upload | Needs review 10, Skip আগে থেকে বাছা। Confirm-এ Not imported 10। Use existing বাছলে Linked to existing 10, No change 10: নতুন placement, contact ও request 0 |
| S-03 | ১০ row-এর batch undo, তারপর একই ১০ row আবার | Reactivate বাছলে Reactivated 10: একই internal ID ও number, নতুন batch-এর origin-এ placement 10। “ভিন্ন child” বাছলে number ছাড়া হয় (audit), Created 10 |
| S-04 | ১টি row-এর existing student এই year-এ অন্য section-এ placed | Not imported 1 (Already placed elsewhere), ওই row-এর কিছুই লেখা হয় না |
| S-05 | ১০ row-এর ৫ নম্বর row-এ student লেখার পরে, commit-এর আগে fault | ওই row Failed। তার student, contact, placement, event ও C3 request 0। বাকি ৯ Created |
| S-06 | ১,০০০ row-এর confirm-এ worker ৪০০ row পরে থামে | একই key-এ বাকি ৬০০ হয়। প্রথম ৪০০ আবার লেখা হয় না। Total stage 4-এর মতো |
| S-07 | Confirm-এ ৩০০ row পরে Registrar-এর access প্রত্যাহার | ৩০০-তে থামে, Blocked — access changed। Revoked user-এর replay deny। অন্য অনুমোদিত Registrar নতুন confirm দিয়ে বাকি ৭০০ চালান |
| S-08 | Undo-তে C3 ৭টি row-এ প্রত্যাহারে ব্যর্থ | ৭টি Waiting — retry, তাদের কিছুই বদলায়নি, batch Partly undone। Retry-তে row ফেরে, প্রতিটি link একবারই `ended` বা `cancelled`। তারপর চূড়ান্ত total |
| S-09 | Undo preview-এর পরে ৩টি row-এ অনুমোদিত edit | Undo-তে ওই ৩টি Conflict (“changed since the preview”) |
| S-10 | Confirm হয়ে যাওয়া batch-এ নতুন key দিয়ে confirm | Conflict, বর্তমান অবস্থা সহ। Count অপরিবর্তিত |
| S-11 | উত্তর campus-এর Registrar-এর row-এ দক্ষিণ campus-এর student-এর number | Needs review, কোনো detail নয়। Skip বা নতুন number। Link-এর চেষ্টা reject |
| S-12 | Confirm-এর ৩১তম দিনে undo | Reject, কারণ সহ। কিছু বদলায় না |
| S-13 | ৫,০০১ row-এর file | Upload reject, কারণ সহ। Batch তৈরি হয় না |
| S-14 | Confirm-এর ৪০০ row পরে flag বন্ধ, তারপর চালু | Blocked — access changed। Flag চালু হলেও চলে না। নতুন confirm-এ বাকি ৬০০ |

## ১৩. Reuse

- Platform-এর school context, entitlement, audit (PLT-006), file service (PLT-013), outbox (INT-002), calendar ও language utility (PLT-015, PLT-016)।
- C3-র authorise।
- Picker-এর জন্য A2-র class data।
- System 8-এর shell।
- DVL-003-এর component।
- System 2-এর C3 link status ও request contract (TD-04, TD-08)।
- Import ও quick registration-এর জন্য একটাই validation engine।
- Duplicate review ও undo conflict-এর জন্য একটাই compare component।
- Batch-এর mapping version ও report DVL-012-এর evidence হিসেবে যায়।

কী আছে তা T0-তে নিশ্চিত হবে।

## ১৪. Migration risk

1. আগের backend-এ অন্য ID type সহ student বা guardian table থাকতে পারে। Mapping বা data migration T0-তে ঠিক হবে (TD-06)।
2. আগের guardian data contact ও access মিশিয়ে রাখতে পারে। A8-র contact ও C3-র grant-এ ভাগ করতে হবে, কাউকে ভুলে access না দিয়ে।
3. Pilot school-এর পুরনো data-য় একই number দুইজনের থাকতে পারে, অথবা আগে ব্যবহার হওয়া number আবার দেওয়া থাকতে পারে। Preview সেগুলো দেখায়। Number-এর নিয়ম OD-01।
4. CSV-র date column-এর জন্য ভুল calendar বাছলে প্রতিটি start date সরে যায়। Preview দুই calendar পাশাপাশি দেখায়।
5. CSV encoding, Amharic নাম ও phone format।
6. বড় batch memory-তে চাপ দিতে পারে ও lock ধরে রাখতে পারে। Row নিজের transaction-এ লেখা হয় এবং staging paged।
7. Event replay downstream-এ duplicate placement বানাতে পারে। Subscriber event ID দেখে repeat বাদ দেয় এবং contract test এটা ঢাকে।
8. Raw CSV-তে শিশুদের data থাকে। এটা scoped storage-এ থাকে, retention period এখনো ঠিক হয়নি (TD-02), এবং event বা log-এ কখনো যায় না।
9. Schema additive হবে, যদি T0-তে existing student বা guardian table না পাওয়া যায়। পাওয়া গেলে migration ও তার ফেরার পথ TD-06-এ ঠিক হবে।
10. Fake-এর evidence আসল integration evidence বলে ভুল হতে পারে। দুটো আলাদা record হয়।
11. A8 আর C3 দুই জায়গায় লেখা। সুপারিশকৃত পথে C3-এর command একই transaction-এ চলে, তাই আলাদা dual write নেই (TD-04)। CX-14-এ বিকল্প পথ বাছা হলে durable outbound record + (batch, row) key-এর idempotent command দিয়ে সামলানো হবে।
12. Undo ৩০ দিন (OD-06), আর raw CSV ও staged row ঠিক তখনই মোছা হয় (TD-02)। Undo-র জন্য origin reference লাগে, raw file নয়, তাই মোছার পরেও audit ও হিসাব থাকে।
13. Pilot cutover-এ প্রায় ৯৯০টা guardian invite। সমাধান ন্যূনতম pilot batch invite (System 2-এর PR-06, CX-13): project owner গৃহীত, client product owner-এর অনুমোদন বাকি। এটি C3-এর আলাদা, ইচ্ছাকৃত ধাপ। A8 শুধু pending link-এর তালিকা ও filter (batch, class, campus) দেয়। Import নিজে কোনো invite পাঠায় না।

## ১৫. Rollout ও rollback

**Rollout**

1. Merge order (OD-13-এর সিদ্ধান্ত): Platform → Identity 1a → A1/A2 slice 0 → Students। Students আগে test-এর fake-এর উপর বানায়।
2. **Merge gate:** (ক) updated base-এ real C1 ও C3 দিয়ে দুই school-এর run। (খ) Real A1/A2 class reference ও roster দিয়ে import → guardian link → প্রথম placement → roster (DEPENDENCIES)। কে দেবে তা OD-13 (সিদ্ধান্ত: System 4-এর slice 0)। Slice 0 merge না হওয়া পর্যন্ত এই gate Pending। (গ) X08-1000 rehearsal। (ঘ) T18-এর সব আবশ্যক check।
3. School Management entitlement ও school-wise feature flag (PLT-005)-এর পেছনে, আগে demo School A ও B-তে।
4. Pilot import [Doc 08](../../08-migration-launch-and-handover.md) মেনে: collect ও map, dry run ও review, rehearse ও reconcile, approved window-এ backup সহ cutover, evidence record। Migration owner DVL-012-এর owner (Student Records and QA)। Cutover-এর পরে role access (Students ও Identity) আর প্রথম attendance (A9 owner) যাচাই। Finance-এর opening balance Finance owner-এর কাজ, Students import-এ নেই।
5. Tracker-এ SIS-002-এর MVP slice evidence ও R1 remainder evidence আলাদা।

**Rollback**

- **Code:** Flag বন্ধ করলে নতুন batch, confirm ও undo শুরু হয় না। চলতি confirm ও undo চলতি row (বা chunk) শেষে Blocked — access changed হয়। Flag আবার চালু হলেও নিজে থেকে চলে না, বাকি row-এর জন্য নতুন confirm (আপনার সিদ্ধান্ত, অংশ ৫)। আগের signed build নতুন table ও অজানা event উপেক্ষা করে চলতে পারে কি না, V19 drill-এ দেখা হবে।
- **Data:** প্রথম পছন্দ batch undo, যা audited। Conflict যায় Registrar-এর কাছে। Database restore হয় শুধু Doc 08-এর incident runbook দিয়ে, আগে আলাদা environment-এ যাচাই করে।
- **Schema:** TD-06 মীমাংসা না হওয়া পর্যন্ত “শুধু additive” শর্তসাপেক্ষ। আপনার সিদ্ধান্ত ছাড়া rollback কোনো confirmed batch-এর data মুছবে না।
- **Contract:** C3 বা A2 contract বদলালে নতুন version ও review হয়, INTERFACES-এর মতো ("Contract বদলালে review")।

## ১৬. Responsive, accessibility ও regression check

- **Responsive:** Design plan-এর অংশ ১১ তিন width-এ (1024 px ও তার উপরে, 600–1023 px, 600 px-এর নিচে); text 200%-এ; table-ছাড়া content 320 px-এ পড়ার মতো।
- **Accessibility (WCAG 2.2 AA, V16):**
  - F1, F2 ও F6-এর শুধু keyboard দিয়ে run।
  - Progress ও count বদলানোর screen-reader ঘোষণা।
  - Step বদল ও drawer-এ focus handling।
  - Status শুধু রঙে নয়।
  - Type করে দেওয়া dual-calendar date।
  - Amharic text-এ ঠিক language tag।
- **Regression:**
  - প্রতিটি commit-এ tenant isolation suite (DVL-014)।
  - Permission test (V02 slice)।
  - প্রতিটি change-এ C1, C3 ও A2 fixture দিয়ে contract test।
  - Repeat-request test।
  - Integration base-এ existing Platform ও Identity test এখনো pass করে।
  - Subscriber-এর repeat-drop test।
  - Event বা log-এ raw CSV বা শিশুর নাম নেই, এই automated check।
  - Replay-তে scope check, আর access প্রত্যাহারের পরে deny।
  - Background কাজ access বা flag বদলালে Blocked হয়, নিজে থেকে আবার চলে না।
  - Undo-তে C3-কে শুধু তালিকার link ID যায়।
  - Undo-র reversal ও event একই transaction-এ।
  - School বদলালে cache খালি।
- **Copy:** English ও Amharic review; Ethiopian ও Gregorian date sample।

## ১৭. সিদ্ধান্ত

২৬ নভেম্বর ২০২৬-এ আপনি সব সুপারিশ মেনেছেন। OD-01 থেকে OD-13-এর সিদ্ধান্ত design plan-এর অংশ ১৭-এ। নিচের table দেখায় কোন সিদ্ধান্ত কোন task-এ বসেছে।

| সিদ্ধান্ত | যে task-এ বসেছে |
|---|---|
| OD-01 | T2, T7, T8, T9, T12, T13 |
| OD-02 | T7, T8, T13 |
| OD-03 | T9, T10, T12, T13, T14, T16 |
| OD-04 | T1, T10, T14, T16 |
| OD-05 | T5, T7, T8, T9, T13, T14, T15, T16 |
| OD-06 | T4, T12, T15, T17, T18, T19 |
| OD-07 | T1, T9, T10, T13, T14 |
| OD-08 | T6, T14 |
| OD-09 | T14, T15, T16, T18 |
| OD-10 | পরের SIS-006 branch |
| OD-11 | T7, T8, T13 |
| OD-12 | T3, T13, T16 |
| OD-13 | T1, T5, T11, T19, অংশ ১৫-এর merge gate |
| System 1-এর OD-7: flag বা access বদলালে Blocked, নতুন confirm (অংশ ৫) | T4, T9, T10, T12, T14, অংশ ১৫-এর rollback |

**Technical সিদ্ধান্ত**

| ID | বিষয় | সিদ্ধান্ত (২৬ নভেম্বর ২০২৬) | প্রভাবিত task | বাকি |
|---|---|---|---|---|
| TD-01 | Undo-র পরে student number | Number undo হওয়া record-এর সঙ্গে থাকে। একই number আবার এলে Registrar compare দেখেন: একই child হলে Reactivate, ভিন্ন child হলে number ছেড়ে নতুন record, audit সহ | T2, T7, T8, T9, T12, T13 | — |
| TD-02 | File-এর সীমা ও রাখার সময় | এক file-এ সর্বোচ্চ ৫,০০০ row ও ১০ MB। Raw CSV ও staged row confirm-এর ৩০ দিন পরে মোছা। Batch-এর হিসাব, mapping ও audit থাকে | T6, T14, T17, T19 | Counsel review (D-03) এলে বদলাতে পারে |
| TD-03 | Confirm ও undo কীভাবে চলে | Background job (Blueprint পৃ. ২৩), page ছেড়ে গেলেও চলে | T9, T12, T14, T18 | — |
| TD-04 | C3-এর command কোথায় চলে | সুপারিশ, আপনি মেনেছেন, Identity-র T-00-এর শর্তে: row-এর transaction-এর ভেতরে (Blueprint পৃ. ১৫)। Confirm-এ savepoint সহ, যাতে C3 ব্যর্থ হলেও row থাকে, access Failed ও পরে idempotent retry। Undo-তে সব-বা-কিছুই-না | T1, T9, T10, T12, T13 | CX-14: Identity-র T-00-এর পরে Identity ও Students owner, তারপর আপনি। বিকল্প: durable outbound record + (batch, row) key-এর idempotent command |
| TD-05 | Matching-এর আগে নাম ও phone গোছানো | অতিরিক্ত space বাদ, Unicode NFC, Latin অক্ষর ছোট হাতের, phone +251 format-এ | T7, T8, T13, T17 | — |
| TD-06 | আগের student বা guardian table | A8-এর নিয়ম মানলে reuse, না মানলে mapping সহ migrate | T0, T2, T3, T5, T19; অংশ ১৪–১৫ | T0-তে code দেখে ঠিক হবে |
| TD-07 | Undo-র আগে অন্য module-এর record দেখা | Owner module-কে সরাসরি জিজ্ঞেস। উত্তর না পেলে row “Waiting — retry” | T1, T12 | প্রতিটি consumer live হওয়ার আগে নিজের check দেবে |
| TD-08 | C3-এ link প্রত্যাহার | Link ID + origin + expected version ধরে, batch ধরে নয় | T1, T12 | — (System 2-এ owner গৃহীত, CX-12) |

## ১৮. পরের branch-গুলো (শুধু outline)

| Feature | Design plan-এর ref | Note |
|---|---|---|
| SIS-007 history view | P1-1 | — |
| SIS-010 search ও filter | P1-2 | প্রথম branch-এর S01 শুধু navigation |
| SIS-016 list ও audit সহ export | P1-3 | `students.export` C3-এর T-01 catalogue-এ আগে থেকেই থাকে। প্রথম ব্যবহার ও V01 export evidence এই branch-এ |
| SIS-006 need-to-know flag | P2-1 | OD-10 ঠিক হয়েছে: Principal সিদ্ধান্ত দেন, Registrar শুধু flag-এর ধরন লেখেন |
| SIS-002 R1 remainder, pickup-contact coverage | — | Evidence আলাদা রাখা হয় |
| PLT-014 Excel (.xlsx) import | OD-08 | R1-এর ভেতরে, PLT-014 Verified হওয়ার আগে |

প্রতিটি নিজের ছোট branch পাবে, [Doc 02](../../02-mvp-release-and-branch-plan.md)-এর মতো। পূর্ণ admissions (ADM-001–020, 022, 023; R2) এবং A13 (R2-তে YRC-001–003, 005–007, 009, 010; R3-তে YRC-008) নিজেদের tracker release-এই থাকে ([features.csv](../../tracker/features.csv))।

## ১৯. Coding শুরুর নিয়ম

Plan approve করা মানে শুধু review sign-off। এতে coding-এর অনুমতি হয় না। আপনি implementation শুরু করতে বললে:

1. Agent আগে আপনাকে বলবে verified integration base থেকে একটি আলাদা feature branch তৈরি বা assign করতে। প্রস্তাবিত নাম `feat/students/registration-import`।
2. আপনি branch নিশ্চিত করে তাতে coding-এর অনুমতি না দেওয়া পর্যন্ত Agent অপেক্ষা করবে।
3. তারপর Agent branch যাচাই করবে এবং আলাদা worktree বা checkout-এ কাজ করবে।

Integration base এখনো Pending। T0-তে সেটি record না হওয়া পর্যন্ত branch তৈরি করা যাবে না।

## পরিবর্তনের রেকর্ড

**Review round 1 · ২৬ নভেম্বর ২০২৬।** Review report-এর findings যাচাই করে এই পরিবর্তনগুলো হয়েছে:

- T12: C3-কে link ID ধরে প্রত্যাহার, Kept (existing) row-এর এই batch-এর link-ও, Conflict row-এর access অক্ষত। Undo চলার সময় আবার যাচাই। Access withdrawal pending ও Waiting for check। U1–U7।
- T9, T10: durable request record, per-operation key, থেমে যাওয়া ও access প্রত্যাহারের পরে আবার শুরু, replay-তে scope check, batch revision।
- Re-import: কার্যকর batch-এর row No change, undo হওয়া row Reactivated (TD-01)।
- §৬: সব mutation-এ key ও version, route-action table, `students.household.edit`, `students.export` SIS-016-এ।
- §৯: undo-তেও same-transaction outbox, envelope, repeat ও পুরোনো version।
- §১১–১২: নতুন scenario, row ID ও number সহ fixture, targeted fixture S-01–S-11, stage-এর হিসাব আরও নির্দিষ্ট।
- T14–T19: Feature ID ও design link স্পষ্ট, S08 T14-এর, T16-এর নির্ভরতা, T18 ও T19-এর acceptance আরও কঠোর।
- Merge gate, cutover owner, flag বন্ধের আচরণ, TD-06-এর শর্ত।
- নতুন খোলা বিষয়: OD-11, OD-12, OD-13, TD-07, TD-08।
- System 1-এর Draft v2-এর queued-job guard খোলা cross-system বিষয় হিসেবে যোগ হয়েছে, মিল ও অমিলসহ (অংশ ৫)। এটি সিদ্ধান্ত নয়।

**আপনার সিদ্ধান্তের পরে · ২৬ নভেম্বর ২০২৬।** আপনি সব সুপারিশ মেনেছেন। এই পরিবর্তনগুলো হয়েছে:

- অংশ ১৭ এখন সিদ্ধান্তের তালিকা। প্রতিটি task-এ “সিদ্ধান্ত” line, আর যেটুকু খোলা তা আলাদা লেখা।
- TD-04: C3-এর command row-এর transaction-এ। Confirm-এ savepoint, undo-তে সব-বা-কিছুই-না। Durable outbound record ও “Sending” status বাদ।
- Undo-র অপেক্ষার অবস্থা একটাই: “Waiting — retry”। ৩০ দিনের সীমা (OD-06), U8 ও S-12।
- Flag বা access বদলালে “Blocked — access changed”, নিজে থেকে আবার চলে না, নতুন confirm (S-14)।
- Undo হওয়া number ভিন্ন child হলে ছাড়া (TD-01)। File-এর সীমা ও ৩০ দিনে মোছা (TD-02, S-13)। তারিখের নিয়ম (OD-05)।
- Merge-এর ক্রম Platform → Identity 1a → A1/A2 slice 0 → Students (OD-13)।
- নতুন ঝুঁকি: pilot-এ guardian invite-এর চাপ (অংশ ১৪)।
- System 1-এর Draft v3-এর চাওয়া অংশ ৫-এ মেলানো হয়েছে: নতুন job type, lease ও claim token, chunk checkpoint, (batch, row) key, `job_integrity` বার্তা, আর access মাঝে বন্ধ হয়ে ফিরলেও Blocked। কোনো অমিল নেই। T9 ও T12-এ বসানো।

**CTO-র cross-check-এর পরে · ২৬ নভেম্বর ২০২৬।** System 2-এর owner-গৃহীত cross-system item-এর সঙ্গে মেলাতে:

- TD-04 এখন সুপারিশ, Identity-র T-00-এর শর্তে (CX-14)। বিকল্পের নাম: durable outbound record + (batch, row) key-এর idempotent command।
- TD-08 বন্ধ: System 2-এ owner গৃহীত (CX-12)।
- প্রত্যাহারের ফল চারটি: `ended`, `cancelled`, `conflict`, `not_found`। Stage 9-এ pending link-এর ফল `cancelled` 988।
- A8 প্রতি row-এ link বা request ID ও শেষ জানা version রাখে, undo তা expected version হিসেবে পাঠায়।
- Pilot batch invite System 2-এর PR-06 (CX-13): C3-এর আলাদা ধাপ, A8 শুধু তালিকা ও filter দেয়।
- এক versioned shared fixture তিন system-এর জন্য (CX-07)। X08-1000 আলাদা।
- MVP-তে Registrar হাতে link শেষ করেন (CX-04)। `student.changed` System 2-এর জন্য cross-system বিষয়।
- access_changed-এ permission matrix-এর ঘর বদল। `students.export` T-01 catalogue-এ, ব্যবহার SIS-016-এ।
- CX-15: `student.changed` Proposed, owner Students ও Identity। প্রথম branch-এ C3 পড়ে না। Undo-র অপেক্ষার কারণের নাম “Access withdrawal pending”। Reactivation পুরোনো link ফেরায় না। Undo হওয়া student সাধারণ read-এ not-found (§৭, T2)।

**Owner-এর নিয়মের পরে · ২৬ নভেম্বর ২০২৬ (CTO-র মাধ্যমে)।** নিয়ম: শুধু Blueprint v2 PDF-এর ভেতরের কাজ, আর scope-এর দাবিতে Blueprint-এর পাতা।
- System 2-এর PR-05 (pupil activation code) বাদ গেছে। এই plan-এ ওই ধরনের কোনো কাজ ছিল না। §৭-এ স্পষ্ট লেখা হলো: Students কোনো pupil account বা activation code তৈরি করে না।
- System 3-এর PR-05 (saved mapping reuse, §১৮) আলাদা জিনিস। Blueprint-এর বাইরের proposal রাখা হবে কি না, আপনার উত্তরের অপেক্ষায়।
- §২: Blueprint PDF এখন পড়া যায়, উল্লেখ করা পাতাগুলো PDF থেকে মেলানো।

**Proposal বাদের পরে · ২৬ নভেম্বর ২০২৬ (owner-এর সিদ্ধান্ত, CTO-র মাধ্যমে)।**
- System 3-এর PR-01 থেকে PR-05 বাদ, কারণ কোনোটিই Blueprint v2-এ নেই। §১৮-এর “Saved mapping reuse” row সরানো।
- §৫-এর Protected download row-এ PR-01-এর উল্লেখ সরানো। এখন শুধু SIS-016-এর export (Blueprint পৃ. ৮১)।
- কোনো task বা acceptance criterion এই proposal-গুলোর ওপর নির্ভর করত না।
- অন্য system-এর proposal-এর সঙ্গে গোলমাল এড়াতে PR-06-কে “System 2-এর PR-06” লেখা হয়েছে। এই plan-এ নতুন proposal আসবে শুধু owner চাইলে, ID S3-PR-nn।

**Student count ও event বাদের পরে · ২৬ নভেম্বর ২০২৬ (CTO-র মাধ্যমে)।**
- Counts-only student aggregate বাদ। এর একমাত্র ব্যবহার ছিল System 1-এর revert panel, যা owner বাদ দিয়েছেন। §৫-এর table row ও §১-এর উল্লেখ সরানো।
- System 2-এর CX-15 reconciliation alert বাদ। C3 student-এর কোনো status event পড়ে না।
- Proposed `student.changed` বাদ। Blueprint পৃ. ১৩-এর Students-এর event তালিকায় এটি নেই (সেখানে আছে `student.enrolled`, `placement.changed`, `student.withdrawn`), আর কোনো module এটি পড়ত না। Undo-র ফল অন্য module জানে `placement.changed` দিয়ে (§৯)।

*বিস্তারিত screen ও state:* [Design plan](UI_UX_DESIGN_PLAN.md)। *QA ও acceptance:* [Doc 07](../../07-qa-pilot-and-acceptance.md)। *Contract:* [INTERFACES](../../integration/INTERFACES.md)।
