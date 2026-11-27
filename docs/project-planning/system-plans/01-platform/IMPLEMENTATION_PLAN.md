# Multi-tenant Platform — Implementation Plan

| বিষয় | মান |
|---|---|
| Status | Draft v4 · ২৬ নভেম্বরের owner সিদ্ধান্ত যাচাই করে যুক্ত · owner review-এর জন্য প্রস্তুত |
| তারিখ | ২৬ নভেম্বর ২০২৬ |
| Scope | C1 Products, plans and entitlements · C2 Operator console · C9 Platform services · C10 Security, privacy and reliability |
| প্রথম slice | PRD-001, PLT-005, PLT-001, PLT-002 (plan input SAS-002, console shell SAS-001) |
| Proposed first branch | `feat/platform/tenant-product-foundation` — এখনো তৈরি হয়নি |
| Integration base (planning repo) | **Verified**: `main` at `2f0b1e9`, `origin/main`-এর সঙ্গে একই, শুধু docs |
| Integration base (backend ও web) | **Pending (OD-1)** |
| সঙ্গের design plan | [UI_UX_DESIGN_PLAN.md](UI_UX_DESIGN_PLAN.md) |

**Label legend।** **Verified** = ২৬ নভেম্বর ২০২৬-এ এই planning session-এ checkout সরাসরি দেখে নিশ্চিত করা। **Observed** = checkout বা source doc-এ দেখা। **Assumption** = ধরে নেওয়া। **Proposed** = এই plan-এ নতুন, আপনার approval লাগবে। **Pending** = আপনার সিদ্ধান্ত বা repository verification-এর অপেক্ষায়।

---

## ১. এক নজরে

- এই plan [UI_UX_DESIGN_PLAN.md](UI_UX_DESIGN_PLAN.md)-এর design কীভাবে বানানো হবে, তার task ও verification ঠিক করে। Design আবার লেখা হয়নি; "Design §…" মানে ওই file-এর section।
- প্রথম slice P0-র ছয়টি feature: PLT-001, PRD-001, PLT-005, SAS-002, PLT-002, SAS-001 (console shell)। Identity ও Students এর উপর নির্ভর করে, তাই Platform আগে merge হয়।
- এই checkout-এ কোনো code নেই। তাই প্রতিটি path **Proposed** (Blueprint পৃ. ৪৮ layout) এবং প্রতিটি build ও test command **Pending (T0)**। T0 repository যাচাই করে path ও command ঠিক করে।
- ১৪টি task (T0–T13)। দুটি suite প্রথম branch-এই চলার পরিকল্পনা: V01 (isolation, §৮) ও V18 (toggle ও restore, §৯)। Platform-এর নিজের surface প্রথম branch-এ প্রমাণ করার পরিকল্পনা; এখনো কিছু চালানো হয়নি। file, search ও export-এর অংশ consumer আসার পর (§৮)।
- Open decision সাতটি (§১৫)। ২৬ নভেম্বর আপনি OD-2, OD-3 (Option A), OD-4, OD-5 (Option A), OD-6 ঠিক করেছেন ও OD-7 অনুমোদন করেছেন। শুধু OD-1 খোলা: backend এলে আপনি জানাবেন; OD-1 না হলে T0 ও তার পরের সব task আটকে থাকে।
- Decision প্রতিটি API request ও প্রতিটি job chunk-এ database থেকে পড়া হয়। কোনো decision cache নেই (§৩)। Event শুধু অন্য module-কে জানায়; decision event-এর উপর নির্ভর করে না।
- Evidence ধাপে ধাপে আসে: contract-ready (T1), API-ready (T5, T6), job-ready (T8), integrated (T13)। কোনো task নিজের স্তরের বেশি দাবি করে না।
- সব "Verified" আসলে docs-এর checkout সম্পর্কে। কোনো architecture, route, component, API বা test দেখা যায়নি, কারণ কিছুই নেই।

## ২. কী যাচাই হয়েছে, কী বাকি

### ২.১ Verified

এই planning session-এ ২৬ নভেম্বর ২০২৬-এ সরাসরি checkout দেখে যাচাই করা।

| বিষয় | যা দেখা গেছে |
|---|---|
| Checkout | `D:\nls-all-projects\Erp-lms`। `git ls-files` ১৮টি file দেখায়: root `README.md` ও `docs/project-planning/`-এর ১৭টি file। Source code, screen, route, component, API definition, test, package manifest ও CI config নেই। |
| Git | শুধু `main`। local `main` ও `origin/main` একই commit `2f0b1e9`। Remote repo নাম `desk-school-platform`। একটি worktree। `feat/platform/tenant-product-foundation` branch নেই। |
| Drift | `integration/CTO-HANDOFF.md` এখনো commit `febdc8f` লেখে। Checkout এখন `2f0b1e9`-এ। |
| Folder | এই planning session-এর note: `docs/project-planning/system-plans/` এই run-এর আগে ছিল না। এটি checkout থেকে পরে আবার যাচাই করা যায় না। |
| Design system | `DVL-003` (Core UI design system and tested prototypes) status Planned। কোনো token বা component file নেই। |
| Tracker | `tracker/features.csv`-এ "Multi-tenant Platform"-এর ৫৪ row। সবার Implementation status "Needs repository verification"; Acceptance evidence খালি। MVP ৩৪ (৩২ MVP candidate + ২ MVP partial: PRD-006, PLT-004); backlog ২০। |
| Tests (Doc 07) | V01 Tenant isolation, V18 Products ও plans। E2E-08 school isolation G1 full। E2E-09 G1 partial (operator-applied states), G2 full (expiry by rule)। |
| Roles (Doc 04) | Super Admin, Platform Support ও School Owner Platform screen সরাসরি ব্যবহার করেন। বিস্তারিত Design §৫। |

কোনো architecture, route, component, API বা test পরীক্ষা করা যায়নি, কারণ checkout-এ তা নেই। সেই পরীক্ষা **Pending** (T0)।

### ২.২ Blueprint baseline, repo-তে যাচাই হয়নি

নিচের সবই Blueprint v2-এর source baseline। কোনো repository-তে verify হয়নি।

| বিষয় | Baseline |
|---|---|
| Backend | Go modular monolith, তিন runtime role (API, worker, render); PostgreSQL with row-level security; Redis শুধু rate limit, short cache ও lock; S3-compatible object storage; transactional outbox ও PostgreSQL-backed job queue (Blueprint পৃ. ৬–৭, ১০–১১) |
| API | একটি versioned REST API `/v1`, OpenAPI 3.1; `Idempotency-Key`; record version; problem details (RFC 9457); cursor pagination; instant UTC (Blueprint পৃ. ৩৫–৩৬) |
| Web ও mobile | Next.js, React, TypeScript; operator console-সহ সব web portal এক component library-তে; Flutter দুটি app (Blueprint পৃ. ১০–১১) |
| Layout (Proposed) | `internal/platform/` (context, ids, money, dates, transactions, outbox, jobs), `internal/tenancy/` (schools, campuses, lifecycle, entitlements), `internal/subscriptions/`, `internal/audit/`, `internal/policy/`, `api/openapi/`, `api/events/`, `migrations/`, `tools/` (boundary policy, generators, isolation suite) (Blueprint পৃ. ৪৮) |
| Events | `school.provisioned`, `school.state_changed`, `entitlement.changed`; event-এ identifier ও fact, pupil content নয় |
| Service objective | server read p95 800 ms-এর নিচে; মাসিক availability 99.5% (Blueprint পৃ. ৪৪; Doc 07, V12) |

### ২.৩ T0-তে যা মিলিয়ে দেখতে হবে

কোনো task শুরুর আগে এই checklist বাস্তব repository-র সঙ্গে মেলাতে হবে।

1. Repository ও তার root।
2. Default ও integration branch ও commit।
3. Build ও test command; untouched base-এ green চালিয়ে।
4. Migration tool ও folder।
5. School/tenant column ও row-level security (যদি আগে থেকে থাকে)।
6. Session/auth middleware ও request context।
7. Job queue ও outbox।
8. Error format ও idempotency handling।
9. OpenAPI ও event schema-র জায়গা ও generator।
10. Web console app, component library ও token।
11. CI config ও required check।
12. বিদ্যমান fixture ও demo data।
13. Database ও library version (time-ordered UUID support)।
14. অন্য module-এর transaction-এর ভেতরে guard ডাকার transaction ও lock pattern; C3-এর `AuthorizeInTx` call-এর ধরন System 2-এর T-00-এর সঙ্গে মিলিয়ে।
15. Job queue-র claim pattern, lease ও claim token-সহ।
16. Test-only code কীভাবে production build-এর বাইরে রাখা হয়।
17. Database role ও grant pattern।

## ৩. Stack ও convention

- Repository-তে যা আছে তাই ব্যবহার হবে। Repository চুপ থাকলে Blueprint baseline (§২.২) অনুসরণ করা হবে: Go modular monolith, forced row-level security, Redis শুধু limit/short cache/lock, outbox ও database-backed job queue, `/v1` REST with OpenAPI 3.1, problem-details error, Idempotency-Key, record version, Next.js console এক component library-র উপর।
- **Module rule।** Platform code অন্য module-এর table পড়ে না, লেখে না। অন্য module Platform-এ পৌঁছায় তার interface বা event দিয়ে (Blueprint পৃ. ১৫, ৪৬)।
- **কোনো "admin API" নেই** যার অধিকার বেশি। Operator console দুই app-এর মতো একই API ও একই authorization service ব্যবহার করে (Blueprint পৃ. ৩৪)।
- Console UI English first, string code-এর বাইরে, Amharic-ready (Assumption A9, Design §২.২)। Design token Proposed, DVL-003 না হওয়া পর্যন্ত (Design §১০)।
- **P0-তে কোনো decision cache নেই (Proposed)।** প্রতিটি API request ও প্রতিটি job chunk নিজের transaction-এর ভেতরে database থেকে বর্তমান decision পড়ে। Decision কখনো cache বা event delivery-র উপর নির্ভর করে না; event শুধু অন্য module-কে জানায়। পরে decision cache যোগ হতে পারে শুধু মাপা কারণ থাকলে, আর তখনও প্রতিটি write নিজের transaction-এ আবার check করবে। School অনুযায়ী cache-key helper (T9) অন্য cache-এর জন্য থাকে।
- **Test-only code (Proposed)।** Test session, C3 stand-in শুধু test build-এ compile হয়। কোনো production build-এ এগুলো থাকলে CI check fail করে।

## ৪. প্রথম branch-এর সীমা

**ভেতরে:**

| Feature | প্রথম branch-এ কী |
|---|---|
| PLT-001 | Provisioning ও school profile। P0-তে campus শুধু provisioning-এর ভেতরে তৈরি হয় |
| PRD-001 | দুই product আলাদাভাবে চালু-বন্ধ |
| PLT-005 | Module flag |
| SAS-002 | Effective-plan input: plan → module set, effective time-সহ। Seeded plan, কোনো price নেই |
| PLT-002 | Platform-এর নিজের data, API, cache ও job-এর isolation এবং isolation suite harness |
| SAS-001 | এই screen-গুলোর জন্য যতটুকু console shell লাগে |
| — | C3 ও A8-র জন্য contract ও fixture |

**বাইরে (কোথায় যায়):**

| বিষয় | কোথায় |
|---|---|
| PLT-004 lifecycle transition (pilot অংশ) | P1 (OD-2, OD-3 ঠিক হয়েছে): pilot trial, active, restricted ও reactivation (restricted → active) ব্যবহার করে; grace, suspended ও offboarding বাকি R1-এ |
| PLT-006 audit store, PLT-007 approval engine, PLT-013 file service, SAS-005 demo school | P1 |
| PRD-006 pilot অংশ (plan price, manual invoice), SAS-004 | P1 |
| আসল Identity session ও role | System 2 |
| আসল student import | System 3 |
| আসল Desk Learn course | System 6 |
| PLT-019 search | R1 backlog |
| SAS-003 usage meter | R1 backlog |
| Provisioning-এর পরে campus যোগ করা | Pending: কে যোগ করবেন (operator না School Admin) এবং PLT-003 (A1, Academics)-এর সঙ্গে সম্পর্ক |

প্রথম branch billing, marks বা app-এর পূর্ণ acceptance দাবি করে না (SYSTEM-AGENT-BRIEFS "Release সীমা")। Tracker label ও release অপরিবর্তিত।

## ৫. Task-এর ক্রম

```
T0 -> T1 -> T2 -> T3 -> T4 -> T5 -> ( T6 | T8 )
T9 can start after T3 (runs beside T4 to T8)
T7 after T6 · T10 after T6, T8, T9 · T11 after T5, T6, T7, T8
T12 starts after T1, finishes after T6 · T13 after all
```

