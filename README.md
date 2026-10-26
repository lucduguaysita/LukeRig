# LukeRig

Experimental rig for Luke: developers state intents, AI writes the code, changes land without merges, and every line traces back to a goal.

> LukeRig is a personal, independent project. It is not affiliated with, sponsored by, or connected to any employer or client of the author.

## What Luke is

Luke is an idea for building and delivering software when AI writes most of the code. Nobody edits the code base by hand. Developers state intents in plain language, each serving a goal set by a product owner. The AI implements them, and a change lands only when it agrees with the rest of the system and with the rules of its area. An independent AI, from a different vendor, writes the tests that decide whether the work is right. Humans keep the stop button.

The whole lifecycle fits in seven verbs:

| Verb | Meaning |
| --- | --- |
| COMMIT | This intent is finished; land it, all or nothing. |
| REVERT | Undo a landed intent, without erasing history. |
| BRANCH | Keep a durable variant for one customer, which keeps tracking mainline. |
| RESOLVE | Runs automatically on every landing; nobody types it. |
| RELEASE | Cut an immutable release. |
| DEPLOY | Put a release into production, in stages, with automatic rollback. |
| AUDIT | Independent verification, reserved for auditors. |

There is no SAVE, because work is saved continuously, and there is no eighth verb.

- [Luke: From Need to Production](docs/luke.md): the idea in plain words, a ten-minute read.
- [Luke specification](docs/luke-spec.md): the full design, verb by verb.

## What LukeRig is

A test rig, not a product. LukeRig tests Luke's claims one at a time, cheapest and most fundamental first. Every experiment has pass and fail criteria frozen before any code is written, so the results stay numbers rather than opinions. A failed experiment is a result: it says which idea broke.

- [LukeRig v0: Experiment Plan](docs/lukerig-v0-plan.md)

## Status

Design stage. Nothing here is usable yet. Experiment 1 comes first: can intents land one at a time, with the AI re-implementing each one on top of the last, so that no merge is ever needed? If that fails, nothing else in Luke stands.

## Discuss

Questions, objections and "here is where it breaks" are welcome in [Discussions](https://github.com/lucduguaysita/LukeRig/discussions). Reports of failure are as useful as reports of success.

## License

MIT. See [LICENSE](LICENSE).
