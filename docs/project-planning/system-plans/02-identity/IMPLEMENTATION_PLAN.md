# System 2 · Identity and Permissions — Implementation plan

**অবস্থা:** Draft v4 · ২৬ নভেম্বরের owner সিদ্ধান্ত যাচাই করে যুক্ত · owner review-এর জন্য প্রস্তুত
**তারিখ:** ২৬ নভেম্বর ২০২৬
**Module:** C3 Identity, roles and access
**প্রথম branch (Proposed):** `feat/identity/role-guardian-permissions`
**Verified integration base:** **Pending** (§২)
**যে design ধরে লেখা:** [UI/UX design plan](UI_UX_DESIGN_PLAN.md)

**Label পড়ার নিয়ম।** **Verified** মানে এই checkout-এ নিজে দেখে মিলিয়েছি। **উৎস** মানে Blueprint v2, tracker বা Docs 02–08 থেকে নেওয়া, কিন্তু code-এ মেলানো হয়নি। **Proposed** মানে নতুন প্রস্তাব। **Pending** মানে সিদ্ধান্ত বা প্রমাণ বাকি। DR, OD, AS ও PR নম্বরগুলো design plan-এর §৪, §৫ ও §১৭ থেকে নেওয়া।

এই plan-এ কোনো সময়ের অনুমান নেই। Backend দেখার আগে সময় বলা অনুমাননির্ভর হবে।

## ১. এক নজরে

- প্রথমে চারটি feature: **IAM-014 → IAM-003 → IAM-004 → IAM-007**। Matrix আগে, কারণ বাকি সব কাজ তার action-এর নাম ব্যবহার করে।
- এই checkout-এ code নেই। তাই প্রতিটি path **Proposed**, এবং প্রথম task (T-00) হলো repo ও contract যাচাই। T-00 শেষ না হলে কোনো code task শুরু হবে না।
- C3 তিনটি জিনিস দেয়: একটি **authorization সিদ্ধান্ত**, **guardian link ও right-এর record**, এবং **তিনটি event**। অন্য module নিজে role বা right-এর নিয়ম লেখে না।
- সবচেয়ে ঝুঁকির জায়গা তিনটি: পুরোনো সিদ্ধান্ত ধরে রাখা (stale allow), queue-তে থাকা notice, এবং এক account-এর একাধিক school।
- §৫.৫-এ CX-01–CX-15। CX-07-এর shared fixture Platform T1-এ। CX-11-এর C3 lock order শেষে custody restriction row। CX-14-এর transaction path T-00-এর পরে ঠিক হবে। CX-15 অনুযায়ী এই scope-এ C3 কোনো student status event consume করে না। CX-13 (PR-06)-এ client product owner-এর অনুমোদন বাকি। Custody-র approval engine ও file service Platform-এর প্রথম slice-এ নেই (CX-01)।
- **Go-live gate (custody ও privileged):** OD-04 ও OD-19 এখন সিদ্ধান্ত হয়েছে। Custody approve, privileged role grant ও privileged invite build ও test হবে, কিন্তু আসল user-এর জন্য বন্ধ থাকবে যতক্ষণ না: IAM-005 চালু হয়, Platform-এর PLT-007 ও PLT-013 পাওয়া যায়, এবং branch 1b merge হয়। Test stand-in শুধু contract প্রমাণ করে।
- **CX-01 · owner গৃহীত (২৬ নভেম্বর ২০২৬):** Identity-র প্রথম branch দুই ভাগে। 1a-তে custody ও audit ছাড়া সব, আগে। 1b-তে custody ও audit, Platform-এর PLT-006/007/013 merge-এর পরে (§৪)। 1b-কে G1 staging-এর আগে merge হতেই হবে, কারণ custody restriction ও audit pilot-এর শর্ত। CTO DEPENDENCIES-এর Identity merge gate বদলাবেন; ততদিন DEPENDENCIES-এর লেখা আগের মতো।
- Coding শুরুর আগে আপনার দেওয়া branch লাগবে (§১৬)। Plan approve করা মানে review-এর সম্মতি, coding-এর অনুমতি নয়।

## ২. কী যাচাই হয়েছে

| বিষয় | ফল | অবস্থা |
|---|---|---|
| Workspace | `D:\nls-all-projects\Erp-lms`, branch `main`, commit `2f0b1e9` | Verified |
| Remote | `origin` · repository `desk-school-platform` · সর্বশেষ দেখা `origin/main` একই commit `2f0b1e9` | Verified |
| Tracked content | Root `README.md` ও `docs/project-planning/`-এর ১৭টি file | Verified |
| Backend, web বা mobile code | নেই | Verified (অনুপস্থিত) |
| Schema, migration, auth middleware, route, component, test | নেই | Verified (অনুপস্থিত) |
| Build ও test command, CI | পাওয়া যায়নি | Pending |
| `docs/project-planning/system-plans/` | আগে ছিল না। এই দুই plan দিয়ে `02-identity/` তৈরি হয়েছে। লেখার মাঝেই অন্য session থেকে `01-platform/` ও `03-student-records/`-এর draft plan এসেছে (untracked)। তাদের C3 সংশ্লিষ্ট অংশ মিলিয়ে দেখা হয়েছে (§৫.৫)। | Verified |
| CTO handoff-এ লেখা commit | `febdc8f`। বর্তমান HEAD এক commit এগিয়ে (`2f0b1e9`, docs)। | Verified, ছোট অমিল |
| পড়া উৎস | Docs 02–08, README, `tracker/`-এর চার CSV-র C3 সংশ্লিষ্ট row, INTERFACES, DEPENDENCIES, SYSTEM-AGENT-BRIEFS, CTO-HANDOFF; Blueprint পৃ. ১০–১১, ২৪–২৫, ৩১–৩৬, ৩৮, ৪৭–৪৮, ৬৩–৬৪, ৯৮, ১০২–১০৩ | Verified |
| **Integration base** | Backend কোন repo-তে, কোন branch থেকে feature branch কাটা হবে, তা জানা নেই | **Pending · OD-16** |

Subagent দিয়ে code খোঁজার দরকার পড়েনি, কারণ খোঁজার মতো code নেই। উৎস docs মূল model নিজে পড়েছে।

## ৩. Stack ও conventions

নিচেরগুলো **উৎস** (Blueprint পৃ. ১০–১১, ৩৫–৩৬, ৪৭–৪৮)। Repo-তে একটিও মেলানো হয়নি। T-00-এ প্রতিটি সারির পাশে actual path ও version বসবে। অমিল পেলে repo-র convention-ই মানা হবে এবং এই plan হালনাগাদ হবে।

| স্তর | উৎস অনুযায়ী পছন্দ | Identity-র জন্য যা মানতে হবে |
|---|---|---|
| Backend | Go, একটি modular codebase, তিনটি runtime role (API, worker, render) | Module-এর সীমা কঠোর। অন্য module-এর table সরাসরি পড়া বা লেখা যাবে না। |
| API | `/v1`-এর নিচে REST ও JSON, OpenAPI 3.1, সেখান থেকে তৈরি Dart ও TypeScript client | State বদলানো call-এ `Idempotency-Key`। Record-এ version। পুরোনো version-এ conflict। Error RFC 9457 problem details। |
| Sign-in | Certified OAuth 2.1 ও OpenID Connect library-র ওপর built-in identity module (D-05)। App-এ authorization code + PKCE। | Crypto বা token-এর নিয়ম নিজে লেখা যাবে না। Short-lived access token, device-এ বাঁধা rotating refresh token। |
| Database | PostgreSQL। প্রতি module-এর নিজস্ব schema ও role। School-এর প্রতিটি table-এ row-level security। | Check ২ ও ৪-এর পেছনে RLS দ্বিতীয় দেয়াল। School ID আসে request context থেকে, client-এর parameter থেকে নয়। |
| Migration | Module অনুযায়ী ক্রমিক SQL। Expand-and-contract। | এক ধাপে পুরোনো column বা table মোছা যাবে না। |
| Event ও job | Transactional outbox ও PostgreSQL-ভিত্তিক job queue | Data বদল ও outbox event একই transaction-এ। Subscriber event ID দিয়ে পুনরাবৃত্তি বাদ দেয়। |
| Cache | Redis: rate limit, স্বল্পস্থায়ী cache ও lock | Access-এর সিদ্ধান্ত Redis টিকে থাকার ওপর নির্ভর করবে না। |
| Web | Next.js, React ও TypeScript, একটি component library | Administration web-এর screen এখানে। |
| Mobile | Flutter, দুই app-এর shared package (networking, authentication, offline sync, design system, localisation)। Device-এ encrypted SQLite। | Session revoke হলে offline data অকেজো। |
| CI | Static analysis, আসল PostgreSQL ও Redis-এ test, API ও event-এর contract compatibility, tenant isolation suite | প্রতিটি commit-এ isolation suite। Definition of done-এ test, contract example, দুই ভাষার translation ও telemetry। |

**Proposed path** (Blueprint পৃ. ৪৮-এর layout থেকে; repo-তে আছে কি না **Pending**):

| Path | কী থাকবে | Owner |
|---|---|---|
| `internal/identity/` | People, credentials, sessions, memberships, guardian rights | C3 |
| `internal/tenancy/` | Schools, campuses, lifecycle, entitlements | C1/C2 — শুধু interface ব্যবহার |
| `internal/policy/` | Approvals (maker-checker) | C9 — শুধু interface ব্যবহার |
| `internal/audit/` | Append-only audit event | C9 — শুধু interface ব্যবহার |
| `internal/students/`, `internal/notifications/` | A8 ও C4 | Consumer — contract test |
| `api/openapi/`, `api/events/` | Versioned API ও event schema | C3 নিজের অংশ লেখে |
| `migrations/` | Identity-র ক্রমিক SQL | C3 |
| `tools/` | Isolation suite, boundary check | Platform — C3 case যোগ করে |
| Web ও mobile-এর path | জানা নেই | **Pending · T-00** |

## ৪. প্রথম branch-এর scope

| ভেতরে | বাইরে (পরের deliverable, §১০) |
|---|---|
| **IAM-014** permission catalogue, matrix-এর data, প্রকাশ ও দেখার screen | IAM-001 পূর্ণ sign-in, OTP ও password |
| **IAM-003** role grant, scope ও authorization সিদ্ধান্ত | IAM-002 invite ও activation |
| **IAM-004** guardian link, প্রতি সন্তানের right ও custody restriction | IAM-005 MFA enrol, challenge ও step-up |
| **IAM-007** সম্পর্ক শেষ ও revocation-এর বিস্তার | IAM-006 session ও device-এর screen, recovery |
| Check ১ ও ২-এর জন্য **ন্যূনতম** person, membership ও session record (OD-15) | IAM-013 school ও workspace switch-এর পূর্ণ UX |
| Privileged grant ও custody approve-এর নিয়ম ও test, **go-live gate-এর পেছনে** (DR-15, OD-04, OD-19) | IAM-010 delegation |
| | IAM-012, IAM-008, IAM-011, IAM-015, IAM-009 (backlog) |

"ন্যূনতম" মানে এতটুকু যাতে server বলতে পারে session বৈধ কি না এবং ব্যক্তি school-এর সক্রিয় সদস্য কি না। Sign-in-এর screen ও OTP এই branch-এ নেই। প্রথম branch-এ C3 ন্যূনতম session record রাখে; পূর্ণ sign-in পরের deliverable-এ (OD-15 গৃহীত)।

**CX-01: প্রথম branch ভাগ (owner গৃহীত, ২৬ নভেম্বর ২০২৬)।**

| ভাগ | কী থাকবে | কখন |
|---|---|---|
| 1a | Custody ও audit evidence ছাড়া সব: matrix, scoped role, guardian link ও right, revocation, A8 contract, right বদলের dispatch check | আগে |
| 1b | Custody restriction ও audit evidence | Platform-এর PLT-006, PLT-007, PLT-013 merge-এর পরে; **G1 staging-এর আগে অবশ্যই**, কারণ custody restriction ও audit pilot-এর শর্ত |

CTO এই ভাগের সুপারিশ করেছিলেন, কারণ এতে Platform-এর প্রথম slice বদলায় না এবং System 1-এর plan আবার খুলতে হয় না। বিকল্প ছিল Platform আগে ন্যূনতম PLT-006/007/013 interface merge করবে। আপনি ২৬ নভেম্বর ২০২৬-এ ভাগটি মেনেছেন। DEPENDENCIES-এর Identity merge gate-এ এখনো custody ও audit আছে; CTO সেটি বদলাবেন। সেই বদল না হওয়া পর্যন্ত DEPENDENCIES-এর লেখা আগের মতো থাকে।

## ৫. Contracts

নিচের সব নাম ও field **Proposed**। [INTERFACES](../../integration/INTERFACES.md)-এর অর্থ ও নমুনা ধরে লেখা। Repo-তে নাম মেলানো ও owner-দের version-সহ সম্মতি T-00-এ।

### ৫.১ Platform C1 — C3 যা নেয়

| Contract | Input → output | যে নমুনায় মিলতে হবে | অবস্থা |
|---|---|---|---|
| `SchoolContext` | Verified session → সক্রিয় membership-এর school, actor ও scope reference | A-র session-এ B-র ID দিলে record অদৃশ্য। School বদলালে পুরোনো cache খালি। | Proposed · E-03 |
| `Decide(context, product, module, action)` → `EntitlementDecision` (নাম System 1-এর draft থেকে) | Context + product + module + action → allowed, access mode, lifecycle, version ও কারণ (`ok`, `product_off`, `module_off`, `school_read_only`, `school_suspended`, `not_yet_effective`) | A School-only, B Learn-only, পরে A-তে দুটোই। Learn-only-তে school import বন্ধ। বন্ধ product-এর route এমন উত্তর দেয় যেন তা নেই। | Proposed · E-03 |
| Event `entitlement.changed`, `school.state_changed` | C1 প্রকাশ করে, C3 পড়ে | Product বন্ধ হলে তার workspace ও action বন্ধ। Data থাকে। | Proposed |

**Check-এর ক্রম (Proposed)।** ১ Session → ২ Membership (এখান থেকে `SchoolContext`: school, actor, scope) → **C1 check** (entitlement, Platform) → ৩ Role → ৪ Scope → ৫ Relationship → ৬ Sensitivity → ৭ State → ৮ Approval। অর্থাৎ আগে session যাচাই, তারপর `SchoolContext`, তারপর `Decide`। এই ক্রম Doc 04 ও Blueprint পৃ. ৩২ মিলিয়ে আমাদের প্রস্তাব। DEPENDENCIES শুধু বলে দুই শর্ত একটি server সিদ্ধান্তে থাকে। প্রকৃত enforcement-এর জায়গা owner-রা যাচাই করবেন।

**ভাগ।** C1 উত্তর দেয় C1 check-এর। C3 দেখে check ১–৫। Check ৬–৮ প্রয়োগ করে সেই module, যে ওই record বা কাজের owner (উৎস: DEPENDENCIES)। Custody restriction ও privileged role grant-এর owner C3 নিজে, তাই সেখানে "নিজে approve নয়" নিয়ম C3 প্রয়োগ করে, approval engine ব্যবহার করে (OD-14)। কোন কাজে approval লাগে এবং কোন role কোন sensitivity class খুলতে পারে, তা matrix-এ লেখা থাকে। সব মিলে একটিই সিদ্ধান্ত caller পায়। Entitlement থাকলেই role-এর সিদ্ধান্ত বাদ যায় না।

**Platform C3-র কাছে যা চায় (System 1-এর draft plan থেকে)।**

