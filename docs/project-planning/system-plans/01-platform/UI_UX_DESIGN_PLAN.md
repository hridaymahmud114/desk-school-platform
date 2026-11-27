# Multi-tenant Platform — UI/UX Design Plan

| বিষয় | মান |
|---|---|
| Status | Draft v4 · ২৬ নভেম্বরের owner সিদ্ধান্ত যাচাই করে যুক্ত · owner review-এর জন্য প্রস্তুত |
| তারিখ | ২৬ নভেম্বর ২০২৬ |
| Scope | C1 Products, plans and entitlements · C2 Operator console · C9 Platform services · C10 Security, privacy and reliability |
| প্রথম deliverable | PRD-001, PLT-005, PLT-001, PLT-002 (plan input SAS-002) |
| Proposed branch | `feat/platform/tenant-product-foundation` (এখনো তৈরি হয়নি) |
| সঙ্গের plan | [IMPLEMENTATION_PLAN.md](IMPLEMENTATION_PLAN.md) |

**Label legend।** **Observed** = checkout বা source doc-এ দেখা। **Assumption** = ধরে নেওয়া। **Proposed** = এই plan-এ নতুন, আপনার approval লাগবে। **Pending** = আপনার সিদ্ধান্ত বা repository verification-এর অপেক্ষায়।

---

## ১. এক নজরে

- এই plan Desk School-এর Platform অংশের UI/UX ঠিক করে। প্রধান screen হলো **Operator console**, যেটা Super Admin ও support agent ব্যবহার করেন। School-এর দিকে Platform-এর state যেভাবে দেখা যাবে, তার নিয়মও এখানে আছে।
- সবচেয়ে বড় ঝুঁকি হলো operator ভুল school বদলে ফেলা। §৭.৪ ও §৭.৫-এ prevention, detection ও recovery তিন স্তরে design করা আছে।
- Tracker-এ এই system-এর ৫৪টি feature। MVP ৩৪টি, backlog ২০টি (§৪)। ৩৪টি MVP feature priority অনুযায়ী P0 ৬, P1 ১২, P2 ১৬ (§১৬)।
- এই checkout-এ কোনো code বা screen নেই। তাই সব screen ও code evidence **Pending** (§২.৩)।
- Open decision সাতটি: OD-1 থেকে OD-7 (§১৮)। ২৬ নভেম্বর আপনি OD-2, OD-3 (Option A), OD-4, OD-5 (Option A) ও OD-6 ঠিক করেছেন এবং OD-7 অনুমোদন করেছেন। শুধু OD-1 খোলা: backend এলে আপনি জানাবেন। §১৭-তে owner-এর scope সিদ্ধান্ত আছে।
- এই v2 owner review-এর সব findings প্রয়োগ করেছে; কী বদলেছে তা §২০-এ।

## ২. Audit — কী দেখেছি, কী ধরে নিয়েছি

### ২.১ Observed

এই planning session-এ ২৬ নভেম্বর ২০২৬-এ সরাসরি checkout ও source doc দেখে যাচাই করা।

| বিষয় | যা দেখা গেছে |
|---|---|
| Checkout | `D:\nls-all-projects\Erp-lms`। `git ls-files` ১৮টি file দেখায়: root `README.md` ও `docs/project-planning/`-এর ১৭টি file। Source code, screen, route, component, API definition, test, package manifest ও CI config নেই। |
| Git | শুধু `main` branch। local `main` ও `origin/main` একই commit `2f0b1e9`। Remote repo নাম `desk-school-platform`। একটি worktree। Proposed branch এখনো নেই। |
| Drift | `integration/CTO-HANDOFF.md` এখনো commit `febdc8f` লেখে। Checkout এখন `2f0b1e9`-এ। |
| Session note (folder) | `docs/project-planning/system-plans/` এই run-এর আগে ছিল না। এই planning session-এর note; checkout থেকে পরে আবার যাচাই করা যায় না। |
| Design system | `DVL-003` (Core UI design system and tested prototypes) status Planned। কোনো token বা component file নেই। |
| Tracker | `tracker/features.csv`-এ "Multi-tenant Platform"-এর ৫৪ row। প্রতিটির Implementation status "Needs repository verification"। Acceptance evidence সব খালি। |
| Tests (Doc 07) | V01 Tenant isolation। V18 Products ও plans। E2E-08 school isolation G1 full। E2E-09 G1 partial (operator-applied states), G2 full (expiry by rule)। |
| Roles (Doc 04) | Super Admin: school, plan, product lifecycle, usage, operator report; school record দেখতে scoped support session লাগে। Platform Support: approved school, issue ও সময়; consent বা logged emergency approval, expiry, audit। School Owner / Director: নিজের school-এর plan ও subscription দেখেন; operator function বন্ধ। |
| Journeys (Doc 05) | "School onboarding" flow আছে: Operator school ও campus setup করেন, product enable করেন, plan দেন, School Admin invite করেন। "Permission বা subscription state" নামে একটি UI state আছে: allowed action, safe message, valid navigation; অন্য school, child বা product-এর details প্রকাশ পায় না। |
| Existing screens | একটিও নেই। Operator console-এর screen-level বর্ণনা কোনো planning doc-এও নেই। |
| Blueprint v2 baseline | নিচের তালিকা। এগুলো source baseline, কোনো repo-তে verify হয়নি। |

**Blueprint v2 baseline — লেখকের উদ্ধৃত baseline, স্থানীয় PDF থেকে নেওয়া, repo-তে নেই।** মূল তথ্য:

- **Lifecycle (Blueprint পৃ. ১৭)।** সাত state: trial, active, grace, restricted, suspended, offboarding, deleted। Release 1-এ operator হাতে school-এর state বদলান, effect enforce করে platform (PLT-004)। Release 2-এ rule দিয়ে স্বয়ংক্রিয় (SAS-006)। PLT-004-এর wording-এ ছয়টি state আছে, "deleted" নেই। Deletion SAS-012-এর (R2 backlog)।
- **সাত isolation wall (Blueprint পৃ. ২৫–২৬)।** Session; API authorization; Module queries; Database (forced row-level security); Files; Cache/queues/search; Exports/reports/logs। Platform staff-এর pupil record-এ standing access নেই (PLT-008)। V01 প্রতি release-এ সব route, export, file link, search ও report অন্য school-এর session দিয়ে চালায়। একটি failure release আটকায়।
- **আটটি server-side check (পৃ. ৩২)।** ক্রম: Session → Membership → Role → Scope → Relationship → Sensitivity → State → Approval। "Hiding a button is never a control."
- **দুটি product (পৃ. ৭৫–৭৬)।** School Management ও E-Learning (Desk Learn) আলাদা entitlement, একটি অন্যটির উপর নির্ভর করে না (ADR-025)। প্রতি request-এ entitlement দেখা হয়। Class link school চালু না করা পর্যন্ত বন্ধ (PRD-007)।
- **Billing (পৃ. ১০৪)।** Platform subscription operator-এর নিজের ledger-এ, school-এর ledger-এ নয়। প্রতি product-এর আলাদা plan, usage meter ও invoice (D-20)। Release 1-এ operator হাতে record করেন, Release 2-এ automated। Amount operator console থেকে ঠিক হয়, code-এ নয় (D-08)।
- **Stack baseline (পৃ. ৬–৭, ১০–১১, ৪৮)।** Go modular monolith; PostgreSQL row-level security; Redis শুধু rate limit, short cache ও lock; S3-compatible storage; outbox ও PostgreSQL job queue; `/v1` REST API (OpenAPI 3.1); Next.js/React/TypeScript web portal (operator console-সহ); Flutter দুটি mobile app। Proposed backend layout: `internal/platform/`, `internal/tenancy/`, `internal/subscriptions/`, `internal/audit/`, `internal/policy/`, `api/openapi/`, `api/events/`, `migrations/`, `tools/`।
- **API rule (পৃ. ৩৫–৩৬)।** State-changing call-এ `Idempotency-Key`; record version, stale update refuse; error problem details (RFC 9457) ও stable code; cursor pagination; instant UTC। Tenancy event: `school.provisioned`, `school.state_changed`, `entitlement.changed`। Event-এ pupil content থাকে না।
- **Operator console (পৃ. ৩৪, ৯৭)।** Desk School-এর নিজের tool। দুই app ও console একই API ও একই authorization service ব্যবহার করে। কোনো wider-rights admin API নেই।
- **Service objective (পৃ. ৪৪, V12)।** Server read p95 800 ms-এর নিচে; মাসিক availability 99.5% (agreed maintenance বাদে)।

**Audit-এ পাওয়া তিনটি ফাঁক (Observed)।**

1. Operator console-এর কোনো screen, layout বা confirmation rule কোথাও লেখা নেই। Doc 05-এ শুধু এক লাইনের flow আছে। এই plan সেটি পূরণ করছে (§৬–§৮)।
2. Restricted বা suspended school-এ কে কী পাবে, Doc 03 ও Doc 04 সেটিকে "agreed policy" বলেছে, কিন্তু সেই policy এখনো agree হয়নি। Blueprint পৃ. ১৭-এ একটি baseline আছে; পরে আপনি OD-2 ও OD-3 ঠিক করেছেন (§১৮)।
3. কোনো design system নেই (DVL-003 Planned)। তাই §১০-এর token সবই Proposed।

### ২.২ Assumptions

| ID | Assumption |
|---|---|
| A1 | Console আলাদা web surface (Blueprint baseline Next.js)। কোনো repo-তে verify হয়নি। |
| A2 | Console desktop-first। Super Admin laptop-এ কাজ করেন। |
| A3 | Pilot এক campus, native Desk Learn (D-01)। |
| A4 | Operator sign-in ও second factor C3-এর (IAM-001, IAM-005)। |
| A5 | Operator-এর normal session কখনো pupil-level content দেখায় না (ADR-023)। আলাদা consented বা break-glass, time-limited support session (PLT-008, P2) নিজের rule-এ approved school-এর record দেখাতে পারে: consent বা recorded approval, expiry, দেখা প্রতিটি page log হয় এবং school-কে দেখা যায়। Console দুটো session কখনো মেশায় না। |
| A6 | Effective time UTC instant হিসেবে থাকে, school-এর time zone-এ দেখায়। Default "Now"। |
| A7 | Product বন্ধ করলে data থাকে (V18)। |
| A8 | Design token **Proposed**, DVL-003 তৈরি না হওয়া পর্যন্ত। |
| A9 | Console UI English first, string Amharic-ready। দুই app PLT-016 অনুসরণ করে। |
| A10 | OD-2 ও OD-3 ঠিক হয়েছে (২৬ নভেম্বর ২০২৬): pilot-এ trial, active, restricted ও reactivation; access rule Blueprint পৃ. ১৭-এর table অনুযায়ী (Option A)। বাকি state বাকি R1-এ। |

### ২.৩ Pending evidence

এই checkout-এ code বা screen নেই। তাই নিচের সব কিছু **Pending**:

- কোনো screen, route, component, API definition বা test আছে কি না।
- Backend ও web code কোন repository-তে থাকবে (Pending (OD-1))।
- Blueprint-এর stack ও folder layout বাস্তবে আছে কি না।
- DVL-003-এর token বা component।
- ৫৪টি feature-এর কোনোটির implementation status। সব "Needs repository verification", acceptance evidence খালি।
- Branch `feat/platform/tenant-product-foundation` তৈরি।

## ৩. Context — একজন Registrar-এর student import

**Proposed** fixture (`integration/INTERFACES.md`): School A, north campus। Registrar student `A-017`-কে Grade 6, section blue-তে import করেন। School B-তেও একই section নাম ও একই student number থাকতে পারে।

### ৩.১ কে school ও product ঠিক করেন

| প্রশ্ন | উত্তর |
|---|---|
| School কে ঠিক করে? | Super Admin C2 console-এ school ও campus provision করেন (PLT-001)। Registrar শুধু sign-in-এর সময় membership বেছে নেন (C3)। Server verified session থেকে school নেয়। Request body, CSV বা URL-এর school ID কখনো tenant বদলায় না। |
| Product কে ঠিক করে? | Super Admin School Management ও Desk Learn আলাদাভাবে চালু করেন (PRD-001), প্রতিটির plan ও effective time দেন (SAS-002), module flag ঠিক করেন (PLT-005)। |
| Registrar-এর অধিকার কে ঠিক করে? | C3: session, active membership, role, action, campus ও record scope, approval state। |
| কে কী লেখে? | A8 student, household ও dated placement লেখে। C3 guardian account–child grant লেখে। Platform pupil সম্পর্কে কিছু লেখে না। |

**নিয়ম।** একটি action তখনই চলে, যখন তিনটি একমত: Platform (school + product + module + lifecycle), C3 (membership + role + scope), এবং owning module-এর নিজের state/approval rule।

### ৩.২ School-only, Learn-only ও both

| Configuration | কী হয় |
|---|---|
| School-only | Entitlement ও Registrar scope মিললে import চলে। Desk Learn-এর কোনো screen, menu বা route নেই। |
| Learn-only | School student import UI-তে নেই, direct API-তেও বন্ধ। Independent course ও enrolment আলাদা learning role দিয়ে চলে। R1-এ Learn-only test school-এর নিজের learner দিয়ে। |
| Both | Import School Management-এর একই rule-এ চলে। Desk Learn-এর জন্য আলাদা role লাগে। Class link default বন্ধ। সাধারণ import-এ course, mark বা invoice তৈরি হয় না। |

### ৩.৩ C3 ও A8 কী পায়

| গ্রহণকারী | Platform থেকে যা পায় | যা পায় না |
|---|---|---|
| C3 | School ID ও campus ID (reference); lifecycle state; প্রতি product ও module-এর effective entitlement ও access mode; event `school.provisioned`, `entitlement.changed`, `school.state_changed` | Plan price, invoice, অন্য school-এর কিছু |
| A8 | Verified `SchoolContext` (school, actor membership reference, campus scope reference); School Management ও import module/action-এর entitlement decision (allow/deny, access mode, reason code, version); C3 Authorize decision। A8 preview, confirm, undo-তে এবং queued job চলার সময় আবার দুটোই প্রয়োগ করে। প্রতিটি write transaction, প্রতিটি job chunk-সহ, প্রথম ধাপে guard চালায় (§১৪.৪, Pending OD-7)। | Plan বা billing data |

বিস্তারিত contract §১৪ দেখুন।

## ৪. MVP ও backlog

