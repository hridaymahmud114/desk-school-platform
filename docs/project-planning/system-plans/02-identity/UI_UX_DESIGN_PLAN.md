# System 2 · Identity and Permissions — UI/UX design plan

**অবস্থা:** Draft v4 · ২৬ নভেম্বরের owner সিদ্ধান্ত যাচাই করে যুক্ত · owner review-এর জন্য প্রস্তুত
**তারিখ:** ২৬ নভেম্বর ২০২৬
**Module:** C3 Identity, roles and access
**সঙ্গের plan:** [Implementation plan](IMPLEMENTATION_PLAN.md)

**Label পড়ার নিয়ম।** **উৎস** মানে Blueprint v2, tracker বা Docs 02–08 থেকে সরাসরি নেওয়া। **অনুমান** মানে উৎসে স্পষ্ট নেই বলে আমরা ধরে নিয়েছি। **Proposed** মানে নতুন প্রস্তাব, আপনার অনুমোদন ছাড়া scope-এ ঢুকবে না। **Pending** মানে সিদ্ধান্ত বা প্রমাণ বাকি, তার ওপর নির্ভরশীল কাজ আটকে আছে।

এই doc-এ চারটি ID বারবার আসবে। **DR** হলো design requirement, **OD** হলো খোলা সিদ্ধান্ত, **AS** হলো অনুমান, **PR** হলো নতুন feature-এর প্রস্তাব। Implementation plan একই ID ধরে কাজ সাজিয়েছে।

## ১. এক নজরে

- Identity-র মূল কথা একটাই। **কে কী করতে পারবেন, সেই সিদ্ধান্ত server নেয়। Screen শুধু সেই সিদ্ধান্ত দেখায়।** Button লুকানো কোনো নিরাপত্তা নয়।
- প্রথমে যাবে চারটি feature: **IAM-003** scoped role, **IAM-004** প্রতি সন্তানে আলাদা guardian right ও custody limit, **IAM-007** সম্পর্ক শেষ হলে access শেষ, **IAM-014** published permission matrix।
- Household contact থাকা আর app-এ সন্তানের তথ্য দেখার অধিকার দুটো আলাদা জিনিস। Screen-এও দুটো পাশাপাশি দুই আলাদা ঘরে থাকবে।
- এই checkout-এ কোনো code বা screen নেই, শুধু planning docs আছে। তাই existing screen-এর audit **Pending**। নিচের সব layout নতুন প্রস্তাব।
- OD-01–OD-19-এর সিদ্ধান্ত হয়েছে (২৬ নভেম্বর ২০২৬, §৪.২)। এখনো খোলা: OD-16-এর repo তথ্য; PR-06 scope বদল, তাই client product owner-এর অনুমোদন ও tracker decision। PR-05 (pupil activation code) বাদ, কারণ তা Blueprint PDF-এর বাইরে (OD-10)।
- **Go-live gate (custody ও privileged কাজ)।** OD-04 ও OD-19 এখন সিদ্ধান্ত হয়েছে। Custody approve, privileged role grant ও privileged invite build ও test হবে, কিন্তু আসল user-এর জন্য বন্ধ থাকবে, যতক্ষণ না: IAM-005 second factor চালু হয়, Platform-এর PLT-007 approval engine ও PLT-013 file service পাওয়া যায় এবং branch 1b merge হয়। Test stand-in শুধু contract প্রমাণ করে।

## ২. Context — C3 কীভাবে কাজ করে

### ২.১ একই সিদ্ধান্ত, একই ক্রমে