| Platform-এর চাওয়া | এই plan-এ কোথায় | অবস্থা |
|---|---|---|
| Verified session → membership (school ও actor) | T-02 (ন্যূনতম)। পূর্ণ রূপ §১০-এর ক্রম ২। | Platform-এর প্রথম slice test stand-in দিয়ে চলে। আসল binding Identity merge-এ। OD-15 |
| Operator role check (Super Admin, Platform Support) | T-01-এর matrix ও T-03-এর `Authorize` | Operator action-এর row Platform প্রস্তাব করবে (OD-13) |
| Job চালানোর সময় আবার check-এর জন্য `Authorize` | T-03, §৬-এর নিয়ম ৬ | Owner গৃহীত · CX-05 |
| প্রথম School Admin-এর invite | §১০-এর ক্রম ৩ (IAM-002) | প্রথম branch-এ নেই |
| Operator-এর sign-in ও second factor | §১০-এর ক্রম ২ ও ৪ | প্রথম branch-এ নেই |
| School বদলালে ওই device-এর পুরোনো session শেষ (`session_ended`; উৎস: Blueprint পৃ. ২৫); P0-তে stand-in | T-02-এর ন্যূনতম session-এ `ended` অবস্থা। পূর্ণ flow §১০-এর ক্রম ২ ও ৫। | C3 দিতে পারে · **Owner গৃহীত · CX-10** |
| Job chunk-এর transaction-এর ভেতরে `AuthorizeInTx`, "confirmedAt-এর পরে revoke হয়েছে কি?" এবং account ও membership state | T-02 (state ও history), T-03 (`AuthorizeInTx`), §৬ নিয়ম ৬ | শর্তসহ দিতে পারে · **Owner গৃহীত · CX-11** |

**Session-এর record (OD-15 গৃহীত)।** Platform stand-in দিয়ে শুরু করে, C3 ন্যূনতম session record দেয়। দুই plan-এর default মেলে। C1-এর চূড়ান্ত shape merge না হওয়া পর্যন্ত **Pending**।

### ৫.২ Student Records A8 — দুই দিকেই

| দিক | Contract | Input → output | নমুনা |
|---|---|---|---|
| A8 → C3 | `Authorize` | Actor/membership + product + action + target school/campus/student → allow বা deny ও approval state। Action-এর নাম System 3-এর draft থেকে: `students.import.preview`, `students.import.confirm`, `students.import.undo`, `students.register`, `students.placement.create`, `students.read`, `students.export`। Household edit-এর জন্য System 3 প্রস্তাব করেছে `students.household.edit` (CX-03)। | একই school-এর অন্য campus। Preview-র পরে role তুলে নেওয়া। মেয়াদ শেষ session। |
| A8 → C3 | Guardian link **request** (import বা household screen থেকে) | Student ID + household contact reference + source batch/row reference → request reference, status ও version। Link `pending` অবস্থায় ও **কোনো right ছাড়া** শুরু হয়। | Field System 3-এর draft-এর সঙ্গে মেলে। C3 ব্যর্থ হলে A8 row-টি Failed দেখায় ও আবার চেষ্টা করে। একই request আবার এলে একই ফল। |
| Registrar → C3 (A-06 screen) | Link **সক্রিয় করা ও right দেওয়া** | Link + guardian-এর account reference + right-এর তালিকা + পরিচয়ের প্রমাণের reference + expected version → status ও নতুন version | তিন সন্তানে আলাদা right। পরিচয় না মিললে `pending` থাকে। Import নিজে কখনো right দেয় না। |
| A8 → C3 | Undo-তে link প্রত্যাহার (System 3-এর TD-08; CX-12, Owner গৃহীত) | প্রতিটি item: link বা request ID + origin (batch ও row) + expected version, সঙ্গে per-operation key → প্রতিটি item-এর ফল: `ended` (আগে সক্রিয় link শেষ), `cancelled` (pending request বাতিল), `conflict` (version বদলেছে বা custody restriction আছে; বর্তমান version ফেরত), `not_found` (এই school-এ নেই বা origin মেলে না)। Batch reference ধরে কখনো নয়। | A8 শুধু Reversed ও Kept row-এ এই batch-এর link পাঠায়। Conflict row-এর link, অন্য batch-এর link ও পরে বদলানো link বাদ যায় বা conflict পায়। একই key আবার এলে একই ফল। `cancelled` মানে C3-তে থাকা pending request undo-তে বাতিল। যে row-এর request কখনো C3-তে পৌঁছায়নি (Failed row), সেটি A8 নিজেই বাতিল করে, C3-কে পাঠায় না। |
| A8 domain events | প্রথম branch-এ A8-এর `student.enrolled` ও `placement.changed`। Undo-তে `placement.changed` দিয়ে A2 তার roster মিলিয়ে নেয়। | A8 প্রকাশ করে, A2 placement consume করে। C3 কোনো student status event consume করে না (CX-15)। | MVP-তে Registrar guardian link হাতে শেষ করেন। Undo-তে A8 সরাসরি CX-12-এর command ব্যবহার করে। A13-এর `student.withdrawn` R2-তে (CX-04)। |
| C3 → A8 | Link status পড়া | Student ID → প্রতিটি guardian link-এর status ও version (right-এর সারাংশ) | A8-এর household screen-এ read-only link status। Access বদলানো হয় C3-র A-06 screen-এ, যা সেখান থেকে খোলে। |
| C3 → A8 ও অন্যরা | Event `guardian_right.changed` | Link, student, version | Screen, list ও export নতুন right ধরে। |

**কে কী রাখে।** Household, contact-এর phone, sibling ও emergency contact রাখে A8। Account-এর যাচাই করা phone বা email, membership, guardian↔student link, right ও custody restriction রাখে C3। C3 student-এর নাম বা contact-এর বিবরণ নিজের table-এ রাখে না। শুধু student ID রাখে।

**§৫.২-এর অতিরিক্ত নিয়ম (Proposed)।**

| বিষয় | নিয়ম |
|---|---|
| Contact lookup | C3 A8-এর interface থেকে শুধু মেলানোর field পড়ে: contact-এর phone বা email ও তার যাচাইয়ের অবস্থা। আর কিছু নয়। |
| Household edit action | প্রস্তাবিত নাম `students.household.edit`; System 3-ও একই নাম প্রস্তাব করেছে (CX-03) |
| Replay key | batch ID + row ID |
| Withdrawal | `student.withdrawn` A13-এর সঙ্গে R2-তে আসবে (CX-04)। তখন link শেষ হবে withdrawal-এর কার্যকর সময়ে; event দেরিতে এলে পাওয়ামাত্র, দেরি মাপা হবে ও alert যাবে। MVP-তে Registrar হাতে link শেষ করেন। |
| Paused import ও undo | থেমে যাওয়া import নিজে থেকে আবার চলে না। আবার শুরু করতে নতুন confirm লাগে, তাতে C3-র check আবার হয় (§৬ নিয়ম ৪)। Undo-ও guard-এর নিচে চলে (CX-11)। System 3 এই নিয়ম মেনেছে। |
| Import | শুধু `pending` request তৈরি করে: zero outbound invite, zero right (CX-09) |

**সিদ্ধান্ত।** A8-এর contact আর C3-র account মেলানো হয় OD-01-এর নিয়মে (CX-02): contact-এর phone ও account-এর যাচাই করা phone মেলে এবং সামনাসামনি যাচাই হয়। Link সক্রিয় হওয়ার সময় কোনো right নিজে থেকে চালু হয় না (OD-02)। `student.withdrawn` R2-তে A13-এর সঙ্গে আসবে (CX-04)। Undo-র contract link ধরে (CX-12, Owner গৃহীত)।

### ৫.৩ অন্য consumer

| Consumer | C3-র কাছে কী চায় | কখন |
|---|---|---|
| C4 Communication | Notice তৈরির সময় প্রাপকের তালিকা। প্রতি প্রাপকের জন্য **প্রতিটি dispatch চেষ্টার ঠিক আগে** আবার check। Guardian detail খুললে আবার check। Area right প্রযোজ্য: সাধারণ notice-এ notice right, area notice-এ (attendance, result, fee) সেই area-র right। Channel preference delivery কমাতে পারে, right যোগ করতে পারে না। প্রতি (event, প্রাপক)-এর জন্য একটি টেকসই delivery record, যা retry, worker ও fallback ব্যবহার করে (C4 contract-এর দায়)। | Notice তৈরি, প্রতিটি dispatch চেষ্টা ও detail খোলা (T-09) |
| C5 Apps | Guardian-এর অধিকার আছে এমন সন্তান ও area-র তালিকা। প্রতিটি child request-এ `Authorize`। | App খোলা ও প্রতিটি request-এ |
| C9 Files | `Authorize` দুবার: link দেওয়ার সময় **এবং** download (redeem) করার সময় (উৎস: INTERFACES) | Link তৈরি ও download |
| C6 Reports | Actor-এর scope: report চালানোর সময় **এবং** তৈরি report বা export collect বা আবার খোলার সময় | চালানো ও collect-এর মুহূর্তে, schedule করার সময় নয় |
| A9 Attendance ও pickup | Teacher-এর scope। Pickup list-এ custody restriction। | Pickup অংশ **Pending · E-04** |
| C9 Approval engine (PLT-007) | Custody ও privileged role-এর approval record | **OD-14** |
| C9 Audit (PLT-006) | Identity-র sensitive ঘটনা লেখা | T-11 |

**নিয়ম।** Target ও প্রয়োজনীয় right বের করে record-এর owner domain, record থেকেই (যেমন file-এর child ID ও area)। Client-এর parameter থেকে কখনো নয়।

### ৫.৪ Event

Identity তিনটি event প্রকাশ করে (**উৎস**: Blueprint পৃ. ৩৬): `membership.changed`, `guardian_right.changed`, `session.revoked`।

Envelope-এর Proposed field: `eventId, eventType, eventVersion, schoolId, recordId, recordVersion, occurredAt`। Event-এ শুধু ID ও তথ্য যায়। সন্তানের নাম, custody document বা কারণ যায় না। **Consumer event-এর ওপর ভরসা করে access দেয় না। ব্যবহারের মুহূর্তে C3-কে আবার জিজ্ঞেস করে।** Event দেরিতে এলেও তাই ভুল access হয় না।

### ৫.৫ System 1 ও System 3-এর draft plan-এর সঙ্গে মিল

দুটো draft plan এই plan লেখার সময়ে এসেছে। একটি Sonnet subagent তাদের C3 সংশ্লিষ্ট অংশ পড়ে তুলনা দিয়েছে। মূল উদ্ধৃতিগুলো আমি নিজে মিলিয়ে দেখেছি। ওই দুই plan আমি বদলাইনি।

**যা মেলে।** Merge-এর ক্রম। Integration base তিন plan-এই Pending। Import ও undo কেবল Registrar করেন। পরিচয় নিশ্চিত না হলে link `pending`, কখনো `active` নয়। A8 কোনো grant রাখে না। Confirm, undo ও job চালানোর সময় আবার check হয়। অন্য school-এর ID "পাওয়া যায়নি" দেখায়। Custody-র screen Identity-র।

**যা মেলে না বা খোলা।**