| ধাপ | কেন আগের ধাপের পরে |
|---|---|
| T0 | বাকি সব path ও command T0-র ফলের উপর নির্ভর করে |
| T1 | Contract আগে ঠিক না হলে C3 ও A8 owner একই জিনিস বোঝে না |
| T2 | School context ও transaction না থাকলে কোনো table নিরাপদ নয় |
| T3 | Schema context ও forced row-level security চায় |
| T4 | Entitlement model school ও campus table-এর উপর দাঁড়ায় |
| T5 | Decision resolver ছাড়া guard বানানো যায় না। Shared write guard এখানে থাকে, কারণ API write ও import confirm-এর ঠিকঠাক চলা T5-এর দায়িত্ব |
| T6, T8 | দুটিই T5 পরে পরস্পর নিরপেক্ষ, একসঙ্গে চলতে পারে। T8 T5-এর guard ব্যবহার করে |
| T9 | T2 ও T3 পরেই শুরু করা যায়; T4 থেকে T8-এর পাশাপাশি চলে |
| T7 | Event আসল operator command-এর transaction-এ লেখা হয় (T6) |
| T10 | Isolation suite আসল operator API (T6), job queue wrapper (T8) ও isolation helper (T9) চায়। T7 (event)-এর জন্য অপেক্ষা করে না। শুধু API ও job assertion |
| T11 | V18 suite decision ও API guard (T5), operator API (T6), event (T7) ও job queue wrapper (T8) চায়। শুধু API ও job assertion |
| T12 | Contract (T1) থেকে শুরু; পূর্ণ করতে operator API (T6) লাগে। সব browser assertion এই task-এর |
| T13 | সব result হাতে না এলে evidence ও report লেখা যায় না; T10, T11-এর API ও job result এবং T12-এর browser result এখানে মেলানো হয় |

## ৬. প্রথম slice-এর task (T0–T13)

প্রতিটি task-এর সব path **Proposed** (Blueprint পৃ. ৪৮ layout), কারণ checkout-এ code নেই। "Verified: কোনোটি নয়"। Command Pending (T0)। Design reference [UI_UX_DESIGN_PLAN.md](UI_UX_DESIGN_PLAN.md)-এর section।

### T0 · Repository ও integration base যাচাই

| Row | বিষয় |
|---|---|
| Design requirement | [Design §২.৩](UI_UX_DESIGN_PLAN.md) |
| Feature ID | প্রথম slice-এর সব |
| Paths | Verified: কোনোটি নয়। |
| Change | কোনো code নয়। §২.৩ checklist চালানো; T1–T13-এর প্রতিটি Proposed path যাচাই করা বা সংশোধিত path দিয়ে বদলানো; integration base branch ও commit লিখে রাখা। OD-1: বর্তমান docs-only `main` code base হবে কি না, সেটি আপনার সিদ্ধান্ত। এই repository-তে code নতুন করে শুরু হলে base এটাই; নইলে base হবে বিদ্যমান backend repository-র integration branch। |
| Acceptance | প্রতিটি path "Verified (exists)", "Verified parent (নতুন file, যে folder আছে এবং convention মেনে চলে)" অথবা সংশোধিত হিসেবে চিহ্নিত। যে file নেই তাকে কখনো "Verified" লেখা হয় না। Build ও test command লেখা এবং untouched base-এ pass। আপনি OD-1 নিশ্চিত করেছেন। |
| Verification | Command output ও commit hash এই plan-এ যুক্ত। |
| Depends on | OD-1 |
| Open decision | Pending (OD-1)। T0 অন্য সব task আটকায়। |

### T1 · Contract আগে (DVL-006)

| Row | বিষয় |
|---|---|
| Design requirement | [Design §১৪](UI_UX_DESIGN_PLAN.md) (বিশেষ করে §১৪.৫), §৩.৩ ও INTERFACES.md-এর "API ও event delivery" |
| Feature ID | PRD-001, PLT-005, PLT-001, PLT-002, SAS-002 |
| Paths | Verified: কোনোটি নয়। Proposed: `api/openapi/`, `api/events/` |
| Change | Operator endpoint ও tenancy event (`school.provisioned`, `entitlement.changed`, `school.state_changed`) বর্ণনা, envelope field `eventId, eventType, eventVersion, schoolId, recordId, recordVersion, occurredAt` এবং প্রযোজ্য হলে product ও effective date। `SchoolContext` ও `EntitlementDecision` সংজ্ঞা। এর সঙ্গে নিচের তিনটি: (a) তিন system-এর জন্য একটিই versioned shared fixture file, যা Platform T1-এ রাখে (CTO cross-check CX-07)। এতে থাকে: Design §১৪.৫-এর ১২টি decision row (প্রতিটিতে Platform expected, C3 expected, domain effect, channel (API / job / UI) ও evidence stage; ১২টিই বর্তমান সংখ্যা); School A ও School B, প্রতিটিতে একটি করে সফল control; দুই school-এ একই section label ও student number (`A-017`, section নীল/blue, System 3-এর row `r17`); এবং Identity-র Fixture A/B (তিনজন linked child ও একজন unlinked চতুর্থ sibling)। Version বদলালে Platform, Identity ও Students owner review করেন। (b) Design §১৪.২-এর response table (`product_off`, `module_off`, `school_read_only`, `school_suspended`, `not_found`, `version_conflict`, `access_changed`, `job_integrity`) contract-এর অংশ। (c) C3-র কাছে contract request: `AuthorizeInTx`, "confirmedAt-এর পর revoke হয়েছে কি?" (মাঝে বন্ধ হয়ে আবার চালু হওয়াও ধরা পড়তে হবে), account ও membership state, school switch-এ `session_ended` (OD-6, OD-7)। |
| Acceptance | Shared fixture file-এ প্রতিটি column ও অংশ আছে; তিন system একই file ও একই version ব্যবহার করে; Identity owner, Students owner ও Integration CTO সেগুলো দেখেছেন। Contract check pass। এটি contract-ready স্তর; fixture চালিয়ে প্রমাণ T5 থেকে। |
| Verification | CI-তে contract compatibility check (command Pending (T0)); review record-এর link। |
| Depends on | T0 |
| Open decision | OD-6 ঠিক হয়েছে (আসল binding System 2 merge-এ); OD-7: আপনার অনুমোদন আছে, Identity ও Students owner-এর সম্মতি Pending |

### T2 · Platform kernel: school context ও transaction

| Row | বিষয় |
|---|---|
| Design requirement | Design §১৪.২ ও §১৪.৪ (server-লেখা job row-ই context-এর একমাত্র অন্য উৎস); Blueprint পৃ. ১৫ (time-ordered UUID) |
| Feature ID | PLT-002 |
| Paths | Verified: কোনোটি নয়। Proposed: `internal/platform/` |
| Change | (আগে reuse দেখুন।) Context carrier: school শুধু verified session বা server-লেখা job row থেকে বসে, কখনো request parameter থেকে নয়। Transaction helper: প্রতি transaction-এ query-র আগে database-এর জন্য school বসায়। Context না থাকলে fail closed। Problem-details error, foreign ID-র জন্য "not found"-সহ। Idempotency-Key handling (একই key ও body-তে replay, ভিন্ন body reject)। Key-র scope (§৭.১): school user-এর জন্য (schoolId, actor membershipRef, operation); existing school-এর operator command-এর জন্য (operator accountRef, operation, target schoolId, এবং path-এ changeId থাকলে changeId); provisioning-এর জন্য (operator accountRef, `provision`)। অন্য school, অন্য target বা অন্য actor-এর একই key আলাদা key। Time-ordered UUID helper। Effective-time test-এর জন্য injectable clock। Test session ও stand-in শুধু test build-এ compile হয়; production build-এ থাকলে CI check fail করে (§৩)। |
| Acceptance | School context ছাড়া query fail করে, কখনো সব row ফেরত দেয় না। Client input থেকে context বানানোর কোনো path নেই (automated check)। Idempotent replay আসল response ফেরত দেয়। অন্য school, অন্য target বা অন্য actor থেকে একই key দিলে স্বাধীন ফল আসে। Production build-এ test-only code থাকলে CI fail করে। |
| Verification | আসল PostgreSQL-এর বিপরীতে unit ও transaction test (command Pending (T0))। |
| Depends on | T0, T1 |
| Open decision | OD-6 ঠিক হয়েছে: school switch-এ `session_ended`। test session Identity merge পর্যন্ত। Trade-off (System 2 জানিয়েছে): school user এক browser-এর দুই tab-এ দুটি school খোলা রাখতে পারবেন না; operator console প্রভাবিত নয়। |

### T3 · Tenancy schema: school ও campus

| Row | বিষয় |
|---|---|
| Design requirement | Design §৭.২, §২.১ ও §১৬ |
| Feature ID | PLT-001, PLT-002 |
| Paths | Verified: কোনোটি নয়। Proposed: `migrations/`, `internal/tenancy/` |
| Change | `school` (id, legal name, display name, unique short code, `city` (বাধ্যতামূলক; search, context bar ও confirmation-এ ব্যবহৃত), country, time zone, default language, default calendar, lifecycle state, version, created/updated metadata) ও `campus` (school id + id key, name, code)। Contact person Platform-এ profile contact হিসেবে থাকে (নাম, role label, phone, email); এরা account নয়, field ন্যূনতম। Owner ও School Admin account আসে C3 invite দিয়ে (IAM-002)। School-owned table-এ forced row-level security, Blueprint পৃ. ২৪-এর pattern অনুযায়ী। Operator path-এর জন্য আলাদা database role: তার policy শুধু tenancy table-এ সব school দেখে; কোনো school-owned pupil schema-তে grant নেই। Expand-only migration। Lifecycle column provisioning-এ প্রথম মান (trial বা active) পায়; তা বদলানোর command এই slice-এ নেই (PLT-004 pilot অংশ, P1 (OD-2, OD-3 ঠিক হয়েছে))। |
| Acceptance | দুই school একই নাম ও code-এর campus রাখতে পারে। A-র session B-র campus পড়লে 0 row। Automated check দেখায় school-owned table-এর 100%-এ forced row-level security। Operator database role-এর নিজের schema-র বাইরে কোনো privilege নেই। School session school table-এ অন্য school-এর row দেখতে পায় না এবং operator endpoint ডাকতে পারে না (404 `not_found`)। |
| Verification | খালি ও seeded database-এ migration up ও down; row-level security test; grant check (command Pending (T0))। |
| Depends on | T2 |
| Open decision | OD-2, OD-3 ঠিক হয়েছে; lifecycle command P1-এ, এই task-এ নেই। |

### T4 · Entitlement model: product, plan ও module flag

| Row | বিষয় |
|---|---|
| Design requirement | Design §৭.৩, §১৪.৫ |
| Feature ID | PRD-001, PLT-005, SAS-002 |
| Paths | Verified: কোনোটি নয়। Proposed: `internal/tenancy/`, `migrations/` |
| Change | দুই product: `school_management` ও `desk_learn`। প্রতি product-এর `plan` ও তার module set (seeded, এই slice-এ কোনো price নেই)। Module registry, `students.import`-এর মতো stable key; প্রতি consumer T1 contract দিয়ে নিজের key যোগ করে। Module key (যেমন `students.import`) Platform-এর entitlement-এর জন্য; C3-এর action আলাদা: `students.import.preview`, `students.import.confirm`, `students.import.undo`। `entitlement_change` append-only history (school, product বা module, before, after, effective time, reason, actor, scheduled cancellation হলে মূল change-এর reference)। Cancellation নতুন row, মূল history row অক্ষত থাকে। এর সঙ্গে version-সহ current-state row: school-এর (lifecycle), প্রতিটি product-এর এবং প্রতিটি module-এর। প্রতিটি history row-র একই transaction-এ এই row আপডেট হয়; T5-এর guard এগুলোই lock করে। Cancel শুধু effective time-এর আগে। কার্যকর change সংশোধনে বর্তমান version দিয়ে নতুন change তৈরি হয় (Design §৭.৫)। Resolver: "এই school-এর, এই product ও module-এর, এই instant-এ effective entitlement কী?" |
| Acceptance | Resolver T1-এর fixture row-গুলোর (১–১২) "Platform expected" column-এর সঙ্গে মেলে (C3 expected বা domain effect column নয়)। Effective instant-এর এক সেকেন্ড আগে পুরোনো উত্তর, ঠিক সেই instant-এ নতুন। বাতিল scheduled change কখনো apply হয় না। নতুন change দিয়ে আগের entitlement state ফিরিয়ে দিলে access ও record retention V18 অনুযায়ী মেলে। এক product বদলালে অন্যটির 0 উত্তর বদলায়। |
| Verification | Controlled clock-এ unit test; history table update ও delete refuse করে এমন test (command Pending (T0))। |
| Depends on | T3 |
| Open decision | — |

### T5 · Entitlement decision ও API guard

