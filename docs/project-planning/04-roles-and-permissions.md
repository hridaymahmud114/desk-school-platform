# Roles ও permissions

প্রতিটি কাজের access check হবে **account → school membership → product entitlement → role → record scope → approval** ধরে। একই account teacher ও guardian হতে পারে। Active school ও product context অনুযায়ী screen এবং permitted actions বদলাবে।

## Role matrix

| Role | Allowed scope | মূল কাজ | Approval বা access limit |
|---|---|---|---|
| **Super Admin** | Platform operators-এর school ও plan scope | School provision, plan, product lifecycle, usage এবং operator reports | School records দেখতে scoped support session লাগবে |
| **Platform Support** | Approved school, issue ও সময় | অনুমতি নিয়ে case-specific support | School consent বা logged emergency approval, expiry ও audit প্রয়োজন |
| **School Owner বা Principal** | School এবং তাঁর অনুমোদিত campuses | Result approval, school summary, key approvals, emergency notice | Finance/HR access role ও policy অনুযায়ী |
| **School Admin** | নিজের school ও assigned campuses | School setup, staff roles, calendar, policy settings ও notices | Privileged grant ও critical policy change-এ approval |
| **Registrar** | Assigned school বা campus | Registration, import, guardian link, placement ও record changes | Custody restriction ও historical changes review করা হয় |
| **Department Head** | Assigned departments, subjects ও grades | Curriculum, lesson review এবং teacher oversight | অন্য department scope আলাদা করে grant হবে |
| **Academic Lead** | Assigned grades ও classes | Marks review, exception ফেরত দেওয়া ও approval queue | Official publish agreed Principal role-এর |
| **Exam Officer** | Assigned exam, term ও campus | Exam setup, seating, duty, candidates ও paper management | Secure paper ও result actions আলাদা scoped grants |
| **Class Teacher** | নিজের homeroom বা assigned classes | Class attendance, daily summary, overall comments ও guardian communication | Closed register correction reason ও approval দরকার |
| **Subject Teacher** | Assigned teaching groups ও subjects | Routine, lesson plan, marks, class sessions ও course links | নিজের assigned scope-এর বাইরে access school grant অনুযায়ী |
| **Substitute বা Delegate** | নির্দিষ্ট period, class ও সময়সীমা | Cover session, approved attendance বা অন্য granted actions | Access time-bound এবং revoke করা যায় |
| **Finance Officer** | Assigned school/campus এবং fee books | Bill, settlement, adjustment, reconciliation ও refund request | Policy threshold-এর ওপরে separate approver |
| **Cashier** | Assigned cash counter ও shift | Cash payment, receipt, shift closing | Posted receipt change হলে audit ও correction process |
| **Learning Admin** | School Desk Learn workspace | Courses, instructors, enrollments ও pricing | Course role assign ও policy settings agreed control অনুযায়ী |
| **Course Instructor বা Assistant** | Assigned course ও allowed activities | Content, assignment, quiz, marks ও course progress | Role-specific edit or view। Outside instructors course scope-এ থাকবেন। |
| **Student** | নিজের official records এবং active courses | Routine, saved lessons, assignments, quiz, published marks ও fees view যেখানে enabled | Age অনুযায়ী messaging ও discovery সীমিত |
| **Guardian** | Linked child ও ওই child-এর granted rights | Attendance, notice, result, fees, receipts, consent ও messages | Custody ও permissions child-by-child। Access বদলালে scope আবার check হবে। |

## পরের release-এর roles

Admission staff R2-এ applications ও offers সামলাবেন। Approved group admin স্কুলের agreed aggregates দেখবেন। Safeguarding lead, HR reviewer, nurse, librarian, transport worker, gate/front-office worker ও canteen staff নিজেদের assigned module ও record scope পাবেন। Advanced finance, procurement, operations ও partner roles অনুযায়ী নতুন actions যোগ হবে।

## কাজ করার নিয়ম

- Published result change, বড় refund, গুরুত্বপূর্ণ policy change ও custody change-এর মতো কাজে **maker এবং approver আলাদা** থাকবে।
- UI menu বা screen hide করার পাশাপাশি server, export, report, file ও event delivery-তে authorization থাকবে।
- Access record অনুযায়ী। Teacher-এর assignment বদলালে old history থাকে, current edit access নতুন assignment অনুযায়ী হয়। Guardian link বন্ধ হলে notice, export ও cached data access নতুন policy অনুযায়ী হয়।
- Subscription restriction-এর policy school data, export ও essential records-এর agreed access ধরে চলবে।
- Login, role change, access revoke, approved support session, sensitive views, result approval ও payments audit করা যাবে।

## Permission review checklist

প্রতিটি workflow-এর জন্য actor, requested action, school, product, campus, target record, allowed roles ও required approval লিখতে হবে। Check করুন actor নিজের school-এ কাজটি করতে পারে, অন্য demo school-এ scope সীমা কাজ করে, same-school অন্য class/child-তেও scope ঠিক থাকে এবং revoked বা expired account প্রত্যাশিত access পায়। V01, V02, V03, V04 এবং V15-এর evidence [QA plan](07-qa-pilot-and-acceptance.md)-এ যাবে।

Source coverage Blueprint পৃষ্ঠা ২৫–৩৪, ৯৮, ১০২–১০৩।
