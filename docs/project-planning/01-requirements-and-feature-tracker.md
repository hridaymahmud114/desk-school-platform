# Requirements ও feature tracker

Client delivery trace করার পথ হলো **requirement → feature বা deliverable → module owner → release → test evidence → client acceptance**। চারটি tracker file-এ Blueprint-এর inventory রাখা হয়েছে। Summary-তে সংক্ষেপ থাকা requirement original SRS পাওয়ার পরে exact wording ধরে review হবে।

## Tracker files

| File | Rows | কীভাবে পড়বেন |
|---|---:|---|
| [requirements.csv](tracker/requirements.csv) | ৩৮২ | Requirement ID, Blueprint summary, page, disposition ও client review status |
| [features.csv](tracker/features.csv) | ৫৪১ | Feature ID, module, system, product, Blueprint release, proposed MVP scope, test refs ও implementation evidence |
| [requirement-links.csv](tracker/requirement-links.csv) | ৯৪৭ | PDF-এর Appendix D ও Part 2-তে মেলা ৯০৪টি feature link, planning review-এর ৭টি feature link ও ৩৬টি deliverable link |
| [deliverables.csv](tracker/deliverables.csv) | ২২ | Data model, design, QA, environment, migration, restore, training, commercial ও launch work |

Excel বা spreadsheet-এ import করার সময় **Requirement ID, Feature ID এবং Deliverable ID text হিসেবে** রাখুন। `18.1.6` ও `E2E-05`-এর মতো ID join করার জন্য একই format দরকার। Filter করতে system, module, release ও proposed scope columns ব্যবহার করা যাবে।

## Scope ও status

| Scope label | মানে | Feature rows |
|---|---|---:|
| MVP candidate | সম্পূর্ণ feature প্রথম pilot scope-এর জন্য প্রস্তাব | ১৩৮ |
| MVP partial | প্রথম flow-তে feature-এর কিছু অংশ থাকবে। R1-এর remaining কাজ আলাদা note-এ আছে | ২৬ |
| R1 backlog | পূর্ণ R1 delivery-তে থাকবে, MVP-এর প্রথম flow-এর পরে | ৫২ |
| R2–R5 backlog | R2 ১৫৮, R3 ৫৮, R4 ৬৫, R5 ৪৪ | ৩২৫ |

Blueprint release সবসময় source baseline হিসেবে থাকবে। Approved timing বদলালে **Approved target release override** এবং **Decision reference**-এ লিখতে হবে। MVP candidate-এর final selection pilot school-এর কাজ ও client scope অনুযায়ী approve হবে। ৩৬টি *Added from practice* feature origin দিয়ে filter করা যাবে। Approval status ও origin আলাদাভাবে track হবে।

Implementation status হবে evidence অনুযায়ী। **Needs repository verification**, **Planned**, **In progress**, **Ready for QA**, **Verified** এবং **Client accepted** ব্যবহার করা যাবে। Repo-তে code পাওয়া, test pass হওয়া ও client acceptance তিনটি আলাদা update। ২৬টি partial row-তে `MVP slice status` ও `MVP slice evidence` দিয়ে pilot অংশ track হবে। `R1 remainder status` ও `R1 remainder evidence` দিয়ে বাকি অংশ track হবে। দুই অংশের evidence মেলার পরে পুরো row Verified করা যাবে। অন্য scope-এর row-তে এই চারটি field খালি থাকবে। Assigned owner, PR বা build এবং test evidence যতগুলো বাস্তবে পাওয়া যায় tracker-এ বসবে।

## Blueprint mapping review

Appendix D-এর ১৩টি item-এ সরাসরি Feature ID ও release-এর pair নেই। এগুলো architecture, data model, scope decisions, test ও handover deliverables-এর সঙ্গে link করা হয়েছে। যেমন `18.1.6` থেকে staging/production, QA pack, monitoring ও restore evidence আলাদা deliverable। `14.3.1`-এর provider coverage-এ Appendix D-এর `INT-004`-এর সঙ্গে Moodle, learning standards ও video class-এর feature links planning review হিসেবে যোগ করা হয়েছে।

| Trace example | কী evidence থাকবে |
|---|---|
| `E2E-05` → `ATT-004`, `ATT-005` → A9 → V07, V08 | Teacher absent/late entry দেন। অনুমোদিত guardian একটি safe notice পান। Correction-এর পরে update notice ও history থাকে। |
| `18.1.6` → `DVL-013`–`DVL-016` | Delivery environments, tests, monitoring ও restore rehearsal-এর links। |
| `SIS-009` → A2 → V11 ও V22 | R1 section move এবং পুরো campus move-এর আলাদা sample, timing decision সহ। |
| School ও course fees → `PRD-005`, `CFE-002`, `CFE-005` → V09 | আলাদা invoice, receipt ও books। একটির payment-এ অন্যটির balance বদলায় না। |

**Source test IDs** Appendix B-তে দেওয়া সরাসরি refs। **Suggested verification IDs** আমাদের feature ধরে বেছে নেওয়া প্রাসঙ্গিক tests। কোনো named test আংশিক coverage দিতে পারে। প্রতিটি row-এর **Feature acceptance case** (যেমন `G5/INV-001`) দিয়ে তার পুরো wording-এর demo, expected result ও evidence রাখা হবে। Suggested field খালি থাকলেও ওই case প্রযোজ্য। একাধিক gate-এ প্রমাণ লাগলে QA plan-এর stage-ও প্রযোজ্য (যেমন SIS-009 ও V22)।

**Baseline decision IDs** দেখায় Blueprint-এর কোন নির্দিষ্ট decision ওই feature-কে সরাসরি প্রভাবিত করে। Project-wide D-13, D-17 ও D-18-এর scope, ownership ও timeline project index, commercial deliverable এবং release plan-এ থাকবে। **Decision reference** ও **Approved target release override** কেবল অনুমোদিত change-এর জন্য। এই দুই field খালি থাকলে Blueprint release-ই কার্যকর।

৩৮২টি summary ও ৫৪১টি feature Blueprint ধরে যাচাই করা হয়েছে। Client-এর original SRS পেলে `DVL-001`-এ exact wording মেলানো হবে। Blueprint-এর V-tests ও X-scenarios, decision baseline, এবং আমাদের MVP/branch planning-এর origin আলাদা। [QA plan](07-qa-pilot-and-acceptance.md)-এ X-01–X-10-এর status ও evidence row আছে।

## Release review

Release review-তে ৩৮২টি source row-এর coverage, ৫৪১টি feature-এর module ও target, partial work এবং non-feature deliverables মিলবে। Test evidence [QA plan](07-qa-pilot-and-acceptance.md)-এ নির্ধারিত format-এ link হবে। Scope exception হলে client decision, reason, নতুন release এবং effect tracker-এ থাকবে। পরের agent [project index](README.md) থেকে current decision ও next action ধরবে।