| Row | বিষয় |
|---|---|
| Design requirement | Design §১৪.২, §১৪.৫ |
| Feature ID | PRD-001, PLT-005, PLT-002 |
| Paths | Verified: কোনোটি নয়। Proposed: `internal/tenancy/` (decision interface), `internal/platform/` (HTTP guard)। Route registration-এর জায়গা Pending (T0)। |
| Change | `Decide(context, product, module, action)` `EntitlementDecision` ফেরত দেয়। প্রতিটি route-কে তার product, module ও action ঘোষণা করতে হবে বা platform-level চিহ্নিত হতে হবে; ঘোষণাহীন route check-এ fail করে, তাই default deny। প্রতিটি request নিজের transaction-এর ভেতরে database থেকে decision পড়ে। কোনো decision cache নেই (§৩)। উত্তর Design §১৪.২-এর table অনুযায়ী (§৭.১-এও আছে): বন্ধ বা এখনো effective নয় এমন product-এ 404 `product_off`; বন্ধ module-এ 404 `module_off`; restricted school-এ write 403 `school_read_only` ও suspended school-এ Owner ছাড়া 403 `school_suspended` (দুটোই OD-3: Option A; আচরণ P1-এ); body সব route-এ একই। **Shared write guard (Proposed; Design §১৪.৪)।** T5 API write ও import confirm-এর ঠিকঠাক চলার দায়িত্ব নেয়; T8 একই guard queue-র job chunk-এ ব্যবহার করে। (a) প্রতিটি guarded write transaction (API write, import confirm, job chunk) প্রথমে দরকারি সব lock নেয়, পুরো system-এ একটাই ক্রমে: Platform-এর row (school → product → module row, module key ascending), তারপর C3-এর row, C3-এর নির্দিষ্ট ক্রমে: account → membership → role grant (grant ID ascending) → guardian link (link ID ascending) → custody restriction row (ID ascending); C3-এর permission matrix কখনো lock হয় না, তার version immutable ও effective time-সহ। Lock commit পর্যন্ত ধরা থাকে (shared row lock বা school ও product দিয়ে advisory lock; T0-তে ঠিক হয়)। (b) তারপর database থেকে একটি authoritative সময় ও বর্তমান version পড়ে; এটিই "decision instant"। (c) তারপর decide করে। Effective time T-র scheduled change-এ: decision instant T-র আগে হলে পুরোনো decision, T বা তার পরে হলে নতুন। Platform বা C3 refuse করলে কিছু লেখে না; refuse হলে confirm কোনো job row তৈরি করে না। (d) C3-র `AuthorizeInTx` একই transaction-এর ভেতরে ডাকা হয়। (e) Operator change command (product, module, plan, lifecycle, cancel) shared guard নেয় না। সে শুরু থেকেই দরকারি exclusive lock একই ক্রমে নেয় এবং history row-র একই transaction-এ current-state row আপডেট করে, তাই shared থেকে exclusive-এ upgrade কখনো হয় না। একই target-এ দুটি concurrent change হলে প্রথম commit জেতে; দ্বিতীয়টি, যে version দেখেছিল সেটি নিয়ে, 409 `version_conflict` পায়। Operator change command শুধু Platform-এর row lock করে, C3-এর কোনো row কখনো নয়; C3-এর change command-ও শুধু নিজের row, Platform-এর row কখনো নয়। lock inspection test-এ এই সীমা যাচাই হবে। (f) Guard ও change command, দুই দিকেই lock timeout থাকে; মান T0-তে ঠিক হয়। Guard-এর timeout হলে transaction কিছু না লিখে rollback হয়; change command-এর timeout হলে "running work is finishing — try again" বলে, একই key-তে retry নিরাপদ। (g) A8-র database role শুধু Platform-এর guard function ডাকতে পারে; Platform-এর table-এ grant নেই (Proposed; T0 বিদ্যমান pattern দেখে)। (h) C3-কে সমতুল্য দিতে হবে (OD-7): grant ও membership revoke-এর জন্য একই lock semantics-সহ `AuthorizeInTx`; "confirmedAt-এর পর revoke হয়েছে কি?" প্রশ্নের উত্তর, যাতে এই actor-এর relevant role, grant বা membership confirmedAt-এর পরে revoke বা suspend হয়ে আবার ফিরে এলেও ধরা পড়ে; এবং account ও membership state (locked account, suspended membership)। System 2-এর শর্ত (CTO cross-check CX-11): access-change-এর নতুন append-only history; account `locked` ও membership `suspended`; lock timeout ও ছোট chunk; lock ক্রম Platform row → C3 row; call-এর ধরন System 2-এর T-00 ও আমাদের T0-তে একসঙ্গে ঠিক হয়। System 2 নিশ্চিত করেছে (System 2-এর plan §৫.৫): revoked-since-confirmedAt-এর উত্তর revoke-এর পরে আবার দেওয়া ও suspend-এর পরে আবার চালু হওয়াও ধরে, database time-এর append-only history থেকে; job row-তে নতুন field লাগে না। |
| Acceptance | বন্ধ product-এর registered route-এর 100% §৭.১-এর body-সহ 404 `product_off` দেয় এবং 0 side effect। এক API instance দিয়ে commit হওয়া change পরের request-এ যেকোনো instance দেখে। Event delivery থামানো থাকলেও decision ঠিক থাকে। ঘোষণাহীন route CI-তে fail। T1-এর fixture file-এর যে row "API" চিহ্নিত, তা pass (T1 থেকে এখানে এসেছে)। Guard আগে Platform row lock করে, তারপর C3 check ডাকে (stand-in-এর call order-এ যাচাই)। যে API write বা chunk T-র আগে অপেক্ষা শুরু করে এবং T-র পরে lock পায়, সে নতুন state-এ decide করে। একই target-এ দুটি concurrent change-এ একটি সফল, একটি 409 `version_conflict`। Lock timeout ও একই key-তে retry, দুই দিকেই (guard ও change command) যাচাই। Refuse হওয়া confirm 0 job row তৈরি করে। A8-র database role Platform-এর table সরাসরি পড়তে গেলে refuse হয়। Platform-এর change command চলাকালে C3-এর কোনো row-তে তার lock থাকে না (lock inspection test)। |
| Verification | দুই-school fixture-সহ HTTP test; event delivery থামিয়ে ও একাধিক instance-এ test; দুই-connection-এর নির্ধারিত test (lock ক্রম, decision instant, concurrent change) (command Pending (T0))। |
| Depends on | T4 |
| Open decision | OD-6 ঠিক হয়েছে; Identity merge না হওয়া পর্যন্ত test session। OD-7 (C3-র in-transaction call): আপনার অনুমোদন আছে, Identity ও Students owner-এর সম্মতি Pending। |

### T6 · Operator API: provision, change, scheduled cancellation, history

| Row | বিষয় |
|---|---|
| Design requirement | Design §৭.১–§৭.৫ |
| Feature ID | PLT-001, PRD-001, PLT-005, SAS-002, SAS-001 |
| Paths | Verified: কোনোটি নয়। Proposed: `internal/tenancy/`, `api/openapi/` |
| Change | §৭.১-এর operator endpoints। Provisioning একটি transaction (school, campus, initial entitlement ও outbox event), Idempotency-Key সহ। সব operator state-changing request-এ reason লাগে (PLT-006)। বিদ্যমান record বদলাতে current version লাগে। Entitlement change-এ effective time রাখুন। Cancel শুধু future scheduled change-এর জন্য (SAS-002)। কার্যকর change ঠিক করতে বর্তমান state review করে নতুন change দিন। History cursor দিয়ে পড়ুন। School list-এ name, short code বা city দিয়ে search। Campus প্রথম slice-এ provisioning-এর ভেতরে তৈরি হয়। C3 operator role check প্রথম slice-এ test stand-in দিয়ে যাচাই হবে। Operator endpoint restricted database role-এ চলে। Change command T5-এর নিয়মে exclusive lock নেয়। |
| Acceptance | API-ready স্তর। একই key একই response দেয়, 0 নতুন row। একই key ভিন্ন body-তে reject। একই operator, key ও body School A ও School B-তে পাঠালে দুটি স্বাধীন ফল; একই school-এ দুটি change ID-তে একই key স্বাধীন। Reason অনুপস্থিত বা blank হলে command refuse হয়, 0 write। Duplicate short code field error পায় এবং error existing school-এর নাম বলে। জোর করে ঘটানো failure-এ 0 orphan row থাকে। Stale version-এ 409 `version_conflict`; body-তে শুধু target-এর current `version`, current state value ও `effectiveAt` থাকতে পারে (tenancy data, pupil data নয়)। একই target-এ দুটি concurrent change-এ একটি সফল, একটি 409। Change command-এর lock timeout-এ "running work is finishing" বার্তা এবং একই key-তে retry ঠিক একবার সফল হয়। Path-এ অন্য school-এর change ID দিলে 404 এবং 0 write। কার্যকর scheduled change cancel করা যায় না। Result unknown হলে একই key দিয়ে recovery। Operator response-এ 0 pupil field। Keyboard criterion T12-তে গেছে, event criterion T7-এ; integrated PLT-001 result T13-এ লেখা হয়। |
| Verification | API test, idempotency test, conflict test (command Pending (T0))। |
| Depends on | T4, T5 |
| Open decision | OD-4 ঠিক হয়েছে: pilot এক Super Admin দিয়ে চলতে পারে (MFA ও প্রতি change-এ reason)। দুই বা তার বেশি operator হলেই live school-এ access কমানো change-এ PLT-007 দিয়ে দ্বিতীয় approver লাগে, তাই P1-c তার আগে merge হতে হবে। প্রথম branch শুধু staging-এ demo school নিয়ে চলে, তাই T6 এর জন্য অপেক্ষা করে না (§১০, P1-c)। OD-6 ঠিক হয়েছে। |

### T7 · Outbox event

| Row | বিষয় |
|---|---|
| Design requirement | Design §৩.৩, §১৪ ও INTERFACES.md-এর "API ও event delivery" (delivery ও replay) |
| Feature ID | PLT-001, PRD-001 |
| Paths | Verified: কোনোটি নয়। Proposed: `internal/platform/` (outbox; আগে থেকে থাকলে reuse), `api/events/` |
| Change | `school.provisioned` ও `entitlement.changed` data-র একই transaction-এ লেখা। `school.state_changed`-এর schema এখন সংজ্ঞায়িত, emit হবে P1 lifecycle slice-এ। Envelope T1-এর মতো। Pupil content নেই। Scheduled change-এর event change তৈরি হওয়ার সময়েই যায়, effective time-সহ; cancel হলে আরেকটি event যায়। Consumer সেই effective time মেনে চলে (Proposed)। |
| Acceptance | প্রতি change-এ ঠিক একটি event; প্রতি school-এ ঠিক একটি `school.provisioned`। Data ও event একসঙ্গে commit বা rollback। Replay একই event ID পৌঁছে দেয়। Decision কখনো event delivery-র জন্য অপেক্ষা করে না (§৩)। |
| Verification | Data লেখার পর জোর করে failure ঘটিয়ে দেখা যে কোনোটিই stored হয়নি; schema validation; পুনরাবৃত্ত event ID উপেক্ষা করে এমন sample subscriber (command Pending (T0))। |
| Depends on | T6 |
| Open decision | — |

### T8 · Job guard: queue-এর কাজ চালানোর আগে আবার check

| Row | বিষয় |
|---|---|
| Design requirement | Design §১৪.৪ |
| Feature ID | PLT-002, PRD-001, PLT-005 |
| Paths | Verified: কোনোটি নয়। Proposed: `internal/platform/` (jobs), `internal/tenancy/` |
| Change | Queue wrapper (Proposed; Design §১৪.৪)। T8 T5-এর shared write guard (lock ক্রম, decision instant, C3-র in-transaction call) ব্যবহার করে; guard-এর নিয়ম T5-এ। (a) Guard field ও `job_integrity`: confirm-এর সময় server job row-তে guard field লেখে, পরে কখনো বদলায় না: schoolId, product, module/action, actor-এর accountRef ও membershipRef, target scope (campusId, batchId), confirmedAt, idempotency key। Worker context ও decision শুধু এই field থেকে নেয়; payload school, actor বা target-এ না মিললে job `failed` হয় `job_integrity` নিয়ে, কিছু লেখে না। (b) Job type: guarded job একটি নতুন job type পায় (যেমন `students.import.v2`) যা পুরোনো worker চেনে না, তাই rollback-এর পরের পুরোনো binary এই job কখনো claim করে না। (c) Atomic claim, lease ও token: শুধু `queued` status-এর job claim হয়, ঠিক একজন worker দিয়ে। Claim status `running` বসায় এবং lease-এর মেয়াদ ও নতুন claim token দেয়; worker lease নবায়ন করে। Lease মেয়াদ পেরোলে job `queued`-এ ফেরে এবং পরের claim-এ নতুন token পায়। `done`, `blocked`, `failed` terminal; redelivery বা দ্বিতীয় worker terminal job পেলে কিছু করে না। Claim runnable status-এর allow-list থেকে নেয়, তাই rollback-এর পরের পুরোনো binary অচেনা status claim করে না। (d) Fencing: প্রতিটি chunk transaction যাচাই করে job-এর বর্তমান claim token worker-এর token-এর সমান কি না; নইলে rollback হয় ও কিছু লেখে না, তাই reclaim-এর পরে থেমে থাকা পুরোনো worker আর লিখতে পারে না। (e) Chunk checkpoint: প্রতিটি chunk নিজের write-এর একই transaction-এ সম্পন্ন হওয়া লেখে, তাই crash-এর আগে বা acknowledgement হারানোর আগে commit হওয়া chunk recovery-তে skip হয় (Platform-এর job framework-এর helper; A8 ব্যবহার করে)। ওই chunk-এর C3 grant write একই transaction-এ হয়, অথবা (batch, row) key-এর idempotent command হিসেবে (CTO cross-check CX-14: একই transaction-এ link request recommended, নইলে durable outbound record ও idempotent command; Identity-র T-00-এর আগে সিদ্ধান্ত হবে না)। (f) `access_changed`: প্রতিটি chunk-এ T5-এর guard চলে। আগে বর্তমান decision (Platform ও C3) হিসাব হয়; refuse করলে সেই reason লেখা হয় (Platform-এর বা C3-র)। Allow করলেও, confirmedAt ও chunk-এর decision instant-এর মাঝে (১) এই school-এর lifecycle, এই product (plan-সহ) বা এই module-এর কোনো change effective হয়ে থাকলে (off-এর পরে on হলেও), অথবা (২) C3-র access-change history দেখালে যে এই actor-এর relevant role, grant বা membership revoke বা suspend হয়েছিল (পরে ফিরে এলেও), অথবা (৩) C3-এর permission matrix-এ এই role × action cell-এর নতুন version কার্যকর হয়ে থাকলে (System 2-এর প্রস্তাব, গৃহীত), chunk `access_changed` দিয়ে refuse হয়, job blocked। Effective না হওয়া বাতিল schedule গোনা হয় না। কিছু নিজে থেকে আবার চলে না (OD-5)। (g) Chunk ছোট রাখা হয় যাতে change command বেশি অপেক্ষা না করে; chunk size A8-র, Students owner-এর সঙ্গে Pending। Chunk-এর lock timeout হলে transaction কিছু না লিখে rollback হয় এবং পরে নিরাপদে আবার চেষ্টা হয়। (h) শুধু session মেয়াদ শেষ হলে job থামে না: job accountRef + membershipRef ধরে চলে, session token ধরে নয়। Account lock বা membership suspend পরের chunk-এ job থামায় (C3 rule, OD-7)। Queue name school অনুযায়ী namespaced (পূর্ণ fair use SAS-013, P2)। |
| Acceptance | Job-ready স্তর। §৯.১-এর P0 চিহ্নিত প্রতিটি variant pass। Chunk-এর lock timeout হলে সে 0 row লিখে rollback হয়। Lock ক্রম ও decision instant-এর ফল T5-এর দুই-connection test-এ যাচাই হয়; এখানে chunk-এর দিকে একই ফল আসে। একই job দুই worker claim করলে ঠিক একটি claim পায়। Worker crash-এর পরে lease মেয়াদ পেরোলে job নতুন token-সহ reclaim হয়, পুরোনো worker-এর দেরিতে আসা chunk token check-এ refuse হয় এবং commit হওয়া chunk আবার চলে না। Commit-এর পরে acknowledgement হারালে ওই chunk skip হয়। Redeliver হওয়া terminal বা blocked job 0 claim ও 0 row। Tamper করা payload `job_integrity` পায়, 0 write। Guarded job-এর নতুন type পুরোনো worker claim করে না (rollback drill, §১৩)। Refused chunk তিনটি fixture effect-এ 0 row লেখে: fixture student table, fixture placement table ও C3 grant stand-in-এর call log। Reason record হয়; operator শুধু count দেখেন। |
| Verification | একটি fixture job যা fixture school-owned table-এ লিখতে চায়, তা নিয়ে integration test (command Pending (T0))। আসল A8 import job পরে Students integration run-এ যাচাই হবে; সেই evidence আলাদা রাখা হয়। |
| Depends on | T5 |
| Open decision | OD-5 ঠিক হয়েছে (Option A)। OD-6 ঠিক হয়েছে। OD-7: আপনার অনুমোদন আছে; Identity ও Students owner-এর সম্মতি Pending। |