Tracker label অপরিবর্তিত। কোনো feature এক release থেকে অন্যটিতে সরানো হয়নি। MVP ৩৪টি = ৩২ MVP candidate + ২ MVP partial (PRD-006, PLT-004)।

### MVP, module অনুযায়ী (৩৪)

| Module | Feature (tracker label) | সংখ্যা |
|---|---|---|
| C1 Products, plans and entitlements | PRD-001 · Two products switched on separately for each school: School Management and E-Learning (MVP candidate) | ৯ |
| | PRD-002 · A separate workspace and menu for each product in both apps, shown only where the school has the product and the role allows (MVP candidate, Added from practice) | |
| | PRD-003 · Separate management: school administration roles and learning administration roles, held independently (MVP candidate) | |
| | PRD-004 · Separate teachers: class and subject allocation in School Management, course instructors in E-Learning (MVP candidate, Added from practice) | |
| | PRD-005 · Separate billing to families: school fees and course fees, each with its own prices, invoices, receipts and reports (MVP candidate, Added from practice) | |
| | PRD-006 · Separate billing to the school: a plan, a usage meter and an invoice for each product (**MVP partial**) | |
| | PRD-007 · Class link, switched on by the school: a class subject linked to a course, sharing the class list and chosen marks (MVP candidate) | |
| | PLT-005 · Module entitlements and feature flags per school (MVP candidate) | |
| | SAS-002 · Plans and entitlements (MVP candidate) | |
| C2 Operator console | SAS-001 · Super Admin console · PLT-001 · School (tenant) provisioning and profile · PLT-004 · School lifecycle states applied by the operator: trial, active, grace, restricted, suspended, offboarding (**MVP partial**) · SAS-004 · Subscription invoices raised by hand and recorded in the operator's own ledger · SAS-005 · Demo school with sample data · SAS-013 · Fair use per school: rate limits, queue fairness, quotas, alerts · SAS-014 · Restore of one school's data · SAS-016 · Security operations: incidents, vendors, changes (সবগুলো MVP candidate, শুধু PLT-004 MVP partial) | ৮ |
| C9 Platform services | PLT-002 · Tenant isolation in data, files, cache, queues, search and exports · PLT-006 · Audit trail for sensitive actions · PLT-007 · One approval engine with maker-checker · PLT-008 · Consented, time-limited support access · PLT-011 · Policy releases: versioned, effective-dated, approved and tested on past data · PLT-013 · File service: scoped storage, malware scan, signed links, quotas · PLT-015 · Ethiopian and Gregorian calendars side by side · PLT-016 · English and Amharic at launch, per-user language · PLT-017 · Low-bandwidth and low-cost-phone budget · PLT-018 · Accessibility to WCAG 2.2 AA on core journeys · PLT-020 · Document templates and rendering: report cards, receipts and certificates in both languages and calendars (সব MVP candidate) | ১১ |
| C10 Security, privacy and reliability | QLT-001 · Availability and response-time objectives, measured per school · QLT-002 · Backups, point-in-time recovery and restore drills · QLT-003 · Security baseline · QLT-004 · Privacy for minors: consent, purpose, minimisation, retention · QLT-005 · Separate environments, synthetic test data, penetration test · QLT-006 · Observability, gradual release and rollback (সব MVP candidate) | ৬ |

### Backlog, release অনুযায়ী (২০)

| Release label | Feature (tracker label) |
|---|---|
| R1 backlog (২) | SAS-003 · Usage metering per school: active pupils, active learners, storage and SMS · PLT-019 · Search across names, courses and questions, within the user's own scope |
| R2 backlog (১২) | SAS-006 · Lifecycle by rule: trial expiry, renewal, grace, restriction and suspension, each with notices · SAS-007 · Automated subscription billing: renewals, proration, upgrades and downgrades · SAS-008 · Tax, discounts and coupons on subscriptions · SAS-009 · Reminders and retries for unpaid subscriptions · SAS-010 · Credit notes and refunds on subscription invoices; billing reports · SAS-011 · Contract history, renewal reminders and onboarding milestones · SAS-012 · Data export package, and offboarding with deletion scheduled under two approvals · SAS-015 · School health: onboarding completion, activity and data usage · SAS-017 · Support desk for schools: tickets, escalations and maintenance announcements · SAS-018 · Groups of schools: grouping and delegated oversight on agreed aggregates · PLT-009 · Fields and forms defined by the school · PLT-010 · Further languages by language pack |
| R3 backlog (২) | SAS-022 · Aggregate analytics for the operator, within contract and privacy rules · SAS-024 · A further operating cell in another country |
| R5 backlog (৪) | SAS-019 · Resellers and partners: portal, provisioning and commissions · SAS-020 · Marketplace of add-on modules and integrations · SAS-021 · Overage charges and enterprise purchase orders · PLT-012 · Dedicated deployment: a cell of its own for a school or group |

SAS-003 R1 backlog, তাই MVP console-এর Usage tab-এ usage meter দেখানো হয় না (§৬.১)।

## ৫. কারা ব্যবহার করবেন

| ব্যবহারকারী | Platform-এর সঙ্গে সম্পর্ক |
|---|---|
| Super Admin | Console-এর প্রধান user। School, plan, product, lifecycle, usage ও operator report। School record দেখতে scoped support session লাগে। |
| Platform Support | শুধু support session দিয়ে। Approved school, issue ও সময়; consent বা logged emergency approval, expiry, audit (PLT-008)। |
| School Owner | School app-এ নিজের plan ও subscription দেখেন। Console user নন, operator function বন্ধ। |
| Registrar, School Admin, Learning Admin, teacher, family | Console কখনো খোলেন না। শুধু access state হিসেবে effect টের পান (§৭.৮)। |

## ৬. Information architecture

### ৬.১ Operator console

| # | Surface | বিষয় | Feature |
|---|---|---|---|
| ১ | Sign-in ও second factor | C3-এর। এখানে design করা হয়নি। | IAM-001, IAM-005 |
| ২ | **Schools** (console home) | List, search, lifecycle state ও product filter। কোনো toggle বা inline action নেই। | SAS-001, PLT-001 |
| ৩ | **New school** | Wizard | PLT-001 |
| ৪ | **School detail** | সবসময় sticky school context bar-এর নিচে। Tabs নিচে। | |
| ৫ | **Plans** | প্রতি product-এর plan catalogue ও module set | SAS-002 |
| ৬ | **Subscription invoices** | Operator-এর নিজের ledger | SAS-004 |
| ৭ | **Demo schools** | | SAS-005 |
| ৮ | **Security operations** | Incident, vendor, change | SAS-016 |
| ৯ | **Audit** | Global, filter করা যায় | PLT-006 |

School detail-এর tab:

| Tab | বিষয় | Feature |
|---|---|---|
| Overview | সর্বশেষ change সবার উপরে | |
| Products & modules | Product on/off, module flag | PRD-001, PLT-005 |
| Plan & billing | প্রতি product-এর plan, manual invoice | SAS-002, PRD-006 (pilot অংশ), SAS-004 |
| Lifecycle | বর্তমান state ও পরের state | PLT-004 |
| Usage & fair use | Fair use; usage meter (SAS-003) R1 backlog, MVP-তে দেখানো হয় না | SAS-013 |
| Support access | | PLT-008 |
| Restore | | SAS-014 |
| History | Change history (audit PLT-006) | PLT-006 |

### ৬.২ School-এর দিকের surface

এই screen গুলো C5/C3 owner বানাবেন। Platform plan শুধু ঠিক করে সেগুলো কী দেখাবে।

| Surface | কাজ |
|---|---|
| Membership (school) picker ও switcher | এক person-এর একাধিক school থাকলে বেছে নেওয়া |
| Product workspace switcher | PRD-002 |
| "Not available" page | Disabled product বা module-এর জন্য, নিরপেক্ষ। API `product_off` বা `module_off` (404) দিলে app এই page দেখায় (§১৪.২) |
| Read-only banner | Restricted state-এ |
| Notice page | Suspended state-এ |
| Owner-এর plan ও subscription view | শুধু নিজের school |

## ৭. Flows

### ৭.১ School selection

1. Schools list-এ name, short code বা city দিয়ে search করুন।
2. Result-এ সবসময় name + short code + city দেখায়, যাতে একই রকম নাম আলাদা করা যায়।
3. School খুলুন। Sticky school context bar আসে।
4. School বদলাতে bar থেকে switch করুন। খোলা draft থাকলে prompt দেখায় ও draft বাদ যায়।

Feature: SAS-001, PLT-001।

### ৭.২ Provisioning (PLT-001)

Wizard-এর ধাপ:

1. **Profile**: legal name, display name, short code (suggested, editable, unique), **city** (বাধ্যতামূলক; search, context bar ও confirmation-এ ব্যবহার হয়), country, time zone, default language (English বা Amharic), default calendar (Ethiopian বা Gregorian), contact person। Contact person Platform-এ profile contact হিসেবে থাকে (name, role label, phone, email); এরা account নয়, ন্যূনতম field। Owner ও School Admin-এর account C3 invite দিয়ে আসে (IAM-002)।
2. **Campus**: অন্তত একটি campus।
3. **Products and plan**: প্রতি product-এর নিজের switch, plan ও start time। Initial lifecycle state trial বা active। (PRD-001, SAS-002)
4. **First School Admin invite**: C3-কে হস্তান্তর (IAM-002)। C3 যুক্ত না হলে এই ধাপ **Pending** দেখায়।
5. **Reason লিখে review ও create**।

Creation একটি idempotent command: পুরো সফল হয়, নয়তো কিছুই তৈরি হয় না। Brand upload, custom domain ও region choice এই wizard-এ নেই (WEB-001 অন্য system-এর R2 backlog; hosting D-03 অনুসারে)।

**Provisioning-এর ফল জানা না গেলে (Proposed)।** Wizard একটি draft-এর জন্য একটি Idempotency-Key রাখে। ফল জানা না পর্যন্ত একই key থাকে, এবং একই browser session-এ page reload করলেও থাকে। Server field error দিলে screen বলতে পারে কিছু তৈরি হয়নি। কোনো উত্তর না এলে বা timeout হলে screen "Result unknown" ও "Check result" দেখায়। "Check result" একই key আবার পাঠায়, আর school তৈরি হয়ে থাকলে আসল ফলই ফেরত আসে। ফল জানার আগে কখনো নতুন key নয়। একই short code দিয়ে পরের draft duplicate-code error পায়; সেই error বিদ্যমান school-এর নাম বলে, যাতে operator সেটি খুলতে পারেন।

### ৭.৩ Product ও module change (PRD-001, PLT-005, SAS-002)

1. School detail → Products & modules tab → **Change**।
2. State, plan ও module বেছে নিন।
3. Effective time: "Now" বা "Schedule"। School-এর time zone-এ দেখায়, পাশে UTC।
4. Reason লিখুন (সব change-এ বাধ্যতামূলক)।
5. **Review**: before → after, effective time, কোন product বা module বদলাবে এবং তার access-এর ফল।
6. **Confirm** (§৭.৪)।

### ৭.৪ Confirmation — ভুল school-এ change আটকানো

সবচেয়ে বিপজ্জনক operator ভুল: ভুল school বদলানো। যেমন School A-র বদলে School B-র Desk Learn চালু করা, বা school-day-তে ভুল school-এর School Management বন্ধ করা। Design তিন স্তরে রক্ষা করে।

**Prevention (আগেই আটকানো)**

| ব্যবস্থা | বিবরণ |
|---|---|
| Schools list-এ কোনো toggle বা inline state-changing action নেই | প্রতিটি change একটি school-এর নিজের page-এর ভেতরে হয়। |
| Sticky school context bar | প্রতি school page-এ: display name, short code, city, campus count, lifecycle chip, product chip। Browser tab title short code দিয়ে শুরু (multi-tab protection)। |
| Search result | সবসময় name + short code + city। |
| School switch | Bar থেকে switch করলে prompt দিয়ে খোলা draft বাদ যায়। Draft কখনো অন্য school-এ যায় না। |
| Review step | Before → after ও effective time দেখান। Product চালু হলে আগে থেকেই অনুমোদিত role-এর access খুলতে পারে। বন্ধ হলে access বন্ধ হয় এবং school data থাকে। |
| Confirm dialog | School name, short code ও city আবার দেখান। প্রতিটি operator state-changing action-এ reason লাগে (Blueprint PLT-006)। Confirm button action ও school-এর পরিচয় দেখায়, যেমন “Turn off Desk Learn for BFA-01”। |
| Record version | Request-এ operator-এর দেখা version যায়। Stale tab refuse হয়, current state দেখায়, review step নতুন data দিয়ে আবার খোলে। |

প্রতিটি operator change-এ reason লাগে। **OD-4 ঠিক হয়েছে (আপনি, ২৬ নভেম্বর ২০২৬):** pilot একজন Super Admin দিয়ে চলতে পারে (MFA, প্রতি change-এ reason)। দুজন বা তার বেশি operator হলে live school-এ access কমানোর change-এ PLT-007 দিয়ে দ্বিতীয় approver লাগবে, তাই P1-c (PLT-007) সেই সময়ের আগে merge হতে হবে। Activation-এ second approver নয়। প্রথম branch শুধু staging-এ demo school নিয়ে চলে, তাই কোনো dependency cycle নেই।

**Detection (ঘটলে দ্রুত ধরা)**

- Success panel school name ও short code আবার বলে এবং "View history" দেয়।
- Change school-এর Overview ও History-র সবার উপরে থাকে।

**Recovery (ফেরানো)** — ভবিষ্যতের change কার্যকর হওয়ার আগে cancel করা যাবে। কার্যকর হয়ে গেলে বর্তমান state দেখে স্বাভাবিক review ও confirmation দিয়ে নতুন change করুন (§৭.৫)।

### ৭.৫ Recovery

কার্যকর হওয়ার আগে scheduled change cancel করা যায় (SAS-002, effective-dated plan)। Cancel request-এ current version ও reason লাগে। কার্যকর change ঠিক করতে operator সঠিক school-এর বর্তমান state দেখে স্বাভাবিক change flow-তে নতুন action দেন। প্রতিটি change-এর নিজস্ব history, actor, সময় ও reason থাকে।