| # | বিষয় | তারা কী বলে | এই plan কী বলে | কার সিদ্ধান্ত |
|---|---|---|---|---|
| CX-01 | Approval engine (PLT-007), audit (PLT-006) ও file service (PLT-013) কখন আসবে | System 1-এ তিনটিই তার প্রথম slice-এর **পরে** | T-07 ও T-11 এদের ওপর নির্ভর করে | Owner গৃহীত (২৬ নভেম্বর ২০২৬): 1a/1b ভাগ; 1b G1 staging-এর আগে। CTO DEPENDENCIES-এর Identity merge gate বদলাবেন; ততদিন DEPENDENCIES-এর লেখা আগের মতো। (অন্য দুই পথ ছিল: Platform আগে ন্যূনতম interface দেয়, বা C3 সাময়িক port রাখে। Stand-in শুধু contract-এর প্রমাণ।) |
| CX-02 | Contact থেকে account মেলানো | System 3 পাঠায় household **contact** reference | Link সক্রিয় করতে লাগে **account** reference | OD-01-এর সিদ্ধান্তে মীমাংসিত: contact-এর phone ও account-এর যাচাই করা phone মেলে এবং সামনাসামনি যাচাই হয়, তারপর account reference দিয়ে link সক্রিয়। |
| CX-03 | Action-এর নাম | System 3-এর উত্তর: household edit-এর জন্য নতুন action `students.household.edit`; বাকি সাতটি `students.*` নাম আগের মতো | T-01-এর catalogue ওই নামগুলোই নেবে (OD-13)। নাম দুই plan-এ মেলে। | Owner গৃহীত। |
| CX-04 | `student.withdrawn` event | System 3-এর উত্তর: এই event A13-এর সঙ্গে R2-তে, প্রথম branch-এ নয় | MVP-তে student চলে গেলে Registrar হাতে link শেষ করেন। স্বয়ংক্রিয় অংশ ও lag metric R2-তে (T-08)। IAM-007-এর label বদলায় না: membership শেষ, role শেষ, link শেষ ও undo MVP-তে প্রমাণ হয়। | Owner গৃহীত। |
| CX-05 | Session শেষ হওয়ার পরে queue-র job | System 1-এর প্রস্তাব: job চলবে, কারণ job membership ও role ধরে চলে। নিয়মটি C3-র বলে রেখেছে। | §৬-এর নিয়ম ৬ (owner গৃহীত) | Owner গৃহীত (§৬ নিয়ম ৬, CX-11-এর সঙ্গে)। |
| CX-06 | Platform-এর operator role check | C3 যাচাই করে, Platform consume করে। | Operator role T-01 ও T-03-এ আছে। Platform-এর impact summary scope থেকে বাদ হওয়ায় member/role aggregate contract প্রত্যাহৃত। | ২৬ নভেম্বরের owner সিদ্ধান্ত। |
| CX-07 | Shared fixture | System 1-এর ১২-row decision table। System 3-এর T3-তে তিন সন্তানের contact test আছে, কিন্তু shared fixture-এ তিন linked সন্তান ও unlinked চতুর্থ sibling ছিল না। | একটি versioned fixture তিন system-এর জন্য: দুই school-এ একই section label ও student number, একই product, প্রতি school-এ একটি সফল control, সঙ্গে Fixture A ও B (§৯) | **CTO-র সিদ্ধান্ত:** Platform-এর T1-এ একটিই versioned shared fixture file, তিন system-এর জন্য, তাতে এই plan-এর Fixture A ও B। Version বদলালে Platform, Identity ও Students তিন owner-এর review লাগে। |
| CX-08 | "নিজে approve নয়" কে প্রয়োগ করে | System 1-এ এটি PLT-007-এর acceptance | Custody ও privileged role-এ C3, engine ব্যবহার করে (§৫.১)। দুই contract-ই "আলাদা ব্যক্তি" check করে। C9 approval record রাখে। C3 target ও কার্যকর restriction check করে ও লেখে। | Owner আমাদের প্রস্তাব মেনেছেন; System 1 ও 3-এর plan-এ মেলানো CTO-র cross-check-এ। |
| CX-09 | A8-এর TD-04 (batch reference সহ সরাসরি command) ও A8-এর OD-07 (confirm-এ সব pending request) | A8 import থেকে pending request তোলে | আমাদের নিয়ম: import থেকে zero outbound invite ও zero right। সরাসরি command গ্রহণযোগ্য। | Owner আমাদের প্রস্তাব মেনেছেন; System 1 ও 3-এর plan-এ মেলানো CTO-র cross-check-এ। |
| CX-10 | Session ও membership-এর ভাগ, school বদলে পুরোনো session শেষ (System 1-এর OD-6) | C3 session ও membership যাচাই করে। Platform শুধু school context বহন করে। Operator role check C3-র। School বদলালে ওই device-এর পুরোনো session শেষ (`session_ended`)। P0-তে stand-in। | C3 দিতে পারে; এই plan-এর সঙ্গে মেলে (check ১–২ C3-র, OD-15)। পুরোনো session শেষ হওয়া Blueprint পৃ. ২৫-এর নিয়ম। প্রথম branch-এ ন্যূনতম session-এর `ended` অবস্থা; switch-এর পূর্ণ flow ক্রম ২ ও ৫-এ। Trade-off: একই browser-এ দুই tab-এ দুই school একসঙ্গে চলে না; অন্য tab পরের request-এ `session_ended` পায় ও reload লাগে। লাভ: আগের school-এর token দিয়ে কিছুই আসে না (SC-13)। | Owner গৃহীত। |
| CX-11 | Queued-job guard (System 1-এর OD-7) | A8 প্রতিটি import chunk-এর write transaction-এর ভেতরে Platform-এর guard ও C3-র check ডাকে। C3 দেবে `AuthorizeInTx` (grant ও membership revoke একই row lock করে), "confirmedAt-এর পরে revoke হয়েছে কি?" এবং account ও membership state (locked account, suspended membership)। | শর্তসহ দিতে পারে। (১) `AuthorizeInTx`: caller-এর transaction-এর ভেতরে C3-র grant ও membership row-এ shared lock নিয়ে সিদ্ধান্ত; revoke command একই row-এ exclusive lock নেয়, তাই chunk ও revoke-এর ক্রম স্পষ্ট। কোন পথে (security definer function, নাকি caller-এর transaction নিয়ে in-process call) তা T-00-এ Platform-এর সঙ্গে ঠিক হবে। (২) "Revoke হয়েছে কি": grant, membership ও account বদলের append-only history, কার্যকর সময়সহ (§৭)। (৩) State: membership-এ `suspended`, account-এ `locked`। Trade-off: revoke চলমান chunk শেষ হওয়া পর্যন্ত অপেক্ষা করতে পারে, তাই chunk ছোট ও lock timeout লাগে; module-এর মধ্যে database-স্তরের coupling বাড়ে; deadlock এড়াতে lock-এর একটি ক্রম লাগে (আগে Platform-এর row, তারপর C3-র); decision cache রাখা যায় না (প্রথম branch-এ এমনিতেই নেই)। লাভ: revoke commit-এর পরে কোনো chunk লেখে না; IAM-007 ও V02-এর শক্ত প্রমাণ। | Owner গৃহীত · lock-এর পথ T-00-এ। |
| CX-12 | Import undo-তে guardian link কীভাবে শেষ হবে (System 3-এর TD-08) | Batch reference ধরে নয়; নির্দিষ্ট link ID + origin + expected version + per-operation key ধরে। কারণ: batch reference ধরলে A8 যে row Conflict হিসেবে রাখে, সেগুলোর link-ও শেষ হয়ে যেত। CTO-ও এটিকে বেশি নিরাপদ বলেছেন। | C3 দিতে পারে। আগের batch-reference contract এর বদলে এটি নেওয়ার প্রস্তাব (§৫.২, T-10)। C3 নিজেও যাচাই করে: একই school, origin মেলে, version মেলে, custody restriction নেই; না মিললে ওই item `conflict` বা `not_found`, বাকি item চলে। প্রতিটি item-এর আলাদা ফল (`ended`, `cancelled`, `conflict`, `not_found`)। `cancelled` শুধু C3-তে থাকা pending request-এর জন্য; যে row-এর request কখনো C3-তে পৌঁছায়নি (Failed row), সেটি A8 নিজেই বাতিল করে। System 3 `cancelled` তার fake ও contract test-এ যোগ করবে। Trade-off: A8-কে প্রতি row-এর link ID ও শেষ জানা version রাখতে হয় এবং `guardian_right.changed` থেকে তা হালনাগাদ রাখতে হয়, নইলে conflict বাড়বে; বড় batch-এ বেশি item, তাই chunk করে guard-এর নিচে পাঠাতে হয়; conflict-এ Registrar-এর হাতের কাজ বাড়ে। লাভ: A8 যে link বলেনি তা কখনো শেষ হয় না; পরে হাতে বদলানো বা restricted link নীরবে শেষ হয় না (SC-12)। | Owner গৃহীত। |
| CX-13 | Pilot-এ বেছে নেওয়া pending guardian link-এ একবারে invite (CTO-র প্রশ্ন, System 3-এর তথ্য) | ১,০০০ pupil-এর import-এ প্রায় ৯৯০টি pending guardian link তৈরি হয়। Pilot-এ শুধু একক invite থাকলে cutover-এ প্রায় ৯৯০টি আলাদা invite। | সম্ভব। প্রথম branch (1a)-এ খরচ নেই; বাড়ে §১০-এর invite deliverable। বিস্তার নিচে। | Project owner গৃহীত, parent verification day-এর পরিকল্পনাসহ · client product owner-এর অনুমোদন ও tracker decision বাকি। |
| CX-14 | Import chunk-এ guardian link request কীভাবে লেখা হবে (System 1-এর v3 প্রশ্ন; Identity ও Students-এর মধ্যে খোলা) | দুই পথ: (ক) row-এর একই transaction-এর ভেতরে একটি savepoint-এ C3-র command (System 3-এর এখনকার পরিকল্পনা); (খ) A8-এর durable outbound record থেকে পরে পাঠানো, (batch, row) key-এ idempotent command। | C3 দুটোই দিতে পারে; command দুই পথেই (batch, row) key-এ idempotent। **Agent-এর সুপারিশ: (ক)**, যদি T-00-এ in-transaction call সম্ভব হয় (CX-11-এর `AuthorizeInTx`-এর একই পথ)। কারণ: C3-র call সফল হলে student ও link request একসঙ্গে commit হয়, আর undo-র জন্য (CX-12) link request-এর ID A8 সঙ্গে সঙ্গে পায়। C3 ব্যর্থ হলে savepoint ফিরিয়ে নেওয়া হয়: student-এর row থাকে, তাতে access-এর অবস্থা দৃশ্যমান "Failed", আর (batch, row) key-এর idempotent command দিয়ে আবার চেষ্টা হয়। তাই নীরব ফাঁক কখনো থাকে না। CTO-র মত: savepoint গ্রহণযোগ্য। খরচ: row-এর transaction একটু লম্বা হয়, আর module-এর মধ্যে database-স্তরের coupling (CX-11-এ এমনিতেই মানা)। T-00-এ সম্ভব না হলে (খ)। | Pending · T-00; Identity ও Students owner, তারপর আপনি |
| CX-15 | Student status event | C3 কোনো student status event consume করে না এবং এ জন্য কোনো reconciliation alert নেই। | A8 undo-তে CX-12-এর নির্দিষ্ট link/request command দেয়। ব্যর্থ হলে A8 “Access withdrawal pending” দেখায় এবং retry করে। Voided placement থেকে roster বদলায় `placement.changed` দিয়ে। `student.withdrawn` A13/R2-এর CX-04-এ আলাদা। | Owner-এর ২৬ নভেম্বরের scope review ও CTO addendum মেলানো হয়েছে। |

#### CX-11-এর বিস্তার: System 1 v3-এর তিনটি চাওয়া

System 1-এর Draft v3 (Platform implementation plan §১৯, design plan §১৪.৪ ও §২০) C3-র কাছে তিনটি জিনিস চায়। প্রতিটি owner-এর গৃহীত CX-10 ও CX-11-এর সঙ্গে মেলে। C3 তিনটিই দিতে পারে, নিচের শর্তে। চূড়ান্ত হবে CTO-র cross-check-এর পরে।

| System 1-এর চাওয়া | C3 দিতে পারে? | শর্ত |
|---|---|---|
| ১. "confirmedAt-এর পরে revoke হয়েছে কি?" প্রশ্নে মাঝে revoke হয়ে আবার দেওয়া, বা suspend হয়ে আবার চালু হওয়াও ধরা পড়বে। তখন job `access_changed` দিয়ে blocked। | হ্যাঁ | Access change history (§৭) append-only, প্রতিটি বদলের একই transaction-এ লেখা, database-এর সময়সহ। ধরা হয়: grant revoke বা মেয়াদ শেষ, membership suspend বা শেষ, account lock। পরে regrant, restore বা unlock হলেও আগের ঘটনা বাতিল হয় না। "প্রাসঙ্গিক" মানে: এই account, এই membership, আর এই membership-এর সেই grant-গুলো যারা confirmedAt-এ এই action ও scope-এর অনুমতি দিত। এটি history ও confirmedAt-এ কার্যকর matrix version থেকে বের হয়, তাই job row-এ নতুন field লাগে না। সঙ্গে C3-র প্রস্তাব: confirmedAt ও decision instant-এর মাঝে matrix-এর এই role × action ঘর বদলালেও `access_changed` (Platform-এর module বদলের মতো)। |
| ২. Decision instant: guarded write আগে সব lock নেয় একটি global ক্রমে (Platform-এর school → product → module, তারপর C3-র row C3-র নিজের ক্রমে), তারপর database থেকে একটি সময় ও version পড়ে decide করে; lock commit পর্যন্ত থাকে। C3-র ক্রম কী? | হ্যাঁ | **C3-র ক্রম:** (১) account row → (২) membership row → (৩) role grant row, grant ID ascending → (৪) guardian link row, link ID ascending → (৫) custody restriction row, ID ascending। Guarded write শুধু দরকারি row নেয়; import chunk নেয় account, membership ও প্রাসঙ্গিক grant, shared lock-এ। Matrix lock হয় না: version বদলায় না এবং তার কার্যকর সময় আছে, তাই decision instant-এ কার্যকর version-টি ব্যবহার হয়। এতে সব school-এর জন্য একটি গরম row তৈরি হয় না। মেয়াদ ও কার্যকর সময় guard-এর সেই একই database সময় দিয়ে মাপা হয়। Platform ও C3 দুই ধরনের row লাগলে সবসময় আগে Platform-এর। |
| ৩. Change command শুরু থেকেই exclusive lock নেয় (shared থেকে exclusive-এ upgrade নেই); একসঙ্গে দুটি এলে প্রথম commit জেতে, দ্বিতীয়টি 409 `version_conflict` পায়। C3-র revoke-ও কি তাই? | হ্যাঁ | C3-র সব change command (grant, revoke, suspend, restore, account lock ও unlock, link ও right বদল, custody restriction) শুরু থেকেই উপরের ক্রমে exclusive lock নেয়। তারা যে version পড়েছিল তা বহন করে। প্রথম commit জেতে; দ্বিতীয়টি 409 `version_conflict` পায়, body-তে শুধু target-এর বর্তমান version ও state, কোনো শিক্ষার্থী বা অন্য school-এর তথ্য নয়। History row ও outbox event একই transaction-এ। C3-র command Platform-এর row lock করে না, তাই Platform/C3 lock-এর boundary test-এ যাচাই হবে; chunk-এর হাতে lock থাকলে revoke অপেক্ষা করে, timeout-এ নিরাপদে আবার চেষ্টা করা যায়। |

#### CX-13-এর বিস্তার (project owner গৃহীত; client product owner-এর অনুমোদন বাকি)

**কী হবে।** Registrar নিজের scope-এর pending guardian link বেছে নেন (batch, class বা campus দিয়ে filter), preview দেখেন, তারপর একবারে invite পাঠান। পূর্ণ bulk onboarding (মানুষের file upload, staff-এর bulk, reminder, ধাপে ধাপে wave, report) বাকি R1-এই থাকে।

| যা লাগবে | বিবরণ |
|---|---|
| Preview | কতজন পাবেন, কতজন বাদ ও কেন: phone নেই; phone অন্য account-এ যাচাই করা, বা একই batch-এ দুজনের একই phone (DR-19); কার্যকর custody restriction; link বাতিল বা undo হয়েছে; SMS-only guardian (তাঁর পথ OD-11)। Confirm-এর আগে SMS-এর সংখ্যা ও segment দেখায়। |
| অনুমতি | শুধু Registrar, নিজের campus scope-এ। Matrix-এ নতুন action `iam.guardian_invite.send_batch` (Proposed)। Confirm-এ second factor (Registrar privileged হলে, OD-05; IAM-005 লাগবে)। |
| পাঠানোর মুহূর্তে check | প্রতিটি invite পাঠানোর ঠিক আগে: link এখনো `pending`, বাতিল বা undo হয়নি, কার্যকর custody restriction নেই, phone অস্পষ্ট নয়। না মিললে সেটি skip, কারণসহ। |
| Rate limit ও SMS provider | Platform-এর job framework ও guard দিয়ে ধীরে ধীরে পাঠানো (CX-11)। প্রতি school-এর quota ও fair use (SAS-013)। অগ্রাধিকার attendance ও emergency notice-এর চেয়ে কম, যাতে এক school-এর bulk send অন্য school-এর notice দেরি না করায় (X-07)। Provider-এর throughput ও খরচ (D-07, COM-012), আর SMS contract ও sandbox (Doc 06-এর client input, **Pending**)। |
| প্রতি invite-এর ফল | প্রতিটির আলাদা অবস্থা: queued, sent, failed (কারণ), skipped (কারণ), expired, activated। Batch-এ সংখ্যা ও তালিকা। |
| Retry | Failed invite একই invite দিয়ে আবার যায়; নতুন token নয়, দ্বিতীয় SMS নয় (idempotency key)। মেয়াদ শেষ হলে নতুন invite, পুরোনোটি বাতিল। |
| SMS-এর লেখা | নিরপেক্ষ: school-এর নাম ও link। সন্তানের নাম বা কোনো তথ্য নেই, কারণ phone-টি অন্য কারও হাতে থাকতে পারে (DR-19)। |
| Access | Invite গ্রহণে শুধু account চালু হয়। সন্তানের link `pending` থাকে, যতক্ষণ না পরিচয় যাচাই (OD-01) ও right বাছা (OD-02) হয়। Batch invite কখনো access দেয় না। |
| Audit | Batch-এর ঘটনা (কে, কখন, filter, সংখ্যা) ও প্রতি invite-এর ঘটনা (PLT-006)। Token কখনো log-এ যায় না। |

**প্রথম branch-এর খরচ।** 1a-তে কিছু বাড়ে না, কারণ invite সেখানে নেই; link request-এর `pending` অবস্থা ও origin আগেই আছে। খরচ বাড়ে §১০-এর ক্রম ৩-এ: selection, preview ও confirm screen; ধীরে পাঠানোর job; প্রতি invite-এর record ও retry; পাঠানোর মুহূর্তের check; C4 ও C8-এর সঙ্গে bulk SMS; আর ১,০০০ invite ও fairness (X-07)-এর test। নির্ভরতা: SMS provider, SAS-013, COM-001, PLT-006 ও IAM-005।

