# Data migration launch ও handover

School data import, release readiness এবং client handover এক plan-এ থাকবে। প্রতিটি stage-এর owner ও evidence tracker-এর relevant deliverable-এ link হবে। Milestone dates actual backend, provider readiness, pilot school ও client approvals অনুযায়ী বসবে।

## School data import

1. **Collect ও map।** School-এর approved export-এ student ID, names, grade, class, guardian links, active status ও agreed fee/opening-balance data ম্যাপ করুন। Source row ID ও mapping version রাখুন।
2. **Dry run ও review।** Demo বা staging-এ file validation, duplicate review, broken guardian link, missing class বা unexpected balance-এর report বের করুন। Registrar master records confirm করবেন। Finance opening balances review করবেন।
3. **Rehearse ও reconcile।** Tenant ID, counts, placement, guardian scope, data totals ও source report মিলিয়ে import করুন। Test-এ undo/rollback করে পুরোনো state verify করুন।
4. **Cutover।** Approved import window, backup, ready checklist ও scheduled communication agree হবে। Pilot live import-এর পর role access, first attendance এবং financial opening totals যাচাই হবে।
5. **Record evidence।** Input file reference, mapping, dry-run/duplicate report, migration owner, accepted totals, timings, rollback point ও review links `DVL-012`-এ থাকবে।

## Launch readiness

| Check | Owner area | Evidence |
|---|---|---|
| **Scope ও permissions** | Product, QA ও platform | Approved MVP/R1 list, remaining backlog, tenant/role checks, support access ও module toggles |
| **Build ও deployment** | Platform ও app/web | Staging build, environment setup, release version, rollout/rollback rehearsal এবং store review status |
| **Provider readiness** | Finance, platform ও client | Payment verification ও payout path, SMS fallback, provider contracts, support contacts ও cost |
| **Data ও infrastructure** | Registrar, finance ও operations | Import totals, backup/restore test, record retention এবং isolated single-school recovery |
| **User readiness** | School lead ও product | English/Amharic guides, teacher/office/guardian training, support contact ও pilot schedule |
| **Pilot ও sign-off** | Client owner ও QA | G1 staging tests, V20 pilot term, fees reconciliation, result accuracy ও issue resolution |

Blueprint target অনুযায়ী core service monthly **৯৯.৫% availability**, critical recovery-তে **১৫ মিনিটের data loss সীমা** ও **৪ ঘণ্টার recovery time** planning targets। Restore evidence-এ measured loss, duration, record totals ও follow-up থাকবে। School-এর capacity এবং provider limits evidence থেকে সময়ের হিসাব update হবে।

## Cutover ও rollback

Launch owner build, migration ও configuration-এর agreed snapshot, rollback point এবং test evidence প্রস্তুত রাখবেন। Cutover-এর পরে school users-এর sign-in, attendance, course access, result view ও payment confirmation sample চালানো হবে।

Data isolation, incorrect published result, lost attendance rows, payment mismatch বা major service issue দেখা গেলে incident owner issue contain করবেন, logs/evidence রাখবেন এবং agreed rollback/recovery scope activate করবেন। Restore আগে isolated environment-এ verify হবে। Result correction নতুন approved version হিসেবে যাবে। Fees correction-এর audit ও reconciliation থাকবে। Guardian notice ও app status issue অনুযায়ী update হবে।

## Operations ও support

| Cadence | কী দেখবে |
|---|---|
| **Daily** | Pending payments, reconciliation differences, attendance/notice queues, backup age ও failed jobs |
| **Weekly** | Release readiness, onboarding progress, support issues, vendor follow-up ও recurring failures |
| **Monthly** | Restore check, access review, provider usage ও cost, alert owners |
| **প্রতি term** | Result/fees surge capacity, restore drill, roll-forward readiness, training refresh |

Desk School-এর service owner, onboarding lead, subscription finance contact, privacy/security owner এবং provider contacts থাকবে। Pilot school-এর School Admin, Principal/Owner, Finance approver, Registrar ও Safeguarding lead-এর role current রাখুন। গুরুতর incident-এর জন্য escalation, acknowledgement target ও client communication owner agreed support terms-এ থাকবে।

## Client handover pack

| Deliverable area | কী যাবে |
|---|---|
| **Scope ও ownership** | Approved requirements, delivery matrix, pending approved backlog, architecture/data/permission docs এবং commercial ownership record |
| **Apps ও operations** | Web, Android, iOS ও operator releases যেখানে agreed। Version list, environment access roles, monitoring ও incident guide। |
| **Data ও training** | Approved migration mappings, restore evidence, bilingual user guides, role-wise training এবং first-week support plan |
| **Testing ও acceptance** | UAT, device/language checks, pilot results, feature evidence ও client approval |
| **Costs ও dependencies** | Hosting, SMS, payment fees, store fees, licences, service owners, contracts এবং future release dependencies |

প্রতি release handover-এর পরে [project index](README.md) ও [master tracker](01-requirements-and-feature-tracker.md)-এ acceptance link, active version, delivery owner ও পরের milestone update হবে। এই record থেকেই নতুন agent কাজ ধরতে পারবে।

Source coverage Blueprint পৃষ্ঠা ৪০–৪৫, ৫১–৬০, ১২৫–১২৬।