| # | Case | কী হয় |
|---|---|---|
| ১ | Scheduled change বাতিল | কার্যকর হওয়ার আগে cancel করা যায়। বাতিল change কখনো apply হয় না। |
| ২ | ভুল school-এ applied change | School চিহ্নিত করে current state review করুন। প্রয়োজন হলে নতুন entitlement change দিয়ে সঠিক state ফেরান। School Admin C3-তে grant এবং Learning Admin Desk Learn-এ course review করেন। |
| ৩ | Product আবার চালু | Product বন্ধে data থাকে। আবার চালু হলে আগের records ফেরে (V18)। আগেই অনুমোদিত role থাকলে সেই user-এর access খুলতে পারে (PRD-003)। |
| ৪ | Lifecycle reactivation | PLT-004 (§৭.৬)। |
| ৫ | এক school-এর data restore | SAS-014, V13। Isolated environment-এ restore করে count ও total মিলিয়ে apply বা discard। Delivery priority P2। |
| ৬ | Provisioning ব্যর্থ | আংশিক school তৈরি হয় না। একই key দিয়ে retry। |
| ৭ | Stale tab বা conflict | Current state নিয়ে আবার review। |
| ৮ | Confirm-এর ফল অজানা | একই key দিয়ে retry করে মূল ফল দেখান। |
| ৯ | School day-এর মাঝে ভুল switch-off | Product বন্ধ থাকলে online write `product_off` পায়, offline register device-এ থাকে এবং queued job পরের check-এ blocked হয়। নতুন change দিয়ে access ফিরলে fresh roster-এর register একবার sync হয়, stale roster review-তে যায় (A9/C5)। Refused online entry আবার দিতে হয়। Blocked import-এর জন্য Registrar নতুন preview চালান (OD-5, Option A)। |

ভুল activation-এর সময় শুধু সঠিক role পাওয়া member access পেতে পারেন। নতুন change দিয়ে product বন্ধ করলে access সরে, আর ওই সময়ে তৈরি records বা grants তাদের owner-এর কাছে থাকে। Operator School Owner-কে জানাবেন। আগে দেখা বা download হওয়া content server action দিয়ে device থেকে ফেরানো যায় না।
- File: প্রতিটি protected download-এর মুহূর্তে current permission ও entitlement দেখা হয় (INTERFACES-এ "current rights")। Download request API check-এর ভেতর দিয়ে যায়: API হয় file নিজে stream করে, নয়তো এক-বার-ব্যবহারযোগ্য link দেয় যার মেয়াদ কয়েক সেকেন্ডের মধ্যে শেষ হয় (সঠিক মান PLT-013 slice-এ ঠিক হবে, Pending)। আগে পাওয়া কোনো link তারপর আর কাজ করে না। বেশি মেয়াদের CDN link (যেমন পরে course video-র জন্য) MVP-তে নেই; লাগলে আপনার স্পষ্ট সিদ্ধান্ত লাগবে, যেখানে file-এর ধরন ও সর্বোচ্চ সময় নাম ধরে থাকবে। Device-এ আগেই download হয়ে যাওয়া content ফেরানো যায় না; সেটি server download থেকে আলাদা। Product বন্ধ থাকলে নতুন link দেওয়া হয় না (§১৪.৩)।
- তারপর operator নির্বাচিত school-এর পরিচয় আবার দেখে নতুন review ও confirmation দিয়ে change করেন।

দুটি পরিস্থিতিতে ভুল activation-এর ফল আলাদা:

| School B-র অবস্থা | কী হয় |
|---|---|
| (a) School B-তে Desk Learn-এর কোনো role holder ছিল না | School role না দেওয়া পর্যন্ত কেউ কিছু দেখে না। |
| (b) School B-তে আগে থেকেই role holder ছিলেন | Product চালু হলে তাঁদের access খুলতে পারে। নতুন change দিয়ে product বন্ধ হলে access সরে। ওই সময়ে তৈরি grant থাকলে School Admin C3-তে review করবেন। |

### ৭.৬ Lifecycle state (PLT-004)

1. Lifecycle tab-এ বর্তমান state, school-এর মানুষের জন্য তার অর্থ, অনুমোদিত পরের state ও history দেখায়।
2. Change একই review ও confirm pattern ব্যবহার করে (§৭.৪)।
3. **OD-2 ঠিক হয়েছে (২৬ নভেম্বর ২০২৬):** pilot trial, active, restricted ও reactivation (restricted → active) ব্যবহার করে। Grace, suspended ও offboarding বাকি R1-এ আসে। **OD-3 ঠিক হয়েছে, Option A:** প্রতি state-এর access rule Blueprint পৃ. ১৭-এর table অনুযায়ী (নিচে)।

| State | School-এ কী হয় (Blueprint পৃ. ১৭) |
|---|---|
| trial | Trial plan-এ provision; plan-এর সবকিছু trial limit-এর মধ্যে; data থাকে |
| active | Plan বেছে payment হয়েছে; plan-এর সবকিছু; data থাকে |
| grace | Renewal মিস; সব কাজ করে; Owner ও admin renewal notice দেখেন; data থাকে |
| restricted | Grace শেষ; সব read, export, subscription pay; নতুন record নয়; data read-only |
| suspended | Restriction শেষ; শুধু Owner sign in করে export বা pay করেন; family ও staff notice দেখে |
| offboarding | Contract শেষ বা school চলে যাচ্ছে; Owner export package পান; agreed তারিখ পর্যন্ত data থাকে |
| deleted | Export পৌঁছেছে ও দুটি approval হয়েছে; deletion report-সহ মোছা। SAS-012 (R2 backlog), PLT-004-এর ছয় state-এ নেই। |

### ৭.৭ Plan ও manual invoice (PRD-006 pilot অংশ, SAS-004)

1. Plan & billing tab-এ দুটি column, প্রতি product-এর একটি। প্রতিটির নিজের plan ও effective time।
2. **New invoice** ঠিক একটি product চায়।
3. Active pupil বা active learner-এর সংখ্যা হাতে টাইপ করা হয় এবং "entered by hand" লেবেল পায়, কারণ meter (SAS-003) R1 backlog।
4. Reason দিয়ে invoice operator-এর ledger-এ record হয়, school-এর ledger-এ নয়।

### ৭.৮ School-এর দিকের access state

| অবস্থা | School user কী দেখে |
|---|---|
| সাধারণ | শুধু entitled ও role-allowed workspace। |
| Product বন্ধ | Menu নেই। Deep link খুললে নিরপেক্ষ "not available" page, কিছু প্রকাশ করে না। |
| Restricted | Read-only banner; write action বন্ধ, সাদা ভাষায় কারণ; যে role-এর আগে থেকে export আছে তার export চলে। Write করলে server `school_read_only` দেয় (OD-3: Option A)। |
| Suspended | Notice page। Owner export বা pay করতে পারেন। Caller Owner না হলে server `school_suspended` দেয় (OD-3: Option A)। |
| অন্য school-এর ID | ঠিক missing record-এর মতো দেখায়। |
| School বদল | নতুন session; cache পরিষ্কার। |
| Access ফেরার পরে | নতুন entitlement change বা reactivation-এর পর user-কে কী আবার করতে হবে: offline register পরের sync-এ নিজে আবার যায়: roster fresh হলে একবার গৃহীত হয়, roster stale হলে register review-এ যায়; window-তে refuse হওয়া online entry আবার দিতে হয়; blocked import বন্ধই থাকে, Registrar আবার preview চালান (OD-5, Option A); আংশিক চলা batch-এ count দেখায়। Operator school owner-কে এটি জানান (§৭.৫, case ৯)। |

Restricted ও suspended-এর access rule ঠিক হয়েছে (OD-3, Option A; আপনি, ২৬ নভেম্বর ২০২৬): Blueprint পৃ. ১৭-এর rule চলবে। Restricted-এ সবাই নিজের role-এর মধ্যে read করেন, export চলে, নতুন record নয়। Suspended-এ শুধু Owner sign in করে pay বা export করেন; বাকি সবাই notice দেখেন। Client requirement মিলিয়ে দেখা হয়েছে: 3.1.4, 12.2.3, 12.2.C ও E2E-09 শুধু বলে data থাকে, export সম্ভব থাকে এবং billing কখনো record মোছে না; suspension-এর সময় কে কী পড়বেন তা কোনোটিই বলে না। মূল client SRS (DVL-001)-এর সঙ্গে তুলনা এখনো বাকি; সেখানে অন্য কিছু থাকলে এই সিদ্ধান্ত আবার দেখা হবে।

## ৮. Screen layouts

ASCII wireframe, UI label English। প্রতিটির নিচে Bangla note।

### ৮.১ Schools list

```
+----------------------------------------------------------------------------+
| Desk School Console                                   [Create school]      |
+----------------------------------------------------------------------------+
| Search [ name, code or city                        ]                       |
| State  [All v]   Product [All v]                                           |
+----------------------------------------------------------------------------+
| School                 Code     City        State         Products         |
|----------------------------------------------------------------------------|
| Bright Future Academy  BFA-01   Addis Ababa [check Active] SM  DL          |
| Bright Future School   BFS-02   Adama       [flask Trial]  SM              |
| Demo School            DEMO-01  -           [check Active] SM  DL  (Demo)  |
+----------------------------------------------------------------------------+
| Showing 3 of 3                                  [Previous]  [Next]         |
+----------------------------------------------------------------------------+
```
এই list-এ কোনো toggle বা inline action নেই। প্রতিটি row শুধু school খোলে। Code ও city দিয়ে একই রকম নাম আলাদা করা যায় (SAS-001, PLT-001)।

### ৮.২ School detail — Overview ও context bar

```
+----------------------------------------------------------------------------+
| BFA-01 | Bright Future Academy | Addis Ababa | 1 campus                      |
| [check Active]  [SM: On]  [DL: On]                      [Switch school v]  |
+----------------------------------------------------------------------------+
| Overview | Products & modules | Plan & billing | Lifecycle | Usage & fair |
| use | Support access | Restore | History                                   |
+----------------------------------------------------------------------------+
| Latest change                                                              |
|   26 Nov 2026, 14:05 EAT  Desk Learn: Off -> On   by A. Operator           |
|   Reason: Requested by owner, ticket 123      [View history]               |
|                                                                            |
| Products            School Management: On    Desk Learn: On                |
| Time zone           Africa/Addis_Ababa       Calendar: Ethiopian           |
+----------------------------------------------------------------------------+
```
Context bar প্রতি school page-এ sticky। Browser tab title `BFA-01` দিয়ে শুরু হয়। সর্বশেষ change সবার উপরে (§৭.৪)।

### ৮.৩ New school wizard — ধাপ ৩ (Products and plan)

```
+----------------------------------------------------------------------------+
| New school       (1) Profile  (2) Campus  [3] Products  (4) Admin  (5) Review |
+----------------------------------------------------------------------------+
| School Management                                      [ On  ]             |
|   Plan   [ Standard v ]      Starts [ Now v ]                              |
| Desk Learn                                             [ Off ]             |
|   Plan   [ -- ]              Starts [ -- ]                                 |
|                                                                            |
| Initial state  ( ) Trial   (x) Active                                      |
|                                                                            |
|                                   [Back]  [Next]                           |
+----------------------------------------------------------------------------+
```
প্রতি product-এর নিজের switch, plan ও start time। Plan নামের "Standard" শুধু উদাহরণ লেবেল (PLT-001, PRD-001, SAS-002)।

### ৮.৪ New school wizard — ধাপ ৫ (Review and create)

```
+----------------------------------------------------------------------------+
| New school       (1) (2) (3) (4)  [5] Review                               |
+----------------------------------------------------------------------------+
| Profile    Bright Future Academy · BFA-01 · Ethiopia · Africa/Addis_Ababa  |
|            English · Ethiopian calendar                                    |
| Campus     North campus                                                    |
| Products   School Management: Standard, starts Now                         |
|            Desk Learn: Off                                                 |
| State      Active                                                          |
| Admin      Pending (identity connection)                                   |
| Reason     [ New school setup, ticket 123 ]                                 |
|                          [Back]  [Create school]                           |
+----------------------------------------------------------------------------+
```
Create একটি idempotent command: পুরো সফল হয়, নয়তো কিছু তৈরি হয় না। Admin invite ধাপ C3 যুক্ত না হলে Pending (IAM-002)।

### ৮.৫ Products & modules tab

```
+----------------------------------------------------------------------------+
| BFA-01 | Bright Future Academy | Addis Ababa        [check Active] SM DL    |
+----------------------------------------------------------------------------+
| School Management                  On   Plan: Standard   Since 01 Sep 2026 |
|   [x] students.import                                                      |
|   [x] attendance                                                           |
|   [ ] ...                                                                  |
|                                                         [Change]           |
|----------------------------------------------------------------------------|
| Desk Learn                         On   Plan: Learn      Since 10 Oct 2026 |
|   [x] courses                                                              |
|   Class link: Off (default)                             [Change]           |
+----------------------------------------------------------------------------+
```
প্রতি product-এর card ও তার module flag row। Module-এর নাম উদাহরণ; প্রকৃত module list Pending। সব change **Change** দিয়ে review drawer-এ যায় (PRD-001, PLT-005)।

### ৮.৬ Change review drawer

```
+------------------------------------------------------+
| Review change                                    [x] |
| School  Bright Future Academy · BFA-01 · Addis Ababa |
| Change  Desk Learn: On -> Off                        |
| When    Now (26 Nov 2026, 14:05 EAT · 11:05 UTC)     |
| Effect  Access turns off. School data is kept.      |
| Reason  [ Requested by owner, ticket 123 ]           |
|                              [Back]  [Continue]      |
+------------------------------------------------------+
```

Review-তে before → after, effective time ও access-এর ফল দেখান। Activation-এ বলে দিন আগে থেকেই অনুমোদিত role থাকলে access খুলতে পারে। ছোট screen-এ full-width panel (§১১)।

### ৮.৭ Confirm dialog

```
+------------------------------------------------------+
| Turn off Desk Learn                              [x] |
| School  Bright Future Academy · BFA-01 · Addis Ababa |
| Change  Desk Learn: On -> Off                        |
| When    Now                                          |
| Reason  Requested by owner, ticket 123               |
|             [Cancel]  [Turn off Desk Learn for BFA-01] |
+------------------------------------------------------+
```

School-এর পরিচয়, change, সময় ও reason আবার দেখান। Version বদলে গেলে current state নিয়ে review খুলুন। OD-4 অনুযায়ী দুই বা বেশি operator-এর live school-এ access কমানোর change PLT-007 approver-এর কাছে যাবে (§৭.৪)।

### ৮.৮ Change history

