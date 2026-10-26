# Luke: From Need to Production

I have worked with source control since before CVS. Countless repositories, countless systems, and every one of them fails at the same point: merging branches. Git did not fix that. It added a staging index, three kinds of reset and a vocabulary that still confuses developers with years of experience.

AI made the problem impossible to ignore. AI agents now write a large share of the code, and they stumble on git every day: they pollute the main branch, they force-push, they delete work that was never committed. Meanwhile QA teams are drowning in AI output, and no amount of hiring will let them keep up.

I think the model itself is obsolete. Here is what I would replace it with. I call it Luke, after my own first name.

## Stop editing code

In Luke, nobody edits the code base. Developers state an intent in plain language: what they want changed, and why. The AI writes the code.

That single decision kills merging. Intents can be drafted in parallel, but they land one at a time. If yours lands after mine, the AI simply re-implements yours on top of mine. There is never a textual merge again. The only question left is the one that always mattered: does your change still make sense now that mine exists?

If it does, it lands. If the conflict is mechanical, like code that moved into a new method, the AI settles it and logs why. If the two changes disagree on where the software should go, the intent is held until the people responsible agree. Nothing is ever deleted.

## Seven verbs, like SQL

SQL has lasted half a century because INSERT, SELECT, UPDATE and DELETE say what you want, never how the engine does it. Luke has seven verbs:

- **COMMIT:** this intent is finished; land it, all or nothing.
- **REVERT:** undo a landed intent, without erasing history.
- **BRANCH:** keep a durable variant for one customer, which keeps tracking mainline.
- **RESOLVE:** runs automatically on every landing; nobody types it.
- **RELEASE:** cut an immutable release.
- **DEPLOY:** put a release into production, in stages.
- **AUDIT:** independent verification, reserved for auditors.

There is no SAVE, because work is saved continuously. And there is a hard rule: no eighth verb. Humans turn simple concepts into a stream of what-ifs in no time, so the vocabulary stays small on purpose.

## No orphan intents

Never leave a developer alone without a goal. He will build a framework. I have watched it happen more than once in my career: someone shows up with a holy framework that only a fraction of the team will ever follow.

So in Luke, every intent must name the goal it serves, and goals come from the product owner: what is needed, for whom, and how we will know it works. A COMMIT without a goal is refused before it reaches the queue. The engine also checks proportionality: if a change does more than its goal requires, it is held. That rule applies to the AI too. Left alone, it over-engineers as happily as any developer.

Every line in production can then answer the questions that matter: why it exists, who asked for it, who approved it, and what proved it works.

## Nothing grades itself

The AI that wrote the code will always say the code is fine. So it never writes the tests that decide.

AUDIT does. It runs on a different model family, ideally from a different vendor, and writes hold-out tests from the product owner's acceptance criteria. The implementing AI never sees those tests, only the failure reports, so it cannot teach to the exam. Mutation testing checks that the tests still have teeth.

AUDIT is reserved. A developer can request an audit, anonymously, but cannot run it, scope it or read its findings. Whoever can run an audit runs it when it is safe. Whoever can scope it steers it away from his own work. Whoever can read the findings learns what to hide next time.

That is where the QA team goes. The people who tested by hand become the auditors, and the owners of each area's charter: a short file of rules the system must never break, which no single person can change.

## Humans keep the red button

Only the AI changes production. Hand-edited config, ad-hoc database fixes and unrecorded firewall rules cause a large share of incidents, and in Luke they simply cannot happen.

But humans keep unconditional stop authority. Any owner can halt a rollout or force a rollback, with no AI approval needed. A stop is always bounded and recoverable, and the less we understand what an AI is doing, the more that button matters. Every change must also come with an explanation its owners can evaluate. If the engine cannot explain it, it does not deploy.

## Production will still break

It will. Releases go out in stages, and health checks set by the owners decide whether each stage widens. A failed check rolls back to the previous release automatically. AUDIT then writes the root cause analysis, the fix arrives as a normal intent, and the failure becomes a new hold-out test. The same failure should never ship twice.

## Code nobody chooses

Neither the product owner nor the developer picks the programming language. The engine does, and what it ships is a low-level, portable form; WebAssembly is an existing example. The intent log never ships. Anyone can disassemble machine code, but nobody can recover why it exists. Once AI can regenerate code from intents, the intent log is the real intellectual property.

Certified software is the exception. DO-178C accepts neither a decompiler nor an intent log on its own: it wants reviewed source, traceability and structural coverage. A certified area hands its authority a full package under confidentiality. The good news is that the structure already fits: goals, intents, hold-out tests and AUDIT are requirements, traceability and independent verification under other names.

## Where does it break?

This is an early design, and some questions are still open. How do owners reach consensus on a held intent? How are areas drawn? Will a certification authority ever accept an AI as an independent verifier?

But the direction is clear to me. We spent decades teaching humans to merge text files. It is time to let them state what they need, and let the machines prove it was done right.

Where does this break for you? I would like to hear it.

*Designed over one late night, in conversation with Claude.*