প্রতিটি request-এ server একই ক্রমে প্রশ্ন করে। আগে C3 check ১–২ (Session ও Membership), তাতে SchoolContext (school, actor, scope) তৈরি হয়। তারপর Platform-এর **C1 check**: school-এ product ও module চালু আছে কি না। তারপর C3 check ৩–৫। অর্থাৎ check ১–৫ C3-র। Check ৬–৮ প্রয়োগ করে সেই module, যে ওই record বা কাজের owner। Custody restriction ও privileged role grant-এর owner C3 নিজে। [Doc 04](../../04-roles-and-permissions.md) · [INTERFACES](../../integration/INTERFACES.md#student-walkthrough) · Blueprint পৃ. ৩২ (**উৎস**)

এই সুনির্দিষ্ট ক্রম আমাদের প্রস্তাব (**Proposed**), Doc 04 ও Blueprint পৃ. ৩২ মিলিয়ে। [DEPENDENCIES](../../integration/DEPENDENCIES.md) শুধু বলে দুটো শর্ত একই server decision-এ থাকবে। আসল enforcement point owner-রা যাচাই করবেন।

| # | Check | প্রশ্ন | কখন আটকায় |
|---|---|---|---|
| ১ | Session | এটি কি চেনা device-এর বৈধ session? | মেয়াদ শেষ, revoked বা পুরোনো token আবার ব্যবহার |
| ২ | Membership | ব্যক্তি কি এই school-এর সক্রিয় সদস্য? এখান থেকে SchoolContext আসে। | চলে গেছেন বা suspended |
| C1 check | Entitlement (Platform) | এই school-এ এই product ও module চালু? | Product বা module বন্ধ, school suspended। Restricted school write আটকায়, তবে policy ও role অনুযায়ী read ও export চলে। |
| ৩ | Role | তাঁর কোনো role কি এই কাজের অনুমতি দেয়? | Role-এর permission-এ কাজটি নেই |
| ৪ | Scope | Role-এর scope কি এই record পর্যন্ত পৌঁছায়? | অন্য campus, grade band বা teaching group |
| ৫ | Relationship | Guardian হলে এই সন্তানের সঙ্গে link ও এই right আছে? | Link নেই, right দেওয়া হয়নি বা custody restriction |
| ৬ | Sensitivity | এই ধরনের record এই role-এর জন্য খোলা? | Health, safeguarding, discipline বা HR |
| ৭ | State | Record-এর এখনকার অবস্থায় কাজটি চলে? | Published result, বন্ধ term, locked policy |
| ৮ | Approval | কাজটিতে দ্বিতীয় ব্যক্তি লাগে? | Requester নিজেই approver হতে চাইছেন |

### ২.২ একজন Registrar — campus scope ও import

ধরুন School A-তে School Management চালু। তার দুটি campus, উত্তর ও দক্ষিণ। Registrar-এর role grant-এ scope লেখা আছে **উত্তর campus**।

- **উত্তরে import।** C1 জানায় product চালু। C3 দেখে session বৈধ, membership সক্রিয়, Registrar role import-এর অনুমতি দেয় এবং target campus তাঁর scope-এ। উত্তর হয় allow। Student Records A8 সেই সিদ্ধান্ত preview, confirm ও undo তিন ধাপেই আলাদা করে প্রয়োগ করে।
- **দক্ষিণে import।** চতুর্থ check-এ আটকায়। Product চালু থাকা যথেষ্ট নয়।
- **School B-র কোনো ID।** Record "পাওয়া যায়নি" দেখায়। ওই record আছে কি নেই, তা বোঝা যায় না।
- **কে import করতে পারেন।** কেবল সেই Registrar, যাঁর scope target campus পর্যন্ত পৌঁছায় (AS-05)। Learning Admin, মেয়াদ শেষ হওয়া session বা প্রত্যাহার করা role দিয়ে import হয় না। Registrar হলেও marks, ledger ও health detail খোলে না।

Pilot school-এ campus একটি (D-01)। তাই অন্য campus-এ আটকানোর প্রমাণ demo school-এ দেখাতে হবে (AS-01)।

### ২.৩ তিন সন্তানের guardian — contact বনাম access

| কাজ | কে করেন | কোন system রাখে | এতে app access আসে? |
|---|---|---|---|
| Household contact, sibling ও emergency contact লেখা | Registrar | Student Records A8 | না |
| Guardian account-এর সঙ্গে নির্দিষ্ট সন্তানের link ও right দেওয়া | Registrar, পরিচয় মেলার পরে | Identity C3 | হ্যাঁ, শুধু ওই সন্তানের ওই right |
| Custody restriction | Registrar অনুরোধ করেন, **অন্য একজন** approve করেন | Identity C3 | Access কমে বা বন্ধ হয় |

নিচের তিন সন্তান উদাহরণ। এগুলো pilot-এর default right নয় (OD-02)।

| সন্তান | Registrar যা নথিভুক্ত করেছেন | Guardian কী পাবেন |
|---|---|---|
| প্রথম | Published result ও notice-এর right | ওই সন্তানের প্রকাশিত ফল ও notice |
| দ্বিতীয় | শুধু attendance-এর right | ওই সন্তানের attendance |
| তৃতীয় | শুধু emergency contact | ওই সন্তানের কোনো portal access নেই |

**Fixture A ও B।** Fixture A (contact বনাম access, ভুল sibling): guardian G। সন্তান ১ = published result ও notice right। সন্তান ২ = attendance right। সন্তান ৩ = G শুধু A8-এ emergency contact, link নেই। সন্তান ৪ = একই household, G-র সঙ্গে link নেই। **GB** = সন্তান ২-এর অন্য guardian, attendance right, push ও SMS fallback। Fixture B (E2E-14): guardian H তিন সন্তানে আলাদা right: X = result + notice, Y = attendance + notice, Z = fees ও receipt + notice। প্রতি সন্তানে আলাদা channel preference (যেমন Y push + SMS, Z শুধু in-app)। নিয়ম: Preference (COM-006, C4) delivery কমাতে পারে, কখনো right বাড়াতে পারে না।

App, report, file download ও notice delivery প্রতিবার সংশ্লিষ্ট সন্তানের **এখনকার** right দেখে। Link pending থাকলে access বন্ধ থাকে। Right প্রত্যাহার হলে পরের request ও queue-তে থাকা notice-এ তা কার্যকর হয়। [INTERFACES, C3 authorization contract](../../integration/INTERFACES.md) · V02 · E2E-14 · X-06

### ২.৪ MVP ও backlog

Scope ও release label [tracker](../../tracker/features.csv)-এর মতোই রাখা হয়েছে। এই plan কোনো label বদলায়নি।

| Scope | Feature |
|---|---|
| **MVP candidate (৯টি)** | IAM-001 sign-in · IAM-003 scoped role · IAM-004 guardian right ও custody · IAM-005 privileged MFA · IAM-006 session, device ও recovery · IAM-007 সম্পর্ক শেষে access শেষ · IAM-010 সময়সীমাসহ delegation · IAM-013 এক account-এ একাধিক role বা school · IAM-014 published permission matrix |
| **MVP partial (১টি)** | IAM-002। Pilot-এ একক invite ও activation। Bulk onboarding বাকি R1-এ। |
| **R1 backlog** | IAM-012 mobile biometric unlock |
| **R2 backlog** | IAM-008 Google/Microsoft school sign-in · IAM-011 class-card sign-in · IAM-015 school-defined role |
| **R5 backlog** | IAM-009 SAML federation |

## ৩. Audit — কী দেখা গেছে, কী দেখা যায়নি

### ৩.১ যা দেখা গেছে (observed)

| # | Finding | উৎস |
|---|---|---|
| O-01 | এই checkout-এ root `README.md` ও `docs/project-planning/` ছাড়া কিছু নেই। Code, screen, route, component বা test নেই। | Working tree, commit `2f0b1e9` |
| O-02 | Doc 04-এ ১৮টি role-এর allowed scope, মূল কাজ ও আটকে থাকা কাজ লেখা আছে। পরের release-এর role আলাদা অনুচ্ছেদে। | [Doc 04](../../04-roles-and-permissions.md) |
| O-03 | Doc 05-এ ৯টি flow আছে। Invite, sign-in, school/role switch, MFA, delegation, revocation ও recovery-র আলাদা flow সেখানে নেই। এই plan সেগুলো প্রথমবার লিখছে। | [Doc 05](../../05-workflows-and-ui-ux.md) |
| O-04 | Doc 05-এর UI state-এ "Permission বা subscription state" আছে: allowed action, safe message, valid navigation; অন্য school, child বা product-এর details প্রকাশ পাবে না। | Doc 05, UI states |
| O-05 | Doc 04-এর access ক্রম ছয় ধাপের, Blueprint পৃ. ৩২-এ আট ধাপ। DEPENDENCIES শুধু দুটোকে জোড়া দিয়েছে: একই server decision-এ দুটোই থাকবে। §২.১-এর সুনির্দিষ্ট ক্রম আমাদের প্রস্তাব। SchoolContext তৈরির আগে বৈধ session লাগে। | [DEPENDENCIES](../../integration/DEPENDENCIES.md) |
| O-06 | Custody restriction-এর maker Registrar, checker "Second approver"। কোন role হবেন তা কোথাও নির্দিষ্ট নেই। | Doc 04, Blueprint পৃ. ৩১ → OD-03 |
| O-07 | "Privileged role" কারা, তার তালিকা কোথাও নেই। শুধু বলা আছে Super Admin ও privileged school role-এ second factor লাগে। | Requirement 3.2.2, Blueprint পৃ. ৩২ → OD-05 |
| O-08 | Second factor-এর পদ্ধতি নির্দিষ্ট নয়। D-05-এর identity comparison Foundation F2-তে হবে। | [Doc 07](../../07-qa-pilot-and-acceptance.md), G0 → OD-06 |
| O-09 | Guardian right-এর আলাদা তালিকা নেই। Doc 04-এর Guardian row-তে ক্ষেত্রগুলো আছে: attendance, notice, published result, fees, receipts, consent ও messages। | Doc 04 → AS-04 |
| O-10 | X-06 বলছে custody restriction pickup list-এও সঙ্গে সঙ্গে চলবে। অথচ pickup contact SIS-002-এর বাকি R1 অংশ এবং ATT-010 R2। | Doc 07, tracker → E-04 |
| O-11 | প্রতি সন্তান ও channel-এর notification preference COM-006 (C4)-এর feature। E2E-14-এ right ও preference দুটোই আছে। Right দেয় C3, preference রাখে C4। | Tracker, requirement E2E-14 |
| O-12 | দুটি app আছে। একই ব্যক্তি guardian ও staff হলে এক account দিয়ে দুটো app ব্যবহার করেন। Administration app-এর ভেতরে কাজ role-ভিত্তিক workspace-এ ভাগ করা। School-এর নাম ও রং sign-in-এর পরে আসে। | Blueprint পৃ. ৪–৫, Doc 05 |
| O-13 | Phone বা email নেই এমন pupil আছে বলে built-in identity module বাছা হয়েছে (পৃ. ১০–১১)। Pupil account upper primary থেকে; ছোটরা guardian ও class card দিয়ে (D-16, পৃ. ৬৪)। Sign-in যাচাই করা phone বা email দিয়ে, one-time code বা password-এ (IAM-001, পৃ. ৩২ ও ৯৮)। Class card (IAM-011) R2 (পৃ. ৯৮)। নিজের phone বা email ছাড়া upper-primary pupil-এর জন্য আলাদা কোনো পথ PDF-এ নেই। | Blueprint PDF পৃ. ১০–১১, ৩২, ৬৪, ৯৮ → OD-10 |
| O-14 | `docs/project-planning/system-plans/` folder আগে ছিল না। এই plan লেখার মাঝে System 1 ও System 3-এর draft plan এসেছে। তাদের সঙ্গে contract-এর তুলনা implementation plan-এর §৫.৫-এ। | Working tree |

### ৩.২ যে প্রমাণ pending

| # | কী পাওয়া যায়নি | কখন মিলবে |
|---|---|---|
| E-01 | Existing sign-in, role বা guardian screen ও তাদের বর্তমান আচরণ | Backend, web ও mobile checkout পেলে |
| E-02 | Existing design system, component library বা token | DVL-003 শুরু হলে |
| E-03 | Platform C1-এর চূড়ান্ত `SchoolContext` ও entitlement contract। Draft আছে, merge হয়নি। | System 1-এর merge |
| E-04 | Pickup list-এ custody restriction-এর প্রয়োগ | Students ও Academics owner-এর সঙ্গে |
| E-05 | Pilot staff ও পরিবারের সঙ্গে prototype test | DVL-003, pilot school access |
| E-06 | Amharic copy-র native review | Client language lead (Doc 06 client input) |

## ৪. অনুমান ও খোলা সিদ্ধান্ত

### ৪.১ অনুমান

| # | অনুমান | ভিত্তি | ভুল হলে কী বদলাবে |
|---|---|---|---|
| AS-01 | দুই demo school-এর অন্তত একটিতে দুটি campus থাকবে। Pilot school single campus (**উৎস** D-01)। | Proposed fixture | Campus scope-এর deny প্রমাণ করা যাবে না |
| AS-02 | Registrar-এর scope grant অনুযায়ী পুরো school বা নির্দিষ্ট campus হতে পারে (**উৎস** Doc 04 "assigned school or campus")। উদাহরণে campus-ভিত্তিক grant ধরা হয়েছে, এই বাছাই Proposed। | **উৎস** (Doc 04); fixture বাছাই Proposed | Scope picker ও matrix-এর Registrar row |
| AS-03 | এক session-এ ওই membership-এর সব role একসঙ্গে কার্যকর। Role বদল মানে workspace বদল, নতুন session নয়। School বদল মানে নতুন session (**উৎস**)। | অনুমোদনসাপেক্ষ (Blueprint পৃ. ৩২ check ৩: "one of the person's roles") | Context bar ও switch flow |
| AS-04 | প্রতি সন্তানের right-এর তালিকা: attendance, notice, published result, fees ও receipt, consent, message। নামগুলো Proposed। নিয়ম: সাধারণ school notice-এ notice right লাগে। Area notice (attendance, result, fee), তার detail ও attachment-এ ওই area-র right লাগে। Channel preference (COM-006, C4) delivery কমাতে পারে, right বাড়াতে পারে না। | অনুমোদনসাপেক্ষ | Rights checklist ও matrix |
| AS-05 | Import করেন কেবল Registrar। School Admin role দেন, import করেন না। | অনুমোদনসাপেক্ষ | Matrix-এর import row |
| AS-06 | Design system-এর owner DVL-003। এই plan identity-র জন্য দরকারি token ও component প্রস্তাব করে। | অনুমোদনসাপেক্ষ | §১০–১১-এর মান |
| AS-07 | Household home ও child switcher screen-এর owner C5 (APP-001)। কোন সন্তান ও কোন area দেখানো যাবে এবং denied state কেমন হবে, তা C3 ঠিক করে। A-06 হলো C3-এর screen, A8-এর household screen থেকে খোলে (navigation, embedding নয়)। A8 সেখানে link status শুধু পড়ার জন্য দেখায়। | অনুমোদনসাপেক্ষ | §৮.২ ও G-01, A-06 |
| AS-08 | Approval queue-এর shell C5 (APP-010) ও engine C9 (PLT-007)। C3 ঠিক করে custody ও privileged role-এর item কেমন দেখাবে। | অনুমোদনসাপেক্ষ | A-08 screen, OD-14 |
| AS-09 | Product-এর ভাষা English ও Amharic (D-12)। এই doc-এর screen copy English-এ, ব্যাখ্যা বাংলায়। | **উৎস** (D-12); copy Proposed | Copy table |
| AS-10 | Sign-in-এর আগের screen-এ Desk School-এর নিরপেক্ষ theme থাকবে। School-এর branding sign-in-এর পরে (**উৎস**)। | **উৎস** (branding sign-in-এর পরে); sign-in-এর আগের নিরপেক্ষ theme Proposed | Token ও S-screen |

### ৪.২ সিদ্ধান্ত (২৬ নভেম্বর ২০২৬ পর্যন্ত খোলা ছিল)

২৬ নভেম্বর ২০২৬-এ আপনি সব সুপারিশ মেনেছেন। নিচে প্রতিটির সিদ্ধান্ত ও অবস্থা।

| # | প্রশ্ন | সিদ্ধান্ত | অবস্থা | যে কাজে প্রভাব |
|---|---|---|---|---|
| OD-01 | Pilot-এ guardian-এর পরিচয় মেলাতে কী প্রমাণ লাগবে? না মিললে কে review করবেন? | দুটো শর্তই লাগে: (১) school-এর household record-এর phone-এ OTP দিয়ে guardian নিজের account যাচাই করেন; (২) Registrar সামনাসামনি সরকারি ID বা school-এর জানা document দেখেন। শুধু প্রমাণের ধরন, কে ও কবে দেখেছেন লেখা থাকে, ID-র নম্বর বা ছবি নয়। কোনোটি না মিললে link pending, Registrar review করেন। Pilot-এ শুধু online-এ activation নেই। (Blueprint-এর feature-এর মধ্যে owner-এর design choice: IAM-001/004, পৃ. ৩২ ও ৯৮।) | গৃহীত | DR-06, F-06 |
| OD-02 | নতুন child link-এ কোনো default right থাকবে? | কোনো right নিজে থেকে চালু হয় না। Registrar প্রতিটি সন্তানের জন্য আলাদা করে বেছে দেন। A-06-এ কোনো box আগে থেকে বাছা থাকে না। | গৃহীত | DR-06, F-06 |
| OD-03 | Custody restriction-এর দ্বিতীয় approver কোন role? কোন scope-এ? | Principal, যাঁর scope সন্তানের campus ঢাকে। Principal নিজে অনুরোধকারী বা অনুপস্থিত হলে School Owner/Director। Requester নিজে কখনো নন। R2-তে Safeguarding lead role এলে তাঁকেও approver করা যাবে। (Blueprint-এর feature-এর মধ্যে owner-এর design choice: maker-checker ও custody approval, পৃ. ৩১ ও ৩৪।) | গৃহীত | DR-07, matrix row |
| OD-04 | Custody request approval-এর অপেক্ষায় থাকলে কী হবে? | Request pending থাকে। আলাদা approver অনুমোদন বা প্রত্যাখ্যান করেন। অনুমোদিত restriction কার্যকর commit-এ একসঙ্গে সব প্রাসঙ্গিক screen, notice ও list-এ কার্যকর হয় (Blueprint পৃ. ৩১, ৫৭, ৫৯)। | গৃহীত · ২৬ নভেম্বরের scope review | DR-07, F-07 |
| OD-05 | কোন কোন role "privileged"? | Super Admin, Platform Support, School Owner, Principal, School Admin, Finance Officer, Registrar ও Learning Admin। শিক্ষক, Cashier ও Exam Officer privileged নন। ফল: IAM-005 চালু না হওয়া পর্যন্ত এই role আসল user-কে দেওয়া যায় না, তাই IAM-005 invite-এর আগে আসে। (Blueprint-এর feature-এর মধ্যে owner-এর design choice: IAM-005 ও privileged sign-in, পৃ. ৩২ ও ৯৮।) | গৃহীত | DR-15, matrix-এর 2F চিহ্ন |
| OD-06 | Second factor কী হবে? হারালে কে reset করবেন? | Authenticator app (TOTP) ও একবারের backup code; SMS second factor নয়। Reset করেন অন্য একজন admin, সামনাসামনি পরিচয় দেখে। Library-র সমর্থন F2-এর identity comparison (D-05)-এ যাচাই হবে। (Blueprint-এর feature-এর মধ্যে owner-এর design choice: IAM-005/006, পৃ. ৩২ ও ৯৮।) | গৃহীত · F2-তে যাচাই | DR-15, F-04 |
| OD-07 | Recycled phone number কীভাবে আটকাবে? | নতুন device-এ শুধু OTP দিয়ে sign-in বা recovery হলে সন্তানের content ও staff role আটকে থাকে, যতক্ষণ না দ্বিতীয় প্রমাণ আসে: চেনা device থেকে অনুমোদন, password বা school office। সময় পেরোলেও নিজে থেকে খোলে না। Password + OTP একসঙ্গে হলে আটকায় না, new-device notice যায়। (Blueprint-এর feature-এর মধ্যে owner-এর design choice: IAM-001/006 ও V03, পৃ. ৩২ ও ৫৬।) | গৃহীত | DR-14, F-10 |
| OD-08 | সম্পর্ক শেষ হলে guardian বা staff কি পুরোনো document দেখতে পাবেন? | সম্পর্ক শেষ হলে পুরোনো document-এও access নেই। দরকারে school নতুন করে দেয়। | গৃহীত | DR-09 |
| OD-09 | Delegation কে তৈরি করবেন? | School Admin delegation দেন। Role-এর মালিক অনুরোধ করতে পারেন। Privileged delegation-এ Owner-এর approval। (Blueprint-এর feature-এর মধ্যে owner-এর design choice: IAM-010 ও scoped role, পৃ. ৩৪ ও ৯৮।) | গৃহীত | DR-17, F-08 |
| OD-10 | Phone বা email ছাড়া pupil pilot-এ কীভাবে sign in করবে? | Upper-primary pupil IAM-001 অনুযায়ী sign in করে: নিজের যাচাই করা phone বা email (school-এর দেওয়া email-সহ), one-time code বা password (PDF পৃ. ৩২, ৯৮; D-16, পৃ. ৬৪)। কোনো activation code নেই; PR-05 বাদ, কারণ তা Blueprint PDF-এর বাইরে। **ফাঁক:** নিজের phone বা email ছাড়া upper-primary pupil pilot-এ নিজে sign in করতে পারে না; তার কাছে পৌঁছানো হয় guardian-এর মাধ্যমে। এমন pupil Desk Learn-এর quiz (V10)-এ নিজে অংশ নিতে পারে না, তাই Desk Learn ও QA owner-কে জানাতে হবে। ছোটদের class card (IAM-011) R2-তে। | গৃহীত (owner, ২৬ নভেম্বর ২০২৬, CTO-র মাধ্যমে; আগের PR-05 সিদ্ধান্ত বাতিল) | F-02, DR-12 |
| OD-11 | Smartphone ছাড়া guardian কি app account ছাড়াই notice-এর right পাবেন? | SMS-only guardian আলাদা "SMS recipient", app account-এর link নয়। Registrar সামনাসামনি phone যাচাই (OTP) করে যুক্ত করেন। School-এর বাছা notice-ধরন শুধু নিরপেক্ষ লেখায় যায়। Number বদলের খবর, অন্য account-এ একই number যাচাই বা SMS bounce হলে delivery আটকে থাকে। | গৃহীত | DR-06, C4 contract |
| OD-12 | Session, OTP ও idle timeout-এর সময়সীমা কত? | শুরুর মান: OTP ৫ মিনিট বৈধ, সর্বোচ্চ ৫ বার চেষ্টা, ৬০ সেকেন্ড পরে resend; staff web-এ ১৫ মিনিট idle-এ sign-out; guardian চেনা device-এ signed in থাকেন, sensitive কাজে step-up। সব মান config থেকে; QLT-003-এ চূড়ান্ত। (Blueprint-এর feature-এর মধ্যে owner-এর design choice: IAM-001/006 ও device/session policy, পৃ. ৩২ ও ৯৮।) | গৃহীত (শুরুর মান) | DR-12, DR-14 |
| OD-13 | অন্য module-এর action matrix-এ কে লিখবে? | C3 matrix প্রকাশ ও role→action সিদ্ধান্তের owner। Module owner action-এর নাম দেন এবং নিজের module-এ check ৬–৭ প্রয়োগ করেন। | গৃহীত | DR-04 |
| OD-14 | Custody approval-এর record কোথায় থাকবে? | Approval-এর record C9-এর engine-এ (PLT-007)। Approve command C3 দিয়ে যায়; C3 একই transaction-এ সিদ্ধান্ত ও restriction লেখে (কার্যকর commit)। তা সম্ভব না হলে C9-এর সিদ্ধান্তের পরে C3-র commit-ই কার্যকর মুহূর্ত। | গৃহীত · T-00-এ যাচাই | Implementation plan §৫.৫ |
| OD-15 | প্রথম branch-এ session কোথা থেকে আসবে? | প্রথম branch-এ C3 ন্যূনতম session record রাখে; পূর্ণ sign-in পরের deliverable-এ। | গৃহীত | Implementation plan |
| OD-16 | Code কোন repo ও কোন integration base-এ হবে? | Backend checkout যাচাইয়ের পরে owner shared integration branch তৈরি ও নিশ্চিত করবেন। প্রতিটি system-এর আলাদা feature branch ওই base থেকে আসবে। | নিয়ম গৃহীত · repo ও branch pending | Coding শুরু |
| OD-17 | Custody restriction থাকা guardian কি school-wide emergency notice পাবেন, এবং তাতে কী থাকতে পারবে? | Emergency notice সন্তান-নিরপেক্ষ; restricted সন্তানের নাম বা তথ্য কখনো থাকে না। ওই school-এ যাঁর একটিও সক্রিয় right নেই, তিনি পান না; দরকারে school সরাসরি যোগাযোগ করে। (Blueprint-এর feature-এর মধ্যে owner-এর design choice: guardian scope ও safe notice, পৃ. ৩৩–৩৪।) | গৃহীত | C4-এর সঙ্গে contract |
| OD-18 | Guardian-এর sensitive সন্তান-content (result, attendance detail, fees, document) কি offline বা app resume-এ cache থেকে দেখানো যাবে? | Guardian-এর sensitive সন্তান-content (result, attendance detail, fees, document) শুধু বর্তমান online check-এর পরে দেখানো হয়। Offline-এ শেষ হালনাগাদের সময় ও "connect to see"। সন্তান-নির্দিষ্ট নয় এমন সাধারণ তথ্য (যেমন school calendar) offline-এও দেখা যায়। | গৃহীত | DR-09, G17, IAM-007 |
| OD-19 | Custody restriction-এর পরে notice-এর নিশ্চয়তা কতটুকু? | কার্যকর commit-এর পরে কোনো dispatch শুরু হয় না। তার আগে provider-এর কাছে চলে যাওয়া push বা SMS ফেরানো যায় না; staff-এর লেখায় তা বলা থাকে। আগে চলে যাওয়াগুলো গোনা ও log হয়। (Blueprint-এর feature-এর মধ্যে owner-এর design choice: custody ও notices, পৃ. ৩১, ৫৬–৫৭ ও ৫৯।) | গৃহীত | DR-07, A-07, F-07 |

## ৫. Design requirements

| # | Requirement | Feature |
|---|---|---|
| DR-01 | সিদ্ধান্ত server-এর। Screen শুধু ফল দেখায়। সব পথে (screen, API, file, report, export, notice) একই সিদ্ধান্ত। | IAM-003, IAM-014 |
| DR-02 | Role grant-এ role, product, scope ও মেয়াদ থাকে। Scope সবসময় chip হিসেবে দেখা যায়। | IAM-003 |
| DR-03 | Deny নিরাপদ ভাষায়। অন্য school, link ছাড়া সন্তান বা scope-এর বাইরের record "পাওয়া যায়নি" দেখায়, অস্তিত্বহীন ID-র মতোই। Guardian-এর সন্তান-নির্দিষ্ট request-এ যেকোনো deny এবং actor-এর scope-এর বাইরের যেকোনো target ঠিক একই not-found result দেয় (status, body, header, redirect)। "No permission" শুধু staff দেখেন, নিজের scope-এর ভেতরের record-এ। | IAM-003, IAM-004 |
| DR-04 | Permission matrix প্রকাশিত, version-সহ, read-only, filter ও export করা যায়। Enforce হওয়া প্রতিটি action-এর row থাকে। | IAM-014 |
| DR-05 | Contact আর access আলাদা ঘরে। Contact-only guardian-এর পাশে স্পষ্ট লেখা থাকে "app access নেই"। Contact save করলে কোনো grant তৈরি হয় না। Contact-only note (A8) ও link status (C3) আলাদা component। | IAM-004 |
| DR-06 | Right প্রতি সন্তানে আলাদা। Link-এর অবস্থা চারটি: pending, active, restricted, ended। প্রতিটি পরিবর্তনের version থাকে। সাধারণ school notice-এ notice right লাগে। Area notice (attendance, result, fee), তার detail ও attachment-এ ওই area-র right লাগে। Channel preference (COM-006, C4) delivery কমাতে পারে, right বাড়াতে পারে না। | IAM-004 |
| DR-07 | Custody restriction-এ document ও কারণ বাধ্যতামূলক। Requester নিজে approve করতে পারেন না। কার্যকর commit-এ সব screen, notice ও list-এ কার্যকর হয়। তার আগে screen কখনো "effective" বলে না। "All access" ও "selected rights"-এ দেখানো হয় কী আটকাবে, কী থাকবে এবং অন্য guardian কী পাবেন। Staff-এর লেখায় বলা থাকে: device-এ পৌঁছে যাওয়া বা SMS-এ পাঠানো content ফেরানো যায় না (OD-19)। Restriction চালু থাকলে সাধারণ "Change rights" দিয়ে তা খোলা যায় না। | IAM-004 |
| DR-08 | Child switcher শুধু অধিকার আছে এমন সন্তান দেখায়। সন্তান বদলালে আগের সন্তানের data screen থেকে সরে যায়। Response-এর সন্তান আর সক্রিয় সন্তান না মিললে কিছু দেখানো হয় না। Switcher-এর তালিকা, count, ছবি ও screen-reader label আসে কেবল অন্তত একটি সক্রিয় right আছে এমন সন্তান থেকে। Pending link দেখানো হয় না। প্রতিটি switch নতুন selection identity পায়। Session, school, সন্তান, selection ও বর্তমান right সব মিললেই result দেখানো হয়। | IAM-004 |
| DR-09 | সম্পর্ক শেষ হলে পরের request থেকেই access শেষ। Session বন্ধ হয়, queue-র notice আবার check হয়, device-এর cache অকেজো হয়। History মোছা হয় না। Access ending-এর কার্যকর মুহূর্তে খোলা online screen-ও access সরায় (Blueprint V02, X-06)। Offline বা resume OD-18 মেনে চলে। | IAM-007 |
| DR-10 | লম্বা flow-র প্রতিটি ধাপে আবার check। Preview-তে অনুমতি থাকলেও confirm-এ আবার দেখা হয়। | IAM-003, IAM-007 |
| DR-11 | Sensitive পরিবর্তনের পাশে দেখা যায় কে, কখন, কেন করেছেন এবং audit reference। | IAM-014 |
| DR-12 | Sign-in phone বা email দিয়ে, OTP বা নিজের সেট করা password-এ। স্থায়ী password কখনো পাঠানো হয় না। ভুলের message থেকে account আছে কি না বোঝা যায় না। Shared device-এ account বদলালে আগের account-এর notification ও cache সরে যায়। | IAM-001 |
| DR-13 | Invite link একবার ব্যবহারযোগ্য ও মেয়াদি। আগের account থাকলে ব্যক্তি ওই account-এ sign in করে invite গ্রহণ করেন। তারপর inviter account-এর নাম দেখে নিশ্চিত করেন যে একই ব্যক্তি। তার আগে membership সক্রিয় হয় না। Inviter অন্য কোনো school-এর কিছু দেখেন না। | IAM-002 pilot |
| DR-14 | নিজের session ও device দেখা ও শেষ করা যায়। Admin একজন user-এর সব session শেষ করতে পারেন; School Admin-এর "end all sessions" শুধু ওই school-এর membership-এর session শেষ করে। নতুন device-এ sign-in হলে জানানো হয়। | IAM-006 |
| DR-15 | Privileged role-এ second factor ছাড়া workspace খোলে না। Sensitive কাজের আগে আবার second factor চাওয়া হয়। Privileged কাজ go-live gate-এর (§১) আগে আসল user-এর জন্য বন্ধ। | IAM-005 |
| DR-16 | সক্রিয় school ও role প্রতিটি screen-এ দেখা যায়। Phone-এ campus ও workspace এক tap দূরে। School বদলালে নতুন session হয় এবং আগের school-এর data সরে যায়। | IAM-013 |
| DR-17 | Delegation-এর শুরু ও শেষ সময় থাকে। Delegator-এর চেয়ে বেশি access দেওয়া যায় না। Delegate একটি স্থায়ী banner দেখেন। | IAM-010 |
| DR-18 | Bulk onboarding-এ preview, validation, ধাপে ধাপে পাঠানো, অগ্রগতি ও ব্যর্থ row-র তালিকা থাকে। | IAM-002, বাকি R1 |
| DR-19 | Phone বা email OTP শুধু দখল প্রমাণ করে, পরিচয় নয়। Match অস্পষ্ট হলে membership যোগ, recovery, link activation ও সন্তান-নির্দিষ্ট delivery আটকে থাকে। | IAM-001, IAM-002, IAM-004, IAM-006 |

## ৬. Information architecture

### ৬.১ কোন surface-এ কী

| Surface | কারা ব্যবহার করেন | Identity-র অংশ |
|---|---|---|
| Sign-in-এর আগের screen (দুই app ও web) | সবাই | ভাষা বাছাই, sign-in, OTP, password, invite activation, second factor, recovery |
| Student and Guardian app | Guardian, Student | School বাছাই, household home ও child switcher-এর অবস্থা, Account ও security |
| Administration app, web | School Admin, Registrar, Principal, Owner ও অন্য office role | Context bar, Users ও access, role grant, invite, guardian access panel, custody request, approval item, permission matrix, delegation |
| Administration app, phone | Teacher ও চলতি পথের approver | Context bar, workspace switch, approval item, Account ও security |
| Operator console | Super Admin, Platform Support | প্রথম Owner বা School Admin-এর invite। Screen-এর owner C2। |

### ৬.২ Navigation

```text
Sign-in-এর আগে
  S-01 Language ও welcome
  S-02 Sign in (phone বা email)
  S-03 One-time code
  S-04 Password / Set password
  S-05 Invite activation
  S-06 Second factor (enrol ও challenge)
  S-07 Choose school (একাধিক membership থাকলে)
  S-08 Recovery
  S-09 Session ended / Access changed

Student and Guardian app
  G-01 Household home + child switcher        (owner C5, অবস্থা C3)
  G-02 Unavailable state                      (shared component)
  G-03 Account ও security: contact, sessions ও devices, language, switch school

Administration app
  A-01 Context bar: school · campus · workspace · role · delegation banner  (P0-তে ন্যূনতম: school ও role। পূর্ণ switch IAM-013-এ)
  A-02 Users ও access (তালিকা)
  A-03 User detail: roles ও scope, sessions, history
  A-04 Grant / change role
  A-05 Invite person                          (pilot: একজন করে)
  A-06 Guardian access panel                  (A8-এর household screen থেকে খোলে; A8 সেখানে link status শুধু দেখায়)
  A-07 Custody restriction request
  A-08 Approval item: custody ও privileged role
  A-09 Permission matrix
  A-10 Delegation                             (P2)
  A-11 Bulk onboarding                        (বাকি R1)
```

Menu-তে কেবল সেই জিনিস আসে যা product entitlement ও role দুটোই অনুমতি দেয়। Menu-তে না থাকা কোনো ঠিকানা সরাসরি খুললে একই server সিদ্ধান্ত প্রযোজ্য।

## ৭. Flows

### F-01 · Invite ও activation — IAM-002 pilot

1. School Admin A-05 খুলে নাম, phone বা email, role, product ও scope দেন। Guardian-এর invite আসে Registrar-এর A-06 থেকে।
2. Privileged role হলে invite পাঠানোর আগে Owner-এর approval ও inviter-এর second factor লাগে (OD-05)। IAM-005 চালু না হওয়া পর্যন্ত privileged invite আসল user-এর জন্য বন্ধ (go-live gate, §১)।
3. ব্যক্তি একটি link পান। Link একবার ব্যবহারযোগ্য ও মেয়াদি। Message-এ কোনো password থাকে না।
4. S-05-এ তিনি phone বা email OTP দিয়ে যাচাই করেন। চাইলে নিজের password সেট করেন।
5. ওই phone বা email-এ আগে থেকে account থাকলে নতুন account হয় না। ব্যক্তি ওই account-এ sign in করে invite গ্রহণ করেন। Inviter account-এর নাম দেখে নিশ্চিত করেন যে একই ব্যক্তি। তার আগে membership সক্রিয় নয় (DR-13, IAM-013)। Inviter-কে conflict-এর message সাধারণ ভাষায় আসে, অন্য কোনো school প্রকাশ পায় না।
6. Privileged role হলে এখানেই second factor enrol করতে হয়।
7. Invite-এর অবস্থা A-02-তে দেখা যায়: Sent, Activated, Expired, Cancelled।

মেয়াদ শেষ বা আগে ব্যবহৃত link-এ access মেলে না। Screen নতুন invite চাওয়ার পথ দেখায়, school office-এর মাধ্যমে।

### F-02 · Sign-in — IAM-001

1. S-01-এ ভাষা (English বা Amharic)। S-02-এ phone বা email।
2. S-03-এ OTP, অথবা S-04-এ নিজের password।
3. ভুল identifier ও ভুল code-এ একই message আসে। বারবার ভুলে অপেক্ষার সময় দেখানো হয়।
4. Privileged role থাকলে S-06।
5. একাধিক membership থাকলে S-07-এ school বাছাই। একটিই থাকলে সরাসরি ভেতরে।
6. নতুন device হলে ব্যক্তির আগের যাচাই করা channel-এ খবর যায়।

Upper-primary pupil একই পথে sign in করে, নিজের যাচাই করা phone বা email দিয়ে (IAM-001; D-16, PDF পৃ. ৬৪)। যার নিজের phone বা email নেই, সে pilot-এ নিজে sign in করতে পারে না; তার কাছে পৌঁছানো হয় guardian-এর মাধ্যমে (OD-10)।

### F-03 · School ও workspace বদল — IAM-013

- **School বদল।** Context bar বা G-03 থেকে school বাছলে নিশ্চিতকরণ চাওয়া হয়। তারপর নতুন session হয় এবং ওই device-এর পুরোনো session শেষ হয় (উৎস: Blueprint পৃ. ২৫; System 1-এর সঙ্গে মেলানো বাকি, implementation plan CX-10)। আগের school-এর data screen ও cache থেকে সরে যায়। একই browser-এর অন্য tab পরের request-এ S-09 দেখায়। অসমাপ্ত form থাকলে আগে সতর্ক করা হয়।
- **Workspace বদল।** Administration app-এ একই school-এর ভেতরে Registrar, teaching বা Desk Learn workspace বদলালে session একই থাকে। শুধু menu ও কাজ বদলায় (AS-03)।
- **Guardian ও staff একই ব্যক্তি।** তিনি দুটি app-ই একই account দিয়ে ব্যবহার করেন। Guardian app-এ staff-এর কোনো কাজ নেই।

### F-04 · Privileged MFA — IAM-005

1. **Enrol।** Privileged role পাওয়ার পরে প্রথম sign-in-এ S-06। পদ্ধতি Authenticator app (TOTP) ও একবারের backup code; SMS second factor নেই (OD-06)। Enrol শেষ না হলে privileged workspace খোলে না।
2. **Challenge।** প্রতিটি নতুন session-এ, privileged workspace-এ ঢোকার সময়।
3. **Step-up।** Role grant বা revoke, custody approve ও একজনের সব session শেষ করার আগে আবার চাওয়া হয়।
4. **Reset।** নিজে একা করা যায় না। অন্য একজন admin পরিচয় নিশ্চিত করে reset করেন। ঘটনাটি audit-এ থাকে (OD-06)।

### F-05 · Role দেওয়া, বদলানো ও তুলে নেওয়া — IAM-003, IAM-007

1. School Admin A-03 থেকে A-04 খোলেন। School session থেকে আসে, বদলানো যায় না।
2. Product, role, scope (campus, grade, section, subject যেটি ওই role-এ প্রযোজ্য), মেয়াদ ও কারণ দেন।
3. Form-এর নিচে matrix থেকে দেখানো হয় এই role কী পারবে ও কী পারবে না।
4. Privileged role হলে অনুরোধ Owner-এর approval-এ যায়। School Admin নিজে approve করতে পারেন না। Owner approve করলেও IAM-005 চালু না হওয়া পর্যন্ত privileged grant কার্যকর হয় না (go-live gate, §১)। সাধারণ scoped role-এ এই gate নেই।
5. Grant বা revoke পরের request থেকেই কার্যকর। Revoke-এর আগে প্রভাব দেখানো হয়: কোন workspace বন্ধ হবে, কোন session শেষ হবে।

### F-06 · Guardian link ও প্রতি সন্তানের right — IAM-004

1. **Contact save (zero grant)।** Registrar A8-এর household screen-এ contact লেখেন। পাশে লেখা থাকে "Contact only — no app access"। কোনো grant তৈরি হয় না।
2. **Link request।** Import থেকে বা "Start access link" থেকে। Link pending থাকে, right শূন্য, portal, notice ও file access শূন্য।
3. **পরিচয় যাচাই।** দুটো শর্তই লাগে (OD-01): guardian school-এর household record-এর phone-এ OTP দিয়ে নিজের account যাচাই করেন, আর Registrar সামনাসামনি সরকারি ID বা school-এর জানা document দেখেন। শুধু প্রমাণের ধরন, কে ও কবে দেখেছেন লেখা থাকে, ID-র নম্বর বা ছবি নয়। কোনোটি না মিললে link pending, Registrar review করেন। OTP শুধু দখল প্রমাণ করে (DR-19)।
4. **এই সন্তানের right বাছা।** কোনো box আগে থেকে বাছা নেই। Confirmation-এ সন্তানের নাম লেখা থাকে (OD-02)।
5. **Conflict।** একই phone অন্য account-এ থাকলে review তালিকায় যায়। অনুমানে কোনো account মেলানো হয় না। অন্য account-এর কিছু দেখানো হয় না।
6. **কার্যকর।** পরিবর্তন কার্যকর commit-এ কার্যকর হয়। প্রতিটি পরিবর্তনে কে, কখন, কেন লেখা থাকে।

### F-07 · Custody restriction — IAM-004, V04, X-06

1. **অনুরোধ।** Registrar A-07-এ সন্তান ও guardian বেছে কোন right বন্ধ হবে তা দেন, document ও কারণসহ। Document ও কারণ ছাড়া submit হয় না।
2. **Approver।** অন্য একজন ব্যক্তি, scope যাঁর সন্তানের campus ঢাকে, second factor সহ (OD-03)। Requester নিজের অনুরোধে approve button দেখেন না, সরাসরি চেষ্টা করলেও server আটকায়।
3. **Pending approval (OD-04)।** অন্য approver সিদ্ধান্ত না দেওয়া পর্যন্ত request pending থাকে। প্রত্যাখ্যান বা ফেরত গেলে প্রস্তাবিত restriction কার্যকর হয় না।
4. **A-08-এর অবস্থা।** Approve হলে আগে "Approved — applying", কার্যকর commit-এর পরেই কেবল "Approved — effective"। “Returned” বা “Rejected”-এ effective notice আসে না।
5. **কী বন্ধ হয়।** "All access" হলে সন্তান guardian-এর list থেকে সরে যায়। "Selected rights" হলে শুধু ওই area-গুলো। অন্য guardian-এর access অপরিবর্তিত।
6. **খোলা screen।** অনুমোদিত restriction কার্যকর commit-এ খোলা online screen-এও কার্যকর হয় (Blueprint পৃ. ৩১, V02, X-06)।
7. **Notice।** নতুন dispatch বন্ধ। যা আগেই পাঠানো হয়ে গেছে তা ফেরানো যায় না (OD-19); আগে চলে যাওয়াগুলো গোনা ও log হয়।
8. **তোলা।** একই পথ: document, কারণ ও অন্য approver। সাধারণ rights edit দিয়ে restriction উল্টানো যায় না।

**Go-live gate।** OD-04 ও OD-19 এখন সিদ্ধান্ত হয়েছে। Custody approve আসল user-এর জন্য বন্ধ, যতক্ষণ না IAM-005 চালু হয়, Platform-এর PLT-007 ও PLT-013 পাওয়া যায় এবং branch 1b merge হয়।

Guardian-কে কারণ দেখানো হয় না। তিনি শুধু দেখেন তথ্যটি এখন পাওয়া যাচ্ছে না এবং school office-এর সঙ্গে যোগাযোগের পথ।

### F-08 · Delegation — IAM-010

1. School Admin A-10-এ delegate, কোন role-এর কোন কাজ, কোন class বা scope, শুরু ও শেষ সময় এবং কারণ দেন (OD-09)।
2. Delegator-এর নিজের যা নেই তা দেওয়া যায় না। নিজের অনুরোধ approve করার ক্ষমতা delegation-এ যায় না।
3. Delegate context bar-এর নিচে স্থায়ী banner দেখেন: কার পক্ষে, কোন scope-এ, কখন পর্যন্ত।
4. সময় শেষ হলে access নিজে থেকে বন্ধ। তার আগেও revoke করা যায়। Audit-এ লেখা থাকে কাজটি কার পক্ষে করা।

### F-09 · Revocation ও সম্পর্ক শেষ — IAM-006, IAM-007

| কী ঘটল | কে শুরু করেন | পর্দায় কী হয় |
|---|---|---|
| নিজের একটি device-এর session শেষ | ব্যক্তি নিজে, G-03 বা account menu | ওই device পরের request-এ S-09 দেখায় |
| একজনের সব session শেষ | School Admin, A-03, second factor সহ। শুধু ওই school-এর membership-এর session শেষ হয়। | ওই school-এর সব device-এ S-09। Offline-এ রাখা data আর খোলে না। |
| Role তুলে নেওয়া বা মেয়াদ শেষ | School Admin বা সময় | ওই workspace menu থেকে সরে যায়। অন্য role অক্ষত। |
| Guardian link শেষ | Registrar (MVP-তে student চলে গেলেও Registrar হাতে); R2-তে student চলে গেলে A13-এর `student.withdrawn` event | সন্তান switcher থেকে সরে যায়। Queue-র notice আটকায়। |
| Membership শেষ | School Admin | ওই school আর S-07-এ আসে না। অন্য school-এর membership অক্ষত। |

Revoke-এর আগে একটি নিশ্চিতকরণ dialog প্রভাব দেখায়। Offline-এ জমা থাকা কাজ (যেমন পাঠানো বাকি register) revoked session দিয়ে আর জমা হয় না। Screen জানায় কাজটি পাঠানো যায়নি এবং school office-এ যোগাযোগ করতে বলে।

### F-10 · Recovery — IAM-006

- **Password ভুলে গেলে।** S-08 থেকে যাচাই করা phone বা email-এ OTP, তারপর নতুন password। পুরোনো session বন্ধ হয়।
- **Phone হারালে বা number বদলালে।** নিজে একা করা যায় না। Account-এর অন্য কোনো school-এ সক্রিয় membership না থাকলে school office contact বদলাতে পারে (Guardian-এর ক্ষেত্রে Registrar, staff-এর ক্ষেত্রে School Admin)। থাকলে ব্যক্তি নিজে Account & security থেকে second factor দিয়ে বদলান, অথবা Desk School support সামলায় (PLT-008)। School office-এর message সাধারণ ভাষায়। নতুন number OTP দিয়ে যাচাই হয়। পুরোনো session বন্ধ হয়।
- **নতুন device + শুধু OTP।** Sign-in বা recovery শুধু OTP দিয়ে নতুন device-এ হলে সন্তানের content ও staff role আটকে থাকে, দ্বিতীয় প্রমাণ না আসা পর্যন্ত (OD-07)। সময় পেরোলেও এই sign-in verification step নিজে থেকে শেষ হয় না; দ্বিতীয় প্রমাণ লাগবেই (OD-07)।
- **Recycled number।** নতুন মালিক ওই number দিয়ে আগের account বা সন্তানের তথ্যে পৌঁছাতে পারবেন না। নিয়ম OD-07-এ ঠিক হয়েছে।

### F-11 · Permission matrix — IAM-014

1. School Admin, Owner বা Principal A-09 খোলেন। ওপরে version ও প্রকাশের তারিখ।
2. Product, module ও role দিয়ে filter করেন। প্রতিটি ঘরে একটি চিহ্ন ও লেখা থাকে: allowed, scope-সহ allowed, অন্য approver লাগে, second factor লাগে বা denied।
3. A-04-এর role grant form একই matrix থেকে সারাংশ দেখায়।
4. R1-এ matrix read-only। School-এর নিজস্ব role IAM-015, R2।

## ৮. Decision mapping

### ৮.১ Registrar import

Fixture: School A-তে School Management চালু, campus উত্তর ও দক্ষিণ। School B আলাদা school। RN হলেন উত্তর campus-এর Registrar।

| # | Actor | Action | Scope বা target | Approval | Allow / Deny | Screen state |
|---|---|---|---|---|---|---|
| R1 | RN | Import preview | A · উত্তর | লাগে না | Allow | Preview: accepted, review ও error-এর সংখ্যা। Preview-তে কোনো student, placement বা grant তৈরি হয়নি। |
| R2 | RN | Import confirm | A · উত্তর, একই batch | লাগে না। Confirm-এ আবার check। | Allow | অগ্রগতি, তারপর ফলের সারাংশ, audit reference ও Undo। |
| R3 | RN | Import preview | A · দক্ষিণ | — | Deny, not found | Campus picker-এ শুধু নিজের scope-এর campus। দক্ষিণ campus-এর ID সরাসরি পাঠালে R4/R5-এর মতোই একই not-found। কোনো data নয়। |
| R4 | RN | দক্ষিণের student খোলা বা খোঁজা | A · দক্ষিণের record | — | Deny, not found | Search-এ ফল নেই। সরাসরি link-এ "Record পাওয়া যায়নি"। |
| R5 | RN | School B-র ID দিয়ে যেকোনো কাজ | B | — | Deny, not found | একই "পাওয়া যায়নি"। কোনো ইঙ্গিত নেই। |
| R6 | A-র Learning Admin | Import | A · উত্তর | — | Deny, check ৩ | School Management workspace menu-তে নেই। সরাসরি ঠিকানায় "এই কাজের অনুমতি নেই"। |
| R7 | RN, A-তে শুধু Desk Learn চালু | Import | A | — | Deny, C1 check | Menu-তে workspace নেই। পুরোনো link বা সরাসরি route একই not-found result দেয়। শুধু RN-এর নিজের landing-এ, যদি তাঁর কোনো role খোলা না থাকে, "This isn't available at your school"। |
| R8 | RN, session-এর মেয়াদ শেষ | Import confirm | A · উত্তর | — | Deny, check ১ | S-09। Sign-in-এর পরে batch-এ ফেরার আগে actor, campus ও batch access আবার check হয়। কিছু লেখা হয়নি। |
| R9 | RN, preview-র পরে role তুলে নেওয়া হয়েছে | Import confirm | A · উত্তর | — | Deny, check ৩ | "আপনার access বদলেছে"। Student বা placement তৈরি হয়নি। |
| R10 | RN | নিজের batch undo | A · উত্তরের batch | A8-এর undo নিয়ম | Allow, state অনুমতি দিলে | Undo-র সারাংশ আলাদা করে দেখায়: বাতিল হওয়া pending request ও প্রত্যাহার হওয়া আগে-সক্রিয় grant। Access link প্রত্যাহার হয় নির্দিষ্ট link ধরে, batch ধরে নয় (implementation plan CX-12)। পরে বদলানো বা restricted link অক্ষত থাকে ও Conflict তালিকায় আসে। |
| R11 | RN | Household contact লেখা | A · উত্তরের student | লাগে না | Allow, লেখে A8 | Saved। পাশে "Contact only — no app access"। |
| R12a | RN | Guardian link request | A · উত্তরের student | লাগে না | Allow, শুধু request | "Pending · no app access yet"। Portal, notice ও file access শূন্য। |
| R12b | RN | Identity check | A · উত্তরের student | OD-01, DR-19 | Allow, লেখে C3 | Verified অথবা "Identity not confirmed"। Right তখনো শূন্য। |
| R12c | RN | এই সন্তানের right বাছা | A · উত্তরের student | Identity verified থাকতে হবে | Allow, লেখে C3 | "Rights updated for Child 1" + effective notice। Confirmation-এ সন্তানের নাম। |
| R13 | RN | Custody restriction-এর অনুরোধ | A · উত্তরের সন্তান | Document + অন্য approver (OD-04) | Pending approval | "Approval-এর অপেক্ষায়" badge। |
| R14 | RN | নিজের custody অনুরোধ approve | একই | — | Deny, check ৮ | Approve button নেই। জোর করলে "নিজের অনুরোধ নিজে approve করা যায় না"। |
| R15 | Approver (OD-03) | Custody restriction approve | একই school; scope-এ সন্তানের campus (OD-03) | অন্য ব্যক্তি; document; second factor (IAM-005) | Allow, go-live gate (§১) খোলার পরে | "Approved — applying", কার্যকর commit-এর পরে "Approved — effective"। Audit reference। |
| R16 | RN | নিজের campus-এর student-এর marks, ledger বা health detail | A · উত্তর | — | Deny, check ৩ (C3) ও ৬ (A8) | ওই tab নেই। সরাসরি ঠিকানায় অনুমতি নেই। |
| R17 | RN | Student list export | A · উত্তর, field list A8-এর export policy (SIS-016) | লাগে না | Allow, শুধু অনুমোদিত field | Export dialog দেখায় কোন field যাচ্ছে। Server শুধু সেই field ছাড়ে। Download-এর সময় C9 আবার check করে। Export log হয়। |

### ৮.২ Guardian child switching

Fixture A (§২.৩): Guardian G, School A। সন্তান ১ = published result ও notice right। সন্তান ২ = attendance right। সন্তান ৩ = G শুধু A8-এ emergency contact, link নেই। সন্তান ৪ = একই household, G-র সঙ্গে link নেই। **GB** = সন্তান ২-এর অন্য guardian, attendance right, push ও SMS fallback।

| # | Actor | Action | Scope বা target | Approval বা শর্ত | Allow / Deny | Screen state |
|---|---|---|---|---|---|---|
| G1 | G | App খোলা | নিজের link | অন্তত একটি সক্রিয় link | Allow | Household home-এ card, count, ছবি ও screen-reader label শুধু সন্তান ১ ও সন্তান ২-এর। |
| G2 | G | প্রথম সন্তান → Results | প্রথম | Result right + প্রকাশিত ফল | Allow | ফলের তালিকা। Header-এ প্রথম সন্তানের নাম ও class। |
| G3 | G | প্রথম সন্তান → Attendance | প্রথম | Right নেই | Deny, not found (check ৫) | Attendance tile নেই। Deny ঠিক not-found result (status, body, header)। পুরোনো link বা notification থেকে এলে G-02। |
| G4 | G | দ্বিতীয় সন্তানে switch → Attendance | দ্বিতীয় | Attendance right | Allow | আগের সন্তানের data সরে skeleton, তারপর দ্বিতীয় সন্তানের attendance। |
| G5 | G | দ্বিতীয় সন্তান → Results | দ্বিতীয় | Right নেই | Deny, not found (check ৫) | Results tile নেই। Deny ঠিক not-found result (status, body, header)। পুরোনো link বা notification থেকে এলে G-02। |
| G6 | G | তৃতীয় সন্তানের যেকোনো কিছু | তৃতীয় | শুধু emergency contact | Deny, not found | Switcher-এ নেই। ID দিয়ে চাইলে "পাওয়া যায়নি"। |
| G7 | G | চতুর্থ সন্তান (ভুল sibling), ID বদলে বা পুরোনো link দিয়ে | চতুর্থ | Link নেই | Deny, not found | অস্তিত্বহীন ID-র মতোই "পাওয়া যায়নি"। নাম, class বা ছবি প্রকাশ পায় না। |
| G8 | G | Pending link-এর সন্তান | Link pending | School-এর যাচাই বাকি | Deny, not found | Pending link-এর সন্তান switcher-এ একেবারেই নেই: কোনো সারি, count বা ইঙ্গিত নেই। সরাসরি request করলে not-found। |
| G9 | G | প্রথম সন্তানে custody restriction-এর কার্যকর commit-এর পরে restricted area-র যেকোনো কাজ | প্রথম | Restriction চালু | Restricted area-য় Deny, not found (check ৫) | "All access" হলে সন্তান ১ switcher থেকে সরে যায়। "Selected rights" হলে শুধু ওই area। কার্যকর commit-এ খোলা online screen G-02 দেখায়। নতুন dispatch নেই। অন্য guardian অপরিবর্তিত। কারণ দেখানো হয় না। |
| G10 | G | দ্রুত সন্তান ১ → ২ → ১ switch, আগের data তখনো আসছে | — | — | প্রযোজ্য নয়, UI-র নিয়ম | প্রতিটি switch নতুন selection identity পায়। আগের selection-এর response ফেলে দেওয়া হয়, child ID মিলে গেলেও। Loading-এর সময় নাম, class, ছবি, notice badge বা আগের announcement নেই। |
| G11 | G | প্রথম সন্তান school ছেড়ে গেছে (MVP-তে Registrar link শেষ করার পরে; স্বয়ংক্রিয় R2-তে) | প্রথম | Link ended | Deny, not found | প্রথম সন্তান switcher-এ নেই। পুরোনো document-এর নিয়ম OD-08 (OD-08 সিদ্ধান্ত)। |
| G12 | G, session-এর মেয়াদ শেষ | যেকোনো কাজ | — | — | Deny, check ১ | S-09। Cache-এর সন্তানের data দেখানো হয় না। |
| G13 | G, অন্য school B-তে teacher | School বদল | B | নতুন session | Allow | নিশ্চিতকরণ, তারপর নতুন session। A-র সন্তানদের data screen ও cache-এ নেই। |
| G14 | G | সন্তান ২-এর attendance notice | দ্বিতীয় | Attendance right, পাঠানোর মুহূর্তে | Allow | Lock screen-এ নিরপেক্ষ preview। Detail খোলার সময় attendance right আবার check হয়। |
| G15 | G | সন্তান ১-এর attendance notice | প্রথম | Attendance right নেই | Deny | পাঠানো হয় না। Notice right area notice ঢাকে না। |
| G16 | G | সন্তান ১-এর সাধারণ school notice | প্রথম | Notice right | Allow | পাঠানো হয়, নিরপেক্ষ লেখায়। Attachment খোলার সময় আবার check। |
| G17 | G | Right বদল বা restriction-এর পরে app resume বা offline | প্রথম | — | Deny, OD-18 | বর্তমান online check-এর আগে cache থেকে কোনো sensitive content নয়। Offline-এ "You are offline" ও শেষ হালনাগাদের সময়, কোনো detail নয়। App-switcher preview-তে সন্তানের content লুকানো। |
| G18 | অন্য ব্যক্তি, G-র shared phone-এ | Account বদলের পরে G-র পুরোনো notification খোলা | — | — | Deny | Sign-in বা G-02। G-র কিছুই দেখা যায় না। |

G7-এর নিয়মটি সবচেয়ে গুরুত্বপূর্ণ। একই household-এ থাকা যথেষ্ট নয়। প্রতিটি সন্তানের জন্য আলাদা link ও right লাগে।

**Fixture B (E2E-14)।** Guardian H, তিন সন্তান, আলাদা right ও আলাদা channel preference।

| সন্তান | Right | Preference | প্রত্যাশিত |
|---|---|---|---|
| X | Result + notice | School-এর default channel | Result ও notice আসে। Attendance বা fee notice আসে না। |
| Y | Attendance + notice | Push + SMS | Attendance ও notice Y-র preference অনুযায়ী push, না পৌঁছালে SMS। Result বা fee notice আসে না। |
| Z | Fees ও receipt + notice | In-app only | Fee, receipt ও notice শুধু in-app। Preference কোনো right বাড়ায় না। |

## ৯. Screen layouts

Layout-গুলো Proposed। লেখা English-এ, কারণ product-এর source ভাষা English। Existing screen না থাকায় তুলনা Pending (E-01)।

### S-02 ও S-03 · Sign in ও one-time code

```text
[Desk School]                         Language: English v
Sign in
Phone or email   [______________________________]
                                      [ Continue ]
Need help? Contact your school office.

--- next ---
Enter the code sent to +251 *** **12
[ _ ][ _ ][ _ ][ _ ][ _ ][ _ ]        Resend in 0:45
[ Use password instead ]              [ Verify ]
```

### A-01 · Context bar

```text
[ School A v ] · North campus · [ School Management v ] · Registrar        [ Hana v ]
( Covering Grade 6 Blue for T. Bekele until 28 Nov 16:00 )   <- শুধু delegation চলাকালে
```

### A-04 · Grant role

```text
Grant role
Person     Hana T. · phone verified
School     School A                    (from session, read-only)
Product    (o) School Management   ( ) Desk Learn
Role       [ Registrar v ]
Scope      Campus   [x] North   [ ] South
           Grade / section / subject: not used for this role
Valid      From [ date ]   To [ no end date v ]
Reason     [______________________________]   required

This role can      Import, registration, household, guardian rights, placement
This role cannot   Marks, ledger, health detail
(!) Privileged role (Registrar): needs Owner approval and your second factor.
    Blocked until second factor is live.

                                   [ Cancel ]   [ Grant role ]
```

### A-06 · Guardian access panel

C3-এর screen। A8-এর household screen থেকে খোলে (navigation, embedding নয়)। উপরের Contact block A8-এর data, শুধু পড়া যায়। নিচের App access table C3-এর data।

```text
Student: Child 1 (A-017) · Grade 6 Blue · North campus
Opened from Student Records > Household

Contact (Student Records, read-only)
  Guardian G · Mother · +251...        Emergency contact: yes
  Guardian F · Father · +251...        Emergency contact: yes
  Contact alone gives no app access.

App access (Identity)
| Person     | Child   | Campus | Access                         | Action                       |
|------------|---------|--------|--------------------------------|------------------------------|
| Guardian G | Child 1 | North  | ACTIVE · Notices, Results      | [ Change rights ] [ End access ] |
| Guardian F | Child 1 | North  | No access link                 | [ Start access link ]        |

Guardian G's other linked children in your scope
| Person     | Child   | Campus | Access                         | Action                       |
|------------|---------|--------|--------------------------------|------------------------------|
| Guardian G | Child 2 | North  | ACTIVE · Attendance            | [ Open ]                     |

Children outside your scope are not shown.

[ Request custody restriction ]
```

কোনো right box আগে থেকে বাছা থাকে না (OD-02)। Rights বাছার ধাপে সন্তানের নাম লেখা থাকে।

### A-07 · Custody restriction request

```text
Request custody restriction
Child       Child 1 (A-017)
Guardian    Guardian G
Restrict    (o) All access to this child
            ( ) Selected:  [ ] Results  [ ] Notices  [ ] Attendance  [ ] Pickup ...
Document    [ Upload ]   required · stored as restricted
Reason      [______________________________]   required
Takes effect   When the approval has been applied (shown as "Approved — effective").
               From then: new requests, notices not yet sent
               and file downloads are blocked. Content already on a phone or SMS
               already sent cannot be recalled (OD-19).
Approver    A different person with approval right — not you
Until approved   Request stays pending. Existing access follows current rights.
                 If approved, the restriction applies at the effective commit.
                 No decision in one working day → goes to the Owner.

                              [ Cancel ]   [ Send for approval ]
```

### A-09 · Permission matrix

```text
Permission matrix · version 0.1 (draft) · published —
Product [ School Management v ]  Module [ Student records v ]  Role [ All v ]     [ Export ]

| Action                        | Registrar  | School Admin | Principal | Guardian     |
|-------------------------------|------------|--------------|-----------|--------------|
| Import students               | A · campus | —            | —         | —            |
| Link guardian and set rights  | A · campus | —            | —         | —            |
| Request custody restriction   | A · campus | —            | —         | —            |
| Approve custody restriction   | —          | —            | AP · 2F   | —            |
| View child's attendance       | —          | —            | —         | A · child · R|

A allowed · AP needs a different approver · 2F second factor · — denied
· campus / · child = scope · R = needs the child right
Approver: Principal whose scope covers the child's campus; School Owner if the Principal is the requester or absent (OD-03)
```

Phone-এ grid-এর বদলে আগে role বাছতে হয়। তারপর ওই role-এর কাজগুলো তালিকা হিসেবে আসে।

### G-01 · Household home ও child switcher

```text
[ School A v ]                              [ notices ]
Your children
+----------------+  +----------------+
| Child 1        |  | Child 2        |
| Grade 6 Blue   |  | Grade 3 Red    |
| selected       |  |                |
+----------------+  +----------------+
Child 1 · Grade 6 Blue
[ Notices ]   [ Results ]            <- শুধু যে area-র right আছে
```

Card, count ও label আসে কেবল অন্তত একটি সক্রিয় right আছে এমন সন্তান থেকে (DR-08)।

### G-02 ও S-09 · Unavailable ও session ended

```text
This information is not available right now.
If you think this is a mistake, contact your school office.
[ Back to home ]

--- S-09 ---
Your session has ended.
Sign in again to continue.              [ Sign in ]
```

## ১০. Reusable components

নিচের component-এর নাম ও নিয়ম সব **Proposed**। Design system-এর owner DVL-003 (AS-06), তাই চূড়ান্ত নাম ও রূপ তিনি ঠিক করবেন। Existing component library নেই (E-02)।

| Component | কোথায় (screen ID) | Variant বা state | নিয়ম |
|---|---|---|---|
| ContextBar | A-01, সব Administration screen | Default, delegation চলছে, phone-এ ছোট রূপ | School · campus · workspace · role সবসময় দেখা যায় (DR-16)। Read-only সারাংশ। অনুমতি এখান থেকে আসে না। |
| SchoolSwitcher | S-07, A-01, G-03 | বন্ধ, খোলা, নিশ্চিতকরণ | School বদল মানে নতুন session (F-03)। আগে নিশ্চিতকরণ চায়। বদলালে আগের school-এর data সরে যায়। |
| WorkspaceSwitcher | A-01 | বন্ধ, খোলা | Session একই থাকে। শুধু menu ও কাজ বদলায় (AS-03)। শুধু সেই workspace দেখায় যা entitlement ও role দুটোই অনুমতি দেয়। |
| ChildSwitcher | G-01 | Selected, unselected | শুধু অধিকার আছে এমন সন্তান দেখায়, pending সারি নেই (DR-08, G8)। প্রতিটি switch নতুন selection identity পায়। Session, school, সন্তান, selection ও বর্তমান right সব না মিললে কিছু দেখায় না। |
| ScopeChip | A-02, A-03, A-04, A-09 | Campus, grade, section, subject, school | Scope সবসময় chip হিসেবে দেখা যায় (DR-02)। শুধু পড়া যায়। |
| ScopePicker | A-03, A-04 | School (read-only), campus ইত্যাদি | Granter যে scope দিতে পারেন, শুধু সেগুলো দেখায়। School session থেকে আসে, বদলানো যায় না (F-05)। |
| RoleBadge | A-02, A-03, A-04, A-09 | Normal, privileged (2F চিহ্নসহ) | Role-এর নাম লেখা থাকে। শুধু রং দিয়ে বোঝানো হয় না। |
| RightsChecklist | A-06 | Per child; checked, unchecked, locked (custody restriction) | প্রতি সন্তানে আলাদা। কোনো box আগে থেকে বাছা নয় (OD-02)। Confirmation-এ সন্তানের নাম। "Apply to all children" নীরবে কখনো নয়। |
| LinkStatusBadge | A-06, A-03 (শুধু staff) | Pending, active, restricted, ended | শুধু C3-এর চার অবস্থা। Guardian-এর কোনো screen-এ দেখানো হয় না। Text ও icon দুটোই থাকে, শুধু রং নয় (DR-06)। |
| ContactOnlyNote | A-06 | একটি রূপ | লেখা "No app access"। Data আসে A8 থেকে। কখনো link status-এর মতো সাজানো হয় না (DR-05)। |
| AccessState | G-02, S-09 ও সব screen | Not-found, forbidden, pending-approval, session-ended, product-off, step-up-required | Deny দেখানোর একমাত্র পথ (§১৪)। অন্য school বা সন্তানের record আছে কি না প্রকাশ করে না। Guardian-এর সন্তান-data-র জন্য শুধু not-found রূপ (G-02)। "Forbidden" শুধু staff, নিজের scope-এর ভেতরে। Guardian-কে custody-র কারণ দেখায় না। |
| ApprovalCard | A-08 | Waiting, Approved — applying, Approved — effective, Returned, Rejected | Requester approve action দেখেন না (F-07)। Maker, document, কারণ দেখায়। ফেরত দিতে কারণ লাগে। |
| ReasonField | A-04, A-07, A-08, A-10 | Empty, filled, error | কারণ বাধ্যতামূলক। খালি থাকলে submit হয় না। |
| RestrictedDocumentAttach | A-07, A-08 (দেখার জন্য) | Empty, uploaded, error | বাধ্যতামূলক। File Platform file service-এ restricted হিসেবে থাকে। Identity শুধু reference রাখে। |
| OtpInput | S-03 | Empty, filling, error, rate-limited | Paste ও autofill চলে। Resend ও সময় config থেকে (OD-12)। |
| PasswordField | S-04 | Hidden, shown, error | Show/hide আছে। Password manager চলে। সাধারণ password হলে স্পষ্ট message। |
| SecondFactorPrompt | S-06, step-up modal | Enrol, challenge, step-up | একই component দুই জায়গায়। Authenticator app code বা backup code (OD-06)। |
| SessionDeviceRow | G-03, A-03 | এই device, অন্য device, শেষ করা | Device, শেষ ব্যবহার ও "End session" দেখায়। Admin-এর "সব session শেষ"-এ step-up লাগে। |
| InviteStatusChip | A-02 | Sent, Activated, Expired, Cancelled | Text ও icon। Expired-এ নতুন invite চাওয়ার পথ। |
| DelegationBanner | A-01 | কার পক্ষে, কোন scope, কখন পর্যন্ত | Delegation চলাকালে স্থায়ী। বন্ধ করা যায় না (DR-17)। |
| MatrixTable | A-09 | Web-এ grid, phone-এ role-first list | Read-only। প্রতিটি ঘরে চিহ্ন ও লেখা (DR-04)। দুই রূপে একই শব্দ। |
| AuditRef | A-03, A-06, A-07, A-08, A-09 | Compact, expanded | কে, কখন, কেন এবং audit reference (DR-11)। |
| ImpactConfirmDialog | Revoke, End access, school বদল | খোলা, নিশ্চিত, বাতিল | কী কী বন্ধ হবে তার তালিকা দেখায়। ফোকাস ভেতরে আটকে থাকে, বন্ধ হলে ফিরে যায়। |
| EffectiveNowNotice | A-06, A-08, A-04 | একটি রূপ | লেখা: "Effective now for new requests, notices not yet sent and file downloads. Anything already delivered to a phone or sent by SMS cannot be recalled." শুধু কার্যকর commit-এর পরে দেখায়, Returned-এর সঙ্গে কখনো নয়। |
| SafeMessage | সব screen | Info, error, rate-limit, outcome unknown | Server-এর problem code ব্যবহারকারীর ভাষায় সরল লেখায় বদলায়। Account বা record আছে কি না বলে না (DR-03, DR-12)। |

**নিয়ম**

1. একটি component সব জায়গায় একটি অর্থই বহন করে। একই চিহ্ন দুই অর্থে ব্যবহার হয় না।
2. Deny দেখানোর একমাত্র পথ AccessState। কোনো screen নিজের ভাষায় deny বানায় না।
3. Component কখনো নিজে অনুমতি ঠিক করে না। সে server-এর সিদ্ধান্ত দেখায় (DR-01)।

## ১১. Design tokens

নিচের সব মান **Proposed**। DVL-003 চূড়ান্ত করবে (AS-06)। Existing token নেই (E-02)।

Sign-in-এর আগের screen-এ নিরপেক্ষ Desk School theme থাকে। School-এর নাম ও রং আসে sign-in-এর পরে (AS-10, **উৎস**)।

**নিয়ম**

- Status-এর রং school theme বদলাতে পারে না।
- School-এর brand রং automated check-এ contrast পাস না করলে নিরপেক্ষ action রং-এ ফিরে যায়।
- Status কখনো শুধু রং দিয়ে বোঝানো হয় না। সঙ্গে icon ও লেখা থাকে।

### ১১.১ রং (semantic)

Hex মানগুলো light theme-এর Proposed উদাহরণ। DVL-003 চূড়ান্ত করবে। Contrast automated check দিয়ে যাচাই হবে।

| Token | কাজ | Proposed hex |
|---|---|---|
| `color.surface` | Page-এর পটভূমি | #FFFFFF |
| `color.surface-raised` | Card, panel, modal | #F6F7F9 |
| `color.text` | মূল লেখা | #1B1F24 |
| `color.text-muted` | সহায়ক লেখা, caption | #4A5260 |
| `color.border` | Divider, input-এর সীমা | #767F8C |
| `color.action-primary` | মূল button ও link। Sign-in-এর পরে school theme দিয়ে বদলানো যায়। | #1F5FBF |
| `color.focus` | Keyboard focus ring | #0B3D91 |
| `color.status.allowed` | Allowed, active | #1E7A3C |
| `color.status.denied` | Denied। নিরপেক্ষ ধূসর, কারণ deny কোনো error নয়। | #5F6670 |
| `color.status.pending` | Pending, approval-এর অপেক্ষা | #9A5B00 |
| `color.status.restricted` | Restricted | #7A3E9D |
| `color.status.danger` | Revoke, End access-এর মতো ক্ষতিকর কাজ | #B3261E |
| `color.status.info` | সাধারণ তথ্য | #1F5FBF |

নোট: `color.border`-এর আগের মান #F6F7F9 পটভূমিতে প্রায় 2.9:1 ছিল, যা 3:1-এর নিচে। নতুন মান হিসাব করে প্রায় 3.8:1। Automated check তা যাচাই করবে।

### ১১.২ Typography

| বিষয় | Proposed নিয়ম |
|---|---|
| Font | Latin ও Ethiopic (Amharic) দুই script-ই ঢাকতে হবে। Fallback স্পষ্ট থাকবে। |
| Scale | caption 13 · body 16 · body-strong 16 (bold) · title 20 · heading 24 |
| Phone | Body 16-এর ছোট নয়। |
| Line-height | Latin-এ 1.5 গুণ। Ethiopic-এ বেশি (যেমন 1.7)। |
| Unit | মাপ scalable unit-এ। 200% text scale-এও layout ভাঙবে না। |

### ১১.৩ Spacing, আকার ও motion

| Token | Proposed মান | নিয়ম |
|---|---|---|
| Spacing | 4 · 8 · 12 · 16 · 24 · 32 · 48 | 4-ভিত্তিক scale। Phone-এ পাশে 16 gutter। |
| Radius | 4 · 8 · 16 | Input ও button 8। Card 16। |
| Elevation | 0 · 1 · 2 | Modal-এ 2। ছায়ার ওপর অর্থ নির্ভর করে না। |
| Motion | 100 · 200 ms | ছোট সময়। Reduced-motion বললে animation বন্ধ। Status শুধু motion দিয়ে বোঝানো হয় না। |
| Touch target | Phone 48dp · coarse pointer web 44px | WCAG 2.2-এর 24px সর্বনিম্নের নিচে কখনো নয়। |
| Breakpoint | 600 · 1024 · 1440 | §১২-এর সীমা এখান থেকে। |

### ১১.৪ Identity-র নিজস্ব token

| Token | কাজ | মান |
|---|---|---|
| `size.otp-cell` | S-03-এর প্রতিটি code ঘর | Proposed: touch target-এর কম নয়। DVL-003 চূড়ান্ত করবে। |
| `size.child-card` | G-01-এর child card | Proposed: নাম ও class 200% text-এ কাটে না। DVL-003 চূড়ান্ত করবে। |
| `size.context-bar` | A-01-এর bar-এর উচ্চতা | Proposed: phone-এ ছোট রূপ। DVL-003 চূড়ান্ত করবে। |
| OTP resend সময় | S-03 | শুরুর মান ৬০ সেকেন্ড (OD-12), config থেকে। |
| Idle warning সময় | Administration app | Staff web-এ ১৫ মিনিট idle (OD-12), config থেকে। |

## ১২. Responsive behaviour

Office-এর কাজ web-first। Family ও sign-in-এর flow phone-first। (**উৎস**: Doc 05। Bulk import, billing ও report web-এ, শিক্ষকের routine phone-first।) Breakpoint **Proposed**, DVL-003 চূড়ান্ত করবে।

| Screen group | Phone (<600) | Tablet (600–1024) | Desktop (≥1024) |
|---|---|---|---|
| S-01…S-09 sign-in | এক column। একটি মূল action। কম দামের Android-এ চলতে হবে (PLT-017, V17)। | এক column, মাঝখানে। | এক column, মাঝখানে। |
| G-01…G-03 guardian app | Child card আড়াআড়ি scroll বা উপর-নিচে। Header-এ নির্বাচিত সন্তানের নাম সবসময়। | Card পাশাপাশি। নাম header-এ। | Card পাশাপাশি। নাম header-এ। |
| A-01 context bar | School-এর নাম ও role সবসময় দেখা যায়। Campus ও workspace এক tap-এ একটি sheet-এ (DR-16)। Delegation banner দৃশ্যমান থাকে। | পুরো bar। | পুরো bar। |
| A-02, A-03 users ও access | তালিকা, তারপর detail আলাদা screen-এ। | তালিকা ও detail পাশাপাশি। | Table, পাশে detail। |
| A-04, A-05 form | এক column। "এই role কী পারে ও কী পারে না" সারাংশ submit button-এর ওপরে। | এক column, চওড়া। | Form আর সারাংশ পাশাপাশি। সারাংশ তবু button-এর কাছে। |
| A-06 guardian access panel | উপর-নিচে। Contact আগে, App access পরে। প্রতিটি ঘরের নিজস্ব heading থাকে, যাতে contact-কে access ভাবা না হয় (DR-05)। | দুই column। | দুই column। |
| A-07, A-08 custody ও approval | Phone-এও পুরো কাজ করা যায়, কারণ approver desk-এ না-ও থাকতে পারেন। Document viewer-এ খোলে। | Phone-এর মতো, চওড়া। | Document পাশে। |
| A-09 permission matrix | Role-first list। আগে role, তারপর তার কাজ। | Grid, scroll-সহ। | Grid। প্রথম column ও header আটকে থাকে (sticky)। |
| A-10 delegation | শুধু অবস্থা দেখা। | শুধু অবস্থা দেখা। | পুরো কাজ (P2)। |
| A-11 bulk onboarding | শুধু অবস্থা দেখা। | শুধু অবস্থা দেখা। | Web-only (বাকি R1, pilot নয়)। |

**নিয়ম**

1. কোনো feature এক size-এ পাওয়া যায় আর অন্য size-এ কোনো ব্যাখ্যা ছাড়া নেই, এমন হবে না। না থাকলে লেখা থাকে কেন ও কোথায় পাওয়া যাবে।
2. Deny সব size-এ একই দেখায়। AccessState একই ভাষা ব্যবহার করে।
3. Sign-in-এ network লাগে। Offline-এ screen তা স্পষ্ট বলে।
4. আগের session থাকা অবস্থায় offline হলে app শেষ হালনাগাদের সময় দেখায়। Session revoke হলে offline data আর খোলে না (**উৎস**: Blueprint পৃ. ৩২)। Guardian-এর sensitive সন্তান-content offline-এ দেখানো হয় না (OD-18)।

**যে test যোগ হলো (Proposed)।**

- Web reflow 320 CSS px-এ।
- A-06-এর row ও child card-এ লম্বা Amharic নাম।
- ছোট Android-এ বড় font-এ OTP row।
- ধীর network-এ child switch।
- App resume।

## ১৩. Accessibility

লক্ষ্য: core journey-তে WCAG 2.2 AA (PLT-018, V16)। Screen না থাকায় প্রমাণ **Pending**। নিচের সব নিয়ম **Proposed**।

| বিষয় | নিয়ম | কোন screen-এ বিশেষভাবে |
|---|---|---|
| Accessible authentication (3.3.8) | OTP ও password-এ paste চলে। Password manager ও one-time-code autofill চলে। কোনো puzzle বা মনে রাখার পরীক্ষা নেই। | S-03, S-04, S-06 |
| Redundant entry (3.3.7) | একই flow-তে যে phone বা email আগে দেওয়া হয়েছে, আবার চাওয়া হয় না। | S-02, S-03, S-05, S-08 |
| Target size (2.5.8) | §১১.৩-এর মাপ মানা হয়। 24px-এর নিচে কিছু নয়। | সব screen, বিশেষ করে S-03-এর code ঘর |
| Focus (2.4.7, 2.4.11) | Focus দেখা যায়। Sticky header বা modal তা ঢাকে না। | A-09 (sticky), সব modal |
| Consistent help (3.2.6) | "Contact your school office" সব S- screen, G-02 ও S-09-এ একই জায়গায়। | S-01…S-09, G-02 |
| Timing (2.2.1) | OTP-র মেয়াদ শেষ হলে সবসময় resend-এর পথ থাকে। Idle warning-এ staff sign-out-এর আগে session বাড়াতে পারেন। সময় config থেকে (OD-12)। | S-03, Administration app |
| Status message (4.1.3) | ফল, deny ও "effective now" focus না সরিয়ে ঘোষণা হয়। | A-06, A-07, A-08, AccessState |
| Error identification (3.3.1, 3.3.3) | ভুল কোথায় তা বলে ও ঠিক করার পথ দেয়। তবে message নিরাপদ থাকে: account বা record আছে কি না বলে না (DR-03, DR-12)। | S-02, S-03, S-05 |
| Error prevention (3.3.4) | Revoke, End access, custody অনুরোধ ও approve-এর আগে ImpactConfirmDialog। | A-03, A-06, A-07, A-08 |
| রং একা নয় (1.4.1) | Allowed, denied, pending, restricted-এ icon ও লেখা। | A-06, A-09, G-01 |
| Contrast (1.4.3, 1.4.11) | লেখা ও non-text অংশ automated check-এ যাচাই হয়। School theme-এর রংও। | সব screen |
| Reflow ও text size (1.4.10, 1.4.4) | 200% text-এ ও ছোট width-এ আড়াআড়ি scroll ছাড়া পড়া যায়। | সব screen |
| Permission matrix | আসল table: caption, row ও column header। প্রতিটি ঘরে লেখা আছে, শুধু icon নয়। Phone-এর list-এও একই শব্দ। | A-09 |
| Child switcher | "Child 1 of 2, selected" ঘোষণা হয়। Switch-এর পরে focus সন্তানের heading-এ যায় ও বদল ঘোষণা হয়। তাই screen-reader ব্যবহারকারী নতুন নামের নিচে আগের সন্তানের data শোনেন না (DR-08)। | G-01 |
| Keyboard | প্রতিটি web flow mouse ছাড়া চলে। Modal focus ধরে রাখে ও বন্ধ হলে ফিরিয়ে দেয়। | A-02…A-09, সব modal |
| ভাষা | প্রতিটি screen English বা Amharic ঘোষণা করে। Screen-reader label দুই ভাষায় থাকে। Amharic copy native reviewer দেখবেন (E-06)। | সব screen |
| সরল ভাষা | Safe message ছোট ও সহজ বাক্যে। | সব SafeMessage |

**যাচাই।** V16 review: S-02/S-03, A-04, A-06, A-07, A-09 ও G-01। V17: কম দামের Android-এ G-01।

**ভাষা ও screen reader।** English ও Amharic-এ audio ও visual check: G-02, S-09, A-08-এর অবস্থা (waiting, applying, effective, returned), error, pending ও revoked state। Switch-এর পরে screen reader নতুন সন্তানকে ঘোষণা করে কেবল ওই সন্তানের data তৈরি হলে। নতুন নামের নিচে আগের data কখনো পড়া হয় না।

## ১৪. Loading, empty, error ও success state

নিচের copy ও আচরণ সব **Proposed**।

### ১৪.১ Screen অনুযায়ী

| Screen | Loading | Empty | Error | Success |
|---|---|---|---|---|
| S-02, S-03 | Button-এ progress। ফর্ম ঠিক থাকে। | প্রযোজ্য নয় | ভুল code ও অচেনা identifier-এ একই নিরপেক্ষ message। Rate-limit হলে অপেক্ষার সময় দেখায়। | পরের ধাপ (S-04, S-06, S-07 বা ভেতরে) |
| S-05 | Link যাচাই হচ্ছে, skeleton। | প্রযোজ্য নয় | মেয়াদ শেষ বা আগে ব্যবহৃত link: "This invite can't be used." সঙ্গে school office-এর কাছে নতুন invite চাওয়ার পথ। | Account চালু। প্রয়োজনে S-06। |
| S-06 | Code যাচাই হচ্ছে। | প্রযোজ্য নয় | ভুল code: আবার চেষ্টা। Reset-এর পথ বলে অন্য admin লাগবে (F-04)। | Workspace খোলে। |
| S-07 | Membership তালিকা skeleton। | একটিই membership: screen আসে না, সরাসরি ভেতরে। | School খোলা যায়নি: AccessState (product-off বা forbidden)। | নতুন session। "You are now in School A." |
| A-02 | Skeleton সারি। | "No people yet." সঙ্গে Invite person (অনুমতি থাকলে)। | তালিকা আনা যায়নি: আবার চেষ্টা। | Invite-এর অবস্থা chip-এ হালনাগাদ। |
| A-03 | Skeleton। | "No roles granted yet." | Forbidden বা not-found AccessState। | Session বা role পরিবর্তনের পরে হালনাগাদ। |
| A-04 | Matrix-সারাংশ skeleton। | Product বা role না বাছা পর্যন্ত সারাংশ ফাঁকা, লেখা থাকে কী বাছতে হবে। | কারণ খালি বা scope অনুমতির বাইরে: field-এর পাশে message। যা লেখা হয়েছে হারায় না। | "Role granted." কখন থেকে কার্যকর ও audit reference। Privileged হলে "Sent for approval", এটি সফলতা নয়। |
| A-05 | Button-এ progress। | প্রযোজ্য নয় | Phone বা email ভুল ধরনের। আগের account থাকলে membership যোগ হয়, তাই duplicate error নেই। | Invite "Sent"। A-02-তে দেখা যায়। |
| A-06 | Panel-এর দুই ঘরের skeleton। | "No guardian has app access for this child yet." Contact তালিকা তবু দেখা যায়। | তিন ফল: নিশ্চিত ("Rights updated"); ব্যর্থ ("Nothing was changed"); অজানা ("We could not confirm the change"), তখন screen বর্তমান version পড়ে আসল অবস্থা দেখায়। Conflict হলে (F-06 ধাপ ৫) নিরপেক্ষ বর্ণনা। | "Rights updated." সঙ্গে EffectiveNowNotice ও AuditRef। |
| A-07 | Upload-এ progress। | প্রযোজ্য নয় | Document বা কারণ ছাড়া submit হয় না, field-এ message। Upload ব্যর্থ হলে ফর্ম থাকে। | "Sent for approval" এবং approver-এর role। কার্যকর commit-এর পরেই কেবল EffectiveNowNotice। তার আগে এটি pending, সফল restriction নয়। |
| A-08 | Skeleton card। | “Nothing waiting for your decision.” | নিজের অনুরোধে approve action নেই। Document খোলেনি: আবার চেষ্টা। | “Pending approval” → “Approved — applying” → “Approved — effective” (কার্যকর commit-এর পরে)। “Returned” বা “Rejected”-এ reason ও audit reference থাকে। |
| A-09 | Skeleton grid, version লাইন আগে দেখায়। | কোনো filter-এ row না থাকলে "No actions match these filters." | Matrix আনা যায়নি: আবার চেষ্টা। আংশিক grid দেখায় না। | Export-এ একই version ও data। |
| G-01 | Switch-এর সময় skeleton। আগের সন্তানের data কখনো নয়। নাম, class, ছবি বা notice badge নেই। | কোনো সন্তানে সক্রিয় right না থাকলে নিরপেক্ষ message ও school office-এর help line। | Data আনা যায়নি: G-02-এর নিরপেক্ষ ভাষা। | সন্তানের নাম ও class header-এ, তারপর data। |
| G-03 | Session তালিকা skeleton। | অন্য device নেই: "This is your only device." | Session শেষ করা যায়নি: আবার চেষ্টা। | "Session ended." ওই device পরের request-এ S-09 পায়। |

### ১৪.২ সব screen-এ একই state (AccessState)

| State | কখন | পর্দায় কী | কী দেখানো হয় না |
|---|---|---|---|
| Not found | অন্য school, link ছাড়া সন্তান বা scope-এর বাইরের record | "Record not found." | Record আছে কি না, নাম, class, ছবি |
| No permission | শুধু staff, নিজের scope-এর ভেতরের record-এ role-এ কাজটি নেই। Guardian পায় not-found রূপ। | "You don't have permission for this." school office-এর পথ | কোন role পেলে হবে তার ভেতরের হিসাব |
| Pending approval | Approval-এর অপেক্ষা | "Waiting for approval." approver-এর role | সফলতার চিহ্ন |
| Session ended (S-09) | Session শেষ, revoke বা মেয়াদ | "Your session has ended. Sign in again." | Cache-এর data |
| Product off | School-এ product বন্ধ | "This isn't available at your school." | অন্য product বা school-এর তথ্য |
| Second factor needed | Privileged workspace বা step-up | SecondFactorPrompt | Second factor-এর আগে data |
| Access changed while open | Screen খোলা অবস্থায় custody restriction বা revoke। কার্যকর commit-এ খোলা online screen-এও access সরে। | Content সরে নিরপেক্ষ state আসে। Cache মোছা হয়। Form-এ রাখা sensitive data-ও সরে যায়। | কারণ। Guardian-কে custody-র কারণ কখনো নয়। |
| Offline | Network নেই। Guardian-এর sensitive content OD-18 অনুযায়ী। | "You are offline." শেষ হালনাগাদের সময়, "connect to see" | Revoked session-এর data; guardian-এর sensitive detail |
| Stale data | বেশি পুরোনো data | "Last updated …" সঙ্গে refresh | পুরোনো data নতুন বলে দেখানো |

**নিয়ম**

1. ফাঁকা screen নয়, skeleton দেখায়।
2. Error-এ ব্যবহারকারীর লেখা কিছু হারায় না।
3. Error message অন্য school বা সন্তানের record আছে কি না জানায় না।
4. প্রতিটি success বলে কী বদলাল, কখন থেকে কার্যকর। Sensitive পরিবর্তনে audit reference দেয়।
5. "Pending approval" আলাদা state। তা কখনো success-এর মতো দেখায় না।
6. Access হারালে form-এ রাখা sensitive data-ও সরে যায়।

## ১৫. IAM-002 — pilot invite বনাম R1 bulk onboarding

IAM-002 tracker-এ **MVP partial**। দুই অংশের প্রমাণ আলাদা ঘরে যাবে (`MVP slice evidence` ও `R1 remainder evidence`)।

| বিষয় | Pilot অংশ (MVP slice) | বাকি R1 অংশ |
|---|---|---|
| কী | একজন করে invite ও activation | Bulk onboarding |
| কে করেন | School Admin (staff), Registrar (guardian), Operator (প্রথম Owner বা School Admin) | School Admin ও Registrar, file দিয়ে |
| Screen | A-05, S-05, A-02-এর invite অবস্থা | A-11 |
| Flow | F-01 | File upload → preview → validation → ধাপে ধাপে পাঠানো → অগ্রগতি → ব্যর্থ row → আবার পাঠানো → মেয়াদ শেষের report |
| নিয়ম | Link একবার ব্যবহারযোগ্য ও মেয়াদি। Role ও scope invite-এই বাঁধা। | একই নিয়ম, সঙ্গে প্রতি school-এর fair-use সীমা (SAS-013) ও duplicate ব্যক্তি চেনা |
| Privileged role | Owner approval + second factor। Privileged invite-এ IAM-005 লাগে। | Role-এর scope, প্রযোজ্য Owner approval ও IAM-005-এর second-factor নিয়ম একই থাকে। |
| Guardian | Invite মানে access নয়। Right আসে F-06 থেকে। Import থেকে তৈরি pending link request কোনো outbound invite পাঠায় না ও কোনো সক্রিয় right দেয় না (A8 OD-07 ও TD-04-এর সঙ্গে মেলে)। | Bulk invite-এও right নিজে থেকে আসে না। |
| বেছে নেওয়া pending guardian link-এ একবারে invite | **PR-06 (CX-13): Project owner গৃহীত · client product owner-এর অনুমোদন ও tracker decision বাকি।** Registrar নিজের scope-এর pending link বেছে একবারে invite পাঠান। থাকবে preview, পাঠানোর মুহূর্তে check, ধীরে পাঠানো ও প্রতি invite-এর ফল। Access দেয় না। বিস্তার implementation plan CX-13-এ। শুধু client product owner-এর অনুমোদনের পরে pilot-এ। | অনুমোদন না এলে এটিও বাকি R1-এ (A-11)। পূর্ণ bulk onboarding যেকোনো অবস্থায় R1-এ। |
| Acceptance | V03-এর invite, reuse ও expiry | V03 + বড় batch-এর অগ্রগতি ও ব্যর্থতার report |
| Design-এর অবস্থা | এই plan-এ পূর্ণ | এই plan-এ শুধু রূপরেখা। বিস্তারিত বাকি R1-এর আগে। |

## ১৬. অগ্রাধিকার — P0, P1, P2

P0, P1 ও P2 হলো **কাজের ক্রম**। এতে scope কাটা হয়নি। দশটি feature-ই tracker-এ আগের label-এ আছে।

| অগ্রাধিকার | Feature | ব্যবহারকারীর লাভ | মাপা যায় এমন acceptance |
|---|---|---|---|
| **P0** | IAM-014 Published permission matrix | School জানে কে কী পারেন। QA একই তালিকা ধরে পরীক্ষা করে। | ১) Enforce হওয়া প্রতিটি action-এর row আছে। Row ছাড়া action থাকলে build আটকায়। ২) Test-এর expected result Doc 04 থেকে আলাদাভাবে লেখা (matrix থেকে generate করা নয়): প্রতি role-এ অন্তত একটি allowed ও একটি blocked কাজ; scoped ঘরে scope-এর ভেতরের ও বাইরের target; স্থায়ীভাবে denied ঘর শুধু deny করে; screen ও export একই version। সব pass। ৩) Screen-এ version ও তারিখ দেখা যায়। Export-এ একই data। |
| **P0** | IAM-003 Scoped role | Registrar নিজের campus-এ কাজ করেন। অন্য campus বা school-এর data সুরক্ষিত। | ১) §৮.১-এর R1–R9 ও R10–R17-এর পথ, screen, সরাসরি API ও export তিন পথেই মেলে। ২) Role বা scope বদলের পরের প্রথম request থেকেই নতুন সিদ্ধান্ত। ৩) Not-found response অস্তিত্বহীন ID-র response-এর সঙ্গে হুবহু এক। |
| **P0** | IAM-004 Guardian right ও custody | পরিবার শুধু অনুমোদিত সন্তানের অনুমোদিত তথ্য দেখে। শিশু সুরক্ষিত থাকে। | ১) Fixture A ও B-তে §৮.২-এর ফল screen, API, file link, report ও notice পাঁচ পথে মেলে, area right (G14–G16) সহ। ২) ভুল sibling-এর ID-তে নাম বা class প্রকাশ শূন্য। ৩) Document ছাড়া custody অনুরোধ submit হয় না। নিজের অনুরোধ approve করার সফল চেষ্টা শূন্য। ৪) কার্যকর commit-এর পরে restricted guardian-এর কাছে নতুন request ও নতুন dispatch শূন্য। Partial restriction শুধু নির্দিষ্ট area আটকায়। খোলা online screen-এ কার্যকর commit-এর সঙ্গে restriction কার্যকর হয়। GB অপরিবর্তিত। ৫) Go-live gate (§১) না মেটা পর্যন্ত custody approve আসল user-এর জন্য বন্ধ। ৬) অনুরোধ pending বা ফেরত থাকলে প্রস্তাবিত restriction কার্যকর হয় না। |
| **P0** | IAM-007 সম্পর্ক শেষে access শেষ | চলে যাওয়া staff বা পরিবার আর school-এর data পান না। | ১) Membership শেষ, role revoke বা grant-এর মেয়াদ শেষ, guardian link শেষ ও student চলে যাওয়া—চার ক্ষেত্রেই পরের প্রথম request থেকে read ও write বন্ধ। Delegation-এর মেয়াদ শেষের প্রমাণ IAM-010-এর সঙ্গে। ২) আগে queue-তে থাকা notice-এর delivery শূন্য। ৩) Revoked device-এ cache-এর data দেখা যায় না। Offline বা resume-এ cache থেকে sensitive content শূন্য (OD-18)। ৪) History record থেকে যায়। |
| **P1** | IAM-001 Sign-in | সবাই নিজের phone বা email দিয়ে নিরাপদে ঢোকেন। | ১) স্থায়ী password কোনো message-এ যায় না। ২) মেয়াদ শেষ বা আগে ব্যবহৃত code-এ access শূন্য। ৩) ভুল identifier ও ভুল code-এ একই message। V03। |
| **P1** | IAM-002 pilot invite | School নিজের লোকদের দ্রুত যুক্ত করে। | ১) মেয়াদ শেষ বা আগে ব্যবহৃত link-এ access শূন্য। ২) আগের account থাকলে duplicate account শূন্য। ৩) Invite-এর অবস্থা তালিকায় দেখা যায়। ৪) (PR-06 অনুমোদিত হলে) preview-এর বাদ পড়া সংখ্যা মেলে; skipped ও failed-এর কারণ থাকে; একই invite-এ দ্বিতীয় SMS শূন্য; bulk send অন্য school-এর notice দেরি করায় না (X-07)। V03। |
| **P1** | IAM-006 Session, device ও recovery | হারানো phone বা বদলানো number-এ account সুরক্ষিত থাকে। | ১) শেষ করা session-এর পরের request বন্ধ। ২) Recovery-র পরে পুরোনো session বন্ধ। ৩) Recycled number দিয়ে আগের account বা সন্তানে access শূন্য (OD-07)। ৪) নতুন device + শুধু OTP হলে সন্তানের content আটকে থাকে (OD-07)। ৫) Shared device-এ account বদলের পরে আগের account-এর data শূন্য। V03। |
| **P1** | IAM-005 Privileged MFA | বেশি ক্ষমতার account চুরি হলেও ক্ষতি আটকায়। | ১) Second factor ছাড়া privileged workspace খোলার সফল চেষ্টা শূন্য। ২) Privileged role grant-এ Owner approval ও second factor দুটোই audit-এ আছে। ৩) Step-up ছাড়া সফল sensitive কাজ শূন্য। ৪) অন্য admin ছাড়া second factor reset শূন্য। V03, V04। |
| **P1** | IAM-013 একাধিক role বা school | একজন ব্যক্তি এক account-এ সব ভূমিকা সামলান। | ১) School বদলের পরে আগের school-এর data screen ও cache-এ শূন্য। ২) সক্রিয় school ও role প্রতিটি screen-এ দেখা যায়; phone-এ campus ও workspace এক tap দূরে। ৩) এক role তুলে নিলে অন্য role অক্ষত। V02, V03। |
| **P2** | IAM-010 Delegation | কেউ অনুপস্থিত থাকলেও কাজ থামে না, access সীমিত থাকে। | ১) শেষ সময় ছাড়া delegation তৈরি হয় না। ২) মেয়াদ শেষের পরের request বন্ধ। ৩) Audit-এ "কার পক্ষে" লেখা থাকে। ৪) শুধু অনুমোদিত grantor delegation দিতে পারেন (OD-09)। ৫) Delegator-এর scope-এর বেশি delegation শূন্য। ৬) আগে revoke কাজ করে। V02। |
| **বাকি R1** | IAM-002 bulk onboarding | বড় school একসঙ্গে অনেককে যুক্ত করে। | Pilot acceptance-এর অংশ নয়। §১৫ দেখুন। |