History-তে before ও after, effective time, actor ও reason দেখান। ভবিষ্যতে কার্যকর হবে এমন change-এ `Cancel scheduled change` action দিন। তার জন্য reason ও current version লাগবে। কার্যকর change ঠিক করতে Products & modules থেকে নতুন change শুরু হবে (§৭.৫)।

### ৮.৯ Lifecycle tab

```
+----------------------------------------------------------------------------+
| BFA-01 | Bright Future Academy | Addis Ababa                               |
+----------------------------------------------------------------------------+
| Current state   [lock Read-only]  (restricted)                             |
| What it means   Everyone reads within their role. Export works.            |
|                 No new records. Data is kept.                              |
| Next states     [Reactivate -> Active]                                     |
|                                                                            |
| Pilot states: trial, active, restricted, reactivation                      |
|                                                                            |
| History                                                                    |
| 12 Nov 10:00  Active -> Restricted   A. Operator   "Unpaid"                |
+----------------------------------------------------------------------------+
```
State chip সবসময় icon + text। Pilot-এ operator trial, active, restricted ও reactivation ব্যবহার করেন (OD-2 ঠিক হয়েছে); grace, suspended ও offboarding বাকি R1-এ। Access rule Blueprint পৃ. ১৭ অনুযায়ী (OD-3: Option A)।

### ৮.১০ Plan & billing tab with manual invoice form

```
+----------------------------------------------------------------------------+
| Plan & billing                                                             |
+--------------------------------------+-------------------------------------+
| School Management                    | Desk Learn                          |
| Plan  Standard                       | Plan  Learn                         |
| From  01 Sep 2026                    | From  10 Oct 2026                   |
| [Change plan]                        | [Change plan]                       |
+--------------------------------------+-------------------------------------+
| New invoice                                                                |
|  Product        ( ) School Management  (x) Desk Learn                      |
|  Active learners this term  [ 120 ]   (entered by hand)                    |
|  Amount         [ 0.00 ] [ ETB v ]                                         |
|  Reason         [                                         ]                |
|                                        [Cancel]  [Record invoice]          |
+----------------------------------------------------------------------------+
```
Invoice ঠিক একটি product-এর হয়; অন্যটির charge যায় না। সংখ্যা হাতে টাইপ, কারণ meter (SAS-003) R1 backlog। Currency list উদাহরণ; প্রকৃত currency Pending। Invoice operator-এর ledger-এ (PRD-006, SAS-004)।

### ৮.১১ School-side — read-only banner

```
+----------------------------------------------------------------------------+
| [lock] Your school is in read-only mode. You can read and export.          |
|        New records cannot be created. Contact your school owner.           |
+----------------------------------------------------------------------------+
| Students                                              [Export]             |
| ...                                                   [Add student] (off)  |
+----------------------------------------------------------------------------+
```
Restricted state-এ write action বন্ধ, সাদা ভাষায় কারণ। Export শুধু সেই role-এর, যার আগে থেকে আছে। Rule: OD-3, Option A।

### ৮.১২ School-side — not-available page

```
+--------------------------------------------------------------+
| This area is not available.                                  |
| Ask your school administrator if you need access.            |
|                                          [Go to home]        |
+--------------------------------------------------------------+
```
Disabled product বা module-এর deep link-এ নিরপেক্ষ page। কোনো product, school বা record সম্পর্কে তথ্য দেয় না।

## ৯. Reusable components

| Component | উদ্দেশ্য | কোথায় | State |
|---|---|---|---|
| SchoolContextBar | School পরিচয় সবসময় দেখানো | সব school page | default, collapsed (ছোট screen) |
| SchoolSearch (combobox) | Name/code/city দিয়ে খোঁজা | Schools list, switcher | idle, loading, results, empty, error |
| LifecycleChip | State icon + text | List, bar, Lifecycle | trial, active, grace, restricted, suspended, offboarding |
| ProductCard | Product-এর অবস্থা ও plan | Products tab | on, off, scheduled |
| ModuleFlagRow | Module on/off | Products tab | on, off, disabled |
| EffectiveTimePicker | Now/Schedule; school time zone + UTC | Change flow | now, scheduled, invalid |
| ChangeSummary | Before → after | Review, confirm | default |
| ReasonField | Mandatory reason | সব change | empty, filled, error |
| ConfirmDialog | School, action, effective time ও reason | Change confirmation | ready, submitting, stale, result unknown |
| ChangeTimeline | Change history | History, Overview | loading, empty, loaded |
| ScheduledChangeAction | কার্যকর হওয়ার আগে scheduled change বাতিল | History | scheduled, submitting, cancelled, stale |
| Stepper/Wizard | ধাপে ধাপে | New school | active, done, error, result unknown |
| DataTable (cursor pagination) | List | Schools, invoices, audit | loading, empty, error, loaded |
| CountStat | Operator-safe count | Overview, Usage | loading, loaded |
| InvoiceForm | Manual invoice | Plan & billing | idle, invalid, submitting, done |
| MoneyField | Minor unit + currency | InvoiceForm | idle, invalid |
| DualDate | Ethiopian ও Gregorian (PLT-015) | সব date | default |
| ReadOnlyBanner | Restricted state | School app | shown, hidden |
| AccessNoticePage | Suspended / not available | School app | suspended, not available |
| JobStatusBadge | Job status | Console-এ Usage & fair use tab; school-এর দিকে A8-এর import batch | queued, running, done, blocked |
| Skeleton | Loading | সব | — |
| EmptyState | খালি কারণ + next action | সব list | — |
| ErrorPanel | Problem-details code + request ID | সব | default |
| InlineAlert/Toast (live region) | Async result | সব | info, success, warning, error |

## ১০. Design tokens (Proposed)

DVL-003 তৈরি হলে সেখানে merge হবে। Contrast মান QA-তে tool দিয়ে যাচাই করতে হবে। এখনো কোনো token file নেই।

| বিষয় | মান |
|---|---|
| Color | bg.canvas `#F7F8FA` · bg.surface `#FFFFFF` · border.default `#D5D9E0` (শুধু সাজসজ্জার divider) · border.control `#7A8496` (input ও control-এর boundary) · text.primary `#1A1F2B` · text.secondary `#4A5365` · brand.primary `#1F5FBF` · focus.ring `#0B57D0` (3px outline, 2px offset) · success `#1B7F4B` · warning `#9A5B00` · danger `#B3261E` · info `#1F5FBF` |
| Contrast target | Text অন্তত 4.5:1; UI component ও focus অন্তত 3:1 |
| Border-এর ব্যবহার | `border.control` bg.surface-এ 3.77:1 ও bg.canvas-এ 3.55:1। `border.default` 1.42:1, তাই এটি শুধু সাজসজ্জার divider; কোনো control-এর একমাত্র boundary কখনো নয়। |
| Contrast (WCAG formula-তে হিসাব; QA tool দিয়ে আবার যাচাই করবে) | bg.surface / bg.canvas-এ text token: text.primary 16.47 / 15.50 · text.secondary 7.73 / 7.28 · brand ও info 6.09 / 5.73 · focus.ring 6.39 / 6.01 · success 5.02 / 4.72 · warning 5.43 / 5.11 · danger 6.54 / 6.15 |
| Typography | Latin: Inter, system fallback। Ethiopic: Noto Sans Ethiopic। Size 12, 14, 16, 20, 24, 32। Form-এ body 16, dense table-এ 14। Line height 1.5 (Latin), 1.6 (Ethiopic)। Weight 400, 600 |
| Spacing | 4, 8, 12, 16, 24, 32, 48 |
| Radius | 4, 8, 12 |
| Elevation | 0, 1, 2 |
| Motion | 120 ms, 200 ms; reduced-motion-এ বন্ধ |
| Breakpoint | sm 480, md 768, lg 1024, xl 1280 |
| Target size | ন্যূনতম 44 × 44 px (WCAG 2.2 AA-তে 24 × 24 লাগে) |

Lifecycle chip সবসময় icon + text, শুধু রঙ নয়:

| State | রঙ | Icon | Text |
|---|---|---|---|
| trial | info | flask | trial |
| active | success | check | active |
| grace | warning | clock | grace |
| restricted | warning | lock | Read-only |
| suspended | danger | pause | suspended |
| offboarding | neutral | exit | offboarding |

## ১১. Responsive behavior

Console desktop-first (Assumption A2)।

| Breakpoint | আচরণ |
|---|---|
| xl / lg | পুরো table; side drawer |
| md | Table-এ মূল column; drawer full-width panel |
| sm | Table card হয়; tab menu হয়; wizard step উপর-নিচে; confirm sheet full-screen (সবসময় পুরো নাম, code ও city দেখায়); context bar sticky, code + ছোট করা (truncated) display name + state chip দেখায়; bar-এ tap করলে পুরো নাম ও city খোলে |
| 320 CSS px | পুরো content এক column-এ reflow হয়, দুই দিকে scroll লাগে না |

কোনো action ছোট screen-এ বাদ যায় না। School-এর দিকের state দুই app-এর নিজের নিয়ম মানে (teacher-এর জন্য phone-first, Doc 05)।

## ১২. Accessibility

PLT-018, WCAG 2.2 AA, V16।

- সবকিছু keyboard-এ চলে। Focus দৃশ্যমান; sticky bar focus ঢাকে না।
- Dialog focus আটকে রাখে ও বন্ধ হলে focus ফেরত দেয়।
- Status ও async result live region-এ ঘোষণা হয়।
- Error field-এর পাশে লেখায় থাকে এবং উপরে সারাংশ।
- Chip ও state রঙের উপর নির্ভর করে না।
- Table-এ সঠিক header।
- Target size §১০-এর মতো।
- Wizard একই data দুবার চায় না।
- Amharic text-এ সঠিক language attribute।
- Date দুই calendar-এ ঠিকভাবে পড়া যায় (PLT-015)।
- Session-timeout warning C3-এর অংশ।
- 200% text zoom ও 320 CSS px reflow-তে কোনো content বা action হারায় না।
- দুই school জুড়ে confirm ও scheduled cancellation keyboard ও screen reader দিয়ে চালিয়ে school, reason ও ফলের ঘোষণা মেলানো হয়।

## ১৩. Loading, empty, error ও success state

সাধারণ নিয়ম:

- **Loading**: আসল layout-এর skeleton, কখনো ফাঁকা page নয়।
- **Empty**: কেন খালি এবং পরের action, যেমন "No schools yet — Create school"।
- **Error**: সাদা ভাষার message, problem-details code ও request ID, কী নিরাপদ আছে, retry। Conflict হলে current state দেখায়।
- **Success**: কী বদলেছে, কোন school-এ (name + code), কখন কার্যকর, এবং View history।
- **ফল অজানা (network ব্যর্থ)**: সেটাই বলে এবং নিরাপদ retry দেয়।

| Screen | Loading | Empty | Error | Success |
|---|---|---|---|---|
| Schools list | Table skeleton | "No schools yet — Create school" | Message, code, request ID, Retry | — |
| School detail Overview | Bar ও tab skeleton | "No changes yet" | Problem code, request ID, Retry | সর্বশেষ change সবার উপরে |
| New school wizard | Step skeleton | — | Server field error: field-এর পাশে ও উপরে সারাংশ; কিছু তৈরি হয়নি। Result unknown (উত্তর বা timeout আসেনি): "Result unknown" ও "Check result"; সেটি একই key-এ আবার পাঠায় এবং school তৈরি হয়ে থাকলে আসল ফল ফেরত আনে | "BFA-01 created" + View school |
| Products & modules | Card skeleton | "No modules in this plan" | Conflict: current state দেখায়, review আবার খোলে | Name + code + effective time + View history |
| Change review / confirm | Change summary skeleton | — | Reason field error, stale হলে নতুন review, network failure-এ result unknown ও safe retry | School, action, effective time ও View history |
| History | Timeline skeleton | "No changes yet" | Message, code, request ID, Retry | নতুন change বা cancellation-এর entry |
| Lifecycle | State skeleton | — | Conflict: current state দেখায় | নতুন state ও কার্যকর সময় |
| Plan & billing | দুই column skeleton | "No invoices yet — New invoice" | Field error; message, code, request ID | Invoice ledger-এ record হয়েছে |
| School-side states | — | — | Not-available page | — |
| School-side, access ফেরার পরে | — | — | Offline-এ রাখা item "not sent" থাকে; window-তে refuse হওয়া online entry আবার দিতে হয় | Offline register পরের sync-এ নিজে যায়: roster fresh হলে একবার গৃহীত হয়, stale হলে register review-এ যায়; user-কে কী আবার করতে হবে তা বলে (§৭.৮) |
| School-side, import job `failed` (`job_integrity`) | — | — | Registrar দেখেন "This import could not run safely. Nothing was saved. Start again." ঘটনাটি platform security review-এর জন্য log হয়; log-এ pupil data থাকে না | — |

## ১৪. Tenant isolation ও C3/A8 contract

### ১৪.১ UI

- School user URL বা form field দিয়ে school বাছতে পারেন না।
- School switch = নতুন session + cache পরিষ্কার।
- Console-এ context bar সবসময় থাকে।
- Operator শুধু count ও health দেখেন, pupil field নয়।
- Operator session ও support session আলাদা। Normal operator session কখনো pupil-level content দেখায় না। আলাদা consented বা break-glass, time-limited support session (PLT-008, P2) নিজের rule-এ approved school-এর record দেখাতে পারে: consent বা recorded approval, expiry, দেখা প্রতিটি page log হয় এবং school-কে দেখা যায়। Console দুটো কখনো মেশায় না।

### ১৪.২ API

- School শুধু verified session থেকে আসে।
- Foreign-school ID missing record-এর মতো একই status ও body shape দেয়।
- Entitlement প্রতি request-এ দেখা হয়। Decision প্রতি request-এ database থেকে পড়া হয়, cache থেকে নয় (§১৪.৪)।
- **Proposed**: access না পাওয়া গেলে উত্তর problem details (RFC 9457) ধরনের। Body-তে শুধু `type`, `title`, `status`, `code`, `requestId` এবং user-এর ভাষায় একটি message থাকে; কখনো record data নয়। একই product বা module-এর সব route-এ body একই। School-এর নিজের app `product_off` / `module_off` দেখে নিরপেক্ষ not-available page দেখায় এবং offline item device-এ রাখে, ফেলে দেয় না (C5 ও A9 consumer contract, Pending সংশ্লিষ্ট owner-এর সঙ্গে)।

