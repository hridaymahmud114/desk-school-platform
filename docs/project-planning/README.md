# Desk School ERP ও Desk Learn planning

এই folder থেকে পরের agent বা team member project-এর scope, MVP, backlog, data rules ও branch plan বুঝে কাজ ধরতে পারবে। **বর্তমান status হলো documentation ready।** এই workspace-এ planning files আছে। Backend checkout এবং GitHub base branch-এর তথ্য implementation শুরু হওয়ার সময় verify হবে। Branch creation GitHub-এ project owner করবেন।

## Project scope

একটি school **School Management**, **Desk Learn**, অথবা দুটোই ব্যবহার করতে পারবে। দুই product-এর কাজ, role access ও billing আলাদা থাকবে। School চাইলে class-এর সঙ্গে course link করবে এবং বেছে দেওয়া course marks school result-এ পাঠাবে। Shared platform থেকে login, permissions, notifications, files, reports ও operator console চলবে।

Blueprint v2-তে **৩৮২টি client requirement, ৫৩টি module এবং ৫৪১টি feature** আছে। আগের plan অনুযায়ী ৫৩টি module **১০টি system**-এ রাখা হয়েছে। প্রথম MVP-তে ৩৬টি module-এর দরকারি অংশ আছে। ১৭টি module পুরোপুরি backlog-এ আছে। R1-এর বাকি কাজ ও R2–R5 একই tracker-এ থাকবে।

## যে decisions ধরে plan সাজানো হয়েছে

| Decision | Working baseline | কীভাবে confirm হবে |
|---|---|---|
| MVP pilot | একটি campus ও upper-primary classes ধরে flow ছোট রাখা হয়েছে | Pilot school ও client scope review |
| Product access | School per tenant। School Management ও Desk Learn আলাদাভাবে চালু হবে | দুই demo school-এর access checks |
| Data ownership | Student history, official results, course marks এবং fees-এর নির্দিষ্ট owner থাকবে | Module map ও integration review |
| Fees | School fees, course fees ও platform subscription আলাদা | Finance policy ও reconciliation review |
| MVP ও পূর্ণ delivery | MVP আগে ব্যবহারযোগ্য হবে। Full R1–R5 tracker-এ থাকবে | Feature-level scope review ও client acceptance |
| Moodle | Default connected mode R2। Pilot school Moodle চালু রাখলে R1-এর প্রয়োজনীয় sync scope review হবে | Pilot decision D-01 |
| Young learners | R1 Early Years reporting tracker-এ আছে। Pilot-এ KG থাকলে প্রথম flow-তে আনতে হবে | Pilot class list ও scope decision |
| Campus transfer | `SIS-009` R1 inventory-তে আছে। Full campus transfer test G2-তে দেখানো হয়েছে | R1 section change ও R2 full transfer-এর acceptance clarify |
| Future extensions | Module contracts ও school-specific access রেখে পরে plugins যোগ হবে | Module dependency, permissions ও rollout review |

Source হলো **Desk School Technical Blueprint v2**, ২২ নভেম্বর ২০২৬। Original client SRS ও agreed commercial scope পাওয়া গেলে section-by-section match approve করতে হবে। Blueprint-এর *Added from practice* items আলাদা origin-এ track করা আছে।

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

Backend ready হলে প্রথমে actual repo, base branch এবং main build/test commands verify হবে। এরপর tracker ধরে existing backend-এর evidence বসবে। Platform, Identity ও Student Records-এর common rules agreed হলে project owner GitHub-এ প্রথম ২–৩টি branch তৈরি করবেন। Branch ধরা agent নিজের feature IDs, impacted roles, test ও evidence এখান থেকে পড়বে।

Milestone শেষ হলে active release, verified features, pending decision, branch বা PR link এবং পরের action এই section ও tracker-এ update হবে। **Planning snapshot-এ সব implementation status repo verification বা planned stage-এ আছে।**

## মূলকথা

আগে scope, tenant rules ও feature delivery হিসাব মিলবে। তারপর verified backend-এর ওপর ছোট branch-এ কাজ ধরে একটি school-এর end-to-end flow প্রমাণ করা হবে।
