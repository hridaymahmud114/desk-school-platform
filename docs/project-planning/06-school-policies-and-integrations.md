# School policies ও integrations

Pilot school-এর approved policy ব্যবহার করেই attendance, grading, fees, school access ও notifications verify হবে। প্রতিটি policy-তে school, version, effective date, owner, approver, example ও exception থাকবে। Policy বদলালে আগের published result বা issued invoice-এর rules trace করা যাবে।

## School policy pack

| Policy | কী agree হবে | Test example |
|---|---|---|
| **Academic setup** | Year, term, grade, section, calendar, subjects, teacher assignment ও timetable | Section move ও cover effective date মেলে। Old history থাকে। |
| **Grading ও publication** | Category weights, rounding, absent/exempt/missing rules, review, approval, correction ও report version | School-এর agreed marks sheet থেকে একই result বের হয়। |
| **Early years** | Rubric ও observation, guardian view, media consent এবং শিশুদের login timing | Narrative report-এ numeric marks আসে না। |
| **Attendance ও custody** | Present/late/absent rules, correction, alerts, authorized pickup ও per-child guardian rights | Corrected entry-তে history থাকে এবং right guardian update পান। |
| **School ও course fees** | Price, installment, credit, discount, bank verification, cash up, refund approvals এবং daily closing | দুই fee books মেলে। Unverified payment pending থাকে। |
| **Subscriptions** | Product-wise plan, active pupil/learner usage, invoicing, grace, restriction ও export | একটি product toggle করলে অন্য product ও school data ঠিক থাকে। |
| **Communication** | Template, language, safe preview, opt-in, SMS-only guardian, quiet hours ও emergency channels | Correct guardian একটি notice পান এবং sensitive details app-এর ভিতর থাকে। |
| **Privacy ও retention** | Purpose, child consent, file retention, support consent, audit, export ও deletion | Role revoke, consent update এবং scoped support session verify হয়। |

## Policy release ও fee collection

Grading বা fee policy approve করার আগে আগের term-এর data-তে নতুন rule চালিয়ে কোন pupil-এর result বা bill কত বদলাত তার report তৈরি হবে। Author ছাড়া অন্য approver report দেখে effective date দেবেন। Grading policy term খোলার সময় lock হবে। পুরোনো result বা issued invoice তার নিজের version ধরে রাখবে। School-এর agreed inputs ও expected outputs (golden files) দিয়ে হিসাব বারবার মেলানো যাবে। PDF পৃ. ১৯, ৩০–৩১, ৫৬, ৫৯।

School fees ও course fees দুটিরই merchant হলো school। টাকা সরাসরি school-এর account-এ যাবে। R2-তে course fees-এর জন্য school চাইলে দ্বিতীয় account দিতে পারবে (CFE-011)। Desk School শুধু নিজের subscription-এর টাকা পায়। Guardian request ও finance approval দুটো থাকলেই school-fee credit course-এ ব্যবহার করা যাবে। Unpaid school fee-তে course চালু থাকবে। Course fee বাকি থাকলে paid period শেষে শুধু সেই course বন্ধ হবে। School record অক্ষত থাকবে। PDF পৃ. ২৭, ৬৩, ১০৪।

Online payment-এ signature, provider-এর সঙ্গে সরাসরি server verification এবং attempt-এর amount, currency ও reference তিনটিই মিলতে হবে। এরপর একবার posting ও receipt হবে। App-এ ফিরে আসা payment-এর প্রমাণ নয়। Invalid confirmation-এ posting হয় না। Unknown status pending থাকে। Bank claim statement-এর সঙ্গে মেলান Finance Officer, reference একবারই ব্যবহার করা যায়। PDF পৃ. ২৯।

<a id="client-inputs"></a>
## Client inputs ও ছয় সপ্তাহের শুরু

PDF পৃ. ৫২ ও ৫৫ অনুযায়ী নিচের সাতটি input হাতে আসার দিনগুলো নথিভুক্ত হবে। সবগুলো পাওয়ার তারিখই D-18-এর শুরুর দিন। প্রতিটি row-এর দায়িত্ব Desk School-এর। Status বদলালে received date ও evidence reference একসঙ্গে বসবে। Password বা ব্যক্তিগত school data-এর বদলে অনুমোদিত storage-এর reference থাকবে।

