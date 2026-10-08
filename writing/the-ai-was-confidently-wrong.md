# The AI was confidently wrong, and the numbers were real

I'm a patrol sergeant in Georgia. Part of my job is final approval on the incident and motor vehicle crash reports my shift writes, roughly 40 to 60 a month. A report can be well written and still be wrong, and the wrong ones rarely look wrong.

On my own time I run ParentPulse (myparentpulse.app), a family organization app. On my own projects I build with Claude Code, directing AI agents that write the code; I set the rules, guardrails and defense-in-depth controls, and I review and verify everything before it ships.

One premium feature is a weekly report, written by a large language model, that tells parents how their family's week went. The day before launch, 24 July 2026, I sat down and read that report the way a parent would, asking of every number where it came from. It was wrong in ways no automated test would have caught:

- **It said the kids earned 227 stars that week.** 227 was a real number from the database: the combined balance sitting in every child's wallet, not a week's earnings. I had taught it that mistake myself, in an example in my own prompt.
- **It congratulated one child on a trophy he hadn't earned.** The trophy was real. It belonged to one of the other kids. Sometimes the model also turned a goal in progress into a goal completed.
- **It guessed children's pronouns,** about once in every twenty reports. The app never stores a child's gender, so every pronoun was a guess about someone's kid.

Every one of these was plausible: a true value in the wrong sentence. That is the failure that matters, in a police report or in an AI report.

## What I did

1. **Measured instead of trusting one good draft.** I generated the report against the live model again and again and counted the failures: about one bad attribution in every eight runs when a trophy and a standout child showed up in the same week.
2. **Fixed causes, not wording.** Each child's status is now stated on that child, as one sentence or an explicit "none", so the model has no separate list to cross-reference and mis-join. I lowered the model's temperature and added a hard rule that a goal in progress is never a trophy. For pronouns: names only, plus a final self-check. And I replaced the misleading headline with a true earned-this-week number, reconciled against the production data.
3. **Proved every fix the same way.** Zero misattributed achievements in 24 live runs. Zero pronoun guesses in 40.

It took one day and thirteen pull requests.

In October I moved the feature to a newer model and measured again, because a fix proven on one model is not proven on the next. The pronoun slips came back, about 3 in every 100 drafts. So a check now reads every draft before it is saved: a draft with a guessed pronoun is sent back once, and if it slips again it is refused. None reach a parent.

## What I took from it

AI output that reads well is not evidence that it is right. You check it against the source, the same way I check a use-of-force report against the body camera footage before it goes up the chain. AI is starting to draft police reports, and the people who check that work will matter as much as the people who build it.

*Samuel Robinson · linkedin.com/in/samuelbrobinson · github.com/srobinson457-cyber*
