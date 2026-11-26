# Desk School · Integration CTO handoff

**বর্তমান CTO Codex। পরের কাজ System 1 · Multi-tenant Platform-এর completion report যাচাই।** Project owner Claude-কে দায়িত্ব দিলে Claude এখান থেকেই কাজ ধরবেন। পরে owner Codex-কে ফিরিয়ে আনলে Codex Claude-এর সর্বশেষ handoff পড়ে কাজ চালাবেন। একই সময়ে একজনই active CTO। দায়িত্ব হস্তান্তরের সময় নিচের বর্তমান অবস্থা ও শেষ রেকর্ড দুটোই হালনাগাদ করবেন।

## বর্তমান অবস্থা · ২৫ নভেম্বর ২০২৬

| বিষয় | সর্বশেষ যাচাই করা অবস্থা |
|---|---|
| দায়িত্ব | Project owner-এর নির্দেশে Codex active। Claude-এর জন্য handoff প্রস্তুত। পরবর্তী দায়িত্ব বদলের সিদ্ধান্ত owner দেবেন। |
| Workspace | `D:\nls-all-projects\Erp-lms`। স্থানীয় planning checkout-এর branch `main`, commit `febdc8f`। চলতি integration docs এই working tree-তে আছে। Backend/app checkout ও তার integration branch-এর অবস্থা **Repository verification pending**। |
| যা তৈরি হয়েছে | ১০টি system-এর dependency ও merge order, shared data/permission contracts, module-wise MVP map এবং প্রথম তিন System Agent-এর English prompt owner-কে দেওয়া হয়েছে। Agent-রা কোন prompt পর্যন্ত গেছেন, তার report CTO-র কাছে আসেনি। |
| প্রথম তিন system | **১ Platform** · C1/C2/C9/C10, ৩২ candidate + ২ partial। **২ Identity** · C3, ৯ candidate + ১ partial। **৩ Students** · A7/A8, ৭ candidate + ১ partial। Partial-এ pilot অংশ ও বাকি R1-এর evidence আলাদা। |
| Report, evidence ও merge | প্রথম তিনটির completion report, সংশ্লিষ্ট PR বা merge commit CTO-র কাছে আসেনি। Local tracker-এর ৫৪১টি feature-ই `Needs repository verification`। Acceptance evidence-এর ঘরগুলি ফাঁকা। Agent-এর বাইরের checkout-এ কী অগ্রগতি হয়েছে, তা report পেলে মিলবে। |
| পরের পদক্ষেপ | System 1-এর assigned MVP কাজ, review ও test শেষ হলে **project owner তার report দেবেন**। বর্তমান CTO সেটি যাচাই করবেন। নতুন CTO এলে একই report থেকেই শুরু করবেন। |

## দায়িত্বের ভাগ

- **Project owner** CTO ও System Agent-এর দায়িত্ব দেন, Agent-এর গুরুত্বপূর্ণ প্রশ্নের উত্তর দেন, design ও implementation plan review করেন এবং coding শুরুর আলাদা নির্দেশ দেন।
- **System Agent** নিজের system বোঝেন, design plan ও implementation plan তৈরি ও review করেন। Coding-এর নির্দেশ পেলে implementation, code review, test ও fix করেন। শেষে Feature ID ও evidence সহ বাংলায় completion report দেন।
- **Integration CTO** system-গুলির scope ও data ownership মিলিয়ে রাখেন, completion report-এর দাবি পরীক্ষা করেন, প্রস্তুত PR যাচাইকৃত target branch-এ merge করেন এবং ক্রম অনুযায়ী পরের report চান বা system-এর task দেন। Source code, schema, migration বা conflict-এ code সম্পাদনার দায়িত্ব সংশ্লিষ্ট System Agent-এর। Client scope বা target release বদলাতে হলে client product owner-এর সিদ্ধান্ত লাগে।

Owner-এর সঙ্গে সহজ বাংলা হরফে কথা বলবেন, “আপনি” সম্বোধন করবেন ও পরিচিত English technical terms রাখবেন। Status দিলে আগে ফল, তারপর তার প্রমাণ, কী বাকি ও কার পরের কাজ তা লিখবেন।

## নতুন CTO কী পড়বেন