**Trade-off।**
- লাভ: cutover বাস্তবসম্মত হয়। ৯৯০টি আলাদা click-এর ক্লান্তিতে ভুল number-এ invite যাওয়ার ঝুঁকি কমে; preview-তে সব বাদ পড়া একসঙ্গে দেখা যায়। Guardian আগে থেকেই বাড়িতে account চালু করে রাখেন, তাই school-এর desk-এ শুধু পরিচয় যাচাই ও right বাছা বাকি থাকে।
- ক্ষতি: এটি scope বদল (IAM-002-এর pilot অংশের বর্ণনা বদলায়), তাই client product owner-এর অনুমোদন ও tracker-এ decision reference লাগে। SMS খরচ ও provider-এর প্রস্তুতির ওপর নির্ভর করে। ভুল বা পুনর্ব্যবহৃত number-এ একসঙ্গে অনেক SMS যেতে পারে, তবে লেখা নিরপেক্ষ এবং invite access দেয় না।
- আসল বাধা থেকে যায়: OD-01-এর সামনাসামনি পরিচয় যাচাই ও প্রতি সন্তানের right বাছা। Batch invite শুধু পাঠানোর কাজ কমায়, desk-এর কাজ নয়। তার জন্য parent verification day-এর পরিকল্পনা থাকবে এবং Registrar প্রতি child-এর right আলাদা করে বাছবেন।
- Import নিজে invite পাঠায় না (CX-09)। Batch একটি আলাদা, ইচ্ছাকৃত কাজ। তাই কোনো SMS যাওয়ার আগেই ভুল import undo করা যায়।

**সিদ্ধান্ত: project owner গৃহীত (client product owner-এর অনুমোদন ও tracker decision বাকি)।** উপরের সীমাসহ ন্যূনতম batch invite pilot অংশে থাকবে, সঙ্গে একটি parent verification day-এর পরিকল্পনা। কারণ: ৯৯০টি আলাদা invite cutover-এ অবাস্তব ও ভুলপ্রবণ। Batch-টি একক invite-এর একই token, নিয়ম ও check ব্যবহার করে, তাই নতুন ঝুঁকি কম।

## ৬. Authorization সিদ্ধান্ত (Proposed)

**Input।** Session reference (আগে session যাচাই হয়; তারপর সেখান থেকে person, membership ও school), product, action (catalogue-এর key), target (school, campus, grade, section, subject, student যেটি প্রযোজ্য), এবং guardian হলে প্রয়োজনীয় child right।

**Output।** সিদ্ধান্ত (`allow`, `deny`, `needs_approval`), ভেতরের কারণ (কোন check-এ আটকাল), বাইরে দেখানোর ফল, ব্যবহৃত grant-এর reference, matrix-এর version ও relationship-এর version।

| বাইরে দেখানোর ফল | কখন | Design-এর state |
|---|---|---|
| `not_found` | অন্য school, link ছাড়া সন্তান, scope-এর বাইরের যেকোনো target, এবং guardian-এর child-specific যেকোনো deny | "পাওয়া যায়নি" (DR-03) |
| `forbidden` | শুধু staff-এর জন্য, তাঁর নিজের scope-এর ভেতরের record-এ কাজটির অনুমতি নেই। Guardian-এর child-specific deny কখনো `forbidden` নয়। | "এই কাজের অনুমতি নেই" |
| `session_ended` | Check ১ | S-09 |
| `product_off` | C1 check | Workspace নেই |
| `step_up_required` | Second factor দরকার | S-06 (পরের deliverable) |
| `pending_approval` | Check ৮ | Approval-এর অপেক্ষায় |

**নিয়ম।**

1. **Default deny।** Catalogue-এ নেই এমন action-এ deny। CI এমন action পেলে build আটকায়।
2. **একই not-found।** `not_found`-এর response অস্তিত্বহীন ID-র response-এর সঙ্গে status, body, header ও redirect-এ হুবহু এক। ভেতরের কারণ শুধু audit-এ যায়।
3. **পুরোনো allow নয়।** প্রথম branch-এ কোনো decision cache নেই। প্রতিটি সিদ্ধান্ত আসে grant, link, session, matrix ও entitlement-এর authoritative অবস্থা থেকে। মেয়াদ শেষ হয়েছে কি না সিদ্ধান্তের সময়েই দেখা হয়। পরে cache লাগলে V12 evidence ও আলাদা review লাগবে। Custody approve, right প্রত্যাহার ও session revoke নিজের transaction-এই version বাড়ায়।
4. **প্রতি ধাপে আবার।** Confirm, undo, job চালানো, notice পাঠানো, file link দেওয়া, file download, report collect বা আবার খোলা এবং notice detail খোলা—প্রতিটিতে নতুন করে check (DR-10)।
5. **Approver আলাদা ব্যক্তি।** তুলনা হয় person ধরে, role ধরে নয়। একজনের দুটি role থাকলেও তিনি নিজের অনুরোধ approve করতে পারেন না। Delegation দিয়েও নয়, দ্বিতীয় role দিয়েও নয়।
6. **Job-এ session নেই।** Worker কারও session নিয়ে চলে না। Enqueue-এর সময় verified context থেকে school, actor, target ও প্রয়োজনীয় right job record-এ লেখা হয়। Worker সেখান থেকেই নেয়, payload থেকে কখনো নয়। দীর্ঘ job প্রতি ধাপে আবার check করে (owner গৃহীত · CX-05, CX-11)। প্রাপকের entitlement check একটি আলাদা function, যা school ও ID নেয়। Enqueue-এর পরে কী হলে job-এর কী হয়:

   | ঘটনা | Job |
   |---|---|
   | Session-এর স্বাভাবিক মেয়াদ শেষ, user-এর নিজের sign-out, device-এর session revoke বা token reuse | চলতে থাকে, কারণ job session ধরে নয়, account ও membership ধরে চলে |
   | Account `locked` (security কারণে) বা membership `suspended` | পরের chunk-এ থামে (CX-11) |
   | Role বা grant হারালে | পরের chunk-এ থামে |
   | Membership শেষ | থামে |
   | Entitlement বন্ধ | থামে (Platform-এর guard) |

   কারও queue-র কাজ থামাতে চাইলে তাঁর session নয়, role, membership বা account বন্ধ করতে হয়। System 1-এর v2-এর প্রস্তাবও একই (CX-11, owner গৃহীত)।

7. **C3 সাড়া না দিলে বন্ধ।** সিদ্ধান্ত না পেলে caller ধরে নেয় deny। Notice হলে পাঠানো স্থগিত রেখে আবার চেষ্টা হয়।
8. **Audit।** Sensitive read ও প্রতিটি পরিবর্তনে actor, school, record ও কারণ যায়। সফল sensitive read-ও audit হয় (restricted document, report বা export)।
9. **Effective commit।** Right বদল বা custody restriction কার্যকর হয় একটি C3 transaction-এ, যা right বদল বা restriction, link-এর নতুন version ও outbox event একসঙ্গে লেখে। ওই মুহূর্তই কার্যকর হওয়ার মুহূর্ত। তার আগে কোনো screen "effective" বলে না। সিদ্ধান্ত (OD-14; T-00-এ Platform-এর সঙ্গে যাচাই): approve command C3-র মধ্য দিয়ে যায় এবং C3 একই transaction-এ C9-এর approval সিদ্ধান্ত লেখে, ফলে commit একটি। তা সম্ভব না হলে C9-এর সিদ্ধান্তের পরে C3-র commit-ই কার্যকর মুহূর্ত।
10. **Target record থেকে।** Target ও প্রয়োজনীয় right বের করে owner domain, record থেকেই। Client-এর parameter থেকে নয়।
11. **Step-up gate।** IAM-005 চালু না হওয়া পর্যন্ত privileged grant, privileged invite ও custody approve `step_up_required` ফেরত দেয়।

### ৬.১ Log-এ কী থাকে (Proposed)

| Log | কী থাকবে | কী থাকবে না |
|---|---|---|
| Delivery log (C4) | event ID, school ID, recipient reference, channel, status, reason code, সময়, provider message ID | body, সন্তানের নাম, document, provider-এর raw payload, ব্যবহারযোগ্য file URL |
| Audit (PLT-006) | actor, school, record ID, action, reason code, আগের ও পরের মান (ID ও status হিসেবে)। Custody-র কারণের লেখা ও document শুধু restricted record-এর reference হিসেবে। | custody কারণের লেখা বা document-এর বিষয়বস্তু |
| Diagnostic log ও trace | school ID, request ID, outcome code | শিক্ষার্থীর কোনো বিষয়বস্তু (উৎস: Blueprint পৃ. ২৫) |

## ৭. Data (Proposed)

প্রথম branch-এর জন্য যতটুকু লাগে শুধু ততটুকু। Table ও column-এর নাম T-00-এ repo-র convention ধরে ঠিক হবে।

| Record | মূল তথ্য | মন্তব্য |
|---|---|---|
| Person | Account, যাচাই করা phone বা email, account অবস্থা (`active`, `locked`) | **School-এর অধীন নয়**। এক ব্যক্তি একাধিক school-এ থাকেন। ন্যূনতম। `locked` অবস্থা CX-11-এর জন্য (owner গৃহীত)। |
| Membership | Person × school, status (`active`, `suspended`, `ended`), তারিখ | School-এর অধীন, RLS। |
| Access change history | Grant, membership ও account-এর প্রতিটি বদল (revoke, মেয়াদ শেষ, suspend, lock, এবং regrant, restore, unlock), database-এর সময়সহ; append-only, বদলের একই transaction-এ লেখা | "confirmedAt-এর পরে revoke হয়েছে কি?"-র উত্তর, মাঝে ফেরত দেওয়া হলেও (CX-11 ও তার বিস্তার)। Audit-এর বদলি নয়। |
| Role grant | Membership, role, product, scope (campus, grade, section, subject), মেয়াদ, কে দিলেন, কারণ, status, version | School-এর অধীন, RLS |
| Session | Membership, device reference, status, মেয়াদ | ন্যূনতম · OD-15 |
| Permission catalogue | Action key, module, product, sensitivity | Versioned। File না table, তা T-01-এ ঠিক হবে। |
| Role permission | Role × action → scope-এর ধরন, approval লাগে কি না, second factor লাগে কি না | Matrix-এর একমাত্র উৎস |
| Matrix version | Version, প্রকাশের সময়, কে প্রকাশ করলেন | |
| Guardian link request | Student ID, school, household contact reference, source batch/row reference, status, version, কে তৈরি করলেন | School-এর অধীন, RLS। পরিচয় নিশ্চিত হওয়ার আগে কোনো Person নেই। |
| Guardian link | Person, student ID, school, status (`pending`, `active`, `restricted`, `ended`), version, কে তৈরি করলেন, পরিচয়ের প্রমাণের reference, origin (batch ও row, বা একক command) | School-এর অধীন, RLS। Link তৈরি হয় activation-এর সময়, Person-সহ। Undo-তে origin ও version মিলিয়ে দেখা হয় (CX-12)। |
| Guardian right | Link, right key, কে দিলেন, কখন | |
| Custody restriction | Link, কোন right বন্ধ, document-এর reference, কারণ, status, কে চাইলেন, কে সিদ্ধান্ত দিলেন, কখন থেকে কার্যকর, approval reference | Document রাখে C9 (PLT-013)। C3 শুধু reference রাখে। কারণের লেখা ও document শুধু restricted record-এ; audit-এ থাকে reference ও reason code। |

Delegation-এর record প্রথম branch-এ নেই। SMS recipient record (OD-11 গৃহীত) প্রথম branch-এ নেই।

## ৮. Task — dependency-র ক্রমে

প্রতিটি task-এ test command **Pending**, কারণ repo যাচাই হয়নি (T-00)। "Verification"-এ তাই test-এর ধরন ও প্রত্যাশিত ফল লেখা।

### T-00 · Repo ও contract যাচাই (code নয়)

- **Design ও feature:** সব · IAM-003, IAM-004, IAM-007, IAM-014
- **Path:** পুরো backend, web ও mobile checkout — **Pending**
- **কাজ:** Actual repo ও integration base, build ও test command, CI gate, existing auth middleware, user/role/session table, migration-এর নিয়ম ও owner খুঁজে লেখা। §৩-এর প্রতিটি সারি ও §৫-এর প্রতিটি contract-এর পাশে actual নাম ও version বসানো। System 1 ও System 3-এর plan-এর সঙ্গে `SchoolContext`, entitlement ও guardian command মেলানো।
- **Acceptance:** এই plan-এর "Pending" path-গুলোর প্রতিটি Verified হয়েছে বা কেন হয়নি তা লেখা আছে। Existing code-এ identity-র কিছু থাকলে তার তালিকা ও reuse-এর সিদ্ধান্ত আছে। `tools/`-এর isolation suite, তৈরি client ও প্রতি commit-এর CI আসল path দিয়ে ও একটি আসল run দিয়ে যাচাই হয়েছে।
- **Verification:** আপনি ও Integration CTO হালনাগাদ plan দেখে সম্মতি দেন।
- **নির্ভর করে:** OD-16
- **এই কাজ subagent-এ দেওয়া যায়:** read-only code খোঁজা (Sonnet বা Haiku)। সিদ্ধান্ত মূল model-এর।

### T-01 · Permission catalogue ও matrix-এর data

- **Design ও feature:** DR-04 · IAM-014
- **Path (Proposed):** `internal/identity/` (catalogue), `api/openapi/` (matrix পড়ার endpoint), `tools/` (row ছাড়া action ধরার check)
- **কাজ:** Action-এর catalogue ও role × action-এর মান (allowed, scope-এর ধরন, approval, second factor) একটি versioned উৎসে রাখা। Doc 04-এর ১৮টি role বসানো। প্রথম batch-এর action পূর্ণ করা: import, household, guardian link ও right, custody অনুরোধ ও approve, placement, role grant ও revoke, সন্তানের area দেখা। Matrix পড়ার API। CI check, যা catalogue-এ নেই এমন action পেলে আটকায়।
- **Acceptance:** Enforce হওয়া প্রতিটি action-এর row আছে। Matrix-এর প্রতিটি ঘর থেকে test তৈরি হয়: allowed ঘরে allow, স্থায়ীভাবে denied ঘরে শুধু deny, scope-যুক্ত ঘরে scope-এর ভেতরের ও বাইরের target। প্রতি role-এ অন্তত একটি allowed ও একটি blocked কাজ থাকে। Matrix-এর version ও প্রকাশের সময় পড়া যায়। Doc 04 থেকে আলাদাভাবে লেখা প্রত্যাশিত ফল আছে (SC-03), matrix থেকে তৈরি নয়। Screen ও export একই matrix version দেখায় (DR-04)।
- **Verification:** Matrix থেকে তৈরি test সব pass, এবং Doc 04-এর স্বাধীন প্রত্যাশিত ফলের সঙ্গে তুলনা মেলে। Row ছাড়া একটি নকল action যোগ করলে CI fail করে।
- **নির্ভর করে:** T-00 · **সিদ্ধান্ত:** OD-03, OD-05, OD-13 (design §৪.২)। Privileged role-এর ঘর OD-05 অনুযায়ী (Learning Admin-সহ)।

### T-02 · Identity-র মূল record

- **Design ও feature:** DR-02 · IAM-003
- **Path (Proposed):** `internal/identity/`, `migrations/`
- **কাজ:** Person, membership, role grant ও ন্যূনতম session-এর record। School-এর অধীন table-এ RLS। Role grant-এ scope ও মেয়াদ।
- **Acceptance:** A-র context-এ B-র membership বা grant কোনো query-তে আসে না। মেয়াদ শেষ grant কার্যকর নয়। এক ব্যক্তির দুই school-এ দুটি আলাদা membership থাকে।
- **Verification:** আসল PostgreSQL-এ module ও transaction test। Isolation suite-এ identity-র table যোগ।
- **নির্ভর করে:** T-00 · **সিদ্ধান্ত:** OD-15 (ন্যূনতম session record)। Person record school-এর অধীন নয়, তাই তার জন্য আলাদা নিয়ম লাগবে (§১২)।