| পরিস্থিতি | HTTP | code |
|---|---|---|
| Product off (এখনো কার্যকর না হলেও): API-র উত্তর; job আলাদা, `blocked` | API 404; job `blocked` | `product_off` |
| Module off: API-র উত্তর; job আলাদা, `blocked` | API 404; job `blocked` | `module_off` |
| School restricted থাকা অবস্থায় write | 403 | `school_read_only` (OD-3: Option A) |
| School suspended, caller Owner নন | 403 | `school_suspended` (OD-3: Option A) |
| অন্য school-এর ID, বা না থাকা record | 404 | `not_found` — status ও body সত্যিকারের missing record-এর হুবহু একই |
| School user operator endpoint call করে | 404 | `not_found` |
| Record version পুরোনো (দুটি change একসঙ্গে একই target-এ হলে প্রথম commit জেতে, দ্বিতীয়টি এটি পায়) | 409 | `version_conflict` (body-তে শুধু target-এর বর্তমান `version`, বর্তমান state value ও `effectiveAt`) |
| Confirm-এর পরে access বদলেছে এবং এখন বন্ধ: job refuse | job status `blocked` | বর্তমান reason (Platform-এর বা C3-র, যেমন `product_off`, `module_off`) |
| Confirm-এর পরে মাঝে বন্ধ হয়ে আবার চালু: job refuse | job status `blocked` | `access_changed` |
| Job payload তার server-লেখা guard field-এর সঙ্গে মেলে না | job status `failed` | `job_integrity` |

- "Record data নয়" নিয়ম access-denied উত্তরের জন্য। `version_conflict`-এর body শুধু target-এর বর্তমান `version`, বর্তমান state value ও `effectiveAt` বহন করতে পারে; এগুলো tenancy data, কখনো pupil data নয়।
- Idempotency-Key-এর scope (Proposed): school user-এর জন্য (schoolId, actor membershipRef, operation)। Operator-এর বিদ্যমান school-এর command-এর জন্য (operator accountRef, operation, target schoolId, এবং path-এ change ID থাকলে changeId); provisioning-এর জন্য (operator accountRef, `provision`)। অন্য school-এ, অন্য change-এ বা অন্য actor-এর পাঠানো একই key আলাদা key, তার ফলও স্বাধীন।
- Operator endpoint path-এ target school নাম রাখে, শুধু operator role-এর জন্য খোলা, শুধু tenancy ও subscription data ছুঁতে পারে, pupil record নয়।
- State-changing call `Idempotency-Key` ও reason নেয়। Existing record-এর command current version নেয়।
- Operator role check C3-র, Platform তা consume করে (OD-6 ঠিক হয়েছে, ২৬ নভেম্বর ২০২৬)। প্রথম branch C3 না আসা পর্যন্ত test session ব্যবহার করে। Trade-off (System 2-এর জানানো, CTO cross-check CX-10): একজন school user এক browser-এর দুই tab-এ দুটি school খোলা রাখতে পারবেন না; operator console এতে প্রভাবিত নয়।

### ১৪.৩ Files

- Object key school prefix দিয়ে শুরু।
- Link স্বল্পমেয়াদি এবং permission ও entitlement check-এর পরেই দেওয়া।
- Product বন্ধ থাকলে তার file-এর নতুন link দেওয়া হয় না।
- PLT-013 Platform-এর নিজের (P1)। প্রতিটি protected download-এর মুহূর্তে current permission ও entitlement দেখা হয় (INTERFACES-এ "current rights" বলা আছে)। Download request API check-এর ভেতর দিয়ে যায়: API হয় file stream করে, নয়তো এক-বার-ব্যবহারযোগ্য link দেয় যার মেয়াদ কয়েক সেকেন্ডের মধ্যে শেষ হয় (সঠিক মান PLT-013 slice-এ ঠিক হবে: Pending)। আগে পাওয়া কোনো link তারপর আর কাজ করে না।
- বেশি মেয়াদের CDN link (যেমন পরে course video-র জন্য) MVP-তে নেই। লাগলে আপনার স্পষ্ট সিদ্ধান্ত লাগবে, যেখানে file-এর ধরন ও সর্বোচ্চ সময় নাম ধরে থাকবে। Device-এ আগেই download হয়ে যাওয়া content ফেরানো যায় না; সেটি server download থেকে আলাদা।
- Import CSV school prefix-এর নিচে, তার batch-এর সঙ্গে বাঁধা, কখনো event-এ যায় না।
- পূর্ণ file evidence PLT-013-এর সঙ্গে আসবে।

### ১৪.৪ Jobs

**Decision cache নেই (P0)।** প্রতিটি API request ও প্রতিটি job chunk নিজের transaction-এর ভেতরে database থেকে বর্তমান decision পড়ে। Decision কখনো cache বা event পৌঁছানোর উপর নির্ভর করে না; event শুধু অন্য module-কে জানায়। Decision cache পরে যোগ হতে পারে শুধু মাপা কোনো trigger থাকলে, আর তখনও প্রতিটি write transaction-এর ভেতরে আবার check করে। T9-এর school-scoped cache-key helper অন্য cache-এর জন্য থাকে।

**Write guard ও queued job (Proposed; OD-7: আপনার অনুমোদন আছে, Identity ও Students owner-এর সম্মতি CTO cross-check-এ)।**

- (a) Job row-এর guard field confirm-এর সময় server লেখে, পরে কখনো বদলায় না: schoolId, product, module/action, actor-এর accountRef ও membershipRef, target scope (campusId, batchId), confirmedAt, idempotency key। Worker তার context ও decision শুধু এই field থেকে নেয়। Payload এদের সঙ্গে না মিললে (school, actor বা target) job `failed` হয় `job_integrity` দিয়ে এবং কিছু লেখে না।
- (b) প্রতিটি guarded write transaction, অর্থাৎ API write, import confirm ও প্রতিটি job chunk, প্রথমে যত lock লাগে সব নেয়, পুরো system-এ একটিমাত্র global ক্রমে: Platform-এর row (school, তারপর product, তারপর module row, module key ascending ক্রমে), তারপর C3-এর row, C3-এর নির্দিষ্ট ক্রমে: account → membership → role grant (grant ID ascending) → guardian link (link ID ascending) → custody restriction row (ID ascending)। C3-এর permission matrix কখনো lock হয় না; তার version immutable, effective time-সহ (System 2-এর plan §৫.৫)। তারপর database থেকে একটি authoritative সময় ও বর্তমান version পড়ে, এবং তারপর decide করে। Lock commit পর্যন্ত ধরা থাকে। এই মুহূর্তটির নাম **decision instant**। Confirm-ও একইভাবে guard-এর নিচে: Platform বা C3 refuse করলে কোনো job row তৈরি হয় না।
- (c) প্রতিটি chunk A8-এর মালিকানার আলাদা write transaction (Blueprint পৃ. ১৫: একটি স্পষ্ট, নামওয়ালা transaction, একটি module-এর মালিকানায়)। Platform-এর guard (b)-এর ক্রমে shared lock নেয়। Shared row lock বা সমতুল্য কিছু (যেমন school ও product-এর key-ওয়ালা advisory lock) ব্যবহার হবে; কোনটি, তা T0-তে ঠিক হবে। Lock-এর ক্রম একটাই: আগে Platform-এর row-গুলো, তারপর C3-এর row (System 2-এর সঙ্গে মেলানো, CTO cross-check CX-11)। Operator-এর change command (product, module, plan, lifecycle, cancel) shared guard নেয় না: সে শুরু থেকেই দরকারি exclusive lock-গুলো একই global ক্রমে নেয়, তাই shared থেকে exclusive-এ upgrade কখনো হয় না। তার history row একই transaction-এ লেখা হয়। একই target-এ দুটি change একসঙ্গে এলে প্রথম যেটি commit হয় সেটি জেতে; দ্বিতীয়টি, যে version দেখে এসেছিল তা বহন করে, 409 `version_conflict` পায়। Operator-এর change command শুধু Platform-এর row lock করে, C3-এর কোনো row কখনো নয়; C3-এর change command-ও শুধু নিজের row lock করে, Platform-এর row কখনো নয়। Guarded write দুই দিকের row একই ক্রমে নেয়, lock order ও timeout test-এ যাচাই হবে।
- (d) Chunk-এর সিদ্ধান্ত হয় decision instant-এ। Chunk T-র আগে অপেক্ষা শুরু করেও lock পায় T-র পরে হলে নতুন state-এই decide করে। সেই মুহূর্তের আগে কোনো change commit হয়ে থাকলে chunk refuse হয় ও কিছু লেখে না। Chunk আগে lock পেলে change অপেক্ষা করে chunk commit পর্যন্ত; সেই chunk-এর লেখা থাকে এবং পরের chunk refuse হয়। Effective time T-র scheduled change-এ: decision instant T-র আগে হলে chunk পুরোনো decision-এ চলে; T বা তার পরে হলে নতুনটিতে।
- (e) `access_changed` (Platform ও C3 দুটোর জন্য): প্রথমে বর্তমান decision হিসাব হয়। Platform বা C3 refuse করলে job সেই reason-এ `blocked` হয়। Platform allow করলেও যদি এই school-এর lifecycle, এই product (plan-সহ) বা এই module-এর কোনো change confirmedAt ও chunk-এর decision instant-এর মধ্যে কার্যকর হয়ে থাকে (off-এর পর on হলেও), chunk `access_changed` দিয়ে refuse হয়। C3 allow করলেও যদি C3-র access-change history দেখায় যে এই actor-এর প্রাসঙ্গিক role, grant বা membership confirmedAt-এর পরে revoke বা suspend হয়েছিল (পরে ফেরত দেওয়া হলেও), job `access_changed` দিয়ে `blocked` হয়। C3-এর permission matrix-এ এই role × action cell-এর নতুন version confirmedAt ও decision instant-এর মধ্যে কার্যকর হলেও job `access_changed` দিয়ে blocked হয়, module-এর নিয়মের মতোই (System 2-এর প্রস্তাব, গৃহীত)। কোনো release-এ matrix version বদলালে তখন চলমান import job আটকে যেতে পারে; Registrar আবার preview চালান (OD-5, Option A)। কার্যকর হয়নি এমন cancelled schedule গণনায় আসে না। কিছুই নিজে থেকে আবার চলে না (OD-5)।
- (f) Atomic claim: শুধু `queued` status-এর job claim করা যায়, এবং ঠিক একজন worker-এর। `done`, `blocked`, `failed` terminal status স্বয়ংক্রিয়ভাবে আর claim হয় না; redelivery বা দ্বিতীয় worker terminal status পেলে কিছু করে না। Claim চলার মতো status-এর একটি allow-list থেকে বাছে, তাই rollback-এর পর পুরোনো binary কোনো অজানা status claim করে না।
- (g) Chunk checkpoint: প্রতিটি chunk তার লেখার একই transaction-এ নিজের সম্পূর্ণ হওয়া record করে, যাতে আগেই commit হওয়া chunk retry হলে বাদ যায় (Platform-এর job framework-এর helper; A8 ব্যবহার করে)।
- (h) Chunk ছোট রাখা হয়, যাতে change command দীর্ঘ অপেক্ষা না করে; chunk-এর মাপ A8-এর (Students owner-এর সঙ্গে Pending)। Guard (chunk) ও change command, দুই দিকেই lock timeout থাকে; মান T0-তে ঠিক হবে। Chunk-এর timeout হলে তার transaction কিছু না লিখে rollback হয় এবং একই key-এ পরে নিরাপদে আবার চেষ্টা হয়। Change command-এর timeout হলে বলে "running work is finishing — try again", এবং একই key-এ retry নিরাপদ: একবারই সফল হয়।
- (i) A8-এর database role শুধু Platform-এর guard function call করতে পারে; Platform-এর table-এ তার কোনো grant নেই (Proposed; T0 বিদ্যমান pattern যাচাই করে)।
- (j) C3-কে সমতুল্য দিতে হবে: grant ও membership revoke-এর জন্য একই lock semantics-এর `AuthorizeInTx`, "confirmedAt-এর পরে revoke হয়েছে কি?"-র উত্তর, এবং account ও membership state (locked account, suspended membership)। এটিই OD-7। System 2 জানিয়েছে C3 এগুলো দিতে পারে এই শর্তে (CTO cross-check CX-11): access-change-এর নতুন append-only history; account `locked` ও membership `suspended` state; lock timeout ও ছোট chunk; lock ক্রম আগে Platform row, তারপর C3 row; call-এর ধরন System 2-এর T-00 ও আমাদের T0-তে একসঙ্গে ঠিক হবে। System 3 guard ও "নিজে থেকে আবার চলে না" নিয়ম মেনেছে। পরে System 2 নিশ্চিত করেছে (System 2-এর plan §৫.৫): "confirmedAt-এর পরে revoke হয়েছে কি?"-র উত্তর revoke-এর পরে আবার দেওয়া ও suspend-এর পরে আবার চালু হওয়াও ধরে, database time-এর append-only history থেকে; job row-তে নতুন field লাগে না। প্রতিটি chunk-এর guardian link request একই transaction-এ হবে (recommended), নইলে durable outbound record ও idempotent command; Identity-র T-00-এর আগে এ বিষয়ে সিদ্ধান্ত হবে না (CTO cross-check CX-14)।
- (k) শুধু session-এর মেয়াদ শেষ হলে job থামে না: job চলে accountRef + membershipRef-এ, session token-এ নয়। Account lock বা membership suspension পরের chunk-এ job থামায় (C3 rule, OD-7)।

বাকি নিয়ম:

- Registrar দেখেন access বদলানোয় import চলেনি; job status ও reason audit-এ থাকে।
- Blocked job নিজে থেকে কখনো আবার চলে না। Access ফিরলেও job বন্ধ থাকে; Registrar আবার preview চালান (OD-5, Option A — আপনার সিদ্ধান্ত, ২৬ নভেম্বর ২০২৬)।
- Queue name ও cache key school অনুযায়ী namespaced। প্রতি school-এর নিজের capacity ভাগ (SAS-013)।

### ১৪.৫ Decision shape ও উদাহরণ

```
EntitlementDecision (Proposed)
  schoolId     from the verified session, or for a job from the server-written job row; never from client input
  product      school_management | desk_learn
  module       for example students.import
  accessMode   full | read_only | owner_only | none
  allowed      true | false for the requested action
  reason       ok | product_off | module_off | school_read_only | school_suspended | access_changed
  lifecycle    trial | active | grace | restricted | suspended | offboarding
  version, decidedAt
```