### T9 · Isolation helper ও automated check

| Row | বিষয় |
|---|---|
| Design requirement | Design §১৪.১–§১৪.৩, §২.১ ও §১৬ (forced row-level security check) |
| Feature ID | PLT-002 |
| Paths | Verified: কোনোটি নয়। Proposed: `internal/platform/`, `tools/` |
| Change | Cache-key builder ও object-key builder, যারা school ছাড়া key বানাতে পারে না (file service নিজে PLT-013, P1)। Automated check: প্রতি school-owned table-এ forced row-level security আছে ও key-তে school আছে। Check যে repository code parameter থেকে school নেয় না (boundary linter থাকলে reuse)। |
| Acceptance | Row-level security ছাড়া fixture table যোগ করলে check fail। কোনো helper school ছাড়া key দেয় না। |
| Verification | Negative fixture-সহ CI run (command Pending (T0))। |
| Depends on | T2, T3 |
| Open decision | — |

### T10 · Isolation suite harness (V01)

| Row | বিষয় |
|---|---|
| Design requirement | Design §১৬ (PLT-002 criteria) |
| Feature ID | PLT-002, PLT-001 |
| Paths | Verified: কোনোটি নয়। Proposed: `tools/` (isolation suite), School A ও School B fixture |
| Change | Harness আসল operator API দিয়ে A ও B provision করে, প্রতি school-এর একটি session খোলে (Identity merge না হওয়া পর্যন্ত stand-in), প্রতিটি registered route অন্য school-এর session ও ID দিয়ে replay করে। Registration point আছে যাতে পরের module file, search, export, report ও job probe যোগ করে। Integration base-এ প্রতি commit-এ চলে (Blueprint পৃ. ২৬, ৪৮; DVL-014)। Positive control যোগ হয় (§৮-এর নতুন row 0): কোনো cross-school probe গোনা হয় না যতক্ষণ তার positive control pass না করে। এই task-এ শুধু API ও job assertion থাকে; browser assertion (school switch-এর console অংশ) T12-এর। |
| Acceptance | এই plan-এর §৮-এর প্রতিটি P0-তে চালানোর মতো API ও job row pass, row 0 সহ। ইচ্ছাকৃত leaky fixture route suite-কে fail করায়। |
| Verification | Doc 07 evidence format-এ CI run record। |
| Depends on | T6, T8, T9 (যেমন আগে ছিল; এখন §৫-এর সঙ্গে মেলে) |
| Open decision | OD-6 ঠিক হয়েছে; school switch-এর device-session test ও stand-in আসল binding পর্যন্ত Planned (§১৫) |

### T11 · V18 suite: toggle, effective time, restore

| Row | বিষয় |
|---|---|
| Design requirement | Design §১৬ (PRD-001, PLT-005, SAS-002 criteria) |
| Feature ID | PRD-001, PLT-005, SAS-002 |
| Paths | Verified: কোনোটি নয়। Proposed: tenancy module-এর পাশে test; `desk_learn`-এর অধীনে fixture route ও একটি fixture table (আসল course System 6-এর সঙ্গে আসে)। |
| Change | §৯-এর scenario test। এই task-এ শুধু API ও job assertion থাকে; browser assertion (console-এর module set, school context ও reason) T12-এর। |
| Acceptance | §৯ ও §৯.১-এর P0-তে চালানোর মতো প্রতিটি API ও job row pass (নতুন row-সহ)। |
| Verification | CI run record। |
| Depends on | T5, T6, T7, T8 |
| Open decision | §৯-এর restricted row ও variant e P1 (OD-2, OD-3 ঠিক হয়েছে); variant f OD-5-এর সিদ্ধান্ত (Option A) মেনে; OD-7 (variant c, d, d2, g, h3, i, j2, m): আপনার অনুমোদন আছে, Identity ও Students owner-এর সম্মতি Pending। |

### T12 · Operator console: প্রথম screen

| Row | বিষয় |
|---|---|
| Design requirement | Design §৬.১, §৭.১–§৭.৫, §৮.১–§৮.৮, §৯–§১৩ (শুধু P0 screen; §৮.৯ Lifecycle, §৮.১০ Plan & billing ও §৮.১১–§৮.১২ school-এর দিক পরে আসে) |
| Feature ID | SAS-001, PLT-001, PRD-001, PLT-005, SAS-002 |
| Paths | Web app path Pending (OD-1)। Blueprint baseline: এক component library-সহ Next.js monorepo (Proposed, verify হয়নি)। |
| Change | Console shell, schools list/search, school context bar, new school wizard, products and modules tab, change review drawer, reason-সহ confirm dialog, success panel এবং history থেকে ভবিষ্যতের scheduled change cancel। Design §৯-এর components ও §১০-এর tokens (DVL-003 pending)। Loading, empty, error, success ও result unknown state (Design §১৩)। English strings code-এর বাইরে। API client T1 contract থেকে। |
| Acceptance | Design §১৬-এর SAS-001 criteria ও wrong-school drills। Name, code বা city দিয়ে search। Browser tab title code দিয়ে শুরু। School বদলালে confirmation-এর পরে draft বাতিল। Keyboard দিয়ে wizard, change ও cancellation করা যায়। Confirm-এ name, short code, city, action ও reason দেখা যায়। Reason না থাকলে field error দেখায়। School switch-এর পর Back বা cache থেকে অন্য school-এর data আসে না (§৮ row 7)। Plan বদলালে console ও API একই module set দেখায় (§৯)। 320 px, 200% zoom ও screen reader check। Design §১৩-এর প্রতিটি P0 screen × state-এর প্রত্যাশিত ফল test-এ মেলে। |
| Verification | Component test; দুই school-এ provision → product change → নতুন change দিয়ে access ফেরানো end-to-end run; এই plan-এর §১৪-এর check (command Pending (T0))। |
| Depends on | T1 শুরুর জন্য, T6 শেষ করার জন্য |
| Open decision | Pending (OD-1) path-এর জন্য। OD-4 ঠিক হয়েছে (pilot-এ এক Super Admin; প্রতি change-এ reason)। |

### T13 · Evidence ও handoff

| Row | বিষয় |
|---|---|
| Design requirement | Design §১৬ ও Doc 07 result format |
| Feature ID | প্রথম slice-এর সব |
| Paths | Verified: কোনোটি নয়। Proposed: নতুন code path নেই। |
| Change | Doc 07 format-এ result: `Test ID \| Requirement/Feature ID \| Build বা PR \| Tenant ও role \| Expected \| Actual \| Evidence link \| Status \| Reviewer`। যা প্রমাণিত শুধু তার জন্য tracker status ও evidence update প্রস্তাব। Integration CTO-র জন্য Bangla completion report: Feature ID, PR, target branch, reviewed commit ও test evidence। Verified contract version Identity ও Students owner-কে দেওয়া। Slice evidence (fixture ও stand-in) আসল integration evidence থেকে আলাদা রাখা। প্রতিটি P0 criterion-এর integrated result লেখা হয়: T10 ও T11-এর API ও job result এবং T12-এর browser result একসঙ্গে মেলানো হয়, আর কোন task তার কোন অংশ প্রমাণ করেছে তা নাম ধরে লেখা হয়। Fixture, stand-in ও আসল integration evidence আলাদা থাকে। |
| Acceptance | Report আসল diff ও CI-র সঙ্গে মেলে। |
| Verification | Report, diff ও CI run পাশাপাশি মিলিয়ে দেখা। |
| Depends on | সব task |
| Open decision | — |

## ৭. API, schema ও auth — যেখানে দরকার

সবই **Proposed**। শুধু প্রথম slice যা চায়।

### ৭.১ API

| Endpoint | কাজ |
|---|---|
| `POST /v1/operator/schools` | Provision (Idempotency-Key ও reason) |
| `GET /v1/operator/schools` | List ও search, cursor |
| `GET /v1/operator/schools/{schoolId}` | Profile, campus, entitlement summary |
| `GET /v1/operator/schools/{schoolId}/entitlements` | বর্তমান ও scheduled |
| `POST /v1/operator/schools/{schoolId}/entitlement-changes` | Product, module বা plan change, current version ও reason সহ |
| `POST /v1/operator/schools/{schoolId}/entitlement-changes/{changeId}/cancel` | Future scheduled change cancel, current version ও reason সহ |
| `GET /v1/operator/schools/{schoolId}/changes` | History |
| `GET /v1/operator/plans` | Seeded plan catalogue পড়া (plan ও price তৈরি P1) |

- **Campus:** P0-তে campus শুধু provisioning-এর ভেতরে তৈরি হয়; আলাদা campus endpoint নেই। Provisioning-এর পরে কে campus যোগ করবেন, সেটি Pending (§১৫)।
- **Access না থাকলে উত্তর (Proposed, problem details RFC 9457)।** Body-তে শুধু `type`, `title`, `status`, `code`, `requestId` এবং user-এর ভাষায় message থাকে; কখনো record data নয়। একই product বা module-এর সব route-এ body একই। পূর্ণ বিবরণ Design §১৪.২।

| Situation | HTTP | code |
|---|---|---|
| product বন্ধ (এখনো effective নয়, তাও) | 404 | `product_off` |
| module বন্ধ | 404 | `module_off` |
| school restricted থাকতে write | 403 | `school_read_only` (OD-3: Option A; P1) |
| school suspended, caller Owner নন | 403 | `school_suspended` (OD-3: Option A; P1) |
| অন্য school-এর ID, বা missing record | 404 | `not_found` (status ও body সত্যিকারের missing record-এর মতোই) |
| school user operator endpoint ডাকে | 404 | `not_found` |
| পুরোনো record version | 409 | `version_conflict` (body-তে শুধু target-এর current `version`, current state value ও `effectiveAt`; tenancy data, কখনো pupil data নয়) |
| confirm-এর পরে access বদলেছে, এখন বন্ধ | job status `blocked` | বর্তমান reason (Platform-এর বা C3-র) |
| মাঝে বন্ধ হয়ে আবার চালু হয়েছে | job status `blocked` | `access_changed` |
| job payload server-লেখা guard field-এর সঙ্গে মেলে না | job status `failed` | `job_integrity` |

- **"Record data নেই" নিয়মটি access-denied উত্তরের জন্য।** `version_conflict` ব্যতিক্রম: সেখানে শুধু target-এর tenancy state থাকে।
- **API ও job আলাদা।** একই কারণে API ও job আলাদা ফল পায়: module বন্ধ → API 404 `module_off`, job `blocked` reason `module_off`; product বন্ধ → API 404 `product_off`, job `blocked` reason `product_off`; confirm-এর পরে বন্ধ হয়ে আবার চালু → job `blocked` reason `access_changed`।
- **Idempotency-Key-র scope:** school user-এর জন্য (schoolId, actor membershipRef, operation); existing school-এর operator command-এর জন্য (operator accountRef, operation, target schoolId, এবং path-এ changeId থাকলে changeId); provisioning-এর জন্য (operator accountRef, `provision`)। অন্য school, অন্য target বা অন্য actor-এর একই key আলাদা key, স্বাধীন ফল।
- **Download (Proposed, P1-d/PLT-013):** প্রতিটি protected download নামার মুহূর্তে বর্তমান permission ও entitlement যাচাই করে (INTERFACES: "current rights")। Download request API check দিয়ে যায়; API হয় file stream করে, নয়তো একবার-ব্যবহারযোগ্য link দেয় যা কয়েক সেকেন্ডের মধ্যে মেয়াদ হারায় (সঠিক মান PLT-013 slice-এ ঠিক হয়)। আগে নেওয়া link তার পরে কাজ করে না। দীর্ঘ মেয়াদের CDN link (যেমন পরে course video-র জন্য) MVP-র অংশ নয়; তার জন্য আপনার স্পষ্ট সিদ্ধান্ত লাগবে, যেখানে file-এর ধরন ও সর্বোচ্চ মেয়াদ নাম ধরে থাকবে। Device-এ নামিয়ে ফেলা content ফেরত আনা যায় না; সেটি server download থেকে আলাদা।
- **School-এর দিক:** user-এর নিজের context-এর entitlement অংশ, যাতে app workspace আঁকতে পারে। Platform interface দিয়ে দেয়। Endpoint-এর নাম ও owner C3-র সঙ্গে Pending।
- **Internal interface** (Blueprint baseline-এ Go): entitlement-এর জন্য `Decide`; A8 ও Academics-এর জন্য campus-reference check; `SchoolContext`।