### T-03 · Authorization সিদ্ধান্ত

- **Design ও feature:** DR-01, DR-03, DR-10 · IAM-003, IAM-014
- **Path (Proposed):** `internal/identity/` (সিদ্ধান্তের interface), API-র middleware — actual জায়গা T-00-এ
- **কাজ:** §৬-এর সিদ্ধান্ত। Check ১–৪। কোন কাজে approval লাগে তা matrix থেকে জানানো (`needs_approval`)। বাইরে দেখানোর ছয়টি ফল। সিদ্ধান্তের সঙ্গে grant reference ও matrix version।
- **Acceptance:** Design plan §৮.১-এর R1–R4, R6, R8, R9 এবং R16-এর role অংশ (check ৩) মেলে। R5 ও R7 মেলে T-04-এ। `not_found` অস্তিত্বহীন ID-র response-এর সঙ্গে হুবহু এক। Role বদলের পরের প্রথম request-এ নতুন সিদ্ধান্ত। `forbidden` শুধু staff-এর নিজের scope-এর ভেতরে; guardian-এর child-specific deny সবসময় `not_found`। কোনো decision cache নেই।
- **Verification:** Matrix থেকে তৈরি test। দুই response-এর byte-level তুলনার test। "Grant বদলাও, সঙ্গে সঙ্গে আবার চাও" test।
- **নির্ভর করে:** T-01, T-02

### T-04 · C1-এর context ও entitlement-এর সঙ্গে মেলানো

- **Design ও feature:** DR-01 · IAM-003
- **Path (Proposed):** `internal/tenancy/`-এর interface (শুধু ব্যবহার), `internal/identity/`
- **কাজ:** `SchoolContext` থেকে school নেওয়া। Entitlement-এর উত্তর ও C3-র সিদ্ধান্ত মিলিয়ে একটি ফল। C1 merge না হওয়া পর্যন্ত fixture দিয়ে contract test।
- **Acceptance:** Design plan §৮.১-এর R5 ও R7 মেলে। Request body, CSV বা নামের মিল দিয়ে school বদলানো যায় না।
- **Verification:** A School-only, B Learn-only ও A-তে দুটোই—তিন fixture-এ contract test। C1 merge হলে একই test আসল interface-এ।
- **নির্ভর করে:** T-03, System 1-এর contract (E-03)। Fixture-এর প্রমাণ ও আসল integration-এর প্রমাণ আলাদা রাখা হবে।

### T-05 · Role দেওয়া ও তুলে নেওয়া

- **Design ও feature:** DR-02, DR-09, DR-15 · IAM-003, IAM-007
- **Path (Proposed):** `internal/identity/`, `api/openapi/`, `api/events/`
- **কাজ:** School Admin-এর grant ও revoke command, কারণসহ। Grantor নিজের authority-র বাইরের scope দিতে পারেন না (DR-02)। Privileged role হলো Super Admin, Platform Support, School Owner, Principal, School Admin, Finance Officer, Registrar ও Learning Admin (OD-05); শিক্ষক, Cashier ও Exam Officer নয়। Privileged role-এ Owner-এর approval, কিন্তু step-up gate-এ আটকানো: IAM-005 চালু না হওয়া পর্যন্ত privileged grant `step_up_required`, সাধারণ scoped grant চলে। `membership.changed` event outbox দিয়ে, একই transaction-এ। Audit event যায় PLT-006 দিয়ে (immutability T-11-এ যাচাই)।
- **Acceptance:** Scope ছাড়া scoped role দেওয়া যায় না। Grantor নিজের scope-এর বাইরে দিতে পারেন না। School Admin নিজের privileged grant approve করতে পারেন না। Privileged grant step-up gate-এ আটকানো। Revoke-এর পরের request-এ ওই role-এর কাজ বন্ধ, অন্য role অক্ষত।
- **Verification:** Command-এর idempotency test। একই transaction-এ data ও event-এর test। Self-approval-এর test। Step-up gate-এর test।
- **নির্ভর করে:** T-03-এর পরে build; আসল write T-04-এর পরে · **সিদ্ধান্ত:** OD-05, OD-06 (TOTP; library F2-তে যাচাই)। **T-00-এ যাচাই:** OD-14। Privileged grant go-live gate-এর পেছনে (§১)।

### T-06 · Guardian link ও প্রতি সন্তানের right

- **Design ও feature:** DR-05, DR-06, DR-08, DR-19 · IAM-004
- **Path (Proposed):** `internal/identity/`, `migrations/`, `api/openapi/`, `api/events/`
- **কাজ:** Link-এর চার অবস্থা। Activation-এ অস্পষ্ট match হলে link আটকানো (DR-19)। Right-এর catalogue (AS-04)। Registrar-এর link ও right command, version-সহ। Identity-proof record শুধু প্রমাণের ধরন, কে ও কবে দেখেছেন রাখে; ID-র নম্বর বা ছবি কখনো নয় (OD-01)। Check ৫ সিদ্ধান্তে যোগ। Guardian-এর অধিকার আছে এমন সন্তান ও area-র তালিকা। `guardian_right.changed` event।
- **Acceptance:** Design plan §৮.২-এর G1–G8 মেলে। Contact save করলে grant শূন্য (R11)। Request `pending` থাকলে portal, notice ও file access শূন্য (R12a)। পরিচয় যাচাই হলেও right বাছাইয়ের আগে right শূন্য (R12b)। Confirmation সন্তানের নাম বলে (R12c)। G3, G5, G8-এ একই not-found। Area right মেলে (G14–G16)। কোনো right আগে থেকে বাছাই করা নয়। একই command আবার এলে দ্বিতীয় link হয় না।
- **Verification:** তিন সন্তান × প্রতিটি right-এর test। ভুল sibling-এর ID-তে not-found তুলনার test। পুরোনো version দিয়ে update করলে conflict।
- **নির্ভর করে:** T-03-এর পরে build; আসল write T-04-এর পরে · **সিদ্ধান্ত:** OD-01, OD-02, OD-11 (design §৪.২)। Link `active` হয় দুটো শর্তে: guardian OTP দিয়ে নিজের account যাচাই করেন এবং Registrar সামনাসামনি প্রমাণ দেখেন; right প্রতি সন্তানে আলাদা করে বাছা হয়।

### T-07 · Custody restriction

- **Design ও feature:** DR-07 · IAM-004 (V04, X-06)
- **Path (Proposed):** `internal/identity/`, `internal/policy/`-এর interface (শুধু ব্যবহার), `api/openapi/`
- **কাজ:** Document-এর reference ও reason নিয়ে request pending রাখুন। Approver requester থেকে আলাদা, scope সন্তানের campus ঢাকে এবং step-up লাগে। Approve হলে restriction, link version ও event একই কার্যকর transaction-এ লিখুন (§৬ নিয়ম ৯)। ফেরত বা প্রত্যাখ্যান হলে active rights বদলায় না। Restriction পুরো access বা নির্বাচিত rights-এ হয়। Restriction তোলার ক্ষেত্রেও ওই approval পথ। PLT-006 audit T-11-এ যাচাই হবে।
- **Acceptance:** Pending ও returned request-এ আগের rights কার্যকর থাকে। Approval প্রয়োগ চললে A-08 “Approved — applying” দেখায়। কার্যকর commit-এর আগে কোনো screen “effective” বলে না। Commit-এ restriction সব প্রাসঙ্গিক screen, notice ও list-এ কার্যকর হয় (Blueprint পৃ. ৩১, V02/X-06)। অন্য guardian অপরিবর্তিত। Partial restriction কেবল নির্বাচিত rights-এ। Document ছাড়া ও নিজের request approve করার চেষ্টা UI ও API-তে ব্যর্থ। Scope ও step-up লাগে। Apply ব্যর্থ হলে idempotent retry, repeat approval একই ফল দেয়। সাধারণ right edit restriction খুলতে পারে না। Audit event যায়।
- **Verification:** প্রতিটি approval route-এ self-approval test। "Approve করো, সঙ্গে সঙ্গে চাও" test। Repeat approval ও failed apply-এর test। Audit event অক্ষত থাকার test (T-11)।
- **Go-live gate:** OD-04 ও OD-19 সিদ্ধান্ত হয়েছে। এই task build ও test হবে, কিন্তু আসল user-এর জন্য বন্ধ থাকবে যতক্ষণ না IAM-005 চালু হয়, Platform-এর PLT-007 ও PLT-013 পাওয়া যায় এবং branch 1b merge হয় (§১)।
- **নির্ভর করে:** T-04, T-06, PLT-007 ও PLT-013 (System 1-এ দুটোই তার প্রথম slice-এর পরে, CX-01) · **সিদ্ধান্ত:** OD-03, OD-04। **এখন খোলা:** OD-14 (T-00-এ যাচাই), CX-01 (1b)।

### T-08 · সম্পর্ক শেষ ও revocation-এর বিস্তার

- **Design ও feature:** DR-09, DR-10 · IAM-007
- **Path (Proposed):** `internal/identity/`, `api/events/`
- **কাজ:** Membership শেষ, grant-এর মেয়াদ শেষ ও link শেষ হলে সংশ্লিষ্ট access বন্ধ। প্রভাবিত session revoke ও `session.revoked` event। History রাখা। Student চলে গেলে MVP-তে Registrar হাতে link শেষ করেন। `student.withdrawn` R2-তে A13-এর সঙ্গে এলে link withdrawal-এর কার্যকর সময়ে শেষ হবে, দেরির মাপ (lag metric) সহ (CX-04)।
- **Acceptance:** তিন ক্ষেত্রেই (membership শেষ, grant-এর মেয়াদ শেষ, link শেষ) পরের প্রথম request-এ read ও write বন্ধ। Withdrawal event-এর কার্যকর সময় ও lag metric R2-তে যাচাই হবে। একই ব্যক্তির অন্য school-এর membership অক্ষত। School Admin-এর "end all sessions" শুধু ওই school-এর membership-এর session শেষ করে; ব্যক্তির অন্য সন্তান ও role অক্ষত থাকে। পুরোনো record মোছা হয়নি।
- **Verification:** প্রতিটি কারণের জন্য "শেষ করো, সঙ্গে সঙ্গে চাও" test। একই event দুবার এলে একই ফল।
- **নির্ভর করে:** T-05, T-06 · **সিদ্ধান্ত:** OD-08 (সম্পর্ক শেষ হলে পুরোনো document-এও access নেই; দরকারে school নতুন করে দেয়)। Staff-এর চাকরি শেষের event কে দেবে তা জানা নেই। MVP-তে School Admin হাতে membership শেষ করবেন (অনুমান)। `student.withdrawn` R2-তে A13-এর সঙ্গে আসবে (CX-04)। প্রথম branch-এ Registrar হাতে link শেষ করেন। A13 event-এর consumer ও evidence R2 deliverable-এ।

### T-09 · পাঠানোর মুহূর্তের check — queue-তে থাকা notice ও file link

- **Design ও feature:** DR-07, DR-09, DR-10 · IAM-004, IAM-007
- **Path (Proposed):** `internal/identity/` (প্রাপকের entitlement function), `internal/notifications/` ও file service-এর সঙ্গে contract test
- **কাজ:** C4 ও C9-এর জন্য দুটি function। একটি দেয় এই student-এর এই right আছে এমন guardian-দের তালিকা। অন্যটি এক প্রাপকের জন্য এখনকার উত্তর দেয়। Worker session ছাড়া এটি ডাকতে পারে। Contract-এ লেখা থাকবে: C4 **প্রতিটি dispatch চেষ্টার ঠিক আগে**, provider-কে ডাকার শেষ ধাপ হিসেবে, আবার check করবে। Retry ও SMS fallback-ও নতুন dispatch। Check provider call-এর ঠিক আগে link-এর version আবার পড়ে।
- **Acceptance:** গ্যারান্টি (OD-19): effective commit-এর পরে শুরু হওয়া কোনো dispatch restricted guardian-এর কাছে যায় না। দুই ক্রমই test হয়। C4 contract: প্রতি (event, প্রাপক)-এর জন্য একটি টেকসই delivery record, যা retry, worker ও fallback ব্যবহার করে। Push-এর ফল অজানা হলে পরের channel-এর আগে provider-এর status মেলানো হয়। তিনটি মাপ আলাদা রাখা: একটি logical notice; প্রতি channel-এ সর্বোচ্চ একটি provider acceptance; পৌঁছেছে (provider যতটুকু প্রমাণ করতে পারে)। অন্য অনুমোদিত guardian ঠিক একটি logical notice পান। Log-এর field §৬.১ অনুযায়ী, বিষয়বস্তু ছাড়া। C3 সাড়া না দিলে পাঠানো স্থগিত থাকে।
- **Verification:** SC-09 (§৯)। Event ইচ্ছে করে দেরি করিয়ে একই test, যাতে প্রমাণ হয় সিদ্ধান্ত event-এর ওপর নির্ভর করে না।
- **নির্ভর করে:** T-06, T-07, C4 ও C9 owner-এর সম্মতি · **সিদ্ধান্ত:** OD-17 (emergency notice সন্তান-নিরপেক্ষ), OD-19। Custody অংশের go-live gate §১ অনুযায়ী।
- **যে ঝুঁকি থেকে যায়:** Provider-এর অজানা ফল; একসঙ্গে retry ও fallback; provider-এ delivery পেন্ডিং; C9 approval ও C3 commit-এর ফাঁক, যদি এক transaction না হয়; খোলা screen-এ নতুন request না এলে (DR-09-এর সীমায় বাঁধা); আগে চলে যাওয়া SMS ফেরানো যায় না। আগে দেওয়া signed link এখন download-এর সময় আবার check হয়। Offline cache OD-18 মানে। এগুলো কমানোর সিদ্ধান্ত C4, C9 ও C5 owner-এর সঙ্গে।

### T-10 · A8-এর import contract

- **Design ও feature:** DR-10 · IAM-003, IAM-004
- **Path (Proposed):** `internal/identity/`, `internal/students/`-এর সঙ্গে contract test
- **কাজ:** Preview, confirm ও undo-তে আলাদা `Authorize`। Origin (batch ও row) সহ guardian link request। Undo-তে link বা request ID + origin + expected version ধরে প্রত্যাহার, প্রতিটি item-এর আলাদা ফল; batch reference ধরে কখনো নয় (CX-12)।
- **Acceptance:** Design plan §৮.১-এর R1, R2, R8, R9 ও R10 মেলে। মাঝপথে guardian grant ব্যর্থ হলে student record-এর অবস্থা ও বাকি ধাপ চেনা যায়। একই confirm আবার এলে দ্বিতীয় link হয় না। Import থেকে zero outbound invite ও zero active right। Undo-তে: origin না মিললে `not_found`; version না মিললে বা custody restriction থাকলে `conflict`, link অক্ষত; বাকি item চলে। Undo-র summary-তে বাতিল হওয়া pending request ও প্রত্যাহার হওয়া আগের active link আলাদা। Replay key = batch + row; undo-র key = per-operation key।
- **Verification:** A8-এর সঙ্গে contract test: একই file দুবার, আংশিক ব্যর্থতা, undo, পরের edit-এ conflict, restricted link-এ conflict, অন্য batch-এর link-এ not_found, একই undo key দুবার।
- **নির্ভর করে:** T-03, T-06, System 3-এর contract · **সিদ্ধান্ত:** CX-12 (owner গৃহীত)