প্রতিটি row P0-তে কীভাবে প্রমাণ হয় এবং পূর্ণ প্রমাণ কখন আসে, শেষ দুই column-এ বলা আছে। এই ১২টি row-ই বর্তমান সংখ্যা (Identity-র plan-এ এখনো ১০টি লেখা)। Row-গুলো তিন system-এর একটিই versioned shared fixture file-এ থাকে, যা Implementation plan T1 রাখে (CTO cross-check CX-07)। Module key `students.import` Platform-এর entitlement-এর জন্য; C3-এর action আলাদা: `students.import.preview`, `students.import.confirm`, `students.import.undo`। Reason-এ এখন `not_yet_effective` নেই: এখনো কার্যকর হয়নি এমন product সরাসরি `product_off`।

| # | School configuration | Actor | Action | Platform says | C3 says | Result | P0-তে কীভাবে প্রমাণ | পূর্ণ প্রমাণ কখন |
|---|---|---|---|---|---|---|---|---|
| ১ | A School-only | A-র Registrar (north campus) | import confirm | allow, full | allow | চলে | API + job, fixture-সহ; C3 stand-in | System 3 merge (real import) |
| ২ | A School-only | একই | Desk Learn course খোলা | `product_off` | জিজ্ঞাসা হয় না | route উত্তর দেয় না | API fixture (404 `product_off`) | C5-এর সঙ্গে menu-র অনুপস্থিতি (P1-g) |
| ৩ | B Learn-only | B-র একজন user | school student import | `product_off` | জিজ্ঞাসা হয় না | menu নেই, API বন্ধ | API + job fixture | C5-এর সঙ্গে menu-র অনুপস্থিতি (P1-g) |
| ৪ | A both | A-র Registrar | import confirm | allow | allow | চলে; কোনো course বা enrolment তৈরি হয় না (class link বন্ধ) | Platform দুই product-ই allow করে, fixture | "কোনো course বা enrolment তৈরি হয়নি" A8 ও Desk Learn-এর সঙ্গে (System 3 ও System 6) |
| ৫ | A both | A-র Registrar | Desk Learn course তৈরি | allow | deny (learning role নেই) | refuse | C3 stand-in deny | System 2 merge |
| ৬ | A School-only, `students.import` module বন্ধ | Registrar | import | `module_off` | জিজ্ঞাসা হয় না | refuse; student list পড়া চলে | API + job fixture; অন্য module-এর fixture route তবুও allowed | System 3 merge |
| ৭ | A restricted | Registrar | import confirm | `school_read_only` | জিজ্ঞাসা হয় না | refuse; read ও export চলে | — | P1 (PLT-004; OD-2, OD-3 ঠিক হয়েছে) |
| ৮ | A | A-র Registrar B-র student ID পাঠায় | read | context A থাকে | — | missing record-এর মতো | API fixture, campus ID-সহ | System 3 merge-এ real student ID |
| ৯ | A | অন্য campus-এর Registrar | north campus-এ import | allow | deny (scope) | refuse | C3 stand-in deny (role থাকে, অন্য campus) | System 2 merge |
| ১০ | A | Registrar confirm করল, job queue-তে, তারপর School Management বন্ধ | job পরে চলে | run-এর সময় `product_off` | — | job blocked; কোনো student বা placement লেখা হয় না | job fixture (Implementation plan §৯.১ a) | System 2 ও System 3 merge |
| ১১ | A School-only | Registrar preview চালালেন, তারপর School Management বন্ধ হলো, তারপর confirm করলেন | import confirm | confirm-এ `product_off` | জিজ্ঞাসা হয় না | confirm refuse, কোনো job row নেই; A8 নিজের rule-এ preview staging রাখে | fixture (Implementation plan §৯.১ j1) | System 3 merge |
| ১২ | A School-only | Registrar preview চালালেন, তারপর Registrar role revoke হলো, তারপর confirm করলেন | import confirm | allow | deny | confirm refuse, কোনো job row নেই | C3 stand-in (Implementation plan §৯.১ j2) | System 2 ও System 3 merge |

## ১৫. PRD-006 ও PLT-004 — pilot অংশ বনাম বাকি R1

| | PRD-006 (MVP partial) | PLT-004 (MVP partial) |
|---|---|---|
| **Pilot অংশ** | প্রতি product-এর আলাদা plan ও নিজের effective time; প্রতি product-এর একটি manual subscription invoice (SAS-004) operator-এর ledger-এ; এক product-এর invoice অন্যটিকে charge করে না। Screen: Plan & billing tab, Plans catalogue, Subscription invoices। Evidence: tracker field "MVP slice status" ও "MVP slice evidence"; test V18 ও G1/PRD-006। | Operator হাতে agreed lifecycle state দেন; platform effect enforce করে। Restricted-এ approved read ও export চলে, write সীমিত, data থাকে; reactivation-এ service ফেরে। Evidence: E2E-09 (G1, operator-applied), V18, "MVP slice" field। Pilot-এ ব্যবহৃত state: trial, active, restricted ও reactivation (restricted → active) (OD-2 ঠিক হয়েছে, ২৬ নভেম্বর ২০২৬; V18 ও E2E-09 G1-এর জন্য যথেষ্ট); grace, suspended ও offboarding বাকি R1-এ। |
| **বাকি R1** | সম্পূর্ণ usage metering (SAS-003, R1 backlog: active pupil, active learner, storage, SMS) ও সংশ্লিষ্ট billing case: term অনুযায়ী active pupil বা learner, meter ও invoice দুই product-এ মেলে। Screen: Usage tab-এ meter; meter থেকে invoice draft ভরা। Evidence: "R1 remainder status" ও "R1 remainder evidence"। | প্রতিটি scripted transition ও ছয় state-এর (grace, suspended, offboarding-সহ) access rule transition matrix দিয়ে verify। Evidence: "R1 remainder" field। Access rule: OD-3, Option A (Blueprint পৃ. ১৭)। |
| **R1-এ নয়** | SAS-007 automated billing, SAS-008 tax/discount/coupon, SAS-009 reminder, SAS-010 credit note (সব R2 backlog); SAS-021 overage (R5 backlog) | SAS-006 lifecycle by rule ও SAS-012 export package ও দুই approval-এ deletion (R2 backlog); V26; E2E-09 G2 |

## ১৬. Priority — P0, P1, P2

- **P0** = প্রথম branch; Identity ও Students-কে unblock করে।
- **P1** = প্রথম school-day demo ও V18 সম্পূর্ণ করতে লাগে।
- **P2** = Results/Fees/Desk Learn batch ও G1 evidence-এর জন্য।

**P0 থেকেই প্রযোজ্য baseline:** প্রতি console screen-এ keyboard, focus ও contrast; string code-এর বাইরে (English আগে, Amharic-ready); শুধু synthetic data; log-এ school ID থাকে, pupil content নয়।

নিচের table-এ আলাদা করে **MVP partial** লেখা না থাকলে প্রতিটি feature tracker-এ MVP candidate, Blueprint release R1। Implementation status সব "Needs repository verification"।

### P0 (৬)

| Feature ID · tracker label | User value | Measurable acceptance criteria | Evidence / test |
|---|---|---|---|
| PLT-001 · School (tenant) provisioning and profile | Super Admin নিরাপদে নতুন school ও campus খোলেন। | (T6) একই key → একই response ও 0টি বাড়তি row; duplicate short code field error পায় যা বিদ্যমান school-এর নাম বলে; campus insert-এর পরে ঘটানো failure-এ 0টি school, campus বা entitlement row থাকে। (T7) `school.provisioned` প্রতি school-এ ঠিক একবার। (T12) A ও B, প্রতিটি campus ও city-সহ, শুধু keyboard দিয়ে তৈরি; result unknown হলে একই key-এ recovery। | V01, E2E-08, G1/PLT-001 |
| PRD-001 · Two products switched on separately for each school: School Management and E-Learning (MVP candidate) | প্রতি school যে product কেনে শুধু সেটাই পায়। | School-only, Learn-only ও both-এর জন্য §১৪.৫-এর P0 চিহ্নিত প্রতিটি row সেই row-এ চিহ্নিত channel দিয়ে fixture-এ প্রত্যাশিত ফল দেয়। API ও job-এর ফল আলাদা: product বন্ধ হলে API 404 `product_off` (§১৪.২-এর body-সহ), job `blocked` `product_off`; confirm-এর পরে product বন্ধ হয়ে আবার চালু হলে job `blocked` `access_changed`। বন্ধ product-এর registered route-এর 100% 404 `product_off` দেয় এবং 0টি side effect থাকে। Off-এর পর আবার on করলে fixture record-এর ID-র set ও তাদের content checksum আগের মতোই থাকে (শুধু count নয়)। এক product বন্ধ করলে অন্য product-এর registered route-এর 0টি decision বদলায়। School-side menu C5-এর সঙ্গে (P1-g), আসল course System 6-এর সঙ্গে প্রমাণ হয়। | V18, G1/PRD-001 |
| PLT-005 · Module entitlements and feature flags per school | Operator প্রতি school-এর module আলাদা চালু-বন্ধ করেন। | Module বন্ধ হলে §১৪.৫-এ ওই row-এ চিহ্নিত channel দিয়ে read ও write refuse হয়: API 404 `module_off`, job `blocked` `module_off` (আলাদাভাবে যাচাই); confirm-এর পরে module বন্ধ হয়ে আবার চালু হলে job `blocked` `access_changed`। একই product-এর অন্য module তবুও allowed। প্রতিটি flag change-এর history entry-তে actor, reason, effective time এবং before ও after field ধরে ধরে মিলিয়ে দেখা হয়। | V18, G1/PLT-005 |
| SAS-002 · Plans and entitlements | Plan অনুযায়ী কোন module পাওয়া যায় তা ঠিক থাকে। | Controlled clock-এ API decision T−1s-এ পুরোনো, T-তে নতুন। T-র আগে confirm হয়ে T-র পরে চলা job §১৪.৪-এর job guard-এর নিয়ম মানে। বাতিল scheduled change কখনো apply হয় না। Plan → module set API decision ও job guard-এ একই উত্তর দেয়; console-ও একই set দেখায় (T12)। | V18, G1/SAS-002 |
| PLT-002 · Tenant isolation in data, files, cache, queues, search and exports | এক school-এর data অন্য school-এ কখনো যায় না। | আগে positive control: প্রতিটি probe-এ A→A ও B→B সফল হয়। তারপর cross-school probe 0টি leak পায়। Foreign ID-র status ও body missing record-এর হুবহু একই। School-owned table-এর 100%-এ forced row-level security (automated inventory)। School switch-এর পর পুরোনো device session `session_ended` পায় (C3; P0-তে stand-in) এবং cache বা Back থেকে A-র কোনো data আসে না। অন্য school-এর change ID বহনকারী operator path 404 দেয় ও কিছু লেখে না। File, search, report, export ও school screen G1-এর আগে তাদের owner-দের সঙ্গে সম্পূর্ণ হয় (PLT-013 Platform P1; PLT-019 Platform R1 backlog; C6 ও A8 export; C5 screen)। | V01, E2E-08, G1/PLT-002 |
| SAS-001 · Super Admin console (শুধু P0 screen-এর জন্য দরকারি console shell; পূর্ণ G1/SAS-001 acceptance পরে) | Operator সঠিক school খুঁজে সেখানে কাজ করেন। | শুধু keyboard দিয়ে name, code বা city-তে search করে Products tab-এ পৌঁছানো যায়। প্রতিটি confirm-এ name, code, city, action ও reason দেখায়। Console API response ও screen-এ 0টি pupil-level field (response schema check)। Wrong-school activation drill এবং school-day-র মাঝে ভুল switch-off drill, দুটোই পাস করে। | V01, V18, G1/SAS-001 (আংশিক) |

### P1 (১২)

PRD-006 ও SAS-004 একই screen ও একই প্রমাণে চলে, তাই এক row-এ রাখা। ফলে ১১টি row-এ ১২টি feature। PLT-006, PLT-007 ও PLT-013 G1 staging-এর আগে merge হতে হবে, কারণ Identity-র branch 1b (custody ও audit) এগুলোর উপর নির্ভর করে (CX-01, owner-এর সিদ্ধান্ত, ২৬ নভেম্বর ২০২৬)।