## ১৭. Owner-এর ব্যতিক্রম · client approval pending

নতুন প্রস্তাব যোগ হবে owner চাইলে। ID হবে `S2-PR-nn`। নিচের সিদ্ধান্তটি pilot scope-এ যাওয়ার আগে client product owner-এর অনুমোদন ও tracker decision লাগে।

| # | প্রস্তাব | কেন | নির্ভর করে |
|---|---|---|---|
| PR-06 | Project owner গৃহীত · client product owner-এর অনুমোদন ও tracker decision বাকি (CX-13)। Pilot-এ বেছে নেওয়া pending guardian link-এ ন্যূনতম batch invite | Cutover-এ প্রায় ৯৯০টি আলাদা invite অবাস্তব ও ভুলপ্রবণ | IAM-002-এর pilot অংশের scope বদল; client product owner-এর অনুমোদন; implementation plan CX-13 |

## ১৮. Traceability

| Requirement | Feature | Design requirement | Flow | Screen | Test |
|---|---|---|---|---|---|
| 3.2.3, 7.4.3 | IAM-003 | DR-01, DR-02, DR-03, DR-10 | F-05 | A-01, A-02, A-03, A-04 | V02 |
| 3.2.4, 5.1.2, 7.1.C, 1.1.4, E2E-14 | IAM-004 | DR-05, DR-06, DR-07, DR-08, DR-19 | F-06, F-07 | A-06, A-07, A-08, G-01, G-02 | V02, V04, X-06 |
| 3.2.6, 3.2.W | IAM-007 | DR-09, DR-10 | F-09 | A-03, S-09, G-02 | V02 |
| Blueprint পৃ. ৩৪ ও ৯৮ · R1 | IAM-014 | DR-04, DR-11 | F-11 | A-09 | V02, V04 |
| 3.2.1, 3.2.C, 17.1 | IAM-001 | DR-12, DR-19 | F-02 | S-01–S-04 | V03 |
| 3.2.6, 3.2.W | IAM-002 | DR-13, DR-18, DR-19 | F-01 | A-05, S-05, A-11 | V03 |
| 3.2.2, 15.1.1 | IAM-005 | DR-15 | F-04 | S-06 | V03, V04 |
| 3.2.2 | IAM-006 | DR-14, DR-19 | F-09, F-10 | G-03, A-03, S-08, S-09 | V03 |
| 3.2.3, 7.1.1 | IAM-013 | DR-16 | F-03 | S-07, A-01 | V02, V03 |
| 3.2.3 | IAM-010 | DR-17 | F-08 | A-10, A-01 | V02 |