### T-11 · Audit evidence

- **Design ও feature:** DR-11 · IAM-014 (V04)
- **Path (Proposed):** `internal/audit/`-এর interface (শুধু ব্যবহার)
- **কাজ:** Event নিজে বের হয় T-05, T-06 ও T-07-এর ভেতরে (role grant ও revoke, guardian right বদল, custody অনুরোধ ও সিদ্ধান্ত, session revoke, sensitive deny): actor, school, record, কারণ, আগের ও পরের মান। T-11 যাচাই করে সম্পূর্ণতা ও immutability। এর সঙ্গে যোগ: সফল sensitive read (restricted document, report বা export) এবং দ্বিতীয় role দিয়ে approval-এর চেষ্টা (delegation-এর ক্ষেত্র পরে, IAM-010-এর সঙ্গে)।
- **Acceptance:** প্রতিটি sensitive পরিবর্তনের একটি event আছে এবং তা বদলানো যায় না। Sensitive read ও দ্বিতীয় role দিয়ে approval-এর চেষ্টাও আছে। Event-এ সন্তানের নাম বা document-এর বিষয়বস্তু নেই (§৬.১)।
- **Verification:** V04-এর audit test। Event বদলানোর চেষ্টা ব্যর্থ হওয়ার test।
- **নির্ভর করে:** T-05, T-06, T-07, PLT-006 (System 1-এ তার প্রথম slice-এর পরে, CX-01)

### T-12 · Administration web-এর screen

- **Design ও feature:** DR-02, DR-04, DR-05, DR-06, DR-07, DR-11 · IAM-003, IAM-004, IAM-014
- **Path:** Web checkout — **Pending · T-00**
- **কাজ:** A-02, A-03, A-04, A-06, A-07, A-08-এর item ও A-09। A-01-এর ন্যূনতম context bar (school ও role; P0)। A-08-এর অবস্থা: pending, applying, effective, returned বা rejected। A-06 হলো C3-র screen, যা A8-এর household screen থেকে খোলে। Matrix screen ও export একই version দেখায়। Design plan-এর §৯ ও §১০–১৪ অনুযায়ী।
- **Acceptance:** A-06-এ contact ও access দুই আলাদা ঘরে। A-04-এ matrix থেকে সারাংশ। Approve button requester দেখেন না। A-08 "Approved — effective" দেখায় শুধু effective commit-এর পরে। Matrix screen ও export-এর version এক। প্রতিটি screen-এর loading, empty, error ও success state আছে।
- **Verification:** Component ও end-to-end test। §১৪-এর check।
- **নির্ভর করে:** T-05, T-06, T-07, T-01, T-11 (audit reference), web skeleton ও DVL-003। Skeleton না থাকলে এই task আলাদা ছোট branch-এ যাবে (Proposed)।

### T-13 · Guardian app-এর অবস্থা

- **Design ও feature:** DR-03, DR-08 · IAM-004 (APP-001-এর contract)
- **Path:** Mobile checkout — **Pending · T-00**। Screen-এর owner C5।
- **কাজ:** সন্তানের তালিকার contract। G-02 ও S-09 shared component। সন্তান বদলালে আগের data সরানো ও দেরিতে আসা response ফেলে দেওয়ার নিয়ম। প্রতি switch-এ নতুন selection identity। App resume-এর আচরণ। App-switcher preview-তে সন্তানের তথ্য লুকানো। Offline-এ OD-18-এর নিয়ম (সিদ্ধান্ত হয়েছে): সন্তানের sensitive content শুধু বর্তমান online check-এর পরে; offline-এ শেষ হালনাগাদের সময় ও "connect to see"; সন্তান-নির্দিষ্ট নয় এমন সাধারণ তথ্য (যেমন school calendar) offline-এও দেখা যায়। Shared device-এ account বদলালে আগের account-এর notification ও cache সরানো।
- **Acceptance:** Design plan §৮.২-এর G6, G7, G9, G10, G12, G17 ও G18 মেলে। Offline বা resume-এ current online check-এর আগে cache থেকে কোনো sensitive content আসে না (OD-18)।
- **Verification:** Low-cost Android-এ দ্রুত switch-এর test। Revoked session-এ cache না দেখানোর test। App resume, app-switcher preview ও shared-device account বদলের test (SC-16)।
- **নির্ভর করে:** T-06, T-07, T-08, Apps owner-এর সম্মতি

### T-14 · V02, V03 ও V04-এর acceptance pack

- **Design ও feature:** সব · IAM-003, IAM-004, IAM-007, IAM-014
- **Path (Proposed):** `tools/`-এর isolation suite ও fixture
- **কাজ:** §৯-এর fixture ও scenario। সব data synthetic। প্রতিটি ফল Doc 07-এর format-এ। তিন আলাদা evidence gate: (১) contract (fixture), (২) আসল service (merge হওয়া Platform ও A8), (৩) UI (T-12, T-13)। C4, C5, A8, A9, C9 ও C6-এর consumer evidence আলাদা তালিকায়। DR-এর scope প্রথম branch-এর মধ্যে (DR-01–DR-11 ও DR-19-এর অংশ)।
- **Acceptance:** §৯-এর প্রতিটি scenario-র expected ও actual ফল ও evidence link আছে। Fixture দিয়ে পাওয়া প্রমাণ ও আসল integration-এর প্রমাণ আলাদা চিহ্নিত। "Evidence-এর সীমা" table (§৯) পূর্ণ।
- **Verification:** CI-তে প্রতি commit-এ চলে।
- **নির্ভর করে:** T-03 থেকে T-13

### T-15 · Completion report (প্রশাসনিক task)

- **Design ও feature:** প্রথম branch-এর DR-গুলো · IAM-003, IAM-004, IAM-007, IAM-014
- **Path:** Report-এর গন্তব্য: tracker-এর row ও CTO
- **কাজ:** Feature ID, PR, target branch, reviewed commit ও test evidence সহ বাংলায় report (§১৭)।
- **Acceptance:** Report §১৭-এর তালিকা ও আসল evidence-এর সঙ্গে মেলে।
- **Verification:** CTO report যাচাই করেন।
- **নির্ভর করে:** T-14

## ৯. V02, V03 ও V04 — scenario

**Fixture (synthetic)।** School A-তে School Management চালু, campus উত্তর ও দক্ষিণ। School B আলাদা school, নিজস্ব Registrar সহ। RN হলেন A-র উত্তর campus-এর Registrar। SA হলেন A-র School Admin। P হলেন A-র Principal। LA হলেন A-র Learning Admin। TG-র একই account-এ A-তে guardian ও B-তে teacher membership।

**Fixture A (contact বনাম access, ভুল sibling)।** Guardian G। সন্তান ১: published result ও notice right। সন্তান ২: attendance right। সন্তান ৩: G শুধু A8-এ emergency contact, link নেই। সন্তান ৪: একই household, G-র সঙ্গে link নেই। **GB** হলেন সন্তান ২-এর অন্য guardian: attendance right, push ও SMS fallback। Fixture A ও B থাকবে Platform-এর T1-এর একটি versioned shared fixture file-এ, তিন system-এর জন্য একই (CX-07, CTO-র সিদ্ধান্ত)।

**Fixture B (E2E-14)।** Guardian H তিন সন্তানের সঙ্গে linked, আলাদা right: X: result ও notice; Y: attendance ও notice; Z: fees ও receipts ও notice। প্রতি সন্তানে আলাদা channel preference (যেমন Y: push ও SMS, Z: শুধু in-app)।

| # | Test | কী করা হয় | প্রত্যাশিত ফল | Task |
|---|---|---|---|---|
| SC-01 | V02 · দুই school | RN উত্তরে import করেন, তারপর দক্ষিণে, তারপর B-র ID দিয়ে। Screen, API ও export তিন পথে। | উত্তরে allow। দক্ষিণে deny। B-তে not found, অস্তিত্বহীন ID-র মতোই। | T-03, T-04 |
| SC-02 | V02 · দুই school, উল্টো দিক | B-র Registrar একই কাজ A-র ID দিয়ে করেন। | নিজের school-এ সফল। A-র record অদৃশ্য। | T-03, T-04 |
| SC-03 | V02 · matrix | Doc 04 থেকে আলাদাভাবে লেখা প্রত্যাশিত ফল (matrix থেকে তৈরি নয়) তৈরি test-এর সঙ্গে তুলনা। প্রতি role-এ অন্তত একটি allowed ও একটি blocked কাজ। Scope-যুক্ত ঘরে scope-এর ভেতরের ও বাইরের target। স্থায়ীভাবে denied ঘরে শুধু deny। R10–R17-এর পথও। | দুই উৎসের ফল মেলে। অমিল হলে matrix বা Doc 04-এর ব্যাখ্যা owner-কে দেখানো হয়। | T-01, T-03 |
| SC-04 | V02, E2E-14 · Fixture B | H তিন সন্তানের প্রতিটি area চান। Screen, API, file link, report ও notice পাঁচ পথে। প্রতি সন্তানের নিজস্ব channel preference। | প্রতি সন্তানে শুধু তার right-এর area। Preference সম্মানিত। Preference কখনো right যোগ করে না। | T-06 |
| SC-05 | V02 · ভুল sibling | G চতুর্থ সন্তানের ID দিয়ে চান, পুরোনো link দিয়েও। | Not found। নাম বা class প্রকাশ শূন্য। | T-06, T-13 |
| SC-06 | V03 · মেয়াদ শেষ session | RN preview করেন। Session-এর মেয়াদ শেষ হয়। তারপর confirm। | Read ও write দুটোই বন্ধ। Student বা placement তৈরি হয়নি। | T-03, T-10 |
| SC-07 | V03 · revoked | Preview-র পরে SA RN-এর role তুলে নেন। তারপর confirm। আলাদা করে SA RN-এর session revoke করেন। | দুই ক্ষেত্রেই পরের request বন্ধ। | T-05, T-08 |
| SC-08 | V04 · নিজে approve নয় | RN document ছাড়া custody অনুরোধ দেন। তারপর document সহ দেন এবং নিজে approve করতে চান, screen ও সরাসরি route দিয়ে। | Document ছাড়া ফেরত। নিজের approve ব্যর্থ। অন্য approver-এর approve সফল। Scope-এর বাইরের approver deny। একই ব্যক্তি দ্বিতীয় role দিয়ে approve করতে চাইলে deny। সাধারণ right edit restriction তুলতে পারে না। তোলার জন্য document ও আলাদা approver লাগে। Audit event অক্ষত। Delegation-এর ক্ষেত্র IAM-010-এর সঙ্গে আসবে। | T-07, T-11 |
| SC-09 | V02, X-06 · custody change ও queue-র notice | সন্তান ২-এর attendance notice G ও GB-র জন্য queue-তে ঢোকে (দুজনেরই attendance right; GB push ও SMS fallback)। G-র ওপর restriction approve হয়। ক্রম (ক): effective commit, তারপর dispatch। ক্রম (খ): G-র check, তারপর commit, তারপর provider call। Event ইচ্ছে করে দেরি করিয়েও। | (ক) G কিছু পান না। (খ) OD-19-এর সীমার ভেতরে, "sent before effective" হিসেবে গোনা ও log করা। GB: একটি logical notice, প্রতি channel-এ সর্বোচ্চ একটি provider acceptance, push-এর ফল অজানা হলে fallback-এর আগে মেলানো; দুই channel-এই পৌঁছালে তা গোনা ও report করা। Event দেরি হলেও একই ফল। সব log বিষয়বস্তু ছাড়া (§৬.১)। কার্যকর commit-এ G-র খোলা online screen G-02 দেখায়। Pickup list **Pending · E-04**। | T-07, T-09 |
| SC-10 | V02 · সম্পর্ক শেষ | প্রথম সন্তান school ছাড়ে। আলাদা করে একজন staff-এর membership শেষ হয়। | Link `ended`। পরের request ও queue-র notice বন্ধ। History আছে। অন্য school-এর membership অক্ষত। | T-08 |
| SC-11 | V04 · privileged role | SA একটি privileged role দিতে চান এবং নিজে approve করতে চান। | Owner-এর approval ছাড়া grant কার্যকর নয়। নিজের approve ব্যর্থ। IAM-005 চালু না হওয়া পর্যন্ত privileged grant `step_up_required` পায়, Owner approve করলেও কার্যকর হয় না। সাধারণ scoped grant চলে। | T-05 |
| SC-12 | V02 · import undo | RN একটি batch undo করেন। A8 Reversed ও Kept row-এর link ID, origin ও version পাঠায়। একটি link-এ পরে হাতে right বদলানো হয়েছিল, একটিতে custody restriction আছে, আর একটি ID অন্য batch-এর। | বাকি link `ended` বা `cancelled`, আলাদা গোনা। হাতে বদলানো ও restricted link `conflict`, অক্ষত। অন্য batch-এর link `not_found`, অক্ষত। Conflict row-এর link পাঠানোই হয় না। একই undo key আবার পাঠালে একই ফল। | T-10 |
| SC-13 | V02, V03 · এক account, দুই school | TG A-র session-এর token দিয়ে B-র data চান, এবং উল্টোটা। | কোনো data আসে না। School বদলের পূর্ণ UX পরের deliverable-এ। | T-03, T-04 |
| SC-14 | V02 · learning ও school role আলাদা | একজন Learning Admin ও একজন বাইরের instructor school-এর student record খুলতে চান। একজন School Admin course খুলতে চান। তারপর আলাদা learning role দেওয়া হয়। তারপর তা তুলে নেওয়া হয়। | তিনটিই deny। Learning role দেওয়ার পরে allow। তুলে নিলে আবার deny। | T-03, T-05 |
| SC-15 | V02 · আগের link ও report | G সন্তান ১-এর result file-এর link নেন right প্রত্যাহারের আগে, তারপর download করেন। আলাদাভাবে, right প্রত্যাহারের আগে তৈরি report প্রত্যাহারের পরে collect করেন। | Download-এ deny। Report collect-এ deny। | T-09 |
| SC-16 | UI · সন্তান বদল, resume ও shared device | সন্তান ১ → ২ → ১ দ্রুত বদল। Restriction-এর পরে app resume। Shared device-এ account বদলের পরে পুরোনো notification খোলা। | পুরোনো selection-এর response ফেলে দেওয়া হয়। Current online check-এর আগে cache থেকে sensitive content নয়। আগের account-এর কিছুই দেখা যায় না। | T-13 |

প্রতিটি ফল এই format-এ লেখা হবে ([Doc 07](../../07-qa-pilot-and-acceptance.md)): `Test ID | Requirement/Feature ID | Build বা PR | Tenant ও role | Expected | Actual | Evidence link | Status | Reviewer`।

V03-এর OTP, password, second factor, device revoke, recovery ও recycled number-এর অংশ IAM-001, IAM-005 ও IAM-006-এর deliverable-এ প্রমাণ হবে। প্রথম branch V03-এর শুধু মেয়াদ শেষ ও revoked session-এর অংশ দেখায়।

### Evidence-এর সীমা