### ৭.২ Schema

Tenancy: `school` (`city`-সহ), `campus`, profile contact (নাম, role label, phone, email; account নয়), `plan`, plan module set, `entitlement_change` (append-only)। এর সঙ্গে version-সহ current-state row: school (lifecycle), product ও module; প্রতিটি history row-র একই transaction-এ আপডেট হয় এবং T5-এর guard এগুলো lock করে। Job row-তে server-লেখা guard field (schoolId, product, module/action, actor accountRef ও membershipRef, target scope, confirmedAt, idempotency key), যা পরে বদলায় না (T8 a); এর সঙ্গে job type, claim status, lease মেয়াদ, claim token ও chunk checkpoint (T8 b–e)। Job table Platform-এর job framework-এর, `internal/platform/`-এ (Proposed)। Idempotency key ও outbox আগে থেকে থাকলে reuse। কোনো pupil data নেই।

এই slice-এ নয়: subscription invoice ও ledger (`internal/subscriptions/`, P1); lifecycle transition history (P1); audit PLT-006 (P1); approval PLT-007 (P1); file metadata PLT-013 (P1)।

### ৭.৩ Auth

Platform Identity-র ভেতরে কিছু বদলায় না। C3 থেকে interface দিয়ে এগুলো লাগে:

1. Verified session → membership (school ও actor)।
2. Operator role check।
3. Job-time re-check-এর জন্য `Authorize`, এবং guarded write transaction ও job chunk-এর ভেতরে `AuthorizeInTx` (grant ও membership revoke-এর জন্য একই lock semantics; T5)।
4. "confirmedAt-এর পর revoke হয়েছে কি?" প্রশ্নের উত্তর (মাঝে revoke হয়ে আবার ফিরে এলেও ধরা পড়ে; T8 f)।
5. Account ও membership state (locked account, suspended membership)।
6. School বদলালে ওই device-এর পুরোনো session শেষ (`session_ended`; Blueprint পৃ. ২৫)।

আইটেম ৬ OD-6 অনুযায়ী ঠিক হয়েছে (trade-off: school user এক browser-এর দুই tab-এ দুটি school খোলা রাখতে পারবেন না; operator console প্রভাবিত নয়)। আইটেম ৩–৫ OD-7: আপনার অনুমোদন আছে, Identity ও Students owner-এর সম্মতি Pending।

প্রথম slice এই interface test stand-in-সহ পাঠায়। আসল binding Identity merge হলে যাচাই হবে, সেই evidence আলাদা। কোনো নতুন token type নেই, কোনো wider-rights API নেই। Database দিকে: আলাদা role ও grant; operator role শুধু tenancy ও subscription schema-তে (OD-6 ঠিক হয়েছে)।

## ৮. V01 — cross-school isolation যাচাই

কোনো row এখনো চালানো হয়নি; এগুলো পরিকল্পিত পরীক্ষা।

| # | Probe | Setup | Expected | প্রথম branch-এ অবস্থা |
|---|---|---|---|---|
| 0 | Positive control | A-র fixture Registrar A-র fixture record পড়ে ও লেখে; B-র ক্ষেত্রেও একইভাবে B | প্রতিটি probe-এর জন্য A→A ও B→B সফল। কোনো cross-school probe তখনই গোনা হয় যখন তার positive control আগে pass করেছে | Planned — fixture |
| 1 | দুই school provision (E2E-08) | Operator A ও B তৈরি করেন, প্রতিটিতে "North" নামের campus | দুটি আলাদা school; একই campus নাম চলে | Planned — fixture |
| 2 | ID দিয়ে read | A-র session B-র campus ID চায় | Missing record-এর একই status ও body shape (404 `not_found`) | Planned — fixture |
| 3 | List | A-র session campus list করে | শুধু A-র | Planned — fixture |
| 4 | Foreign school-সহ write | A-র session body, query বা header-এ B-র school ID পাঠায় | Field গৃহীত হয় না বা উপেক্ষিত; B-তে কিছু তৈরি হয় না | Planned — fixture |
| 5 | Database | A-র context-এ WHERE ছাড়া query; এবং context ছাড়া query | শুধু A-র row; context না থাকলে fail closed | Planned — fixture |
| 6 | Cache-key helper | A একটি cached মান বসায়, B একই logical key চায় | B কখনো A-র মান পায় না | Planned — fixture |
| 7 | School switch | A ও B-তে membership থাকা একজন switch করেন | A-র পুরোনো token আবার ব্যবহার করলে, A-র যে record সেই token দিয়ে আগে সফলভাবে পড়া হয়েছিল তার জন্য, `session_ended` আসে; নতুন B session B পড়ে ও A-র ID-তে 404; cache বা Back থেকে A-র কোনো data আসে না। API অংশ T10-এর; browser অংশ (Back ও cache) T12-এর; app অংশ C5-র সঙ্গে | Planned — stand-in; real at System 2 merge। App অংশ: Pending — consumer |
| 8 | Job | A-র জন্য job queue-তে, data-তে B-র ID আছে | Worker context A থাকে; B অক্ষত | Planned — fixture |
| 9 | Operator | Operator session fixture pupil-data route ডাকে | Refuse; operator database role-এ grant নেই | Planned — fixture |
| 10 | Operator path-এ অন্য school-এর change ID | Operator A-র path-এ B-র change ID দেন | 404 এবং 0 write | Planned — fixture |
| 11 | Entitlement read | A-র session B-র entitlement চায় | Not found | Planned — fixture |
| 12 | Self-test | ইচ্ছাকৃত leaky fixture route | Suite fail করে | Planned — fixture |
| 13 | File link, search, export, report ও school screen | — | একই rule। Owner ও gate: PLT-013 — Platform P1-d; PLT-019 (R1 backlog) — Platform; C6-র report ও export — System 8; A8-র field-limited export — System 3; C5-র school screen — System 8। সবই G1-এর আগে | Pending — P1 (file: PLT-013); search PLT-019 R1 backlog; Pending — consumer (report, export, school screen)। Harness-এ registration point আগেই আছে |
| 14 | School session operator endpoint ডাকে | A-র school session `/v1/operator/...`-এর যেকোনো endpoint ডাকে | 404 `not_found` এবং 0 write | Planned — fixture |
| 15 | A8-র database role | A8-র role দিয়ে Platform-এর guard function ডাকা, এবং Platform-এর table সরাসরি পড়ার চেষ্টা | শুধু guard function ডাকা যায়; table সরাসরি পড়া refuse | Planned — fixture; real role A8 merge-এ (System 3) |
| 16 | File: আগে নেওয়া link, revoke-এর পরে | Link নেওয়ার পরে permission revoke; আগের link আবার ব্যবহার | Refuse (PLT-013-এর download নিয়ম, §৭.১) | Pending — P1 (file: PLT-013, P1-d); consumer-এর সঙ্গে |
| 17 | File: cross-school link | School A-র link School B-র session থেকে ব্যবহার | Missing file-এর মতো, not found | Pending — P1 (file: PLT-013, P1-d); consumer-এর সঙ্গে |
| 18 | File: import CSV | Import CSV-র file key ও event দেখা | CSV তার batch-এ বাঁধা থাকে; কোনো event-এ কখনো দেখা যায় না | Pending — P1 (file: PLT-013, P1-d); consumer-এর সঙ্গে |

V01-এর পূর্ণ pass condition (screen, API, file, search, report ও export) তাই দুই ধাপে প্রমাণ হয়: প্রথম branch Platform-এর নিজের surface ও harness প্রমাণ করে; বাকিটা consumer-এর সঙ্গে আসে। G1-এর জন্য সবটা লাগে।

## ৯. V18 — toggle ও restore যাচাই

কোনো row এখনো চালানো হয়নি; এগুলো পরিকল্পিত পরীক্ষা।

| # | Scenario | Expected | প্রথম branch-এ অবস্থা |
|---|---|---|---|
| 1 | A School-only | School Management fixture route ও job উত্তর দেয়; Desk Learn fixture route API-তে 404 `product_off` দেয়। School-এর menu C5-এর (P1-g) | Planned — fixture |
| 2 | B Learn-only | Desk Learn fixture route উত্তর দেয়; school-import fixture route API-তে 404 `product_off`, এবং ওই job `blocked` reason `product_off` | Planned — fixture |
| 3 | A both হয় | দুই product উত্তর দেয়; প্রতিটির নিজের role লাগে (শুধু Registrar grant থাকলে course তৈরি refuse) | Planned — stand-in; real at System 2 merge |
| 4 | Module flag বন্ধ (`students.import`) | ওই module-এর read ও write: API 404 `module_off`, job `blocked` reason `module_off`; অন্য module চলে | Planned — fixture |
| 5 | Effective time | Desk Learn T সময়ে চালুর schedule; API decision T-র এক সেকেন্ড আগে (T−1s) পুরোনো, T-তে নতুন। T-র আগে confirm হওয়া ও T-র পরে চলা job §৯.১-এর job guard নিয়ম মেনে চলে | Planned — fixture |
| 6 | Cancel | T-র আগে scheduled change বাতিল; T-তে উত্তর এখনো পুরোনো | Planned — fixture |
| 7 | বন্ধ, তারপর চালু (restore) | বন্ধ থাকতে ওই product-এর registered route-এর 100% সংজ্ঞায়িত body-সহ 404 `product_off` দেয়, fixture record একই থাকে; আবার চালুতে fixture record ID-র সেট ও content checksum আগের সঙ্গে হুবহু এক (শুধু count নয়) | Planned — fixture; "course ফেরে"-র আসল evidence Desk Learn (System 6)-এর সঙ্গে: Pending — consumer |
| 8 | Independence | Desk Learn বন্ধে School Management-এর 0 decision বদলায়, উল্টোটাও | Planned — fixture |
| 9 | Cache ছাড়া decision | Change-এর পর পরের request যেকোনো instance-এ নতুন উত্তর পায়; event delivery থামানো থাকলেও decision ঠিক | Planned — fixture |
| 10 | History field | প্রতিটি flag change-এর history entry-র actor, reason, effective time, before ও after field ধরে ধরে মেলানো | Planned — fixture |
| 11 | Plan → module set | Plan থেকে পাওয়া module set API decision ও job guard-এ একই উত্তর দেয়, এবং console একই set দেখায় (T12) | Planned — fixture |
| 12 | Stale tab | পুরোনো version-সহ change পাঠালে 409 `version_conflict` ও current state | Planned — fixture |
| 13 | Result unknown | Provisioning-এর উত্তর আসেনি; একই key আবার পাঠালে আসল result ফেরে, 0 duplicate | Planned — fixture |
| 14 | Wrong-school activation drill | B-তে ভুলে Desk Learn চালু এবং window-তে fixture grant/course তৈরি। B-এর পরিচয় ও current state দেখে reason-সহ নতুন change দিয়ে B বন্ধ। B-এর route 404 `product_off`, অথচ records থেকে যায়। A-তে আলাদা review ও confirm দিয়ে intended activation। History-তে প্রতিটি change-এর actor, school, effective time ও reason থাকে। | Planned — stand-in, real grant System 2 এবং course System 6-এ |
| 15 | Mid-day wrong switch-off drill (Design §৭.৫, case ৯) | Fixture offline register ও queued fixture job রেখে ভুল school-এ School Management বন্ধ। Window-তে register 404 `product_off` পায় এবং client fixture সেটি রেখে দেয়; job refuse হয়; 0 write। নতুন change দিয়ে access ফেরার পর: roster তাজা হলে register ঠিক একবার গৃহীত হয়; roster পুরোনো হলে register review-তে যায় (A9-এর নিয়ম); refuse হওয়া online entry আবার replay হয় না, আবার লিখতে হয়; blocked import blocked-ই থাকে, নতুন preview লাগে (OD-5, Option A) | Planned — fixture; app-এর আসল আচরণ: Pending — consumer (offline-sync owner: A9 ও C5) |
| 16 | Restricted ও reactivation | Read ও export চলে, নতুন record নয় (`school_read_only`), data থাকে; reactivation service ফেরায় | P1 (PLT-004 pilot অংশ; OD-2, OD-3 ঠিক হয়েছে) |
| 17 | Access সরানোর আগে queue-তে থাকা import | §৯.১ দেখুন | §৯.১ |
| 19 | Wrong campus (A-র অন্য campus-এর Registrar north campus-এ import confirm করেন) | C3 deny, 0 job row | Planned — stand-in; real at System 2 merge |
| 20 | Scheduled switch-off বাতিল, সময়ের আগে (T-তে product-off schedule, T-র আগে বাতিল; queue-তে job আছে) | Queue-তে থাকা job ওই schedule-এর কারণে refuse হয় না | Planned — fixture |
| 21 | Change command-এর lock timeout (চলমান chunk-এর কারণে lock পাওয়া যায় না) | "running work is finishing" বার্তা; একই key-তে retry ঠিক একবার সফল | Planned — fixture |
| 22 | দুটি concurrent change, একই target (একই version দেখে) | একটি সফল, একটি 409 `version_conflict` | Planned — fixture |
| 23 | Operator state-changing request-এ reason নেই বা blank | Field error এবং 0 write। Valid reason দিলে স্বাভাবিক review ও authorization-এর পরে change/audit-এ reason থাকে। | Planned — fixture |
| 24 | Operator key: দুই school (একই operator, একই key ও body School A ও School B-তে পাঠানো) | দুটি স্বাধীন ফল | Planned — fixture |
| 25 | Operator key: দুই change ID (এক school, দুটি change ID, একই key) | দুটি স্বাধীন ফল | Planned — fixture |

