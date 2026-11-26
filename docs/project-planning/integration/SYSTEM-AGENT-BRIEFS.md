# System team-এর planning brief

**তিনজন System Agent-এর দায়িত্ব।** ১ নম্বর Agent Multi-tenant Platform, ২ নম্বর Agent Identity and Permissions, ৩ নম্বর Agent Student Records and Admissions-এর দায়িত্বে। Project owner প্রত্যেকের সঙ্গে context নিয়ে কথা বলবেন। System Agent নিজের design ও implementation plan review করবেন। Project owner coding-এর দায়িত্ব দিলে সেই Agent বাস্তবায়ন, code review, test ও fix শেষ করবেন। CTO completion report যাচাই করে প্রস্তুত PR merge করবেন। [Report নেওয়ার ক্রম](DEPENDENCIES.md#cto-completion) · [একই student import-এর উদাহরণ](INTERFACES.md#student-walkthrough)

**Prompt দেওয়ার নিয়ম।** প্রতি system-এর জন্য একটি English prompt দিন। Agent প্রথমে context বুঝে সংক্ষেপে জানাবেন, তারপর একই run-এ design plan ও সেই design ধরে draft implementation plan দেবেন। সিদ্ধান্ত বদলে দিতে পারে এমন তথ্য ছাড়া assumption লিখে এগোবেন। কোনো গুরুত্বপূর্ণ উত্তর বাকি থাকলে প্রশ্ন করবেন এবং যে অংশ তার ওপর নির্ভর করে সেটি pending রাখবেন। Agent নিজেই দুই plan-এর জন্য আলাদা review prompt দেবেন। Owner design বদলালে প্রভাবিত কাজগুলো আপডেট করবেন। Implementation শুরু করতে বললে Agent আগে owner-এর কাছে আলাদা feature branch তৈরি বা বরাদ্দের অনুরোধ করবেন। Owner branch নিশ্চিত করে ওই branch-এ coding-এর অনুমতি দিলে Agent আলাদা worktree বা checkout-এ কাজ শুরু করবেন। [Model ও subagent-এর নিয়ম](CTO-HANDOFF.md#model-and-token)

Chat ও docs দুজায়গাতেই একটি copy block-এর মধ্যে context, design ও implementation draft এবং review-এর section দিন। প্রতিটি section-এর আগে ফাঁকা লাইন ও একটি ছোট divider রাখুন।

**উৎস** হলো Blueprint v2-র feature/role/release, tracker-এর origin ও QA gate। **পরিকল্পনার সিদ্ধান্ত** হলো Doc 02-এর MVP ভাগ এবং কোন ছোট deliverable আগে নেব। **Proposed** মানে যাচাইয়ের জন্য নতুন sample বা interface shape। Backend, app skeleton ও integration base-এর অবস্থা **Repository verification pending**। নিচের সব acceptance check ভবিষ্যৎ পরীক্ষার প্রত্যাশিত ফল। [Doc 01](../01-requirements-and-feature-tracker.md) · [Feature tracker](../tracker/features.csv)

## কোন system-এর কোন module MVP-তে

মাঝের কলামে সেই module-গুলি আছে যেগুলিতে অন্তত একটি প্রস্তাবিত MVP feature আছে। প্রতি module-এ ঠিক কোন Feature ID নিতে হবে তা দেখতে `features.csv`-তে `System` ও `Module ID` ধরে `Proposed scope` দেখুন। **MVP candidate** হলে পুরো feature প্রস্তাবিত। **MVP partial** হলে pilot-এর অংশ এবং বাকি R1 কাজ আলাদা লিখুন। একই module-এর backlog feature তার নির্ধারিত release-এ থাকবে। [Doc 03-এ ৫৩টি module-এর নাম ও দায়িত্ব](../03-system-module-and-data-map.md) · [Feature tracker](../tracker/features.csv)

| System | MVP feature আছে যে module-এ | পুরো module backlog ও target release |
|---|---|---|
| 01 Multi-tenant Platform | C1, C2, C9, C10 | — |
| 02 Identity and Permissions | C3 | — |
| 03 Student Records and Admissions | A7, A8 | A13 (R2/R3) |
| 04 Academics and Attendance | A1, A2, A3, A4, A5, A6, A9 | A14 (R1/R2) |
| 05 Exams and Results | A10, A11, A12 | — |
| 06 Desk Learn | B1, B2, B3, B4, B5, B6, B7, B8, B10, B11, B12, B14 | B9 (R2/R3) |
| 07 Fees and Accounts | A16, A17, B13 | — |
| 08 Apps Communication and Reports | C4, C5, C6 | A27 (R2/R3) |
| 09 School Operations | MVP-তে নেই | A15 (R2/R3), A18 (R4), A19 (R4), A20 (R4/R5), A21 (R4), A23 (R4/R5), A24 (R3/R4), A25 (R4/R5), A26 (R5) |
| 10 Extensions and Advanced Features | C8 | A22 (R5), A28 (R2/R5), B15 (R2/R3), C7 (R5) |

মোট ৩৬টি module-এ MVP কাজ আছে, ১৭টি module পুরো backlog। A14-এর Pre-KG narrative report G1 staging-এ লাগবে। Pilot-এ KG থাকলে তার scope review হবে। B15-এর default connected mode R2। Pilot school Moodle চালু রাখলে D-01 অনুযায়ী প্রয়োজনীয় R1 sync review হবে। [Doc 02](../02-mvp-release-and-branch-plan.md) · [Doc 07](../07-qa-pilot-and-acceptance.md)

### প্রথম তিন System Agent-এর Feature ID তালিকা

| Agent | Module | MVP candidate | MVP partial |
|---|---|---|---|
| ১ | C1 Products, plans and entitlements | PRD-001–005, PRD-007, PLT-005, SAS-002 | PRD-006 |
| ১ | C2 Operator console | PLT-001, SAS-001, SAS-004, SAS-005, SAS-013, SAS-014, SAS-016 | PLT-004 |
| ১ | C9 Platform services | PLT-002, PLT-006, PLT-007, PLT-008, PLT-011, PLT-013, PLT-015, PLT-016, PLT-017, PLT-018, PLT-020 | — |
| ১ | C10 Security, privacy and reliability | QLT-001–006 | — |
| ২ | C3 Identity, roles and access | IAM-001, IAM-003, IAM-004, IAM-005, IAM-006, IAM-007, IAM-010, IAM-013, IAM-014 | IAM-002 |
| ৩ | A7 Admissions and enrolment | ADM-021 | — |
| ৩ | A8 Student records | SIS-001, SIS-006, SIS-007, SIS-010, SIS-016, PLT-014 | SIS-002 |

তৃতীয় Agent প্রথম placement-এর জন্য **SIS-003** ও class list-এর জন্য **CLS-001** উল্লেখ করবেন। দুটোই Academics A2-তে গণনা করা আছে। Students A8 placement history লেখে, Academics A2 তারিখ অনুযায়ী roster দেখায়। [Doc 03](../03-system-module-and-data-map.md) · [Import উদাহরণ](INTERFACES.md#student-walkthrough)

<a id="platform-first"></a>
## 01 · Multi-tenant Platform — প্রথম module-এর পূর্ণ brief

**C1 Products, plans and entitlements।** আজকের প্রশ্নটি হলো, “এই membership থেকে এই school-এর এই product ও module ব্যবহার করা যাবে কি?” একই উত্তর পেলে Identity তার ওপর role check বসাতে পারে এবং Students নিরাপদে import plan করতে পারে। **প্রথম deliverable** হলো import-এর জন্য tenant/product সিদ্ধান্ত, তার লিখিত contract ও পরীক্ষার cases। এর branch scope Doc 02-এর `feat/platform/tenant-product-foundation`। C2 school তৈরি করে, C9 school isolation দেয়। এই দুই dependency-কে C1 feature হিসেবে গণনা করা হবে না।

| ধাপ | Feature IDs ও scope | ফল ও acceptance |
|---|---|---|
| প্রথম branch-এর MVP | C1 **PRD-001, PLT-005**। C2 **PLT-001**, C9 **PLT-002**। Effective plan জানার C1 input **SAS-002**। সব R1 MVP candidate। | Super Admin দুই পৃথক school রাখেন। Server session-এর school-এ product ও module access নির্ণয় করে। Entitlement off হলে UI, direct API, import/job ও export তা মানে। V01/E2E-08, V18। |
| C1-এর পরের MVP integration | **PRD-002, PRD-003, PRD-004**। Scope পাবে দুই app, Identity ও Learn। | Entitlement ও role মিলে workspace আসে। School Admin আলাদা learning role পেলে course খুলতে পারেন। Class teacher ও course instructor grant আলাদা। V02/V10/V18। |
| টাকা ও class link | **PRD-005, PRD-007** R1 MVP candidate। Fees, Learn, Academics ও Results consumer। | School/course invoice, receipt ও reports নিজ নিজ book-এ। Class link default off। দুই product ও explicit school link থাকলে roster/assignment মেলে ও selected mark একবার যায়। Unlink করলে history থাকে। V05/V09/V10/V18। |
| **PRD-006 · MVP partial** | **Pilot-এ** প্রতি product-এর আলাদা plan ও manual subscription invoice (C2 SAS-004)। **বাকি R1-এ** পূর্ণ usage metering ও সংশ্লিষ্ট billing cases (C2 SAS-003)। | Pilot-এ Operator এক product-এর invoice করলে অন্য product charge পায় না। R1-এ active pupil/learner per term, meter ও দুই product-এর invoice মিলবে। V18 এবং G1/PRD-006-এ দুই অংশের evidence পৃথক। |
| C2 dependency **PLT-004 · MVP partial** | **Pilot-এ** agreed lifecycle states operator প্রয়োগ করেন। **বাকি R1-এ** সব scripted transition ও access rule যাচাই হবে। | Restricted school-এ অনুমোদিত read/export, writes সীমিত, data থাকে। Reactivation-এ service ফেরে। E2E-09 G1 operator-applied, G2 automatic expiry। |

**Release সীমা।** C1-এর ৯টি feature-ই R1। প্রথম branch-এ দুই product-এর billing, marks ও apps-এর পূর্ণ acceptance দাবি করা হবে না। সেই evidence সংশ্লিষ্ট consumer-এর MVP flow-তে আসবে। SAS-006/007 automated lifecycle/billing R2। LRN-082/085 বাইরের learner ও training centre R2। SAS-020 marketplace R5। School-specific plugin activation-এর contract পরে কাজে লাগবে। [Doc 02](../02-mvp-release-and-branch-plan.md) · [Doc 06](../06-school-policies-and-integrations.md) · Blueprint পৃ. ৭৫–৭৬, ৯৭–৯৮, ১০৪, ১০৬।

**Inputs।** C2 school/campus reference, C3 active membership-এর sample, C1 product/plan/flags ও effective date, proposed action, C2 lifecycle state, school-এর restriction/export policy। Test-এ Super Admin, Registrar, School Admin ও Learning Admin থাকবে। V01-এ A ও B দুই school-এই School Management ও নিজস্ব Registrar রাখুন। প্রত্যেকে নিজের school-এ সফল হওয়ার পর অন্যটির ID দিয়ে একই request চালান। V18-এ একই fixture-এর entitlement বদলে School-only, Learn-only ও both চালান। এরপর link off/on ও product off/on আলাদা করে দেখুন। R1-এর Learn-only test-এ school-এর নিজস্ব learner থাকবে। টাকা, password বা বাস্তব pupil data দরকার নেই।

**Expected result।** Platform owner প্রতি request-এ বিশ্বস্ত school context ও কার্যকর product/module-এর সিদ্ধান্ত দেন। C3-এর role/record decision মিললে তবেই Students action চালাতে পারে। Planner দেবেন producer/consumer table, example inputs ও expected decisions, allowed/denied test matrix, fixture, source references এবং repo-তে কী মেলাতে হবে তার তালিকা। [Contract-এর নমুনা](INTERFACES.md) · [Doc 04](../04-roles-and-permissions.md)

| প্রথম slice-এর acceptance check | Pass হলে যে ফল দেখা যাবে | Evidence reference |
|---|---|---|
| A-র Registrar নিজের campus-এ A-র student import চান। B-র school ID request-এ বসিয়েও চেষ্টা করেন। | A-তে entitlement মেলে। B-র record অস্তিত্ব প্রকাশ করে না। UI/API, search/file/export ও queued job-এ A/B isolation থাকে। | V01, E2E-08, G1/PLT-001, G1/PLT-002 |
| School-only, Learn-only ও both-এ দুই product ও একটি disabled module-এর read/write চান। | Config অনুযায়ী route, workspace ও actions মেলে। Learn-only-তে school import বন্ধ। Both-এও আলাদা role লাগে। Campus mismatch C3 আটকায়। | V02, V18, G1/PRD-001, G1/PLT-005 |
| Effective date-এর আগে/পরে plan বদলান। Desk Learn বন্ধ করে আবার খোলেন। | ঠিক সময় থেকে access বদলায়। Closed product-এর direct API-ও বন্ধ। আবার খুললে আগের courses ফেরে, School Management-এর data ও access তার policy ধরে চলে। | V18, G1/SAS-002, D-08/19/20 |
| একই person school বদলান। Product বন্ধ বা membership revoked হওয়ার আগে import preview হয়েছিল, পরে confirm আসে। | পুরোনো school cache/session দিয়ে data আসে না। Proposed confirm/job check বর্তমান grant ও entitlement আবার দেখে, blocked write-এ student বা placement তৈরি হয় না। | V01/V02/V18, consumer contract cases |

**Trace।** Requirement 12.1.2 → PRD-001/PLT-001/PLT-005, 1.1.2 ও E2E-08 → PLT-002, 12.2.1 → SAS-002, 12.2.2 → PRD-006। DVL-004 (owner map), DVL-006 (contract), DVL-007 (isolation design) requirement 18.1.2-এর deliverables। DVL-014-এ পরীক্ষার ফল যাবে। PRD-002/004/005 tracker-এ *Added from practice*। প্রতিটির নিজস্ব feature acceptance case আছে। [Requirements](../tracker/requirements.csv) · [Links](../tracker/requirement-links.csv) · [Deliverables](../tracker/deliverables.csv)

<a id="system-1-prompts"></a>
**System 1 · একটি planning prompt copy করুন**

```text
You own Desk School System 1 — Multi-tenant Platform.

Complete this in one planning run. Write the two plans in plain Bangla, address me as “আপনি”, and keep Feature IDs and familiar English technical terms. During planning, write only the two documents below. Keep source code, config, dependencies, schema, migrations and Git branches untouched. Leave secret files and credentials unread. Use the assigned main model for decisions. If useful and supported, delegate targeted read-only evidence collection to a smaller subagent.

------------------------

1. Understand the context

Read the relevant Docs 02–08, tracker/features.csv and integration/INTERFACES.md under docs/project-planning/. Scope covers C1 Products, plans and entitlements, C2 Operator console, C9 Platform services and C10 Security, privacy and reliability. Use one Registrar importing a student to explain who sets the school and product, what differs across School-only, Learn-only and both, and what C3 and A8 receive. Summarize MVP and backlog. State assumptions. Ask about decisions that would change scope, ownership or access, and mark the affected work pending.

------------------------

2. Create the design plan

Act as a senior UI/UX designer, product strategist and feature architect. Audit existing screens, roles, features and journeys. Separate observed findings from assumptions. If the checkout lacks code or screens, mark that evidence as pending.

Write docs/project-planning/system-plans/01-platform/UI_UX_DESIGN_PLAN.md. Define information architecture, flows, screen layouts, reusable components, design tokens, responsive behavior, accessibility and loading, empty, error and success states. Show school selection, provisioning, product changes, confirmation and recovery in the operator console. Define tenant isolation and C3/A8 contracts across the UI, API, files and jobs. Separate the pilot and R1 portions of PRD-006 and PLT-004. Prioritize agreed MVP work as P0/P1/P2 with user value and measurable acceptance criteria. Keep new features as proposals and preserve scope and release labels.

------------------------

3. Draft the implementation plan

Act as a senior software architect. Read the design draft above and inspect the relevant architecture, routes, components, APIs and tests. Write docs/project-planning/system-plans/01-platform/IMPLEMENTATION_PLAN.md. Mark it pending owner review. Use the existing stack and conventions. Order tasks by dependency. For each task link the design requirement and Feature ID, list verified or proposed paths, describe the change, and define acceptance and verification. Flag open decisions and avoid speculative estimates. Include API, schema or auth changes only where needed. Identify reuse, migration risks, rollout, rollback and responsive, accessibility and regression checks.

Start with PRD-001, PLT-005, PLT-001 and PLT-002. Verify V01 cross-school isolation and V18 toggle and restore, including an import queued before access is revoked. Proposed first branch is feat/platform/tenant-product-foundation. Record the verified integration base or mark it pending.

------------------------

4. Return review prompts and wait

In your reply, provide two copy-paste English review prompts, one for each plan. Reference the file, sections, Feature IDs and evidence. Use wrong-school activation for the design review and the queued import after revocation for the implementation plan review. Ask reviewers for findings in Bangla with impact, a fix and readiness. Tell me which document to attach to each prompt.

A request to start implementation triggers a branch check. First ask me to create or assign a dedicated feature branch from the verified integration base, and wait for me to confirm the branch and authorize coding on it. Then verify the branch and use a separate worktree or checkout for coding. Plan approval alone is review sign-off.
```

## 02 · Identity and Permissions

**C3 Identity, roles and access।** প্রথম branch `feat/identity/role-guardian-permissions`। IAM-003 scoped role, IAM-004 guardian link/rights, IAM-007 relationship শেষে access বন্ধ ও IAM-014 published action/role matrix হলো প্রথম deliverable। IAM-014-এর origin *Added from practice*। 3.2.3 → IAM-003, 5.1.2/E2E-14 → IAM-004, 3.2.6 → IAM-007। [Doc 04](../04-roles-and-permissions.md) · [Doc 07](../07-qa-pilot-and-acceptance.md) · Blueprint পৃ. ৩১–৩৪, ৯৮, ১০২–১০৩।

- **Inputs।** Platform-এর school/product decision, active session/membership, action, campus/record ID, প্রয়োজন হলে child, right, relationship version ও approval। A8 দেয় student ID ও household reference। দুই school, এক person-এর একাধিক role ও তিন child-এর ভিন্ন অধিকার দিয়ে plan হবে।
- **Expected result।** C3 caller-কে server-side decision দেয় এবং authorised Registrar-এর command-এ guardian account–child link ও rights রাখে। A8-এর household contact একা grant দেয় না। Self-approval বন্ধ। School membership বা relationship শেষ হলে current access প্রত্যাহার হয়।
- **Acceptance।** Registrar নিজের campus-এ import/placement পারেন, অন্য campus, marks/ledger/health detail বন্ধ। Guardian তিন child-এর permitted view-ই পান। Custody document ও দ্বিতীয় approver-এর পরে screen, notice ও pickup list সঙ্গে সঙ্গে restriction মানে। Revoked/expired session-এ read/write বন্ধ। V02/E2E-14/X-06, V03, V04। V15 scoped support/child privacy প্রযোজ্য।
- **Release।** IAM-002 partial-এ pilot-এ individual invite/activation, বাকি R1-এ bulk onboarding। IAM-001/005/006/010/013 অন্য R1 MVP candidate। IAM-012 biometric unlock R1 backlog। IAM-008/011/015 school SSO, class-card ও custom role R2। IAM-009 SAML R5।

<a id="system-2-prompts"></a>
**System 2 · একটি planning prompt copy করুন**

```text
You own Desk School System 2 — Identity and Permissions.

Complete this in one planning run. Write the two plans in plain Bangla, address me as “আপনি”, and keep Feature IDs and familiar English technical terms. During planning, write only the two documents below. Keep source code, config, dependencies, schema, migrations and Git branches untouched. Leave secret files and credentials unread. Use the assigned main model for decisions. If useful and supported, delegate targeted read-only evidence collection to a smaller subagent.

------------------------

1. Understand the context

Read the relevant Docs 02–08, tracker/features.csv and integration/INTERFACES.md under docs/project-planning/. Scope covers C3 Identity, roles and access. Use one Registrar and a guardian with three children to explain campus scope, who can import, and who records a household contact versus who grants access to each child. Summarize MVP and backlog. State assumptions. Ask about decisions that would change scope, ownership or access, and mark the affected work pending.

------------------------

2. Create the design plan

Act as a senior UI/UX designer, product strategist and feature architect. Audit existing screens, roles, features and journeys. Separate observed findings from assumptions. If the checkout lacks code or screens, mark that evidence as pending.

Write docs/project-planning/system-plans/02-identity/UI_UX_DESIGN_PLAN.md. Define information architecture, flows, screen layouts, reusable components, design tokens, responsive behavior, accessibility and loading, empty, error and success states. Cover invite, sign-in, school and role switching, privileged MFA, delegation, revocation and recovery. Map Registrar import and guardian child switching to actor, action, scope, approval, allow or deny, and the screen state. Separate IAM-002 pilot invites from R1 bulk onboarding. Prioritize agreed MVP work as P0/P1/P2 with user value and measurable acceptance criteria. Keep new features as proposals and preserve scope and release labels.

------------------------

3. Draft the implementation plan

Act as a senior software architect. Read the design draft above and inspect the relevant architecture, routes, components, APIs and tests. Write docs/project-planning/system-plans/02-identity/IMPLEMENTATION_PLAN.md. Mark it pending owner review. Use the existing stack and conventions. Order tasks by dependency. For each task link the design requirement and Feature ID, list verified or proposed paths, describe the change, and define acceptance and verification. Flag open decisions and avoid speculative estimates. Include API, schema or auth changes only where needed. Identify reuse, migration risks, rollout, rollback and responsive, accessibility and regression checks.

Start with IAM-003, IAM-004, IAM-007 and IAM-014. Identify C1 and A8 contracts. Use V02, V03 and V04 for two schools, three children, an expired session and a custody change. Proposed first branch is feat/identity/role-guardian-permissions. Record the verified integration base or mark it pending.

------------------------

4. Return review prompts and wait

In your reply, provide two copy-paste English review prompts, one for each plan. Reference the file, sections, Feature IDs and evidence. Use access to the wrong sibling for the design review and a notice queued before a custody restriction for the implementation plan review. Ask reviewers for findings in Bangla with impact, a fix and readiness. Tell me which document to attach to each prompt.

A request to start implementation triggers a branch check. First ask me to create or assign a dedicated feature branch from the verified integration base, and wait for me to confirm the branch and authorize coding on it. Then verify the branch and use a separate worktree or checkout for coding. Plan approval alone is review sign-off.
```

## 03 · Student Records and Admissions

**A8 Student records, A7 quick registration।** প্রথম branch `feat/students/registration-import`। ADM-021 (A7), SIS-001, SIS-002, PLT-014 (A8) প্রথম import flow। প্রথম placement SIS-003 ও roster CLS-001 দুটিই A2 feature, কিন্তু **placement writer Students A8**। Academics A2 class reference ও dated roster-এর owner। [Data map](../03-system-module-and-data-map.md) · [Import flow](INTERFACES.md#student-walkthrough) · [Doc 08](../08-migration-launch-and-handover.md)

- **Inputs।** Verified tenant ও School Management entitlement, C3 Registrar grant, approved student numbering/mapping, source row ও batch ID, guardian/account reference, A1/A2 school/campus/year/grade/section, effective date। UI-তে CSV preview, invalid row/duplicate review ও confirm। A7 দিয়ে একক registration-ও একই student owner-কে ডাকে।
- **Expected result।** A8 এক স্থায়ী student ও versioned household/placement রাখে। C3 অনুমোদিত guardian link লেখে। A2 ওই তারিখ থেকে section ও subject-group list বের করে। Pending link বা invalid class-এর row চেনা যায়। Existing student অন্য school-এর number দেখে merge হয় না।
- **Acceptance।** Dry run-এ write শূন্য। অজানা class ও broken link ধরা পড়ে। Confirm retry-তে এক record, guardian per-child limit মেলে। Effective date-এর দুপাশে roster, import undo ও reconciled counts দেখাতে হবে। দুই school-এ allowed/blocked import ও field-limited export পরীক্ষা হবে। V11/X-08-এ ১,০০০ pupil dry run/duplicate/undo, V01/V02/V04, E2E-08/14। DVL-012-তে mapping ও reconciliation, DVL-014-তে tests। Requirement 5.1.1, 5.1.2, 5.1.3, 5.1.5 ও 18.1.5-এর links অনুসরণ করো।
- **Release।** SIS-002 partial-এ pilot guardian, sibling ও emergency contacts। বাকি R1-এ pickup-contact coverage। IAM-004 custody check pilot-এই প্রযোজ্য। SIS-006/007/010/016 বাকি A8 MVP candidates। SIS-004/008 R1 backlog। SIS-009 R1 backlog, G1 section change ও G2 full campus transfer। Duplicate review PLT-014 R1, record merge SIS-012 R2। Full A7 admissions funnel R2 (D-15), A13 rollover/leaving R2, alumni handover R3।

<a id="system-3-prompts"></a>
**System 3 · একটি planning prompt copy করুন**

```text
You own Desk School System 3 — Student Records and Admissions.

Complete this in one planning run. Write the two plans in plain Bangla, address me as “আপনি”, and keep Feature IDs and familiar English technical terms. During planning, write only the two documents below. Keep source code, config, dependencies, schema, migrations and Git branches untouched. Leave secret files and credentials unread. Use the assigned main model for decisions. If useful and supported, delegate targeted read-only evidence collection to a smaller subagent.

------------------------

1. Understand the context

Read the relevant Docs 02–08, tracker/features.csv and integration/INTERFACES.md under docs/project-planning/. Scope covers A7 quick registration and A8 Student records. Keep full admissions and A13 in their tracker releases. Follow a Registrar CSV row to the first class roster. Explain who writes and reads the student ID, household contact, guardian grant and dated placement. Summarize MVP and backlog. State assumptions. Ask about decisions that would change scope, ownership or access, and mark the affected work pending.

------------------------

2. Create the design plan

Act as a senior UI/UX designer, product strategist and feature architect. Audit existing screens, roles, features and journeys. Separate observed findings from assumptions. If the checkout lacks code or screens, mark that evidence as pending.

Write docs/project-planning/system-plans/03-student-records/UI_UX_DESIGN_PLAN.md. Define information architecture, flows, screen layouts, reusable components, design tokens, responsive behavior, accessibility and loading, empty, error and success states. Show CSV preview, row correction, duplicate review, confirmation, pending guardian access, placement, dated roster and undo. A8 owns student and placement history, C3 owns access grants, and A2 owns class references and the roster read. Treat SIS-003 and CLS-001 as A2 dependencies. Separate SIS-002 pilot contacts from R1 pickup coverage. Prioritize agreed MVP work as P0/P1/P2 with user value and measurable acceptance criteria. Keep new features as proposals and preserve scope and release labels.

------------------------

3. Draft the implementation plan

Act as a senior software architect. Read the design draft above and inspect the relevant architecture, routes, components, APIs and tests. Write docs/project-planning/system-plans/03-student-records/IMPLEMENTATION_PLAN.md. Mark it pending owner review. Use the existing stack and conventions. Order tasks by dependency. For each task link the design requirement and Feature ID, list verified or proposed paths, describe the change, and define acceptance and verification. Flag open decisions and avoid speculative estimates. Include API, schema or auth changes only where needed. Identify reuse, migration risks, rollout, rollback and responsive, accessibility and regression checks.

Start with ADM-021, SIS-001, SIS-002 and PLT-014, including the C1/C3/A2 dependencies. Cover an invalid section, repeat confirmation, partial failure, undo and V11/X-08 reconciliation for 1,000 pupils. Proposed first branch is feat/students/registration-import. Record the verified integration base or mark it pending.

------------------------

4. Return review prompts and wait

In your reply, provide two copy-paste English review prompts, one for each plan. Reference the file, sections, Feature IDs and evidence. Use the Registrar’s duplicate-review experience for the design review and check that undo preserves an existing student and a later placement for the implementation plan review. Ask reviewers for findings in Bangla with impact, a fix and readiness. Tell me which document to attach to each prompt.

A request to start implementation triggers a branch check. First ask me to create or assign a dedicated feature branch from the verified integration base, and wait for me to confirm the branch and authorize coding on it. Then verify the branch and use a separate worktree or checkout for coding. Plan approval alone is review sign-off.
```

### পরের সাত system-এর সংক্ষিপ্ত brief

নিচে “pilot → বাকি R1” tracker-এর partial row-এর দুই অংশ বোঝায়। এক অংশের evidence দিয়ে পুরো feature Verified করা যাবে না। Feature owner পরে tracker-এর `MVP slice evidence` ও `R1 remainder evidence` আলাদা রাখবেন। [Doc 01](../01-requirements-and-feature-tracker.md) · [Features](../tracker/features.csv)

#### 04 · Academics and Attendance

A1/A2 থেকে class/year/section, A5/A6 থেকে routine ও assigned teacher। প্রথম deliverable A9-এ ATT-001–005। Students-এর dated placement থেকে CLS-001 roster নিয়ে assigned teacher offline register দেবেন। Reconnect-এ একবার submission, stale roster review, reason/approval সহ correction → guardian event (V07/V08/V11)। A14-এর GRD-008/021 R1 backlog ও G1 Pre-KG narrative sample রাখতে হবে। Early Years care R2, advanced schedule/attendance নিজ R2/R4 release-এ।

**PLT-003** এক campus ও দরকারি hierarchy → পুরো hierarchy-র cases। **SET-004** class/section/room → বাকি homeroom/house cases। **SET-005** pilot subject assignment → elective/stream। **CUR-001** pilot subject/unit → objectives, fields ও hours। **CUR-003** basic lesson/material → full lesson-plan fields। **TCH-001** pilot profile/allocation → qualification, grade band ও documents। [Doc 03](../03-system-module-and-data-map.md) · [Doc 07](../07-qa-pilot-and-acceptance.md)

#### 05 · Exams and Results

A10 gradebook, A11 exam, A12 published report। R1-এ scoped teacher, dated roster, approved grading sample ও optional B11 selected marks লাগে। GRD-001–007/010 দিয়ে missing বনাম zero, review, Principal approval, School Admin release ও correction version দেখাতে হবে (V05/V06)। A10–A12-এর advanced features নিজ R2–R5 release-এ।

**EXM-008** on-screen marks → print ও spreadsheet import। **GRD-007** pilot grades → Grades 1–12। **GRD-010** pilot batch/code → full school batch/release। **APP-012** report card ও receipt verify → certificate এবং প্রতিটি document-এর public field limits। [Doc 03](../03-system-module-and-data-map.md) · [Doc 05](../05-workflows-and-ui-ux.md)

#### 06 · Desk Learn

R1 B2 independent course দিয়ে শুরু। B1 Learning Admin, B3 instructor, B4 enrolment, B7 quiz ও B11 course gradebook যুক্ত হবে (LRN-051/062/071/077/006/115)। Learn entitlement লাগে। Independent course-এর জন্য school class লাগে না। Class link-এর জন্য দুই product, dated roster ও explicit school selection লাগে। Native pilot D-01। বাইরের learners R2 (D-21), Moodle connected R2 default, migration ও learning parity R3, পরের standards/features tracker অনুযায়ী।

**LRN-002** basic course page → visibility/scheduled release। **LRN-003** agreed materials → সব specified resource type। **LRN-005** submission/deadline/feedback → rubric ও late rules। **LRN-006** pilot quiz types → সব agreed R1 question type। **LRN-095** timing/window/attempt basics → বাকি review/shuffle সহ settings। [Doc 05](../05-workflows-and-ui-ux.md) · [Doc 06](../06-school-policies-and-integrations.md)

#### 07 · Fees and Accounts

A16 school fees, B13 course fees ও A17 student receivables R1। প্রথম deliverable FIN-006/010/014, CFE-002/005। C8 provider contract, verified payer/bill, separate prices/books ও finance policy লাগে। V09-তে provider-এর কাছে সরাসরি verification, amount/currency/reference match, retry-তে একবার receipt এবং আলাদা closing মেলাতে হবে। C1/C2 platform subscription আলাদা। **FIN-004 partial**-এ pilot bill creation, বাকি R1-এ agreed proration cases। দ্বিতীয় course settlement account CFE-011 R2, full general ledger FIN-026 R5। [Doc 03](../03-system-module-and-data-map.md) · [Doc 06](../06-school-policies-and-integrations.md)

#### 08 · Apps Communication and Reports

C4 guardian notice, C5 child switch/day view, C6 scoped reports R1। A9 event, C3 current rights, C8 provider ও C9 scoped file লাগে। ATT-005/COM-005/APP-001 দিয়ে per-event/guardian delivery, neutral preview ও correction যাচাই (V02/V08)। A27 helpdesk R2/R3, advanced apps/reports নিজ R2–R5 release-এ।

**COM-001** in-app/push/SMS fallback → email ও সব channel rules। **APP-002** class day, attendance, homework ও notice → সব day-view state। **APP-007** essential teacher web tasks → পুরো teaching workspace। **APP-009** pilot office screens → পুরো office workspace list। **APP-010** approval queue → full leadership dashboard। **RPT-001** core summaries → full pack ও PDF/XLSX/CSV। [Doc 03](../03-system-module-and-data-map.md) · [Doc 05](../05-workflows-and-ui-ux.md)

#### 09 · School Operations

প্রথম module A15, **R2 backlog**। BHV-005–007 support plan, escalation ও safeguarding case। লাগবে scoped student ID, assigned reviewer, school policy ও restricted access log। G2-তে পৃথক key ও permitted role দিয়ে case demo। A24-তে clubs/trips R3–R4, HR/library/clinic সহ মূল operations R4, boarding/alumni-সহ পরের অংশ R5। G4 appraisal privacy V25। [Doc 03](../03-system-module-and-data-map.md) · [Doc 07](../07-qa-pilot-and-acceptance.md)

#### 10 · Extensions and Advanced Features

C8-এর **INT-001–004 MVP/R1**। Versioned internal/app API ও events-এর contract প্রথম batch থেকেই review হয়। প্রথম provider deliverable payment/SMS adapters। Fees/C4 owner-এর expected outcome ও provider sandbox লাগবে। Verified repeat callback-এ এক posting, fallback-এ safe delivery ও retry evidence দিতে হবে। B15 Moodle ও A28 site R2 থেকে, Moodle migration/public API R3, device integration R4, A22 inventory/C7 AI R5। Pilot Moodle রাখলে D-01 review হবে। Future plugin-এর জন্য প্রতি school activation, scope, version ও deactivation record পরিকল্পনায় থাকবে। [Doc 03](../03-system-module-and-data-map.md) · [Doc 06](../06-school-policies-and-integrations.md)

### Evidence ফেরত দেওয়ার format

System Agent implementation, review ও test শেষে CTO-কে একটি completion report দেবেন। এতে থাকবে system ও module, assigned MVP Feature IDs, কোনগুলি শেষ বা বাকি, বাকি কাজের release, repo/PR ও target branch, যে commit/build-এ review ও test হয়েছে, এবং acceptance evidence। কোনো check ব্যর্থ বা চালানো বাকি থাকলে তার ফল ও কারণ লিখবেন। একাধিক PR-এ system-এর কাজ হলে কোন ID কোন PR-এ হয়েছে তা দেবেন। Plan ও review notes-এর link এই report-এই দেওয়া যাবে।

Report সহজ বাংলা হরফে লিখবেন। ব্যবহারকারীকে “আপনি” বলবেন, পরিচিত English technical terms ও Feature ID রাখবেন। প্রথমে সরাসরি ফল বলবেন। তারপর কী পরীক্ষা হয়েছে ও তার প্রমাণ, কী বাকি এবং পরের দায়িত্ব কার তা সংক্ষেপে দেবেন। Review prompt ব্যবহার করে পাওয়া findings-ও একই ভাষায় জানাবেন।

প্রতি check-এর সঙ্গে test ও requirement/feature ID, base/PR বা build, school/product/role, input, expected ও actual ফল, evidence link, status ও reviewer দেবেন। Unverified case pending থাকবে। Source test আর proposed check-এর পরিচয় রাখবেন। Partial feature-এর pilot ও বাকি R1-এর ফল দুই জায়গায় যাবে। Branch pass হওয়ার পরে full V-test, G1 staging, live pilot V20 ও client acceptance-এর নিজস্ব evidence লাগবে। [Doc 07](../07-qa-pilot-and-acceptance.md) · [Tracker deliverables](../tracker/deliverables.csv)
