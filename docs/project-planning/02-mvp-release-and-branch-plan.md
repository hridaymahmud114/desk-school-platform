# MVP release ও branch plan

এই plan-এর প্রথম goal হলো **school setup থেকে offline attendance ও guardian notice** পর্যন্ত একটি flow চালানো। এরপর **Results, Fees ও Desk Learn** parallel-এ এগোবে। Branch তৈরি করবেন project owner তাঁর GitHub account-এ, backend skeleton ready ও base branch verify হওয়ার পরে।

## MVP ও phased delivery

| Phase | কাজ | কী চালিয়ে দেখাতে হবে |
|---|---|---|
| **Foundation** | Existing backend ও app skeleton check। Tenant, role, student ও class rules agree। দুই demo school ও sample users। | দুই school-এ অনুমোদিত data দিয়ে ছোট school flow চলে। Access checks, product toggle এবং old history verify হয়। |
| **MVP school day** | Registration/import → class placement → teacher routine → offline attendance → guardian notice। | Offline register reconnect-এ একবারই submit হয়। Class বদলালে access ও history ঠিক থাকে। Notice সঠিক guardian পান। |
| **MVP result money ও learning** | Exam → marks → approval → report card। Invoice → verified payment → receipt → closing। Course → enrollment → lesson → assignment/quiz → progress। | সঠিক result, আলাদা school/course ledgers ও independent course। Class link চালু করলে selected marks official gradebook-এ যায়। |
| **R1 completion ও pilot** | MVP partials ও remaining R1 scope, apps, language, backup, payment, security, performance, device tests ও training। | Staging checks ও বাস্তব pilot term-এর attendance, coursework, result ও fees cycle পাস। |
| **R2–R5** | Admissions ও year cycle, learning depth, school operations এবং full accounting, scale ও AI। | প্রতিটি release-এর [acceptance evidence](07-qa-pilot-and-acceptance.md), rollout dependency ও handover। |

Tracker অনুযায়ী MVP প্রস্তাবে **১৩৮টি candidate + ২৬টি partial = ১৬৪টি feature row**, ৩৬টি module-এর মধ্যে। আরও **৫২টি R1 backlog**, এবং ২৬টির remaining checks পূর্ণ R1-এ track হবে। **১৭টি module** প্রথম MVP-এর বাইরে। A13–A15, A18–A28, B9, B15 এবং C7। Early Years-এর A14-তে R1 কাজ আছে এবং pilot-এ KG থাকলে relevant অংশ আগে আসবে। Moodle-সহ pilot হলে D-01 অনুযায়ী B15-এর প্রয়োজনীয় connection কাজ R1-এ review হবে। [Feature tracker](tracker/features.csv)

Blueprint-এর ৬ সপ্তাহ staging build ও handover-এর planning target। Live pilot term school calendar, store review ও external readiness-এর ওপর চলবে। Team estimate actual backend verification-এর পর বসবে।

## সব system-এর প্রথম feature branch

এই নামগুলো প্রথম deliverable-এর জন্য। এক system-এ পরের deliverable নতুন ছোট branch পাবে। Verified base review-এর পর GitHub-এ branch তৈরি হবে।

| System | Planned branch | Scope ও check |
|---|---|---|
| Platform | `feat/platform/tenant-product-foundation` | দুটো school, membership context, product access ও separation। PLT-001, PLT-002, PRD-001, PLT-005। |
| Identity | `feat/identity/role-guardian-permissions` | Role, class scope, guardian rights, revoked access। IAM-003, IAM-004, IAM-007, IAM-014। |
| Students | `feat/students/registration-import` | Registrar registration, student history, preview, duplicates ও guardian link। ADM-021, SIS-001, SIS-002, PLT-014। |
| Academics | `feat/academics/offline-attendance` | Teacher register, correct class, offline queue, duplicate prevention ও correction। ATT-001–005। |
| Results | `feat/results/marks-to-report-card` | Marks, grading, review, approval ও versioned report card। GRD-001–007, GRD-010। |
| Desk Learn | `feat/learn/independent-course` | Learning Admin, instructor assignment, independent course, enrollment ও quiz। LRN-051, LRN-062, LRN-071, LRN-077, LRN-006। |
| Fees | `feat/fees/verified-payment-receipt` | Separate fee books, verified payment, receipt ও day closing। FIN-006, FIN-010, FIN-014, CFE-002, CFE-005। |
| Apps ও communications | `feat/app/guardian-attendance-notice` | Guardian child view, safe notice ও correction। APP-001, APP-002, COM-001, COM-005, ATT-005। |
| School Operations | `feat/operations/student-support-case` | R2 restricted support case, access log ও reviewed escalation। BHV-005–007। |
| Integrations | `feat/integrations/payment-sms-adapters` | First approved providers, verified callbacks, SMS fallback ও duplicate checks। INT-003, INT-004। |

## প্রথম parallel batch

1. **আগে common rules।** Backend-এর actual default/integration branch, schema, auth context, existing tests ও owner খুঁজে verify হবে। Verified tenant ID, role scope, student ID, product access এবং app/event shape লিখে সবাইকে দেওয়া হবে।
2. **Platform, Identity ও Students branch।** Platform school এবং entitlement check চালাবে। Identity membership ও guardian access চালাবে। Students একই contract ধরে import, preview, duplicate ও office flow test শুরু করবে। Sample fixtures দিয়ে parallel কাজ চলতে পারবে।
3. **Merge order।** Shared tenant rules আগে review ও merge হবে। Identity-এর role checks মেলার পরে Students flow real access দিয়ে run হবে। Shared interface বদলালে সংশ্লিষ্ট module owner review করবেন।
4. **প্রথম demo।** দুটো school-এ import, guardian link ও role scope verified হলে Academics branch-এ attendance এবং App branch-এ guardian notice যোগ হবে। Demo evidence হবে correct list → offline register → একবার sync → correct guardian notice।
5. **পরের batch।** Results, Fees ও Desk Learn নিজেদের domain flow চালাতে পারবে। Release integration-এ class link, fee verification, report card ও product access একসঙ্গে test হবে। Payment/SMS adapter provider test account পাওয়া থেকে parallel চলতে পারবে।

## Branch handoff

প্রতি branch handoff-এ base, branch name, feature IDs, expected result, changed module/data contract, tests, PR/evidence ও pending কাজ লিখতে হবে। এক workspace-এ একাধিক parallel agent কাজ করলে প্রত্যেকের আলাদা checkout বা worktree ব্যবহার হবে। Verified change ছোট রাখলে নিয়মিত review ও merge করা যায়।

Platform contract, shared schema, roles ও event changes সংশ্লিষ্ট owner review করবেন। Verified change integration base-এ merge হলে অন্য branches সেই updated base ধরে integration test করবে। [QA plan](07-qa-pilot-and-acceptance.md) ও [project index](README.md)-এ milestone status এবং evidence রাখা হবে।