### ৯.১ Access সরানোর আগে queue-তে থাকা import

**Sequence (Proposed)**

কোনো row এখনো চালানো হয়নি; এগুলো পরিকল্পিত পরীক্ষা।

1. Registrar preview করেন। Preview কোনো student, placement বা guardian grant তৈরি করে না; A8 batch, staging ও mapping রাখতে পারে।
2. Registrar confirm করেন। Confirm নিজের transaction-এর ভেতরে T5-এর guard চালায় (Platform বা C3 refuse করলে কোনো job row হয় না) এবং server-লেখা guard field job row-তে লেখে (T8 a)।
3. Access বদলায়।
4. Worker job claim করে (lease ও claim token-সহ, T8 c)। প্রতিটি chunk-এর নিজের transaction-এর ভেতরে T5-এর guard lock-সহ চলে: job row থেকে school বসে, decision instant-এ database থেকে decision আসে এবং C3-র check চলে। Chunk token check-ও করে (T8 d)।
5. Refusal-এ job "blocked" হয় reason-সহ, কিছু লেখে না। A8 batch-টি "চলেনি — access বদলেছে" দেখায়। Status ও reason audit-এ থাকে।
6. Access ফিরলেও job বন্ধ থাকে এবং নিজে থেকে চলে না; Registrar আবার preview চালান (OD-5, Option A — আপনার সিদ্ধান্ত)।

**Variants**

| # | Queue-র পরে, run-এর আগে কী বদলায় | কে refuse করে | Reason | প্রত্যাশিত write | প্রথম branch-এ অবস্থা |
|---|---|---|---|---|---|
| a | School Management বন্ধ (PRD-001) | Platform | `product_off` | 0 student, 0 placement, 0 guardian grant (তিনটি fixture effect-এ মাপা: fixture student table, fixture placement table, C3 grant stand-in-এর call log) | Planned — fixture |
| b | Module `students.import` বন্ধ (PLT-005) | Platform | `module_off` | 0 (একই তিনটি fixture effect-এ মাপা) | Planned — fixture |
| c | Scheduled product-off, effective time T queue ও run-এর মাঝে পৌঁছায় (SAS-002); T5 অনুযায়ী: যে chunk-এর decision instant T-র আগে সে পুরোনো decision-এ চলে, T বা তার পরে নতুন decision-এ refuse হয় | Platform | `product_off` | T-র আগের chunk-এর write থাকে; T বা তার পরের chunk 0 | Planned — fixture; Pending (OD-7) |
| d | Registrar-এর role প্রত্যাহার এবং membership প্রত্যাহার, আলাদা দুটি আসল case | C3 | C3-র নিজের reason code | 0 | Planned — stand-in; real at System 2 merge; Pending (OD-7) |
| d2 | Confirm-এর পরে, run-এর আগে C3-তে Registrar-এর role revoke হয়ে আবার দেওয়া হয়; এবং আলাদাভাবে membership suspend হয়ে আবার চালু হয়। দুটোই পরের chunk-এর আগে, আর C3 এখন allow দেয় | C3 (history দেখে) | `access_changed` (job blocked) | 0 (তিনটি fixture effect-এই: fixture student table, fixture placement table ও C3 grant stand-in-এর call log) | Planned — stand-in; real at System 2 merge; Pending (OD-7) |
| e | School restricted হয় (PLT-004) | Platform | `school_read_only` | 0 | P1 (lifecycle slice; OD-2, OD-3 ঠিক হয়েছে) |
| f | Confirm-এর পরে, run-এর আগে product বন্ধ ও আবার চালু | Platform | `access_changed` | 0; job blocked হয় এবং নিজে থেকে আর চলে না (T8 f) | Planned — fixture (OD-5: Option A) |
| g | Chunk-এ চলা run-এর মাঝখানে access সরে; T5: chunk আগে lock পেলে change অপেক্ষা করে, ওই chunk-এর write থাকে, পরের chunk refuse হয়; change আগে commit হলে chunk refuse হয় ও কিছু লেখে না। T-র আগে অপেক্ষা শুরু করা chunk T-র পরে lock পেলে নতুন state-এ decide করে | পরের chunk-এ Platform বা C3 | উপরের মতো | আগে commit হওয়া chunk থাকে (record রাখা হয়); আর কোনো chunk চলে না; batch done ও not-run count-সহ আংশিক ফল দেখায় | Planned — fixture (দুই ক্রমে নির্ধারিত test); Registrar পরে কী করতে পারেন (undo বা continue) A8-র rule, Students owner-এর সঙ্গে Pending; Pending (OD-7) |
| h1 | Blocked job আবার deliver হয় (retry) | Guard | একই reason | 0 claim, 0 row; blocked record এখনো একটিই | Planned — fixture |
| h2 | দুই worker একই queued job একসঙ্গে claim করতে চায় | Claim (T8 c) | — | 0; ঠিক একটি claim হয় | Planned — fixture |
| h3 | Claim-এর পরে worker crash করে: lease মেয়াদ পেরোয়, job নতুন token-সহ reclaim হয়; পুরোনো worker-এর দেরিতে আসা chunk token check-এ refuse হয়। আলাদাভাবে: commit-এর পরে acknowledgement হারায় | Fencing (T8 d) | — | Commit হওয়া chunk আবার চলে না, কোনো row দ্বিতীয়বার লেখা হয় না; acknowledgement হারানো chunk skip হয়; পুরোনো worker 0 লেখে | Planned — fixture; C3 grant write-এর idempotent রূপ Identity ও Students owner-এর সঙ্গে Pending (OD-7) |
| i | Registrar-এর শুধু session মেয়াদ শেষ হয়; role ও membership বহাল থাকে | কেউ নয় | — | Proposed: job চলে, কারণ job accountRef + membershipRef ধরে চলে, session token ধরে নয় (T8 h)। Account lock বা membership suspend পরের chunk-এ job থামায় | Planned — stand-in; real at System 2 merge; Identity owner-এর সঙ্গে Pending (OD-7) |
| j1 | Preview-র পরে, confirm-এর আগে Platform-এর access সরে (product বা module বন্ধ) | Platform, confirm-এ | `product_off` বা `module_off` | Confirm refuse হয়; 0 job row | Planned — fixture |
| j2 | Preview-র পরে, confirm-এর আগে C3-র access সরে (role বা membership) | C3, confirm-এ | C3-র নিজের reason code | Confirm refuse হয়; 0 job row | Planned — stand-in; real at System 2 merge; Pending (OD-7) |
| k | Payload tamper করা (school, actor বা target server-লেখা guard field-এর সঙ্গে মেলে না) | Job guard | `job_integrity` (job status `failed`) | 0 write | Planned — fixture |
| l | Scheduled change effective time-এর আগে বাতিল হয় (queue ও run-এর মাঝে) | কেউ নয় | — | বাতিল schedule `access_changed`-এ গোনা হয় না; job চলে | Planned — fixture |
| m | Confirm-এর পরে, run-এর আগে C3-এর permission matrix-এ Registrar role × import action cell-এর নতুন version কার্যকর হয় | C3 | `access_changed` | 0 (তিনটি fixture effect-এ মাপা); job blocked, নিজে থেকে চলে না | Planned — stand-in; real at System 2 merge |

এই branch fixture ও stand-in evidence দেয়। আসল A8 import প্রমাণ হয় Students integration run-এ (System 3 merge, Doc 02-র ক্রমে তৃতীয়), আর আসল C3 প্রমাণ হয় System 2 merge-এ। দুটি আলাদা লেখা হয়।

## ১০. পরের slice

প্রতিটি slice-এর নিজের ছোট branch ও নিজের task list হবে, প্রথম slice merge হওয়ার পর এবং T0-র তথ্য জানা হলে (Doc 02)। সব P2 row MVP candidate। প্রতিটি পরের slice নিজের V-test row যোগ করবে (যেমন product অনুযায়ী invoice ও operator ledger, suspended Owner-এর pay, export ও notice, lifecycle transition ইত্যাদি)।

| Slice | Feature ID (label) | নির্ভর করে | Pending সিদ্ধান্ত | Verification |
|---|---|---|---|---|
| P1-a Lifecycle, pilot অংশ | PLT-004 (MVP partial, pilot অংশ) | প্রথম slice | — (OD-2, OD-3 ঠিক হয়েছে: pilot trial, active, restricted, reactivation; restricted-এ Blueprint পৃ. ১৭-এর নিয়ম, Option A) | E2E-09 at G1; V18-এর restricted clause |
| P1-b Audit | PLT-006 (MVP candidate) | প্রথম slice | — (G1 staging-এর আগে merge, কারণ Identity 1b নির্ভর করে; CX-01) | V04 |
| P1-c Approval engine | PLT-007 (MVP candidate) | PLT-006 | OD-4 ঠিক হয়েছে: pilot এক Super Admin দিয়ে চলতে পারে। দুই বা তার বেশি operator হলে live school-এ access কমানো change-এ দ্বিতীয় approver লাগে, তাই P1-c সেই বিন্দুর আগে merge হতে হবে। প্রথম branch শুধু staging-এ demo school নিয়ে চলে, তাই কোনো cycle নেই। যেকোনো ক্ষেত্রে G1 staging-এর আগে merge (Identity 1b; CX-01)। PLT-007-এর no-self-approval operator-এর gate; C3 একই engine custody ও privileged role-এর জন্য ব্যবহার করে (C9 record রাখে, C3 restriction লেখে), তাই দুটো আলাদা gate (CX-08) | V04 |
| P1-d File service | PLT-013 (MVP candidate); download নিয়ম §৭.১ | প্রথম slice | — (G1 staging-এর আগে merge, কারণ Identity 1b নির্ভর করে; CX-01) | V01-এর file probe (§৮ row 16–18: আগে নেওয়া link revoke-এর পরে refuse, অন্য school-এর link not found, import CSV batch-এ বাঁধা ও event-এ অনুপস্থিত), V14 |
| P1-e Plans ও manual invoice | PRD-006 (MVP partial, pilot অংশ), SAS-004 (MVP candidate); SAS-002 Plans catalogue screen = P0 feature SAS-002-এর বাকি অংশ, আলাদা P1 feature নয় | প্রথম slice | — | V18, G1/PRD-006 MVP slice evidence, G1/SAS-004 |
| P1-f Demo school | SAS-005 (MVP candidate) | প্রথম slice | — | G1/SAS-005 |
| P1-g Consumer integration | PRD-002, PRD-003, PRD-004, PRD-005, PRD-007 (MVP candidate) | Identity merged এবং consumer system (apps, Desk Learn, Fees, Academics, Results)। Platform decision ও contract test দেয়; screen বানায় consumer। | — | V02, V05, V09, V10, V18 |
| P2-a Support access | PLT-008 | PLT-006, C3 | — | V04, V15 |
| P2-b Fair use | SAS-013 | Job guard | PR-06 (pilot batch invite) client অনুমোদন দিলে ন্যূনতম per-school send quota এবং attendance ও emergency notice-এর নিচের priority class P2 থেকে pilot-এ আসে (G1-এর আগে); নইলে P2-তেই থাকে (CX-13) | V12, X-07 |
| P2-c Restore ও backup | SAS-014, QLT-002 | Hosting decision D-03 (DVL-020) | — | V13, E2E-13 |
| P2-d Policy release | PLT-011 | PLT-007 | — | V04 |
| P2-e Calendar, language, rendering | PLT-015, PLT-016, PLT-020 | Render role | — | V06, V16 |
| P2-f Budget ও accessibility | PLT-017, PLT-018 | সব screen | — | V12, V16, V17 |
| P2-g Security operations ও reliability | SAS-016, QLT-001, QLT-003, QLT-004, QLT-005, QLT-006 | Environment (DVL-013), hosting D-03 | — | V12, V14, V15, V19 |
| SAS-001 rest | SAS-001 (MVP candidate), console-এর বাকি screen | P1-a (Lifecycle tab), P1-b (Audit), P1-e (Plans, Subscription invoices), P1-f (Demo schools), P2-a (Support access), P2-b (Usage & fair use tab), P2-c (Restore), P2-g (Security operations) | — | G1/SAS-001-এর পূর্ণ evidence এই slice-গুলো আসার পর |

**বাকি R1 (MVP slice নয়):** PRD-006-এর বাকি অংশ SAS-003 (R1 backlog)-সহ; PLT-004-এর বাকি অংশ; PLT-019 (R1 backlog)। বিস্তারিত Design §১৫।

## ১১. Reuse