| Feature ID · tracker label | User value | Measurable acceptance criteria | Evidence / test |
|---|---|---|---|
| PLT-004 · School lifecycle states applied by the operator: trial, active, grace, restricted, suspended, offboarding (**MVP partial**, pilot অংশ) · P1 (OD-2, OD-3 ঠিক হয়েছে) | বকেয়া school নিরাপদে সীমিত হয়, পরে ফিরতে পারে। | Restricted-এ read ও export চলে, 0টি নতুন record তৈরি হয়, সব data থাকে। Reactivation আগের access ফেরায়। প্রতি transition-এ actor, reason ও time। | E2E-09 (G1), V18 |
| PRD-002 · A separate workspace and menu for each product in both apps, shown only where the school has the product and the role allows (Added from practice) | User শুধু নিজের product ও role-এর workspace দেখে। | Workspace ও menu শুধু entitlement ও role দুটো মিললে দেখায় (C5/C3 বানায়; Platform decision দেয়)। | V02, V18 |
| PRD-003 · Separate management: school administration roles and learning administration roles, held independently | School administration ও learning administration আলাদা থাকে। | School Admin learning role ছাড়া course খুলতে পারেন না (C3 ও Desk Learn বানায়; Platform product context দেয়)। | V02, V10, V18 |
| PRD-004 · Separate teachers: class and subject allocation in School Management, course instructors in E-Learning (Added from practice) | Class teacher ও course instructor আলাদা হিসেবে চলে। | Class-teacher ও instructor grant আলাদাভাবে শুরু ও শেষ হয়। | V02, V10, V18 |
| PRD-005 · Separate billing to families: school fees and course fees, each with its own prices, invoices, receipts and reports (Added from practice) | Family-র school fee ও course fee আলাদা থাকে। | School-fee ও course-fee workspace প্রতিটি নিজের product-এর entitlement মানে (Fees বানায়)। | V09, V18 |
| PRD-007 · Class link, switched on by the school: a class subject linked to a course, sharing the class list and chosen marks | School ইচ্ছা করলে class ও course জোড়া দেয়। | Class link default বন্ধ; শুধু দুই product চালু থাকলে পাওয়া যায়; unlink করলে history থাকে (Desk Learn, Academics, Results বানায়)। | V05, V10, V18 |
| PRD-006 · Separate billing to the school: a plan, a usage meter and an invoice for each product (**MVP partial**, pilot অংশ) · SAS-004 · Subscription invoices raised by hand and recorded in the operator's own ledger | Operator প্রতি product-এর আলাদা plan ও manual invoice দেন; একটির জন্য অন্যটির charge হয় না। | Manual invoice-এ অন্য product-এর 0টি line বা charge। Invoice operator-এর ledger-এ, কখনো school-এর ledger-এ নয়। | V18, G1/PRD-006 (MVP slice), G1/SAS-004 |
| PLT-006 · Audit trail for sensitive actions | প্রতিটি সংবেদনশীল change-এর অক্ষত প্রমাণ থাকে। | প্রতি provisioning, entitlement, lifecycle change ও support session-এর append-only audit event, actor, school, record ও reason-সহ; edit বা delete করা যায় না। | V04 |
| PLT-007 · One approval engine with maker-checker | Request-কারী নিজে নিজের request approve করতে পারে না। | Requester কখনো নিজের request approve করতে পারে না। দুজন বা তার বেশি operator হলে live school-এ access কমানোর change-এ second approver হিসেবে ব্যবহার হবে (OD-4 ঠিক হয়েছে); তাই P1-c সেই সময়ের আগে, আর যেকোনো ক্ষেত্রে G1 staging-এর আগে merge হতে হবে (CX-01)। PLT-007-এর no-self-approval হলো operator-এর gate; C3 একই approval engine custody ও privileged role-এর জন্য ব্যবহার করে (C9 record রাখে, C3 restriction লেখে), তাই দুটো একই gate নয় (CX-08)। | V04 |
| PLT-013 · File service: scoped storage, malware scan, signed links, quotas | File নিরাপদে সংরক্ষিত ও শুধু অনুমোদিত user-কে দেওয়া হয়। | File key-তে school prefix; প্রতিটি protected download-এর মুহূর্তে current permission ও entitlement দেখা হয়; API file stream করে বা এক-বার-ব্যবহারযোগ্য, কয়েক সেকেন্ডে মেয়াদ-শেষ link দেয়; আগে পাওয়া link revoke-এর পর refuse হয়; অন্য school-এর link missing file-এর মতো আচরণ করে (§১৪.৩)। | V01, V14 |
| SAS-005 · Demo school with sample data | Demo-র জন্য নিরাপদ school থাকে। | Demo school synthetic sample data দিয়ে তৈরি এবং স্পষ্টভাবে Demo চিহ্নিত। | G1/SAS-005 |


### P2 (১৬)

প্রতিটির pass condition [Doc 07](../../07-qa-pilot-and-acceptance.md)-এর নির্দিষ্ট test থেকে নেওয়া। এই feature-গুলোর পূর্ণ evidence G1-এ লাগবে।

| Feature ID · tracker label | User value | Pass condition (Doc 07) | Evidence / test |
|---|---|---|---|
| PLT-008 · Consented, time-limited support access | Support শুধু অনুমোদিত, সময়সীমার মধ্যে school record দেখেন। | V04: sensitive action-এর audit অক্ষত। V15: অনুমোদিত, time-limited access ও log মেলে। | V04, V15 |
| SAS-013 · Fair use per school: rate limits, queue fairness, quotas, alerts | এক school-এর ভারী কাজ অন্য school-কে ধীর করে না। | V12: এক school-এর bulk send-এ অন্য school-এর attendance notice দেরি হয় না (X-07)। PR-06 (pilot batch invite) client অনুমোদন দিলে: ন্যূনতম per-school send quota এবং attendance ও emergency notice-এর নিচের একটি priority class P2 থেকে pilot-এ আসে (G1-এর আগে); নইলে P2-তেই থাকে (CX-13)। | V12, X-07 |
| SAS-014 · Restore of one school's data | এক school-এর data নিরাপদে ফেরানো যায়। | V13: isolated restore থেকে এক school extract করে result, student ও financial record recovery point-এর সঙ্গে মেলে। E2E-13 (G1 full): backup isolated test environment-এ restore হয় এবং academic ও financial record recovery-র লক্ষ্যের সঙ্গে মেলে। | V13, E2E-13 |
| SAS-016 · Security operations: incidents, vendors, changes | Incident, vendor ও change এক জায়গায় থাকে। | V19: controlled fault-এ alert পৌঁছায় ও incident drill-এর record থাকে। | V19 |
| PLT-011 · Policy releases: versioned, effective-dated, approved and tested on past data | Policy বদল approved ও পুরোনো data-তে পরীক্ষিত হয়। | V04: অন্য approver, effective date ও আগের হিসাব আবার চালিয়ে মেলার প্রমাণ। | V04 |
| PLT-015 · Ethiopian and Gregorian calendars side by side | Date দুই calendar-এ ঠিক দেখায়। | V06: Ethiopian ও Gregorian date ও code verify। V16: দুই calendar/PDF sample approve। | V06, V16 |
| PLT-016 · English and Amharic at launch, per-user language | প্রতি user নিজের ভাষা বেছে নেয়। | V06: English ও Amharic verify। V16: Amharic native review ও English copy approve। | V06, V16 |
| PLT-017 · Low-bandwidth and low-cost-phone budget | কম দামের phone ও ধীর network-এ কাজ চলে। | V12: objective-গুলো launch peak-এর তিন গুণে ৬০ মিনিট মেলে। V17: low-cost Android, older iPhone ও pilot network-এ daily flow time/data budget-এ চলে। | V12, V17 |
| PLT-018 · Accessibility to WCAG 2.2 AA on core journeys | প্রতিবন্ধী user-ও core কাজ করতে পারেন। | V16: core flow-এর WCAG 2.2 AA review। | V16 |
| PLT-020 · Document templates and rendering: report cards, receipts and certificates in both languages and calendars | Document দুই ভাষা ও দুই calendar-এ ঠিক ছাপে। | V06: card, date ও code ঠিক। V16: দুই calendar/PDF sample approve। | V06, V16 |
| QLT-001 · Availability and response-time objectives, measured per school | প্রতি school-এর service মাপা হয়। | V12: objective তিন গুণ peak-এ ৬০ মিনিট মেলে এবং ২৪ ঘণ্টার soak report। (Blueprint লক্ষ্য: p95 800 ms-এর নিচে, 99.5% availability।) | V12 |
| QLT-002 · Backups, point-in-time recovery and restore drills | Data হারালে ফেরানো যায়। | V13: critical data loss সর্বোচ্চ ১৫ মিনিট, recovery সর্বোচ্চ ৪ ঘণ্টার target-এর সঙ্গে measured result তুলনা। | V13 |
| QLT-003 · Security baseline | System নিরাপত্তা মানদণ্ড মানে। | V14: OWASP ASVS (API ও web) ও MASVS (mobile) checklist pass; scan clean; open critical/high finding 0। | V14 |
| QLT-004 · Privacy for minors: consent, purpose, minimisation, retention | শিশুর data উদ্দেশ্য অনুযায়ী সুরক্ষিত। | V15: purpose অনুযায়ী child consent, audit ও retention check। | V15 |
| QLT-005 · Separate environments, synthetic test data, penetration test | Test-এ আসল data যায় না; বাইরের নিরাপত্তা পরীক্ষা হয়। | V14: external security test শেষে G1-এর আগে open critical/high finding 0। | V14 |
| QLT-006 · Observability, gradual release and rollback | Release নিরাপদ ও ফেরানো যায়। | V19: staging-এর একই signed build production-এ গেছে ও build ID মেলে; rollback-এ data loss 0; alert পৌঁছায়। | V19 |

## ১৭. Proposal ও scope সিদ্ধান্ত

২৬ নভেম্বরের owner সিদ্ধান্ত অনুযায়ী UI proposal P1–P7 scope থেকে বাদ হয়েছে। নতুন proposal owner চাইলে `S1-PR-nn` ID-তে আলাদা সিদ্ধান্ত হিসেবে লিখবেন।

## ১৮. Open decisions ও pending কাজ

| ID | প্রশ্ন (সহজ ভাষায়) | Recommended default | উত্তর না আসা পর্যন্ত যা Pending |
|---|---|---|---|
| OD-1 | Backend ও web code কোন repository-তে থাকবে, আর কোন branch integration base? | **অবস্থা:** backend এলে আপনি জানাবেন (২৬ নভেম্বর ২০২৬)। Recommendation নেই, এটি আপনার জানানো তথ্য। Doc 02 বলছে existing backend ও app skeleton আগে check হবে। সেই repo ও base branch জানালে যাচাই করা হবে। বর্তমান docs-only `main` code base হবে শুধু যদি আপনি ঠিক করেন code এই repository-তে নতুন করে শুরু হবে; নইলে base হলো existing backend repository-র integration branch। | Implementation plan-এর প্রতিটি path; branch `feat/platform/tenant-product-foundation` তৈরি |
| OD-2 | Pilot মানে প্রথম বাস্তব school-এর প্রথম পূর্ণ term (D-01, V20)। সেই সময়ে operator school-এর কোন কোন status (lifecycle state) ব্যবহার করবেন? | **সিদ্ধান্ত (আপনি, ২৬ নভেম্বর ২০২৬):** pilot trial, active, restricted ও reactivation (restricted → active) ব্যবহার করবে। Grace, suspended ও offboarding বাকি R1-এ আসবে | — |
| OD-3 | School platform-এর subscription-এর টাকা না দিলে প্রথমে "restricted", পরে "suspended" হয়। তখন school-এর মানুষ কী কী পারবেন, তার জন্য Blueprint পৃ. ১৭-এর নিয়ম মানবেন কি? (Restricted: role অনুযায়ী read, export চলে, নতুন record নয়। Suspended: শুধু Owner sign in করে export বা pay করেন; family ও staff notice দেখে, তাই Guardian আগে প্রকাশিত result-ও খুলতে পারেন না।) | **সিদ্ধান্ত (আপনি, ২৬ নভেম্বর ২০২৬): Option A।** Blueprint পৃ. ১৭-এর rule চলবে: restricted-এ সবাই নিজের role-এর মধ্যে read করেন, export চলে, নতুন record নয়; suspended-এ শুধু Owner sign in করে pay বা export করেন, বাকি সবাই notice দেখেন। Client requirement 3.1.4, 12.2.3, 12.2.C ও E2E-09 শুধু বলে data থাকে, export সম্ভব থাকে ও billing কখনো record মোছে না; suspension-এ কে কী পড়বেন তা কোনোটি বলে না | মূল client SRS (DVL-001)-এর সঙ্গে তুলনা বাকি; সেখানে অন্য কিছু থাকলে সিদ্ধান্ত আবার দেখা হবে |
| OD-4 | প্রতিটি operator state-changing action-এ reason লাগে (§৭.৪)। এখনো খোলা: live school-এ access কমানোর change-এ কি দ্বিতীয় approver-ও লাগবে? Pilot-এ কতজন Super Admin থাকবেন? Blueprint ও client requirement-এ সংখ্যা বলা নেই। Requirement 12.4.1 platform-এ "privileged approval chains" চায়; দুই approval স্পষ্ট লেখা শুধু school মুছে ফেলায় (SAS-012, R2); Super Admin-এর MFA লাগবে (requirement 3.2.2, IAM-005)। | **সিদ্ধান্ত (আপনি, ২৬ নভেম্বর ২০২৬):** সুপারিশ গ্রহণ। Pilot এক Super Admin দিয়ে চলতে পারে (MFA, প্রতি change-এ reason)। দুজন বা তার বেশি operator হলে live school-এ access কমানোর change-এ PLT-007 দিয়ে second approver লাগবে, তাই P1-c সেই সময়ের আগে merge হতে হবে। Activation-এ second approver নয়। প্রথম branch শুধু staging-এ demo school নিয়ে চলে, তাই dependency cycle নেই | — |
| OD-5 | তিন ধরনের job-এ access বদলালে কী হবে? (i) এখনো claim হয়নি, বা chunk-এর মাঝে, আর confirm-এর পরে access বদলেছে (off-এর পর on হলেও)। (ii) Blocked job। (iii) আংশিক চলা job। | **সিদ্ধান্ত (আপনি, ২৬ নভেম্বর ২০২৬):** (i) গ্রহণ: confirm-এর পরে access বদলালে job blocked হয়: এখন বন্ধ থাকলে বর্তমান reason (Platform বা C3); মাঝে বন্ধ হয়ে আবার চালু হলে `access_changed`। নিজে কখনো চলে না। (ii) Option A: job বন্ধ থাকে, Registrar আবার preview চালান (Option B বাদ)। (iii) যে chunk commit হয়েছে তা থাকে; batch done ও not-run count দেখায়; পরের কাজ A8-এর rule অনুযায়ী (Students owner-এর সঙ্গে Pending)। Retry করতে পারেন শুধু সেই user, যাঁর ওই মুহূর্তে import right আছে; operator কখনো নয় | শুধু (iii): আংশিক চলা job-এর পরের কাজ, A8-এর rule (Students owner-এর সঙ্গে) |
| OD-6 | Login ও অনুমতির কাজ Identity (System 2) আর Platform-এর মধ্যে এভাবে ভাগ করা মানেন কি? C3 (Identity) session ও membership যাচাই করে; Platform school context বহন করে ও database-এ বসায়; operator role check C3-র, Platform consume করে। এর সঙ্গে: কেউ school বদলালে ওই device-এর পুরোনো session শেষ হয় (`session_ended`), এটি Blueprint পৃ. ২৫-এর C3 rule। | **সিদ্ধান্ত (আপনি, ২৬ নভেম্বর ২০২৬):** গ্রহণ। Platform আগে merge হয়, তাই প্রথম branch C3 আসা পর্যন্ত test session ব্যবহার করে; `session_ended`-ও P0-তে stand-in। Trade-off (System 2-এর জানানো, CTO cross-check CX-10): একজন school user এক browser-এর দুই tab-এ দুটি school খোলা রাখতে পারবেন না; operator console এতে প্রভাবিত নয় | — |
| OD-7 | Queued-job guard contract (নতুন)। সহজ ভাষায়: A8 প্রতিটি chunk-এর write transaction-এর ভেতরে Platform-এর guard ও C3-র check call করে; change ও revoke command একই row lock করে; chunk "শুরু" হয় যখন সে lock ধরে; C3 দেয় `AuthorizeInTx`, "confirmedAt-এর পরে revoke হয়েছে কি?" এবং account/membership state। Identity owner, Students owner ও Integration CTO কি মানেন? | **আপনার অনুমোদন:** ঠিক আছে (২৬ নভেম্বর ২০২৬)। System 2 তিনটি চাহিদাই দিতে পারবে বলে নিশ্চিত করেছে (System 2-এর plan §৫.৫); matrix cell-এর নিয়ম গৃহীত (§১৪.৪ e)। Identity ও Students owner-এর সম্মতি CTO-র cross-check-এ বাকি | T8-এর real binding; Implementation plan §৯.১ c, d, d2, g, h3, i, j2-এর real evidence |