| Input | Desk School-এর contact | পাওয়া গেলে যে evidence থাকবে | Status | Received date / evidence |
|---|---|---|---|---|
| Scope decision দেওয়ার ক্ষমতাসহ product owner | Client lead | নাম ও scope authority | Pending | — |
| Pilot school ও registrar, finance, academic lead, teacher ও family access | Pilot coordinator | School ও অংশগ্রহণকারীদের agreed access/schedule | Pending | — |
| Pilot policies ও rehearsal data | School lead | Data agreement, অনুমোদিত copy-এর reference ও policy samples | Pending | — |
| Payment ও SMS contract এবং sandbox | Provider account owners | দুই provider-এর contract ও sandbox access confirmation | Pending | — |
| App store developer accounts | App account owner | Google ও Apple account access confirmation | Pending | — |
| Data location ও children’s data নিয়ে counsel review | Client legal contact | লিখিত মতামতের reference | Pending | — |
| Amharic translation review | Client language lead | Native reviewer-এর নাম ও review availability | Pending | — |

## Provider ও readiness register

| Integration | Baseline | কোন তথ্য বা evidence লাগবে |
|---|---|---|
| **Payment gateway** | একটি aggregator দিয়ে শুরু। Market ও agreement অনুযায়ী bank/cash flows। | Provider owner, sandbox ও live credentials-এর safe location, verification rules, currency, settlement, cancellation, refund ও fallback। |
| **SMS, push ও email** | Common provider interface, language templates ও channel fallback। | Sender/region approval, test numbers, costs, delivery logs, retry/duplicate handling এবং outage route। |
| **Moodle** | R2 connected mode ও R3 course migration। Pilot school Moodle রাখলে D-01 অনুযায়ী প্রয়োজনীয় R1 connection। | Supported versions, user/course/assessment mapping, sync delay, conflict report, retry এবং school-owned official result flow। |
| **Learning content ও live meeting** | R2 meeting link। R3 supported H5P, SCORM ও LTI coverage। | Version/permission matrix, sample course এবং completion ও marks evidence। |
| **Hosting ও backup** | Foundation-এ region ও service level confirm হবে। | Client decision, local legal input, access/retention, restore region ও service owner। |
| **App distribution** | দুটি Desk School listing। School branding login-এর পরে। | Google/Apple owner, review readiness, build evidence, privacy details ও release account। |
| **Future plugins** | School অনুযায়ী module বা provider activate হবে। | Module dependencies, data access, scopes, version, limits, deactivation effect ও audit owner। |

Provider register-এ owner ও due date, test-ready/live-ready status, cost, contract, fallback, data handling ও evidence link থাকবে। Credentials secret storage-এ থাকবে। Planning doc-এ credential owner ও reference থাকবে।

## Blueprint decision references

| Decisions | কী track হবে |
|---|---|
| **D-01, D-02** | Pilot Moodle ব্যবহার করবে কি না, এবং Moodle 5.1 parity baseline। |
| **D-03, D-04, D-05** | Hosting ও data location, payment route এবং identity baseline। |
| **D-06, D-07, D-08** | App Store listing, SMS account ও pricing/usage plan। |
| **D-09, D-10, D-11** | Accounting ও payroll depth, pilot policies, sensitive data এবং GPS/AI timing। |
| **D-12, D-13, D-14** | Language/calendars, AISE source exclusion in Blueprint এবং launch size/service tier। |
| **D-15, D-16, D-17, D-18** | Admissions timing, young pupil accounts, ownership/licences এবং six-week staging estimate। |
| **D-19, D-20, D-21** | দুই product আলাদা, pricing/billing আলাদা, external learners R2 থেকে। |

Decision বদলালে old value, new value, approver, effective date, affected IDs ও release impact [scope index](README.md) এবং tracker-এ থাকবে। Pending client input-এর প্রমাণ [deliverables tracker](tracker/deliverables.csv)-এ link হবে।

Source coverage Blueprint পৃষ্ঠা ২৭–৩৩, ৩৫–৩৭, ৪০–৪৫, ৫১–৫৫, ৬৩–৬৪, ১০৪–১০৬।