1. এই file-এর বর্তমান অবস্থা ও সবশেষ দায়িত্ব বদলের রেকর্ড পড়ুন। Owner আরও সাম্প্রতিক handoff বা report দিলে সেটিও নিন। Workspace বদলালে চলতি working copy-টি ব্যবহার করুন। নতুন evidence-এর সঙ্গে পুরোনো status-এর অমিল হলে কোনটি কবে যাচাই হয়েছে তা লিখুন।
2. System-এর কাজ ও review-এর ক্রম, কে কোন data লেখে, এবং Agent-কে দেওয়া হুবহু prompt এই তিন জায়গায় আছে। [Dependencies ও merge order](DEPENDENCIES.md) · [Interfaces ও student import-এর উদাহরণ](INTERFACES.md#student-walkthrough) · [System briefs ও prompt](SYSTEM-AGENT-BRIEFS.md)
3. মূল planning docs প্রথমবার পড়ার ক্রম **README → 01 → 03 → 04 → 02 → 05 → 06 → 07 → 08**। Feature থেকে requirement, release ও acceptance evidence মিলিয়ে দেখতে চারটি CSV ব্যবহার করুন। অস্পষ্ট নিয়ম হলে সংশ্লিষ্ট Blueprint page দেখুন। স্থানীয় PDF `C:\Users\Hriday\Downloads\Desk-SchoolERP-Technical-Blueprint-v2-team.pdf`। Blueprint-এর নিয়ম, আমাদের planning decision ও নতুন proposal-এর পরিচয় রাখুন। [Planning index](../README.md) · [Requirements](../tracker/requirements.csv) · [Features](../tracker/features.csv) · [Requirement links](../tracker/requirement-links.csv) · [Deliverables](../tracker/deliverables.csv)

**প্রথম batch-এর common rule।** School হলো tenant। School Management ও Desk Learn আলাদাভাবে চালু হয়। Registrar import করলে Platform verified school ও product entitlement দেয়, Identity C3 role, campus ও child rights যাচাই করে, Students A8 student, household ও dated placement লেখে। Guardian account–child access grant C3 লেখে। Academics A2 placement থেকে class roster পড়ে। এই সীমা তিনটি report-এই মিলতে হবে। [Data ও permission contract](INTERFACES.md#student-walkthrough)

## System Agent-কে কীভাবে task দেবেন

নতুন System Agent-এর prompt দেওয়ার আগে active CTO owner-কে বাংলায় বলবেন, planning-এর মূল **model ও reasoning level** কী হবে, কেন, আর কোন নির্দিষ্ট কাজে ছোট subagent লাগতে পারে। Owner-এর client-এ কোন model চালানো যায় তা জানা থাকলে সেই তালিকা থেকে নাম বলবেন। Options জানা না থাকলে প্রয়োজনীয় tier ও effort বলবেন। তারপর থামবেন। Owner এগোতে বললে prompt দেবেন। Codex ও Claude দুজনের জন্য এই ধাপ প্রযোজ্য।

প্রতি system-এর জন্য **একটি English planning prompt** দিন। Agent একই run-এ context, design plan ও সেই design থেকে draft implementation plan দেবেন। সমাধান বদলে দিতে পারে এমন প্রশ্ন করলে কেবল তার ওপর নির্ভরশীল কাজ pending থাকবে। দুই plan-এর জন্য Agent নিজে আলাদা English review prompt দেবেন। কী পাঠাতে হবে ও কখন পাঠাতে হবে বাংলায় বলবেন। Owner design বদলালে শুধু প্রভাবিত অংশগুলো আপডেট হবে। [বর্তমান copy block](SYSTEM-AGENT-BRIEFS.md#system-1-prompts)

**Coding শুরুর বাধ্যতামূলক ধাপ।** Owner implementation শুরু করতে বললে Agent প্রথমে verified integration base থেকে আলাদা feature branch তৈরি বা বরাদ্দ করতে বলবেন। Owner branch নিশ্চিত করে ওই branch-এ coding-এর অনুমতি দেওয়া পর্যন্ত Agent অপেক্ষা করবেন। তারপর branch যাচাই করে আলাদা worktree বা checkout-এ কাজ শুরু করবেন। Plan approval হলো review-এর সিদ্ধান্ত।

Chat ও docs দুজায়গাতেই একটি copy block-এর মধ্যে context, design, implementation draft ও review-এর section দিন। প্রতিটির আগে ফাঁকা লাইন ও একটি ছোট divider রাখুন।

<a id="model-and-token"></a>
### Codex ও Claude Code-এর model ও token ব্যবহার

| কাজ | Codex | Claude Code |
|---|---|---|
| মূল planning model | **GPT-6.2 Sol · Medium** দিয়ে শুরু। একই plan-এ tenant, permissions বা data migration নিয়ে কঠিন সিদ্ধান্ত থাকলে ওই অংশের review **High**-এ, প্রয়োজনে **GPT-6.2 Astra**। | **Sonnet · Medium** দিয়ে শুরু, client-এ effort সেট করা গেলে। একই জটিল সিদ্ধান্তের targeted review-তে **High**, প্রয়োজনে **Opus**। |
| ছোট, read-only কাজ | **GPT-6 Luna · Low** দিয়ে Feature ID, নির্দিষ্ট file বা test evidence খোঁজা। Client-এ পাওয়া গেলে Codex agent config-এ `model` ও `model_reasoning_effort` দিয়ে সেট করুন। | **Haiku** দিয়ে একই ধরনের খোঁজ। Claude Code subagent-এর model `haiku` হিসেবে বেছে দিন। Main session Opus হলে ছোট কাজে সেটিও inherit হতে পারে, তাই ছোট subagent-এর model স্পষ্ট করে দিন। |

**Codex।** একই system-এর design ও draft এক thread-এ রাখুন। ছোট read-only কাজের জন্য নির্দিষ্ট path, Feature ID ও প্রশ্ন দিন। Agent-এর উত্তর হিসেবে সংক্ষিপ্ত finding ও source reference নিন। আলাদা subagent নিজস্ব model ও tool call চালায়, তাই একই file পুরোটা parent ও child দুজনকে বারবার পড়ালে usage বাড়ে। [OpenAI model selection](https://developers.openai.com/api/docs/guides/model-selection) · [Codex subagents](https://learn.chatgpt.com/docs/agent-configuration/subagents)

**Claude Code।** `/model` দিয়ে main model এবং supported হলে `/effort` দিয়ে effort বাছুন। `/usage` দিয়ে session বা plan usage দেখুন। Context বড় হলে `/compact`-এ বলুন যেন Feature IDs, সিদ্ধান্ত, branch/PR ও pending test রেখে দেয়। আলাদা system-এর কাজে নতুন session নিলে তার planning files ও handoff দিন। [Claude Code cost guide](https://code.claude.com/docs/en/costs) · [Model settings](https://code.claude.com/docs/en/model-config) · [Subagent model](https://code.claude.com/docs/en/sub-agents#choose-a-model)

**দুই tool-এই।** শুধু স্বতন্ত্র কাজে প্রয়োজনমতো subagent ব্যবহার করুন। Main Agent design, data ownership, permission ও shared contract-এর সিদ্ধান্ত এক জায়গায় মিলিয়ে নেবেন। Agent-কে পুরো Blueprint, tracker বা long test log বারবার paste করার দরকার নেই। প্রাসঙ্গিক section, ID, ফল ও source link দিন। Model বা effort client-এ পাওয়া না গেলে কাছের supported option বলুন। এগুলি ব্যবহার করার নিয়ম। এই docs দিয়ে কোনো client setting স্বয়ংক্রিয়ভাবে বদলায় না।

## Report পেলে কীভাবে যাচাই ও merge করবেন

1. **Report-এর scope ধরুন।** System/module, assigned Feature IDs, partial-এর pilot অংশ, বাকি R1/later কাজ, repo/PR, target branch, reviewed commit ও test evidence নিন। একটি PR শুধু প্রথম module বা slice শেষ করলে system-এর বাকি assigned কাজ পরের review-এর জন্য খোলা থাকবে।
2. **দাবির সঙ্গে প্রমাণ মেলান।** Actual PR diff ও বর্তমান head-এ CI/test ফল দেখুন। দুই school-এর isolation, product/role scope, affected consumer-এর contract ও feature-specific expected/actual ফল মেলান। Evidence অন্য commit-এর হলে মাঝের পরিবর্তন ও প্রাসঙ্গিক checks যাচাই করুন। Mock result, real integration run ও client acceptance-এর status আলাদা রাখুন। [QA ও acceptance](../07-qa-pilot-and-acceptance.md)
3. **ফল অনুযায়ী কাজ করুন।** ঘাটতি থাকলে Feature ID, failing case, প্রত্যাশিত ফল ও প্রয়োজনীয় evidence দিয়ে System Agent-কে ফেরত দিন। Fix এলে updated PR যাচাই করুন। Checks মিললে নির্ধারিত integration branch-এ প্রস্তুত PR merge করে target-এ merge হওয়া নিশ্চিত করুন। PR, merge commit ও result handoff-এ লিখুন। Tracker-এ যতটুকু প্রমাণ হয়েছে ততটুকুই আপডেট করুন।
4. **Report চাওয়ার ক্রম রাখুন।** প্রথম তিন Agent পাশাপাশি কাজ করতে পারেন। CTO **System 1 পুরো যাচাই ও merge শেষ করে System 2-এর report চাইবেন**। System 2 শেষ হলে System 3-এর report চাইবেন। তিনটি শেষ হলে System 4 **Academics and Attendance**-এর জন্য পরের copy block দেবেন। তার প্রথম কাজ **A9 offline attendance**। এরপর dependency table ধরে চলবেন। [CTO-র completion ও merge নিয়ম](DEPENDENCIES.md#cto-completion)

## Codex ও Claude-এর মধ্যে দায়িত্ব বদল

**যিনি ছাড়বেন।** Owner বদল চাইলে বা session/usage limit কাছাকাছি এলে এই একই file আপডেট করুন। সর্বশেষ verified কাজ, কোন report এখন বিচারাধীন, কোনো finding কার কাছে আছে এবং ঠিক কোন action থেকে আবার শুরু করতে হবে লিখুন। Merge বা test মাঝপথে থাকলে তার সর্বশেষ দেখা অবস্থা ও reference দিন। প্রতিটি merge, ফেরত দেওয়া finding বা পরের report চাওয়ার পর checkpoint হালনাগাদ করলে হঠাৎ session থামলেও কাজটি চেনা যাবে।

**যিনি নেবেন।** Owner দায়িত্ব দেওয়ার পরে সর্বশেষ checkpoint থেকে শুরু করুন। প্রথমে শুধু প্রয়োজনীয় repo/PR/CI ও tracker state মিলিয়ে নিন। আগের CTO merge শুরু করে থাকলে PR ও target branch দেখে বুঝুন merge ইতিমধ্যে হয়েছে কি না। Result অজানা থাকলে pending রাখুন। Owner-কে সংক্ষেপে জানান আপনি কোন system নিয়েছেন, শেষ verified ফল কী এবং পরের কাজ কী। আগের CTO-র জায়গায় ফিরে এলেও এই নিয়ম একই।

### দায়িত্ব বদলের রেকর্ড

পুরোনো entry রেখে প্রতিবার নতুনটি নিচে যোগ করুন। উপরের **বর্তমান অবস্থা** ও প্রথম লাইনের active CTO/পরের কাজও মিলিয়ে নিন।

| তারিখ | দায়িত্বের অবস্থা | শেষ verified কাজ ও evidence | খোলা কাজ, owner ও পরের action |
|---|---|---|---|
| ২৫ নভেম্বর ২০২৬ | Codex active · Claude-এর জন্য handoff প্রস্তুত | প্রথম batch-এর ৩টি prompt block, মোট ৯টি prompt। [System briefs](SYSTEM-AGENT-BRIEFS.md) | System 1 report-এর অপেক্ষা। Project owner দেবেন, তখন active CTO যাচাই করবেন। |

পরের entry-তে এই তথ্যগুলি নিন। **তারিখ ও সময় → outgoing CTO / incoming CTO বা দায়িত্ব দেওয়ার অপেক্ষা → system, module, stage ও Feature IDs → report/PR, target, reviewed commit, CI ও merge commit → খোলা finding/decision ও তার owner → পরের নির্দিষ্ট action।** কোনো reference এখনও না এলে “পাওয়া যায়নি” লিখুন। Owner কাকে দায়িত্ব দিয়েছেন জানা গেলে সেই অনুযায়ী active CTO লিখুন।

### Owner-এর copy-paste prompt · Claude বা Codex দুজনের জন্য

```text
Take over as the active Integration CTO for Desk School. Read the latest
docs/project-planning/integration/CTO-HANDOFF.md and any newer handoff I provide.
Resume from the last verified checkpoint. Tell me in plain Bangla what is
complete, what is pending, and your next action. Before sharing a new System
Agent prompt, recommend the model and reasoning level, explain why in Bangla,
then pause until I ask you to share the prompt. Keep this handoff current so
the next CTO can continue from where you stop.
```