- Repository-তে যা আছে তাই জেতে: context ও middleware, outbox, job queue, migration tool, error format, test harness, component library। দ্বিতীয় একটি বানানো হবে না।
- Blueprint pattern reuse: row-level security pattern (পৃ. ২৪), idempotency rule (পৃ. ৩৫), outbox (পৃ. ৩৬)।
- T1-এর একটিই versioned shared fixture file (১২টি decision row, School A ও B-র control, `A-017`/`r17`, Identity-র Fixture A/B) তিন system একসঙ্গে ব্যবহার করে (CX-07)।
- Isolation suite প্রতিটি পরের module reuse করবে।
- Review ও confirm component পরে lifecycle ও invoice flow-তে reuse হবে।

## ১২. Migration risk

| # | Risk | কেন গুরুত্বপূর্ণ | কীভাবে সামলানো হয় |
|---|---|---|---|
| ১ | School column ছাড়া বিদ্যমান table | Isolation ভাঙে | Expand, bounded batch-এ backfill, read বদল, তারপর contract। Pending (T0) |
| ২ | Forced row-level security school context ছাড়া চলা বিদ্যমান query, job ও migration ভাঙতে পারে | হঠাৎ failure | Table ধরে ধরে, test-এর পেছনে চালু; job ও migration-কে স্পষ্ট context বা role দেওয়া |
| ৩ | Connection pooler | Session-level setting পরের request-এ leak করতে পারে | School setting transaction-local; pooler বসিয়ে test |
| ৪ | Context না থাকা | অস্পষ্ট server error ভুল বোঝায় | Fail closed ও পরিষ্কার report |
| ৫ | Effective time ও time zone | ভুল conversion product change কয়েক ঘণ্টা সরায় | UTC-তে সংরক্ষণ, school-এর zone-এ প্রদর্শন; দিন-সীমানা-সহ controlled-clock test |
| ৬ | অন্য cache-এর staleness | Decision-এর জন্য cache নেই (§৩), তবু অন্য cache-এ পুরোনো মান থাকতে পারে | Decision ও প্রতিটি write database থেকে পড়ে; cache-key helper school অনুযায়ী (T9) |
| ৭ | Seed ও demo data | আসল school-এ মিশলে ক্ষতি | শুধু synthetic, স্পষ্ট চিহ্নিত, আসল school-এ কখনো মেশে না |
| ৮ | Time-ordered UUID support database ও library version-এর উপর নির্ভর করে | Support না থাকলে ID helper ব্যর্থ | T0-তে যাচাই |
| ৯ | Operator database role | একটি ভুল grant নীরবে pupil schema খুলে দেয় | CI-তে grant check |
| ১০ | Platform, Identity ও Students পাশাপাশি plan করায় contract drift | তিন team ভিন্ন জিনিস বানায় | Versioned contract, shared fixture, merge-এর আগে owner review |
| ১১ | Lock wait ও deadlock | Change command, chunk ও C3 revoke একে অপরকে আটকাতে পারে | পুরো system-এ একটাই lock ক্রম: Platform (school → product → module, module key ascending), তারপর C3 (account → membership → role grant → guardian link → custody restriction row; matrix lock হয় না); operator change command C3-এর row lock করে না, C3-এর change command Platform-এর row lock করে না; guarded write সব lock আগে নিয়ে তবে decision instant পড়ে; change command শুরু থেকেই exclusive lock নেয়, কখনো shared থেকে upgrade করে না (T5); ছোট chunk; guard ও change command দুই দিকেই lock timeout |
| ১২ | Stand-in code production-এ পৌঁছানো | Test session বা C3 stand-in আসল access খুলে দিতে পারে | Stand-in শুধু test build-এ compile হয়; production build-এ থাকলে CI check fail করে (§৩) |
| ১৩ | প্রতি request-এ database read | Latency বাড়তে পারে | Primary-key lookup; V12-তে মাপা। Cache শুধু মাপা কারণ থাকলে (§৩) |
| ১৪ | ব্যস্ত row-তে shared lock | লেখা ধীর হতে পারে | Lock mode T0 ও V12-তে বেছে নিয়ে মাপা হয় |
| ১৫ | Reclaim-এর পরে থেমে থাকা পুরোনো worker লেখে | একই batch দুবার লেখা হতে পারে | Claim token ও fencing: প্রতিটি chunk transaction token মেলায়, না মিললে rollback (T8 c, d); chunk checkpoint (T8 e) |
| ১৬ | পুরোনো download link | Revoke-এর পরেও file নামতে পারে | প্রতি download-এ বর্তমান permission ও entitlement; single-use link কয়েক সেকেন্ডে মেয়াদ হারায় (§৭.১, P1-d) |

## ১৩. Rollout ও rollback

**Rollout ক্রম:** contract review → schema (expand only) → service → operator API → console → দুই suite green → verified integration base-এ merge → staging (Blueprint: merge-এ staging) → staging-এ দুটি demo school → verified contract version Identity ও Students-কে দেওয়া, তারা আপডেট হওয়া base-এ এগোয় (Doc 02: Platform আগে merge)। কোনো live school ছোঁয়া হয় না; এখনো production নেই।

**Rollback:**

- Code এক version পিছনে যায় (Blueprint পৃ. ২৪)।
- Expand-only migration থেকে যায়।
- ভুল entitlement ঠিক করতে বর্তমান state review করে reason-সহ নতুন change দিন।
- Rollback ধাপ হিসেবে row-level security কখনো বন্ধ করা হয় না; তা করতে আপনার স্পষ্ট অনুমোদন লাগে।
- **Staging rollback drill (Proposed):** pending schedule, cancelled change, blocked job, outbox replay ও পুরোনো binary নিয়ে চালানো হয়। দেখা হয় row-level security ও grant অক্ষত, record count অপরিবর্তিত, আর blocked job 0 row লেখে।
- Job claim runnable status-এর allow-list থেকে নেয় (T8 c), তাই পুরোনো binary অচেনা status claim করে না।
- Guarded job নতুন job type পায় (যেমন `students.import.v2`) যা পুরোনো worker চেনে না (T8 b); rollback drill দেখে যে rollback-এর পরের পুরোনো binary এই job কখনো claim করে না।
- পুরোনো worker নতুন job format পড়তে না পারলে rollback-এর আগে worker থামানো হয়।
- প্রথম Platform release-এর জন্য "এক version পিছনে" মানে কী, তা T0-র উপর নির্ভর করে।
- Consumer গ্রহণ করার পর contract বদলালে নতুন version ও INTERFACES-এর "Contract বদলালে review" ([INTERFACES.md](../../integration/INTERFACES.md))।

## ১৪. Responsive, accessibility ও regression check

| বিষয় | Check |
|---|---|
| Responsive | প্রতি P0 screen 320, 480, 768, 1024 ও 1280 px-এ এবং 200% text zoom-এ; ছোট size-এ কোনো action হারায় না; context bar দৃশ্যমান থাকে (Design §১১) |
| Accessibility | Provisioning, product change ও scheduled cancellation শুধু keyboard-এ; focus order ও focus return; result ও error ঘোষিত; reason field error ঘোষিত; result-unknown retry; দুই school জুড়ে confirm ও cancellation-এ একটি screen-reader run; token contrast tool দিয়ে যাচাই; target size; একটি automated scan ও একটি manual pass। V16 evidence P2, কিন্তু check P0 থেকে চলে (Design §১২) |
| Regression | প্রতি integration-base commit-এ isolation suite ও V18 suite; contract compatibility; row-level security ও grant check; idempotency replay; ১২টি consumer fixture row-এর মধ্যে শুধু যেগুলো P0-তে প্রযোজ্য সেগুলো green থাকতে হবে। Restricted row এখন contract-এর shape যাচাই করে, তার access policy P1-এ |

## ১৫. Open decisions ও pending task

| ID | যা Pending থাকে |
|---|---|
| OD-1 (backend এলে আপনি জানাবেন) | T0, প্রতিটি path, branch তৈরি, T12-এর web app path |
| OD-2, OD-3 (সিদ্ধান্ত: ২৬ নভেম্বর) | কিছু আটকায় না। Slice P1-a, §৯-এর restricted row ও §৯.১ variant e P1 কাজ। Pilot trial, active, restricted ও reactivation (restricted → active) ব্যবহার করে; grace, suspended ও offboarding বাকি R1-এ। OD-3: Option A (Blueprint পৃ. ১৭: restricted-এ সবাই নিজের role-এর মধ্যে পড়েন, export চলে, নতুন record নয়; suspended-এ শুধু Owner pay বা export করতে sign in করেন, বাকিরা notice দেখেন)। Client requirement-এর যাচাই: 3.1.4, 12.2.3, 12.2.C ও E2E-09 শুধু বলে data থাকে, export সম্ভব থাকে ও billing কখনো record মোছে না; suspension-এ কে কী পড়বেন তা কোনোটিই বলে না। মূল client SRS (DVL-001)-এর সঙ্গে তুলনা বাকি; তাতে ভিন্ন কিছু থাকলে এটি আবার দেখা হবে |
| OD-4 (সিদ্ধান্ত: ২৬ নভেম্বর) | কিছু আটকায় না। Pilot এক Super Admin দিয়ে চলতে পারে (MFA ও প্রতি change-এ reason)। দুই বা তার বেশি operator হলে live school-এ access কমানো change-এ PLT-007 দিয়ে দ্বিতীয় approver লাগে, তাই P1-c সেই বিন্দুর আগে merge হতে হবে (T6, T12, slice P1-c) |
| OD-5 (সিদ্ধান্ত: Option A, ২৬ নভেম্বর) | শুধু আংশিক চলা batch-এর পরের কাজ (A8-এর rule, Students owner-এর সঙ্গে)। নিয়ম: confirm-এর পরে access বদলালে job blocked হয়: এখন বন্ধ থাকলে বর্তমান reason (Platform বা C3); মাঝে বন্ধ হয়ে আবার চালু হলে `access_changed`। |
| OD-6 (সিদ্ধান্ত: ২৬ নভেম্বর) | কিছু আটকায় না। Device-session test (§৮-এর school switch row) ও stand-in আসল Identity merge পর্যন্ত Planned (T1, T2, T5, T6, T10)। Trade-off (System 2 জানিয়েছে, CTO cross-check CX-10): school user এক browser-এর দুই tab-এ দুটি school খোলা রাখতে পারবেন না; operator console প্রভাবিত নয় |
| OD-7 (আপনার অনুমোদন আছে, ২৬ নভেম্বর) | System 2 CX-11-এর শর্তে দিতে পারে এবং তিনটি চাহিদাই নিশ্চিত করেছে (System 2-এর plan §৫.৫); System 3 গ্রহণ করেছে; CTO cross-check-এ চূড়ান্ত। Matrix cell-এর নিয়ম (System 2-এর প্রস্তাব) গৃহীত। Guardian link write-এর পদ্ধতি CX-14, T-00-এর পরে। যে test এর উপর নির্ভর করে: queued job-এর test (§৯.১ c, d, d2, g, h3, i, j2, m); T1, T5, T8, T11 |

প্রশ্নের পূর্ণ বয়ান ও recommended default: Design §১৮। শুধু OD-1 এখনো খোলা।

**আরও যা Pending (source doc-এ নেই)।** Build ও test command (T0); route registration-এর জায়গা (T0); module-এর প্রকৃত তালিকা (প্রতিটি consumer system T1 contract-এ নিজের key দেবে); seeded plan-এর নাম; school-এর দিকের entitlement read-এর endpoint নাম ও owner (C3-র সঙ্গে); chunk-এ চলা import মাঝপথে থামলে Registrar কী করতে পারবেন (A8-র নিয়ম, Students owner-এর সঙ্গে); chunk size (A8, Students owner-এর সঙ্গে)।

**Pending, কিন্তু OD নয় এবং P0 আটকায় না (pilot-এ এক campus):** provisioning-এর পরে campus কে যোগ করবেন, operator না School Admin, এবং PLT-003 (A1, Academics)-এর সঙ্গে তার সম্পর্ক কী। সিদ্ধান্ত না হওয়া পর্যন্ত campus শুধু provisioning wizard-এ তৈরি হয়।

## ১৬. Evidence ও handoff

- তিনটি আলাদা record: slice evidence, integration evidence, client acceptance (CTO-HANDOFF: "Mock result, real integration run ও client acceptance-এর status আলাদা")।
- Partial row-এ tracker-এর "MVP slice" ও "R1 remainder" field ব্যবহার হয়।
- Result Doc 07 format-এ (T13)।
- Completion report Integration CTO-র কাছে যায় আপনার মাধ্যমে।

## ১৭. Coding শুরুর আগে branch check

এই plan approve করা মানে শুধু review sign-off। এতে coding শুরু হয় না। আপনি implementation শুরু করতে বললে agent আগে আপনাকে verified integration base থেকে একটি আলাদা feature branch (প্রস্তাবিত নাম `feat/platform/tenant-product-foundation`) তৈরি বা নির্ধারণ করতে বলবে। আপনি branch নিশ্চিত করে ও তাতে coding-এর অনুমতি দেওয়ার আগে কিছু হবে না। তারপর agent branch ও তার base commit যাচাই করবে এবং আলাদা worktree বা checkout-এ কাজ করবে। তার আগে কোনো code, config, dependency, schema, migration বা branch ছোঁয়া হয় না।

**OD-1 নোট।** বর্তমান docs-only `main` code base হবে শুধু যদি আপনি সিদ্ধান্ত নেন যে code এই repository-তে নতুন করে শুরু হবে। নইলে base হবে বিদ্যমান backend repository-র integration branch।

## ১৮. Source