**আরও যা Pending (source doc-এ নেই)।** Module-এর প্রকৃত তালিকা: §৮.৫-এ শুধু উদাহরণ আছে; প্রতিটি consumer system নিজের module key contract-এ দেবে। Plan ও currency-র নাম: §৮.৩ ও §৮.১০-এ শুধু উদাহরণ; D-08 অনুযায়ী amount operator console-এ বসান, code-এ নয়। Wireframe-এর school, operator ও ticket-এর নাম সাজানো উদাহরণ। Campus provisioning-এর পরে কে যোগ করবে, operator না School Admin, এবং PLT-003 (A1, Academics)-এর সঙ্গে তার সম্পর্ক কী: Pending (OD নয়; P0 আটকায় না, কারণ pilot-এ এক campus)। সিদ্ধান্ত না হওয়া পর্যন্ত campus শুধু provisioning wizard-এ তৈরি হয়।

## ১৯. Source

| Source | Link |
|---|---|
| Implementation plan | [IMPLEMENTATION_PLAN.md](IMPLEMENTATION_PLAN.md) |
| MVP release ও branch plan (Doc 02) | [02-mvp-release-and-branch-plan.md](../../02-mvp-release-and-branch-plan.md) |
| Module ও data map (Doc 03) | [03-system-module-and-data-map.md](../../03-system-module-and-data-map.md) |
| Roles ও permissions (Doc 04) | [04-roles-and-permissions.md](../../04-roles-and-permissions.md) |
| Workflows ও UI/UX (Doc 05) | [05-workflows-and-ui-ux.md](../../05-workflows-and-ui-ux.md) |
| School policies ও integrations (Doc 06) | [06-school-policies-and-integrations.md](../../06-school-policies-and-integrations.md) |
| QA, pilot ও acceptance (Doc 07) | [07-qa-pilot-and-acceptance.md](../../07-qa-pilot-and-acceptance.md) |
| Migration, launch ও handover (Doc 08) | [08-migration-launch-and-handover.md](../../08-migration-launch-and-handover.md) |
| Interfaces (student import walkthrough) | [INTERFACES.md](../../integration/INTERFACES.md) |
| System 1 brief ও acceptance check | [SYSTEM-AGENT-BRIEFS.md](../../integration/SYSTEM-AGENT-BRIEFS.md) |
| Dependencies ও merge order | [DEPENDENCIES.md](../../integration/DEPENDENCIES.md) |
| CTO handoff | [CTO-HANDOFF.md](../../integration/CTO-HANDOFF.md) |
| Feature tracker | [features.csv](../../tracker/features.csv) |

**Blueprint v2 পৃষ্ঠা যা ব্যবহার হয়েছে:** পৃ. ৬–৭, ১০–১১, ১২–১৫, ১৭, ২৪–২৬, ৩২, ৩৪–৩৬, ৪৪, ৪৬, ৪৮, ৬৩–৬৪, ৭৫–৭৬, ৯৭–৯৮, ১০১, ১০৪–১০৬। Blueprint PDF এই repo-তে নেই। এই planning session-এ স্থানীয় PDF থেকে পৃষ্ঠাগুলো মিলিয়ে নেওয়া হয়েছে। এগুলো source baseline, কোনো code repository-তে verify হয়নি।

## ২০. Review-এর পরে সংশোধন (২৬ নভেম্বর ২০২৬)

Owner-এর reviewer-এর findings যাচাই করে সব গ্রহণ করা হয়েছে (OD-1 সংক্রান্ত মন্তব্য সংকীর্ণ করে)। নিচে প্রতিটি finding, কী বদলেছে ও কোথায়।

| Finding (severity) | কী বদলেছে | কোথায় |
|---|---|---|
| Design Major 1: revert, grant ও course, মাঝের change | Revert শুধু একই target-এর সর্বশেষ change-এর জন্য (`revert_not_latest`); revert শুধু entitlement বদলায়; window summary শুধু count; grant ও course কারা review করবেন; revert যা ফেরাতে পারে না তা স্পষ্ট বলা | §৭.৪, §৭.৫, §৮.৮, §৯, §১৪.২ |
| Design Major 2: file link ও তিন ধরনের job | প্রতিটি download-এর সময় permission ও entitlement check; signed link-এর মেয়াদ PLT-013-এ Pending; job-এর write guard, lock, claim ও `job_integrity`; OD-5 তিন ধরনের job-এ; decision cache নেই | §১৪.৩, §১৪.৪, §১৮ |
| Design Major 3: operator বনাম support session, মাপযোগ্য বন্ধ-উত্তর, row অনুযায়ী প্রমাণের ধাপ, একই record ID | Normal operator session আর support session আলাদা; বন্ধ access-এর উত্তর `product_off` / `module_off` ইত্যাদি code-সহ table; §১৪.৫-এ "P0-তে কীভাবে প্রমাণ" ও "পূর্ণ প্রমাণ কখন" column; off → on-এর পর একই record ID ও content checksum | §২.২ A5, §১৪.১, §১৪.২, §১৪.৫, §১৬ |
| Design Major 4: provisioning-এর ফল অজানা | একটি draft-এ একটি key, "Result unknown" ও "Check result", duplicate-code error বিদ্যমান school-এর নাম বলে | §৭.২, §৯, §১৩ |
| Design Minor 5: session note, Proposed fixture, Blueprint baseline | Folder row session note হলো; Blueprint list "লেখকের উদ্ধৃত baseline" হিসেবে চিহ্নিত ও ফাঁকের block তার পরে সরানো; fixture **Proposed** | §২.১, §৩, §৩.৩ |
| Design Minor 6: ছোট screen-এ পরিচয়, `border.control` | ছোট screen-এ code + ছোট করা নাম + state chip, tap-এ পুরো নাম ও city; `border.control` token ও contrast হিসাব; 320 px reflow ও 200% text zoom | §১০, §১১, §১২ |
| Narrative: activation-এও typed code | প্রতিটি বিদ্যমান school-এর change-এ reason ও typed code, activation-সহ; mismatch-এ button disabled | §৭.৪, §৮.৭ |
| Narrative: আগে থেকে role থাকা member | Activation-এ "Members who will get access immediately" count; দুই পরিস্থিতির table | §৭.৪, §৭.৫, §৮.৬ |
| Narrative: school-day-র মাঝে ভুল switch-off | নতুন recovery case ৯; school-side "Access ফেরার পরে" row; drill-এর criterion | §৭.৫, §৭.৮, §১৩, §১৬ |
| Narrative: operator-এর content দেখা | Operator session কখনো pupil-level content দেখায় না; support session আলাদা | §২.২ A5, §১৪.১ |
| Part C: row ১১–১২ ও প্রমাণ column | নতুন row ১১ ও ১২; দুটি নতুন column | §১৪.৫ |
| Part C: P0 criterion channel ও task-এ বাঁধা | ছয়টি P0 criterion API, job বা UI channel ও task (T6, T7, T12) অনুযায়ী লেখা | §১৬ |
| Part C: result unknown state | Wizard-এর state ও screen state table-এ যোগ | §৯, §১৩ |

**আপনার উত্তর (২৬ নভেম্বর ২০২৬)।** OD-5: Option A। OD-7: অনুমোদিত; Identity ও Students owner-এর সম্মতি CTO-র cross-check-এ বাকি। OD-1: backend এলে জানাবেন। OD-2, OD-3, OD-4 ও OD-6-এর প্রশ্ন §১৮-এ সহজ ভাষায় আবার লেখা হয়েছিল; পরে এগুলোও ঠিক হয়েছে (নিচে)। এখন শুধু OD-1 খোলা।

**CTO cross-check CX-11 (২৬ নভেম্বর ২০২৬)।** System 2-এর শর্ত মিলিয়ে §১৪.৪-এ যোগ হয়েছে: পুরো system-এর lock ক্রম Platform row → C3 row (c); guard ও change command দুই দিকেই lock timeout (h); C3-এর শর্ত ও call-এর ধরন ঠিক করার সময় (j)।

**Re-review-এর পরে (২৬ নভেম্বর ২০২৬)**

| Finding (severity) | কী বদলেছে | কোথায় |
|---|---|---|
| Blocker — C3 revoke তারপর আবার grant | Job guard C3-র ক্ষেত্রেও `access_changed` দেয়: C3 এখন refuse করলে C3-র reason; allow করলেও confirm-এর পরে role, grant বা membership revoke বা suspend হয়ে থাকলে (পরে ফেরত দিলেও) `access_changed`। OD-5-এর শব্দ সব জায়গায় একই | §১৪.৪ (e), §১৮ OD-5 |
| Major — decision instant ও change command | সব lock আগে এক global ক্রমে, তারপর authoritative সময় ও version পড়া, তারপর decide; operator-এর change command শুরু থেকেই exclusive lock নেয়, upgrade নেই; একই target-এ দুটি change হলে একটি জেতে, অন্যটি 409 `version_conflict` | §১৪.৪ (b), (c), (d), (h) |
| Major — claim recovery | Claim lease, fencing ও checkpoint-এর নিয়ম Implementation plan-এ; এই plan-এ §১৪.৪ (f), (g) অপরিবর্তিত, আর §১৮ OD-7-এর Pending cell-এ নতুন variant-এর তালিকা | §১৮ OD-7 |
| Major — operator key scope | বিদ্যমান school-এর command-এর scope: operator accountRef, operation, target schoolId ও (path-এ থাকলে) changeId; provisioning-এর scope: operator accountRef ও `provision` | §১৪.২ |
| Major — download | প্রতিটি protected download-এর মুহূর্তে current permission ও entitlement; API stream বা কয়েক সেকেন্ডের এক-বার link; আগের link অচল; বেশি মেয়াদের CDN link MVP-তে নেই | §৭.৫, §১৪.৩, §১৬ PLT-013 |
| Major — API ও job-এর ফল আলাদা | `product_off`/`module_off`-এ API 404 ও job `blocked` আলাদা লেখা; response table-এ job-এর দুটি row; `version_conflict` body-তে শুধু tenancy data; PRD-001, PLT-005 ও SAS-002 criterion channel অনুযায়ী | §১৪.২, §১৬ |
| Major — drill ও recovery | Fresh roster-এ register একবার গৃহীত, stale roster-এ review; নতুন `job_integrity` state row | §৭.৫ case ৯, §৭.৮, §১৩ |
| Minor | §১৪.৫ row ১১ ও ১২ Implementation plan §৯.১ j1 ও j2-এর সঙ্গে যুক্ত; server-side `confirmCode` check (**Proposed**) ও 422 `confirm_code_mismatch` | §৭.৪, §১৪.২, §১৪.৫ |
| OD-2 (সিদ্ধান্ত) | Pilot-এ trial, active, restricted ও reactivation; grace, suspended ও offboarding বাকি R1-এ | §১, §৭.৬, §৮.৯, §১৫, §১৬ PLT-004, §১৮ |
| OD-3 (সিদ্ধান্ত) | Option A: Blueprint পৃ. ১৭-এর rule; client requirement মিলিয়ে দেখা হয়েছে; মূল SRS (DVL-001)-এর সঙ্গে তুলনা বাকি | §৭.৮, §১৪.২, §১৮ |
| OD-4 (সিদ্ধান্ত) | সুপারিশ গ্রহণ: pilot-এ এক Super Admin; দুজন বা বেশি operator হলে PLT-007 দিয়ে second approver, তাই P1-c আগে merge | §৭.৪, §৮.৭, §১৬ PLT-007, §১৮ |
| OD-6 (সিদ্ধান্ত) | গ্রহণ; trade-off: এক browser-এর দুই tab-এ দুটি school নয় (operator console বাদে) | §১৪.২, §১৮ |

**CTO cross-check CX-11 বিস্তার ও CX-14 (২৬ নভেম্বর ২০২৬)।** C3-এর lock ক্রম (account → membership → role grant → guardian link → custody restriction row) ও "matrix lock হয় না" যোগ (§১৪.৪ b)। Operator change command C3-এর row lock করে না, C3-এর change command Platform-এর row lock করে না, lock inspection test-এ এই সীমা যাচাই হবে (§১৪.৪ c)। System 2-এর প্রস্তাব গৃহীত: permission matrix-এর role × action cell বদলালেও `access_changed` (§১৪.৪ e)। Revoke-এর পরে আবার দেওয়া ধরা পড়ার নিশ্চয়তা ও guardian link write-এর পদ্ধতি (CX-14) §১৪.৪ j-তে।

**CTO cross-check-এর alignment (২৬ নভেম্বর ২০২৬)।** (১) তিন system-এর একটিই versioned shared fixture file (CX-07); ১২টি decision row বর্তমান সংখ্যা; module key ও C3 action আলাদা (§১৪.৫)। (২) PLT-006, PLT-007 ও PLT-013 G1 staging-এর আগে merge, কারণ Identity 1b এগুলোর উপর নির্ভর করে (CX-01; §১৬ P1)। (৩) PR-06 অনুমোদিত হলে SAS-013-এর ন্যূনতম quota ও priority class pilot-এ (CX-13; §১৬ SAS-013)। (৪) PLT-007-এর operator gate আর C3-এর custody ও privileged-role gate আলাদা (CX-08; §১৬ PLT-007); guardian link write-এর সিদ্ধান্ত Identity-র T-00-এর আগে নয় (CX-14; §১৪.৪ j)।

**২৬ নভেম্বর ২০২৬ · Owner-এর PDF scope review প্রয়োগ।** UI proposals P1–P7, typed confirmation, impact counts ও dedicated revert flow বাদ। School context ও reason (PLT-006, পৃ. ৩২ ও ১০১), scheduled change cancellation (SAS-002, effective-dated plan, পৃ. ৯৭) এবং দুই বা বেশি operator-এর access-reducing change-এ দ্বিতীয় approver (PLT-007, পৃ. ১০১ ও requirement 12.4.1) রাখা হয়েছে। আগের review-এর entries ইতিহাস হিসেবে পড়ুন। C3 lock order-এর শেষ ধাপ custody restriction rows (CX-11)।
