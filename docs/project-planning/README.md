# Desk School ERP ও Desk Learn planning

এই folder থেকে পরের agent বা team member project-এর scope, MVP, backlog, data rules ও branch plan বুঝে কাজ ধরতে পারবে। **CTO handoff-এর জন্য Blueprint v2 planning baseline প্রস্তুত।** Feature delivery ও client sign-off-এর status tracker-এ আলাদা আছে। এই workspace-এ planning files আছে। Backend checkout এবং GitHub base branch-এর তথ্য implementation শুরু হওয়ার সময় verify হবে। Branch creation GitHub-এ project owner করবেন।

## Project scope

একটি school **School Management**, **Desk Learn**, অথবা দুটোই ব্যবহার করতে পারবে। দুই product-এর কাজ, role access ও billing আলাদা থাকবে। School চাইলে class-এর সঙ্গে course link করবে এবং বেছে দেওয়া course marks school result-এ পাঠাবে। Shared platform থেকে login, permissions, notifications, files, reports ও operator console চলবে।

Blueprint v2-তে **৩৮২টি client requirement, ৫৩টি module এবং ৫৪১টি feature** আছে। আগের plan অনুযায়ী ৫৩টি module **১০টি system**-এ রাখা হয়েছে। প্রথম MVP-তে ৩৬টি module-এর দরকারি অংশ আছে। ১৭টি module পুরোপুরি backlog-এ আছে। R1-এর বাকি কাজ ও R2–R5 একই tracker-এ থাকবে।

## যে decisions ধরে plan সাজানো হয়েছে

| Decision | Working baseline | কীভাবে confirm হবে |
|---|---|---|
| MVP pilot | D-01 অনুযায়ী একটি campus ও native Desk Learn। Upper-primary class দিয়ে প্রথম demo আমাদের planning assumption। D-16 শুধু pupil login-এর বয়স নিয়ে। | Pilot class list confirm হবে। G1 staging-এ Pre-KG narrative report-ও দেখাতে হবে। |
| Product access | School per tenant। School Management ও Desk Learn আলাদাভাবে চালু হবে | দুই demo school-এর access checks |
| Data ownership | Student history, official results, course marks এবং fees-এর নির্দিষ্ট owner থাকবে | Module map ও integration review |
| Fees | School fees, course fees ও platform subscription আলাদা | Finance policy ও reconciliation review |
| MVP ও পূর্ণ delivery | MVP আগে ব্যবহারযোগ্য হবে। Full R1–R5 tracker-এ থাকবে | Feature-level scope review ও client acceptance |
| Moodle | Default connected mode R2। Pilot school Moodle চালু রাখলে R1-এর প্রয়োজনীয় sync scope review হবে | Pilot decision D-01 |
| Young learners | R1 Early Years reporting tracker-এ আছে। Pilot-এ KG থাকলে প্রথম flow-তে আনতে হবে | Pilot class list ও scope decision |
| Campus transfer | `SIS-003` দিয়ে প্রথম placement। `SIS-009` মূল inventory-তে R1। E2E-07-এ G1 section change এবং G2 full campus transfer। | দুই gate-এর আলাদা evidence থাকবে। R1 feature-এর final sign-off-এ এই ভাগটি client-এর সঙ্গে নথিভুক্ত হবে। |
| Future extensions | Module contracts ও school-specific access রেখে পরে plugins যোগ হবে | Module dependency, permissions ও rollout review |

Source হলো **Desk School Technical Blueprint v2**, ২২ নভেম্বর ২০২৬। Original client SRS ও agreed commercial scope পাওয়া গেলে section-by-section match approve করতে হবে। Blueprint-এর *Added from practice* items আলাদা origin-এ track করা আছে।

## Scope-এর সীমা ও বাইরের dependency

Blueprint-এর পৃ. ৫২ অনুযায়ী school-এর fee নিজের কাছে রাখা, Moodle source code বা third-party plugin compatibility, default camera/screen monitoring, statutory payroll filing, নিজস্ব curriculum content, school-পিছু আলাদা code, admission/discipline/grades নিয়ে automated decision, hardware supply ও legal advice বর্তমান scope-এর বাইরে। ভবিষ্যৎ Desk School plugins আমাদের extension plan-এর অংশ। Scope বদলালে feature ID, gate, সময়, খরচ ও approver লিখে change record হবে।

Hosting, SMS, payment/store ও connected service-এর fees, devices, external security test এবং launch-এর পর operations/support আলাদা খরচ। বাইরের contract, school-এর Moodle বা legal clearance লাগলে sandbox-এ কাজের evidence ও live readiness আলাদা থাকবে। ছয় সপ্তাহের শুরুর inputs ও তাদের বর্তমান status [client readiness register](06-school-policies-and-integrations.md#client-inputs) থেকে দেখা যাবে।

## Docs খোলার order

| Document | কী কাজে লাগবে |
|---|---|
| [Requirements ও feature tracker](01-requirements-and-feature-tracker.md) | Feature, deliverable, release ও acceptance খুঁজতে |
| [MVP release ও branch plan](02-mvp-release-and-branch-plan.md) | প্রথম তিনটি branch, merge order ও next phase বুঝতে |
| [System module ও data map](03-system-module-and-data-map.md) | ৫৩টি module-এর owner ও data rules দেখতে |
| [Roles ও permissions](04-roles-and-permissions.md) | কে কোন কাজ বা record access করতে পারবে বুঝতে |
| [Workflows ও UI UX](05-workflows-and-ui-ux.md) | School ও Desk Learn-এর screens, daily flow এবং offline states দেখতে |
| [School policies ও integrations](06-school-policies-and-integrations.md) | Pilot rules, payment, SMS, Moodle ও external dependency confirm করতে |
| [QA pilot ও acceptance](07-qa-pilot-and-acceptance.md) | Test cases, demo evidence ও release gates চালাতে |
| [Migration launch ও handover](08-migration-launch-and-handover.md) | Import, cutover, recovery, training ও handover plan দেখতে |

[Requirements](tracker/requirements.csv), [features](tracker/features.csv), [requirement links](tracker/requirement-links.csv) এবং [deliverables](tracker/deliverables.csv) হলো planning data-এর মূল তালিকা।

## পরের agent কোথা থেকে ধরবে

Codex বা Claude Integration CTO-এর দায়িত্ব নিলে সর্বশেষ যাচাই করা কাজ, কোন system-এর report নিতে হবে ও দায়িত্ব ফেরত দেওয়ার নিয়ম এখান থেকে পড়বেন। [Integration CTO handoff](integration/CTO-HANDOFF.md)

Backend ready হলে প্রথমে actual repo, base branch এবং main build/test commands verify হবে। এরপর tracker ধরে existing backend-এর evidence বসবে। Platform, Identity ও Student Records-এর common rules agreed হলে project owner GitHub-এ প্রথম ২–৩টি branch তৈরি করবেন। Branch ধরা agent নিজের feature IDs, impacted roles, test ও evidence এখান থেকে পড়বে।

Milestone শেষ হলে active release, verified features, pending decision, branch বা PR link এবং পরের action এই section ও tracker-এ update হবে। **Planning snapshot-এ সব implementation status repo verification বা planned stage-এ আছে।**

## মূলকথা

আগে scope, tenant rules ও feature delivery হিসাব মিলবে। তারপর verified backend-এর ওপর ছোট branch-এ কাজ ধরে একটি school-এর end-to-end flow প্রমাণ করা হবে।