## ১৯. Review-এ যা বিশেষভাবে দেখতে হবে

§২০-এর প্রতিটি বদল ঠিকভাবে বসেছে কি না দেখুন। বিশেষ করে:

1. **ভুল sibling।** G1, G3, G5, G8, G10, G17, G18: কোনো পথে (ID বদল, পুরোনো notification, দ্রুত switch, cache, shared phone) অন্য সন্তানের তথ্য ফাঁস হয় কি না।
2. **Custody ও কার্যকর commit।** F-07, A-07, A-08: "effective" শব্দ কার্যকর commit-এর আগে কোথাও আসে কি না; staff-এর লেখায় "ফেরানো যায় না" সীমা স্পষ্ট কি না।
3. **Shared বা recycled phone।** DR-19, F-01, F-10: OTP-কে পরিচয়ের প্রমাণ ধরা হয়েছে কি না।

## ২০. Review-এর পরে যা বদলেছে

| # | Review-এর বিষয় | কী বদলেছে | কোথায় |
|---|---|---|---|
| 1 | Check ক্রম অস্পষ্ট, পুরোনো entitlement label | নতুন ক্রম: Session → Membership → C1 check → ৩–৮। এটি আমাদের প্রস্তাব | §২.১, O-05, R7 |
| 2 | "Effective" কখন | কার্যকর commit একটি; তার আগে screen "effective" বলে না | OD-14, DR-07, F-07, A-07, A-08 |
| 3 | Custody-র অপেক্ষার সময় | OD-04 এখন সিদ্ধান্ত হয়েছে: জরুরি hold ও Owner-এর কাছে escalation | OD-04, PR-01, go-live gate |
| 4 | Notice-এর নিশ্চয়তা | OD-19: commit-এর পরে নতুন dispatch নেই, পাঠানো ফেরে না | OD-19, DR-07, A-07 |
| 5 | Wrong sibling ও not-found | Deny ঠিক একই not-found; pending link switcher-এ নেই; selection identity | DR-03, DR-08, G3, G5, G8, G10 |
| 6 | Contact বনাম access | Contact save zero grant; ContactOnlyNote আলাদা; A-06 নতুন layout | DR-05, F-06, A-06, §১০ |
| 7 | Area right | Area notice-এ ওই area-র right; preference right বাড়ায় না | AS-04, DR-06, G14–G16, Fixture B |
| 8 | Offline ও resume | OD-18: cache থেকে sensitive content নয়; ১ মিনিট bound | OD-18, DR-09, G17, §১৪ |
| 9 | Shared ও recycled phone | DR-19: OTP শুধু দখল; OD-07 নতুন default; shared device-এ account switch | DR-12, DR-19, OD-07, F-10, G18 |
| 10 | Invite conflict | সাধারণ message; ব্যক্তি sign in করে গ্রহণ, inviter নিশ্চিত করেন | DR-13, F-01 |
| 11 | Session ও admin সীমা | "End all sessions" শুধু ওই school-এর membership | DR-14, F-09 |
| 12 | Privileged ও go-live | Go-live gate; OD-05, OD-06 default লেখা; A-04 mock | §১, DR-15, OD-05, OD-06, A-04 |
| 13 | SMS-only guardian ও emergency notice | আলাদা "SMS recipient"; emergency notice সন্তান-নিরপেক্ষ | OD-11, OD-17 |
| 14 | অনুমানের ভিত্তি | AS table-এ "ভিত্তি" column; Fixture A ও B সংজ্ঞা | §৪.১, §২.৩, §৮.২ |
| 15 | Accessibility ও contrast | `color.border` #767F8C; নতুন test ও screen-reader নিয়ম | §১১.১, §১২, §১৩ |
| 16 | Acceptance ও R-rows | Doc 04 থেকে আলাদা expected result; R12a–R12c; R15, R17 সংশোধন | §৮.১, §১৬ |
| 17 | CTO-র প্রশ্ন: pilot-এ batch invite | PR-06 (project owner গৃহীত; client product owner-এর অনুমোদন বাকি); IAM-002 table-এ আলাদা সারি | §১৫, §১৭ |
| 18 | Owner-এর সিদ্ধান্ত (২৬ নভেম্বর ২০২৬) | OD-01–OD-19 সিদ্ধান্ত; জরুরি hold; Learning Admin privileged; TOTP; scope বদল দুটি client অনুমোদনের অপেক্ষায় | §১, §৪.২, F-02, F-04, F-07, F-10, A-04, A-07, §১০, §১১.৪, §১৫–§১৭ |
| 19 | PR-05 বাদ (owner-এর সিদ্ধান্ত, CTO-র মাধ্যমে, ২৬ নভেম্বর ২০২৬) | Owner-এর নিয়ম: Blueprint PDF-এর বাইরে কিছু নয়, আর PDF-এ pupil activation code নেই। OD-10 এখন IAM-001 ও D-16 (PDF পৃ. ৩২, ৬৪, ৯৮) অনুযায়ী। PR-05 §১৭ থেকে সরানো। Client অনুমোদনের তালিকায় শুধু PR-06। ফাঁক নথিভুক্ত: নিজের phone বা email ছাড়া pupil guardian-এর মাধ্যমে, V10-এ প্রভাব। | §১, O-13, §৪.২ OD-10, F-02, §১৭ |
| 20 | Owner-এর PDF scope review · ২৬ নভেম্বর | Emergency hold, rights preset, periodic access review, view-as-role, অতিরিক্ত usability target, এক মিনিটের সীমা ও bulk privileged-role blanket rule বাদ। Custody approval পর্যন্ত pending, কার্যকর commit-এ প্রভাব। IAM-014-এর source এখন Blueprint পৃ. ৩৪ ও ৯৮। PR-06-এর client approval pending। আগের entries ইতিহাস। | OD-02/04/16, DR-09, F-07, A-07/A-08, §১৫–§১৮ |