| Test | প্রথম branch কী প্রমাণ করে | অন্য owner-এর evidence লাগে |
|---|---|---|
| V02 | School, campus ও scope-এর isolation এবং matrix | C4, C5, A8, A9-এর consumer evidence |
| X-06 | C3-র সিদ্ধান্ত ও dispatch check-এর contract | C4-এর আসল dispatch; pickup E-04 |
| E2E-14 | Fixture B-তে C3-র right ও সিদ্ধান্ত | C5 app, C4 channel preference |
| V03 | শুধু মেয়াদ শেষ ও revoked session | Invite, reuse, expiry: ক্রম ৩। OTP, password, device, recovery, recycled phone: ক্রম ২। MFA: ক্রম ৪। |
| V04 | Custody ও privileged-এর নিয়ম, approval ও audit (stand-in দিয়ে contract পর্যন্ত) | Grading ও fee simulation এবং policy lock তাদের নিজস্ব domain-এ |

## ১০. পরের deliverable

প্রতিটি নতুন ছোট branch পাবে ([Doc 02](../../02-mvp-release-and-branch-plan.md))। নাম আপনি দেবেন। এখানে শুধু ক্রম ও নির্ভরতা।

| ক্রম | Deliverable | Feature | Design | আগে কী লাগবে |
|---|---|---|---|---|
| ২ | Sign-in, session, device ও recovery | IAM-001, IAM-006 | DR-12, DR-14 · F-02, F-09, F-10 | প্রথম branch, OD-07, OD-12। ন্যূনতম session এখানে পূর্ণ হয়। Pupil-ও IAM-001 অনুযায়ী sign in করে (OD-10; D-16, PDF পৃ. ৬৪); আলাদা activation code নেই। |
| ৩ | একক invite ও activation; CX-13 (project owner গৃহীত · client product owner-এর অনুমোদন ও tracker decision বাকি): অনুমোদনের পরে বেছে নেওয়া pending guardian link-এ ন্যূনতম batch invite-ও | IAM-002 (pilot অংশ) | DR-13 · F-01 | ক্রম ২; batch-এর জন্য SMS provider, SAS-013 ও IAM-005 |
| ৪ | Privileged MFA: Authenticator app (TOTP) ও backup code (OD-06) | IAM-005 | DR-15 · F-04 | ক্রম ২, OD-05, OD-06 (F2-এর identity comparison) |
| ৫ | School ও workspace switch | IAM-013 | DR-16 · F-03 | ক্রম ২, C1-এর PRD-002 |
| ৬ | Delegation | IAM-010 | DR-17 · F-08 | প্রথম branch, OD-09 |
| ৭ | Bulk onboarding | IAM-002 (বাকি R1) | DR-18 · A-11 | ক্রম ৩, SAS-013 |

**সম্পন্ন করার ক্রম: ক্রম ২ → ক্রম ৪ → ক্রম ৩ → ক্রম ৫ → ক্রম ৬ → ক্রম ৭।** কারণ: OD-05-এ Registrar privileged, তাই invite-এর আগে MFA লাগে।

Privileged invite (ক্রম ৩) নির্ভর করে ক্রম ৪-এর (MFA) ওপর। Pilot-এ custody approve ও privileged grant চালুর আগে MFA চালু থাকতে হবে।

এই ক্রম শুধু কাজের ধারাবাহিকতা। কোনো label বদলায়নি: ক্রম ২–৬ সবই tracker-এ MVP (ক্রম ৩ IAM-002-এর pilot অংশ), ক্রম ৭ বাকি R1। এই plan-এ এগুলোর task ভাঙা হয়নি। প্রতিটির আগে আলাদা করে বিস্তারিত লেখা হবে।

## ১১. Reuse

এই checkout-এ code নেই। তাই T-00 শেষ না হওয়া পর্যন্ত কোনো কিছুকেই "reuse করা যায়" বলে নিশ্চিত করা যাবে না। নিচের তালিকা হলো Identity নতুন করে না বানিয়ে যা ব্যবহার করবে।

| কী reuse হবে | Owner | Identity কীভাবে ব্যবহার করবে | অবস্থা |
|---|---|---|---|
| `SchoolContext` ও entitlement-এর সিদ্ধান্ত | Platform C1/C2 | Identity নিজে school বা product বের করে না (T-04)। | Proposed · E-03 |
| Approval engine, maker-checker (PLT-007) | Platform C9 | Custody restriction ও privileged role-এর approval। দ্বিতীয় approval engine বানানো হবে না (OD-14)। | Pending |
| Audit trail (PLT-006) | Platform C9 | Identity-র সব audit event (T-11)। | Pending |
| File service (PLT-013) | Platform C9 | Custody document restricted file হিসেবে থাকে। Identity শুধু reference রাখে (T-07)। | Pending |
| Transactional outbox ও job queue (Blueprint পৃ. ১০, `internal/platform/`) | Platform | তিনটি identity event। | উৎস · T-00-এ যাচাই |
| Certified OAuth 2.1 ও OpenID Connect library (D-05) | Platform/Identity | পরের deliverable-এর token ও sign-in। হাতে লেখা crypto নয়। | উৎস |
| `tools/`-এর tenant isolation suite (DVL-014) | Platform/QA | Identity নিজের table ও route একই suite-এ যোগ করে (T-02, T-14)। | উৎস |
| OpenAPI contract ও তৈরি করা Dart/TypeScript client | Architecture | Screen তৈরি client ডাকে। হাতে লেখা request code নয়। | উৎস |
| Problem-details error format (RFC 9457) | Platform | §৬-এর ছয়টি ফল স্থির code-এ মেলে। UI-র SafeMessage component code থেকে লেখা বসায়। | উৎস |
| `Idempotency-Key` ও record-version-এর নিয়ম | Platform | Identity-র প্রতিটি command (T-05, T-06, T-07, T-10)। | উৎস |
| A8-এর import batch ও row identifier | Students | Undo-র জন্য guardian link-এ batch reference (T-10)। | Proposed |
| C4-এর delivery log | Apps | "Suppressed" delivery সেখানেই লেখা হয়। Identity-র নতুন log নেই (T-09)। | Proposed |
| একটি web component library ও shared Flutter package (authentication, design system, localisation); design system DVL-003 | Product/design, App/web | T-12, T-13। | Pending |
| Permission matrix নিজে | Identity | একটিই উৎস: enforcement, A-09 screen, A-04-এর role সারাংশ, তৈরি test ও প্রকাশিত document (T-01)। | Proposed |

**নিয়ম।** T-00-এ identity-র কোনো code (user, role, session) পাওয়া গেলে default হলো সেটি বাড়ানো। কোনো task শুরুর আগে এই plan হালনাগাদ হবে।

## ১২. Migration-এর ঝুঁকি

নিচের সব ঝুঁকি **Proposed** বিশ্লেষণ। Backend না দেখা পর্যন্ত কোনোটি নিশ্চিত নয়।

| # | ঝুঁকি | কেন গুরুত্বপূর্ণ | কমানোর উপায় | Task |
|---|---|---|---|---|
| ১ | আসল backend-এ user, role ও session table আগে থেকে থাকতে পারে, অন্য নামে বা অন্য অর্থে | T-00-এর আগে জানা যায় না | T-02-এর আগে মিলিয়ে নেওয়া। সমান্তরাল দ্বিতীয় user table নয়। | T-00, T-02 |
| ২ | Person record school-এর অধীন নয় (এক account, একাধিক school), অথচ বাকি সব row-level security-র নিচে | School Admin phone দিয়ে খুঁজে জেনে ফেলতে পারেন যে ব্যক্তি অন্য school-এরও | Person খোঁজা শুধু এখনকার school-এর membership যা দেখায় তাই ফেরায়। "এই school-এ আগের account যোগ" চলে invite ও যাচাইয়ের পথে, search result দিয়ে নয়। | T-02 |
| ৩ | বিদ্যমান code-এর role-এর নাম Doc 04-এর ১৮টি role-এর সঙ্গে না-ও মিলতে পারে | Matrix-এর row নিঃশব্দে প্রযোজ্য হবে না | একটি mapping table। T-01 প্রকাশের আগে owner সেটি review করেন। | T-01 |
| ৪ | Import করা guardian-এর contact যেন app access না হয়ে যায় | Doc 08-এর import school-এর file থেকে "guardian link" নেয়। সেগুলো A8-এর household link | Import করা guardian শুরুতে access ছাড়া। Link `active` হয় শুধু সিদ্ধান্ত-করা নিয়মে (OD-01, OD-02)। | T-06, T-10 |
| ৫ | Phone number-এর format ও uniqueness: একই নম্বর ভিন্ন format-এ, পরিবারের শেয়ার করা নম্বর, পুনর্ব্যবহৃত নম্বর | ভুল account মিলে গেলে অন্যের সন্তানের data যেতে পারে | এক format-এ normalise। আন্দাজে account merge নয়। Conflict যায় review-তে (OD-07)। | T-06 |
| ৬ | Matrix-এর একটি row বদলালে ওই role যাঁদের আছে সবার access বদলায় | এক পরিবর্তন অনেক school-এ প্রভাব ফেলে | প্রতিটি পরিবর্তন নতুন matrix version। Identity ও module owner review করেন। প্রকাশের আগে তৈরি test নতুন version-এ চলে। | T-01 |
| ৭ | Schema বদল এক ধাপে করলে rollback ভাঙে (Expand-and-contract, উৎস: Blueprint পৃ. ৪১) | পুরোনো build নতুন schema-য় চলে না | আগে যোগ, তারপর সুইচ, পরে মোছা। একই release-এ কখনো মোছা নয়। | T-02, T-06 |
| ৮ | Event schema-র version: consumer (A8, C4, C5) পুরোনো version-এ চলতে পারে | Event ভাঙলে consumer-এর notice বা access ভুল হয় | এক version-এর ভেতরে শুধু additive বদল। ভাঙা বদল নতুন version, পুরোনোটির শেষ তারিখসহ (উৎস: INTERFACES)। | T-05, T-06, T-08 |
| ৯ | Custody document শিশুদের সংবেদনশীল data। Data কোথায় থাকবে ও কতদিন, তা এখনো counsel-এর কাছে (D-03, Doc 06 client inputs) | **Pending**। ভুল জায়গায় রাখলে আইনি ঝুঁকি | Identity document-এর বিষয়বস্তু রাখে না, শুধু reference। Retention মানে file service-এর নীতি। | T-07 |
| ১০ | প্রথম branch ন্যূনতম session record ব্যবহার করে (OD-15), পরের deliverable পূর্ণ sign-in দিয়ে তা বদলাবে | দুটি session model তৈরি হওয়ার ঝুঁকি | ন্যূনতম record হবে চূড়ান্ত record-এর উপসেট। ফেলে দেওয়ার মতো table নয়। | T-02 |
| ১১ | Fixture বনাম আসল integration: C1 বা A8-এর fixture-এ pass করা test, merge হওয়া interface-এর test-এর চেয়ে কম প্রমাণ দেয় | ভুল আত্মবিশ্বাস | দুই ধরনের প্রমাণ আলাদা চিহ্নে রাখা। | T-04, T-10, T-14 |
| ১২ | Pilot data: pilot-এর guardian ও staff আসবেন import ও invite দিয়ে | আসল শিক্ষার্থী বা guardian-এর data test-এ গেলে গোপনীয়তা ভাঙে | Test-এ আসল data নয়। Data agreement না হওয়া পর্যন্ত শুধু synthetic data (Doc 06, **Pending**)। | T-14 |
| ১৩ | Global Person: invite-এর conflict message-এ অন্য school-এর তথ্য ফাঁস | School Admin অন্য school-এর সঙ্গে ব্যক্তির সম্পর্ক জেনে ফেলতে পারেন | Invite conflict-এর message generic। School office contact বদলান শুধু যখন account-এর অন্য school-এ active membership নেই; নইলে ব্যক্তি নিজে (step-up-সহ) বা Desk School support (PLT-008)। Bulk duplicate report শুধু ওই school-এর নিজের data ঢাকে। School Admin-এর "end all sessions" শুধু ওই school-এর membership ঢাকে। | T-02, T-08 |

## ১৩. Rollout ও rollback

### Rollout

1. **Merge-এর ক্রম (উৎস: DEPENDENCIES)।** Platform আগে, Identity দ্বিতীয়, Students তৃতীয়। Platform-এর merged context-এ allowed, denied, revoked, custody change ও audit check pass না করা পর্যন্ত Identity merge হবে না।
2. **প্রতি ধাপে default deny।** Matrix-এ row নেই এমন action deny। নতুন consumer-এর row প্রকাশ না হওয়া পর্যন্ত তার কোনো access নেই।
3. **আগে demo school, synthetic data।** দুটি school, একটিতে দুই campus (AS-01)। Staging-এ V02-এর scenario pass করার পরেই pilot school।
4. **Go-live gate (custody ও privileged)।** Custody approve, privileged role grant ও privileged invite আসল user-এর জন্য চালু হয় শুধু যখন IAM-005 second factor চালু, Platform-এর PLT-007 ও PLT-013 পাওয়া গেছে এবং branch 1b merge হয়েছে (§১)। OD-04 ও OD-19 সিদ্ধান্ত হয়েছে।
5. **School ধরে চালু।** যেখানে user-এর আচরণ বদলায়, সেখানে Platform-এর feature flag (PLT-005) দিয়ে school-ভিত্তিক চালু। **Proposed**, C1-এর ওপর নির্ভরশীল।
6. **Staging ও production।** Merge হলে staging। Production-এ একই signed build অনুমোদিত promotion দিয়ে (উৎস: V19, DVL-013)।
7. **Matrix version ১।** কোনো school screen ব্যবহারের আগেই প্রকাশ। পরের version §১২-এর ঝুঁকি ৬ মানে।
8. **Release-এর পরে কী দেখা হবে (Proposed signal; school identifier থাকবে, শিক্ষার্থীর বিষয়বস্তু নয়):**
   - কারণ ধরে deny-এর হার
   - not-found-এর হার
   - অপেক্ষায় থাকা approval ও তাদের বয়স
   - right বদলের কারণে আটকানো delivery
   - session revoke-এর সংখ্যা
   - authorization-এর error ও timeout

### Rollback

| নিয়ম | অর্থ |
|---|---|
| এক version পিছনে, data হারানো ছাড়া (উৎস: V19) | Schema বদল expand-and-contract, তাই পুরোনো build নতুন schema-য় চলে। |
| **Rollback কখনো access আবার খুলবে না।** | নতুন version-এ করা custody restriction, revocation, শেষ হওয়া link ও membership পুরোনো build-এও কার্যকর থাকে। পুরোনো build নতুন restriction মানতে না পারলে ওই বদলের rollback অনুমোদিত নয়। এটি release-এর আগে যাচাই হয়। |
| Identity উত্তর না দিলে deny | Caller deny ধরে (§৬-এর নিয়ম ৭)। Notice ধরে রাখা ও আবার চেষ্টা হয়, "যদি যায়" ভেবে পাঠানো হয় না। "Error হলে allow" নামে কোনো switch নেই। |
| Release-এর পরে ভুল grant ধরা পড়লে | পুরোনো data ফেরত নয়। Audit-সহ revoke করে সামনের দিকে ঠিক করা। |
| Runbook | সন্দেহজনক cross-school access, security/privacy ঘটনা ও release rollback। Severity 1-এর সংজ্ঞা Blueprint পৃ. ৬০ অনুযায়ী। Wrong-child exposure security/privacy ঘটনার runbook ও V02-তে যাচাই হবে। |
| Rollback-এর মহড়া | Staging-এ মহড়া প্রথম branch-এর প্রমাণের অংশ (Proposed)। মহড়ায় এগুলো দেখা হয়: পুরোনো build ও worker, পুরোনো matrix, school flag বন্ধ, বাকি থাকা signed link, pending queue। সবগুলোতে revoked বা restricted access deny দেখাতে হবে। না মিললে access বন্ধই থাকবে এবং সামনের দিকে ঠিক করা হবে। |