| Source | Link |
|---|---|
| Design plan | [UI_UX_DESIGN_PLAN.md](UI_UX_DESIGN_PLAN.md) |
| MVP, release ও branch plan (Doc 02) | [02-mvp-release-and-branch-plan.md](../../02-mvp-release-and-branch-plan.md) |
| Module ও data map (Doc 03) | [03-system-module-and-data-map.md](../../03-system-module-and-data-map.md) |
| Roles ও permissions (Doc 04) | [04-roles-and-permissions.md](../../04-roles-and-permissions.md) |
| Workflows ও UI/UX (Doc 05) | [05-workflows-and-ui-ux.md](../../05-workflows-and-ui-ux.md) |
| School policy ও integration (Doc 06) | [06-school-policies-and-integrations.md](../../06-school-policies-and-integrations.md) |
| QA, pilot ও acceptance (Doc 07) | [07-qa-pilot-and-acceptance.md](../../07-qa-pilot-and-acceptance.md) |
| Migration, launch ও handover (Doc 08) | [08-migration-launch-and-handover.md](../../08-migration-launch-and-handover.md) |
| Feature tracker | [features.csv](../../tracker/features.csv) |
| Interfaces | [INTERFACES.md](../../integration/INTERFACES.md) |
| Dependencies | [DEPENDENCIES.md](../../integration/DEPENDENCIES.md) |
| System agent briefs | [SYSTEM-AGENT-BRIEFS.md](../../integration/SYSTEM-AGENT-BRIEFS.md) |
| CTO handoff | [CTO-HANDOFF.md](../../integration/CTO-HANDOFF.md) |

**Blueprint v2 পৃষ্ঠা যা ব্যবহার হয়েছে:** পৃ. ৬–৭, ১০–১১, ১৫, ১৭, ২৪–২৬, ৩২, ৩৪–৩৬, ৪৪, ৪৬, ৪৮, ৭৫–৭৬, ৯৭–৯৮, ১০১, ১০৪–১০৬। Blueprint PDF এই repo-তে নেই। এই planning session-এ স্থানীয় PDF থেকে পৃষ্ঠাগুলো মিলিয়ে নেওয়া হয়েছে। এগুলো source baseline, কোনো code repository-তে verify হয়নি।

## ১৯. Review-এর পরে সংশোধন (২৬ নভেম্বর ২০২৬)

Owner-এর reviewer-এর findings এই plan-এ যেভাবে প্রয়োগ হয়েছে। Section নম্বর বদলায়নি; শুধু §৮ ও §৯-এর row নম্বর নতুন করে সাজানো হয়েছে।

| Finding (severity) | কী বদলেছে | কোথায় |
|---|---|---|
| Blocker 1 — chunk race | প্রতিটি chunk নিজের write transaction-এ guard lock নেয়; change ও chunk-এর দুই ক্রমের ফল নির্ধারিত (এখন T5); নতুন OD-7 | T5, T8, §৯.১ variant c, g, §১৫ |
| Blocker 2 — cache | Decision cache সম্পূর্ণ বাদ; প্রতি request ও chunk database থেকে পড়ে (§৩); v1-এর দুটি cache বাক্যই সরানো | §১, §৩, T5, §৯ row 9, §১২ |
| Major 3 — job integrity, claim, key scope | Server-লেখা guard field ও `job_integrity`; atomic claim ও allow-list; Idempotency-Key scope (§৭.১) | T2, T8, §৭.১, §৭.২, §৯.১ variant h1–h3, k |
| Major 4 — preview থেকে confirm, তিনটি effect | Preview কিছু তৈরি করে না; confirm guard-এর ভেতরে; তিনটি fixture effect-এ মাপা; নতুন variant j | §৯.১ step ১, ২, variant a, b, j1, j2 |
| Major 5 — V01 positive control ও পুরোনো session | নতুন row 0; school switch-এ `session_ended`; operator path-এ অন্য school-এর change ID-র row | §৮ row 0, 7, 10, T10 |
| Major 6 — city, contact, count, operator role, campus endpoint | `city` ও profile contact; operator role-এর policy; counts-only interface; campus endpoint সরানো | T1, T3, T6, §৭.১, §৭.২, §৪ |
| Major 7 — task ক্রম, acceptance স্তর, OD-4 cycle, §১৫ সারাংশ | নতুন dependency diagram (T10 আর T7-এর অপেক্ষায় নয়); contract-ready, API-ready, job-ready, integrated স্তর; OD-4 no-cycle note; §১৫ table হালনাগাদ | §১, §৫, T1, T5, T6, T8, T13, §১০, §১৫ |
| Major 8 — rollback drill | Staging rollback drill, claim allow-list, worker থামানো, "এক version পিছনে" T0-র উপর | §১৩ |
| Minor 9 — status word, পাঁচ-cell row, 320 px, T0 path label | "চলে"-র বদলে Planned / Pending status word ও matrix-এর উপরের note; §৯-এর পাঁচ-cell row ঠিক; 320 px ও 200% zoom; "Verified (exists)" / "Verified parent" label | §৮, §৯, §৯.১, §১৪, T0 |
| Part C — fixture column | Fixture file-এ Platform expected, C3 expected, domain effect, channel ও evidence stage; row ১–১২ | T1 |
| Part C — T12 screen ও state | P0 screen-এর তালিকা, পাঁচ state, UI assertion list | T12 |
| Part C — SAS-001 rest | বাকি console screen কোন slice-এ আসে (P2-b Usage & fair use tab যোগ হয়েছে) | §১০ |
| Part C — SAS-002 বাকি অংশ | Plans catalogue screen = P0 feature SAS-002-এর বাকি, আলাদা P1 feature নয় | §১০ P1-e |
| Part C — design reference | T2, T3, T9, T12, T13-এ Design section-এর reference যুক্ত | T2, T3, T9, T12, T13 |
| Part C — nested ID row | Operator path-এ অন্য school-এর change ID দিলে 404 ও 0 write; `not_found`-এর status ও body missing record-এর মতোই | §৮ row 10, T6, §৭.১ |

**আপনার উত্তর (২৬ নভেম্বর ২০২৬)।** OD-5: Option A। OD-7: অনুমোদিত; Identity ও Students owner-এর সম্মতি CTO-র cross-check-এ বাকি। OD-1: backend এলে জানাবেন। OD-2, OD-3, OD-4 ও OD-6 পরে আপনি ঠিক করেছেন (নিচে)।

**CTO cross-check CX-11 (২৬ নভেম্বর ২০২৬)।** System 2-এর শর্ত মিলিয়ে যোগ হয়েছে: পুরো system-এর lock ক্রম Platform row → C3 row (T5, §১২ row ১১); guard ও change command দুই দিকেই lock timeout (T5, T8 g); C3-এর শর্ত (T5 h); call-এর ধরন (§২.৩ item 14)।

### Re-review-এর পরে (২৬ নভেম্বর ২০২৬)

Re-review-এর findings ও আপনার সিদ্ধান্ত যেভাবে প্রয়োগ হয়েছে। Section নম্বর বদলায়নি; §৮ ও §৯-এ নতুন row শেষে যোগ হয়েছে, আর §৯.১-এ variant h, j নতুন নাম h1–h3, j1–j2 পেয়েছে এবং d2 ও l যোগ হয়েছে।

| Finding (severity) | কী বদলেছে | কোথায় |
|---|---|---|
| Blocker — C3 revoke, তারপর আবার grant | Job guard-এ C3-র access-change history দেখা হয়: মাঝে revoke বা suspend হয়ে আবার ফিরলেও job blocked `access_changed`; এখন বন্ধ থাকলে বর্তমান reason। নতুন variant d2 | T5 h, T8 f, §৭.৩, §৯.১ variant d2, §১৫ (OD-5 নিয়ম) |
| Major — decision instant ও change command | Guarded write আগে সব lock নেয়, তারপর একটি authoritative সময় ও version পড়ে; change command শুরু থেকেই exclusive lock নেয়, upgrade নেই; concurrent change-এ একটি সফল, একটি 409 | T5, T6, §৯ row 21, 22, §৯.১ variant c, g, §১২ row ১১ |
| Major — claim recovery ও fencing | Lease ও claim token; fencing check; chunk checkpoint; নতুন job type; variant h1–h3 | T8 b–e, §৭.২, §৯.১ variant h1–h3, §১২ row ১৫, §১৩ |
| Major — operator key scope | Existing-school command-এর key (operator accountRef, operation, target schoolId, প্রযোজ্য হলে changeId); provisioning-এর key (operator accountRef, `provision`) | T2, T6, §৭.১, §৯ row 24, 25 |
| Major — download | প্রতি download-এ বর্তমান permission ও entitlement; single-use link; V01 file probe আলাদা | §৭.১, §৮ row 16–18, §১০ P1-d, §১২ row ১৬ |
| Major — API ও job-এর ফল আলাদা | Response table-এ API ও job আলাদা; `version_conflict` body-তে শুধু tenancy state; V18 row সংশোধন | §৭.১, §৯ row 1, 2, 4, 5, 7 |
| Major — task dependency | Shared write guard T5-এ; T8 queue wrapper; T9 T3-র পরে শুরু; T10, T11 শুধু API ও job; browser assertion T12-এ; regression-এ P0-প্রযোজ্য row | T4, T5, T8, T10, T11, T12, T13, §৫, §১৪ |
| Major — drill ও recovery | Wrong-school drill-এর status ও window count; mid-day drill-এর fresh ও stale roster নিয়ম | §৯ row 14, 15 |
| Minor | §১-এর ভবিষ্যৎ ভাষা; §২.১ Folder row session note; নতুন V01 ও V18 row; `confirm_code_mismatch`; SAS-001 rest-এ P2-b; T2, T7, T12 সংশোধন; §১৫ OD-6, OD-7 মিলিয়ে নেওয়া | §১, §২.১, §৮ row 14–18, §৯ row 18–25, §১০, §১৫, T2, T6, T7, T12 |
| OD-2 সিদ্ধান্ত | Pilot trial, active, restricted ও reactivation (restricted → active); grace, suspended ও offboarding বাকি R1-এ | §১, §৪, §১০ P1-a, §১৫ |
| OD-3 সিদ্ধান্ত (Option A) | Blueprint পৃ. ১৭-এর নিয়ম বহাল; DVL-001-এর সঙ্গে তুলনা বাকি | §৭.১, §১৫ |
| OD-4 সিদ্ধান্ত | Pilot এক Super Admin; দুই বা তার বেশি operator হলে PLT-007 দিয়ে দ্বিতীয় approver, তাই P1-c আগে merge | T6, T12, §১০ P1-c, §১৫ |
| OD-6 সিদ্ধান্ত | System 2-এর trade-off গৃহীত: এক browser-এর দুই tab-এ দুটি school নয়; console প্রভাবিত নয় | T2, §৭.৩, §১৫ |

**CTO cross-check CX-11 বিস্তার ও CX-14 (২৬ নভেম্বর ২০২৬)।** C3-এর lock ক্রম ও "matrix lock হয় না" যোগ (T5 a, §১২ row ১১)। Operator change command C3-এর row lock করে না, C3-এর change command Platform-এর row lock করে না; lock inspection test (T5 e ও Acceptance)। System 2-এর নিশ্চয়তা: revoke-এর পরে আবার দেওয়াও ধরা পড়ে, job row-তে নতুন field লাগে না (T5 h)। System 2-এর প্রস্তাব গৃহীত: matrix-এর role × action cell বদলালে `access_changed` (T8 f, §৯.১ variant m)। Guardian link write-এর পদ্ধতি CX-14 (T8 e)।

**CTO cross-check-এর alignment (২৬ নভেম্বর ২০২৬)।** (১) T1 এখন তিন system-এর একটিই versioned shared fixture file রাখে: ১২টি decision row (বর্তমান সংখ্যা), A ও B-র control, `A-017`/`r17`, Identity-র Fixture A/B; version বদলালে তিন owner review করেন (CX-07; T1, §১১)। Module key ও C3 action আলাদা (T4)। (২) PLT-006, PLT-007 ও PLT-013 G1 staging-এর আগে merge, কারণ Identity 1b নির্ভর করে (CX-01; §১০ P1-b, P1-c, P1-d)। (৩) PR-06 অনুমোদিত হলে SAS-013-এর ন্যূনতম quota ও priority class pilot-এ (CX-13; §১০ P2-b)। (৪) PLT-007-এর operator gate আর C3-এর custody ও privileged-role gate আলাদা (CX-08; §১০ P1-c); guardian link write-এর সিদ্ধান্ত Identity-র T-00-এর আগে নয় (CX-14; T8 e)।

**২৬ নভেম্বর ২০২৬ · Owner-এর PDF scope review প্রয়োগ।** Dedicated revert API, impact/consumer aggregates এবং typed confirmation contract ও তার পরীক্ষার ফল বাদ। ভুল applied change-এ current state থেকে নতুন change হবে। Future scheduled cancellation SAS-002 (পৃ. ৯৭) অনুসরণ করে। Reason PLT-006 (পৃ. ১০১), এবং প্রয়োজনীয় দ্বিতীয় approver PLT-007 ও requirement 12.4.1। আগের review-এর entries ইতিহাস। §৯.১-এর j1/j2 এখানে typed-code test নয়। এগুলি import preview-এর পরে Platform/C3 access সরে গেলে confirm প্রত্যাখ্যানের পরীক্ষা, তাই রাখা হয়েছে।

**আপনার সিদ্ধান্ত (২৬ নভেম্বর ২০২৬)।** OD-2, OD-3 (Option A), OD-4 ও OD-6 ঠিক হয়েছে। OD-7: System 2 CX-11-এর শর্তে দিতে পারে, System 3 গ্রহণ করেছে; CTO cross-check-এ নিশ্চিত করবেন। শুধু OD-1 খোলা।