## ১৪. Responsive, accessibility ও regression check

Screen-এর বিস্তারিত design plan-এর §১২–১৪-এ। এখানে লেখা আছে task "done" বলার আগে কী যাচাই করতে হবে।

**ক. Responsive** (T-12, T-13)

| Check | Screen |
|---|---|
| Phone-এ ওপরে Contact, নিচে App access। দুটোর আলাদা heading থাকে। | A-06 |
| Phone-এ role-অনুযায়ী list, grid-এর সঙ্গে একই ভাষা | A-09 |
| Phone-এ ব্যবহারযোগ্য | A-07, A-08 |
| প্রতি size-এ context bar-এ school ও role দেখা যায় | সব screen |
| Low-cost Android-এ সন্তান বদল PLT-017/V17-এর budget-এর মধ্যে | G-01 |
| Deny প্রতি size-এ একই রকম দেখায় | সব screen |
| Web-এ ৩২০ CSS px-এ reflow | সব web screen |
| সন্তানের নামে লম্বা Amharic নাম | G-01, A-06 |
| App resume ও app-switcher preview-তে সন্তানের তথ্য লুকানো | G-01 |

**খ. Accessibility** (T-12, T-13)

| Check | Screen |
|---|---|
| WCAG 2.2 AA review (PLT-018, V16) | A-04, A-06, A-07, A-09, G-01 |
| শুধু keyboard দিয়ে role grant ও custody অনুরোধ/approve চালানো | A-04, A-07, A-08 |
| Screen reader: matrix header-সহ table হিসেবে পড়া হয় | A-09 |
| Screen reader: child switcher কোন সন্তান নির্বাচিত ও বদলের পরের অবস্থা ঘোষণা করে | G-01 |
| Status ও deny message ঘোষিত হয় | সব screen |
| শুধু রং দিয়ে কোনো অবস্থা বোঝানো হয় না | সব screen |
| ২০০% text-এ কিছু হারায় না | সব screen |
| English ও Amharic copy আছে (Amharic native review **Pending**, E-06) | সব screen |

**গ. Regression**

| Check | কী ধরে | কখন চলে |
|---|---|---|
| Tenant isolation suite, identity-র table ও route-সহ | অন্য school-এর data দেখা | প্রতি commit (উৎস: DVL-014, Doc 08) |
| Matrix-এর প্রতিটি ঘর থেকে তৈরি test (allowed ঘরে allow, denied ঘরে deny, scope ঘরে ভেতর ও বাইরে) এবং Doc 04 থেকে আলাদাভাবে লেখা প্রত্যাশিত ফল | Matrix আর enforcement-এর অমিল | প্রতি commit |
| "Row নেই, action নেই" CI check | Catalogue-এ নেই এমন action | প্রতি commit |
| অভিন্ন not-found test (অস্তিত্বহীন ID-র সঙ্গে byte-level তুলনা) | Not-found থেকে তথ্য ফাঁস | প্রতি commit |
| "বদলাও, তারপর সঙ্গে সঙ্গে আবার চাও" test: role revoke, right প্রত্যাহার, custody approve, session revoke | পুরোনো allow | প্রতি commit |
| প্রতিটি approval route-এ self-approval test, দ্বিতীয় role দিয়েও | নিজে approve | প্রতি commit |
| SC-09, event ইচ্ছে করে দেরি করিয়ে | Queue-র notice ও event-নির্ভরতা | প্রতি commit |
| Dispatch-এর দুই ক্রমের test (effective commit আগে বা পরে) | Check ও provider call-এর ফাঁক | প্রতি commit |
| টেকসই delivery record test: প্রতি (event, প্রাপক)-এ একটি | দ্বৈত বা হারানো delivery | প্রতি commit |
| File download ও report collect-এ আবার check (SC-15) | আগে দেওয়া link বা report দিয়ে access | প্রতি commit |
| Offline বা resume-এ cache থেকে sensitive content নেই (OD-18) | Cache-এর মাধ্যমে ফাঁস | প্রতি commit |
| C1, A8, C4-এর fixture-এ contract test। প্রতিটি system merge হলে আসল interface-এ আবার | Interface-এর অমিল | Commit ও প্রতিটি merge-এর পরে |
| Idempotency: একই command দুবার, ফল একটি | দ্বৈত link বা grant | প্রতি commit |
| প্রতিটি sensitive বদলের audit event আছে ও অপরিবর্তনীয় | Audit ফাঁক | প্রতি commit |
| Rollback-এর মহড়া, যাতে restriction কার্যকর থাকার প্রমাণ মেলে | Rollback-এ access খুলে যাওয়া | Release-এর আগে (Proposed) |

কোনো task "done" নয়, যতক্ষণ এই তিন table-এ তার row-গুলোর ফল Doc 07-এর format-এ লেখা না হয়।

## ১৫. এখনো যা আটকে আছে

| বিষয় | কী আটকে আছে | কার কাজ |
|---|---|---|
| OD-16 repo ও base | সব coding, T-00 | আপনি (তথ্য), CTO |
| Client product owner-এর অনুমোদন (PR-06/CX-13) | ক্রম ৩-এর batch invite; tracker-এ decision reference | Client product owner, আপনি |
| OD-10-এর ফাঁক: নিজের phone বা email ছাড়া upper-primary pupil pilot-এ নিজে sign in করতে পারে না | Desk Learn-এর quiz (V10)-এ এমন pupil-এর অংশগ্রহণ | Desk Learn ও QA owner; CTO জানাবেন |
| DEPENDENCIES-এ 1a-র merge gate (CX-01) | Identity 1a-র merge | CTO |
| T-00-এর যাচাই: OD-14 (এক transaction), CX-11 (lock-এর পথ), OD-06 (TOTP library, F2) | T-05, T-07; T-02, T-03; ক্রম ৪ | Identity ও Platform owner |
| System 1 ও 3-এর plan-এ মেলানো (CX-03, CX-04, CX-08, CX-09, CX-12, CX-13, CX-15, এবং CX-11-এর বিস্তার; CTO alignment পাঠিয়েছেন) | আসল interface-এ contract test | CTO |
| CX-07: Platform T1-এর shared fixture file | T-14-এর fixture; version বদলে তিন owner-এর review | Platform owner (file), Identity ও Students (review) |
| CX-14: chunk-এ link request একই transaction-এ, নাকি idempotent command | T-10, T-02 ও System 3-এর T9/T10 | Identity ও Students owner, T-00-এর পরে; তারপর আপনি |
| Platform-এর PLT-006, PLT-007, PLT-013 | Branch 1b (T-07, T-11) ও custody go-live | Platform |
| E-04 pickup | পূর্ণ X-06 | Students ও Academics owner |
| AS-03–AS-08 (অনুমান) | নিজ নিজ অংশের চূড়ান্ত রূপ | Re-review-এ |

বাকি সব OD ২৬ নভেম্বর ২০২৬-এ সিদ্ধান্ত হয়েছে (design plan §৪.২)।

## ১৬. Branch ও coding শুরুর gate

1. এই plan approve করা মানে review-এর সম্মতি। তাতে coding শুরু হয় না।
2. আপনি implementation শুরু করতে বললে আমি প্রথমে আপনাকে অনুরোধ করব, যাচাই করা integration base থেকে একটি আলাদা feature branch তৈরি বা বরাদ্দ করে দিন। প্রস্তাবিত নাম `feat/identity/role-guardian-permissions`।
3. আপনি branch নিশ্চিত করে ওই branch-এ coding-এর অনুমতি দেওয়া পর্যন্ত আমি অপেক্ষা করব।
4. তারপর আমি branch যাচাই করব (নাম, base commit, remote) এবং আলাদা worktree বা checkout-এ কাজ শুরু করব।
5. Branch আমি নিজে তৈরি করব না।

**Merge-এর ক্রম (উৎস: [DEPENDENCIES](../../integration/DEPENDENCIES.md))।** Platform আগে, Identity দ্বিতীয়, Students তৃতীয়। Identity merge-এর আগে Platform-এর merged context দিয়ে allowed, denied, revoked, custody change ও audit check চালাতে হবে।

## ১৭. Completion report ও tracker

কাজ শেষ হলে report-এ থাকবে system ও module, Feature ID, IAM-002-এর pilot অংশ ও বাকি R1 আলাদা করে, repo ও PR, target branch, reviewed commit, CI ও test evidence এবং খোলা কাজ।

Tracker-এ যতটুকু প্রমাণ হয়েছে ততটুকুই বদলাবে: `Implementation status`, `Acceptance evidence` ও `Assigned owner`। IAM-002-এ `MVP slice status` ও `R1 remainder status` আলাদা। এই plan tracker-এর কোনো row বদলায়নি। C3-র ১৫টি row এখনো `Needs repository verification`।

## ১৮. Review-এর পরে যা বদলেছে

| # | Review-এর বিষয় | কী বদলেছে | কোথায় |
|---|---|---|---|
| ১ | Check-এর ক্রম ও পুরোনো শূন্য-নম্বর label | নতুন ক্রম: Session → Membership → C1 check → Role → Scope → ... ; পুরোনো label সরানো | §৫.১, §৬ |
| ২ | Custody-র কার্যকর মুহূর্ত | Effective commit সংজ্ঞা; এক transaction-এর প্রস্তাব (OD-14) | §৬ নিয়ম ৯, T-07 |
| ৩ | Custody ও privileged কাজ কখন আসল user পাবে | Go-live gate | §১, §৪, §১৩, T-05, T-07 |
| ৪ | প্রথম branch-এর আকার ও Platform-এর অপেক্ষা | 1a/1b ভাগের সুপারিশ; merge gate CTO আবার ঠিক করবেন | §৪, CX-01 |
| ৫ | Guardian-এর ভুল সন্তান ও not-found | সব child-specific deny `not_found`; `forbidden` শুধু staff | §৬, T-03, T-06 |
| ৬ | Area right ও channel preference | Area right প্রযোজ্য; preference right যোগ করে না; Fixture A ও B | §৫.৩, §৯, SC-04 |
| ৭ | Decision cache ও job | কোনো cache নয়; job-এর নিয়ম table; step-up gate | §৬ নিয়ম ৩, ৬, ১১ |
| ৮ | File ও report-এ পুরোনো access | Download ও collect-এ আবার check; target record থেকে | §৫.৩, §৬ নিয়ম ৪, ১০, SC-15 |
| ৯ | Notice-এর গ্যারান্টি ও দ্বৈত delivery | OD-19; টেকসই delivery record; দুই ক্রমের test; SC-09 নতুন করে | T-09, SC-09 |
| ১০ | Log-এ কী যাবে | Log-এর তিন ধরন | §৬.১ |
| ১১ | Audit-এর সীমা | Event T-05–T-07-এ; T-11 যাচাই করে; সফল sensitive read | T-11, §৬ নিয়ম ৮ |
| ১২ | Contact ও import | Contact lookup-এর সীমা; import-এ zero invite ও zero right; Link request record | §৫.২, §৭, T-10, CX-09 |
| ১৩ | Withdrawal ও session | Withdrawal-এর lag metric; "end all sessions" শুধু ওই school | T-08, §১২ |
| ১৪ | Shared fixture ও scenario | CX-07 সংশোধন; SC-14, SC-15, SC-16; Evidence-এর সীমা | §৫.৫, §৯ |
| ১৫ | System 1-এর v2-এর চাওয়া (CTO-র বার্তা) | CX-10 ও CX-11 খোলা item হিসেবে যোগ; CX-06-এ role-এর সংখ্যা; job-এর নিয়ম account lock ও membership suspend ধরে; account ও membership-এর অবস্থা এবং access change history | §৫.১, §৫.৫, §৬ নিয়ম ৬, §৭, §১৫ |
| ১৬ | CTO-র উত্তর (CX-01) | ভাগটি এখন "CTO-র সুপারিশ, owner pending"; 1b-কে G1 staging-এর আগে merge হতে হবে | §১, §৪, §৫.৫ |
| ১৭ | System 3-এর উত্তর (CTO-র বার্তা) | CX-03: `students.household.edit` দুই plan-এ মেলে। CX-04: `student.withdrawn` R2-তে, MVP-তে হাতে link শেষ। নতুন CX-12: undo link ID + origin + expected version ধরে (TD-08), C3 দিতে পারে। Paused import নিজে থেকে চলে না। | §৫.২, §৫.৫, §৭, T-08, T-10, SC-12, §১৫ |
| ১৮ | CTO-র প্রশ্ন: pilot-এ batch invite | নতুন খোলা item CX-13: কী লাগবে, প্রথম branch-এর খরচ, trade-off ও সুপারিশ; সিদ্ধান্ত owner-এর | §৫.৫, §১০, §১৫ |
| ১৫ | Rollout, rollback ও checks | Go-live gate step; rollback-এর মহড়ার তালিকা; নতুন responsive ও regression check | §১৩, §১৪, §১৫ |
| ১৯ | Owner-এর সিদ্ধান্ত (২৬ নভেম্বর ২০২৬) | OD-01–OD-19 ও CX-01–CX-13-এর অবস্থা; জরুরি hold (T-07, SC-17); MFA invite-এর আগে; §১৫ নতুন করে | §১, §৪, §৫.১, §৫.৫, §৬, §৮, §৯, §১০, §১৫ |
| ২০ | System 1 v3-এর তিনটি চাওয়া (CTO-র বার্তা) | CX-11-এর বিস্তার: মাঝে ফেরত দিলেও revoke ধরা; C3-র lock-এর ক্রম; exclusive change command ও 409। নতুন খোলা item CX-14 (chunk-এ link request: একই transaction, নাকি idempotent command) | §১, §৫.৫, §৭, §১৫ |
| ২১ | CTO-র cross-check (চারটি ছোট বিষয়) | CX-14-এর লেখা savepoint অনুযায়ী (সফল হলে একসঙ্গে commit, ব্যর্থ হলে দৃশ্যমান Failed ও retry); CX-12-এ `cancelled` বনাম A8-এর নিজের বাতিলের ভাগ; CX-07 CTO-র সিদ্ধান্ত (Platform T1-এ shared fixture, ১২-row table); নতুন CX-15 (`student.changed` C3 প্রথম branch-এ পড়ে না) | §১, §৫.২, §৫.৫, §৯, §১৫ |
| ২২ | PR-05 বাদ (owner-এর সিদ্ধান্ত, CTO-র মাধ্যমে, ২৬ নভেম্বর ২০২৬) | Owner-এর নিয়ম: Blueprint PDF-এর বাইরে কিছু নয়। Pupil sign-in IAM-001 ও D-16 অনুযায়ী (PDF পৃ. ৩২, ৬৪, ৯৮); activation code নেই। Client অনুমোদনের তালিকায় শুধু PR-06 (CX-13)। নিজের phone বা email ছাড়া pupil-এর ফাঁক ও V10-এর প্রভাব §১৫-এ। | §১, §১০, §১৫ |
| ২৩ | Owner-এর PDF scope review ও CX-15 addendum · ২৬ নভেম্বর | Emergency hold, hold row ও SC-17 বাদ। Pending request স্বাভাবিক approve/reject flow-এ থাকে। Counts-only consumer contract, rights preset, CX-15 alert, বাড়তি usability/SLA ও same-school Sev1 label বাদ। Platform ও C3-এর lock order মেলানো হয়েছে। PR-06 client approval pending। আগের entries ইতিহাস। | §৫.১/৫.২/৫.৫, §৭, T-02/07/12/13, §৯, §১৩–১৫ |
