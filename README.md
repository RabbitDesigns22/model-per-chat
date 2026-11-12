# model-per-chat

**Pay for intelligence where a mistake is expensive, and pay for volume nowhere.**

`model-per-chat` is an agent skill for Claude Code (and any agent that reads `SKILL.md` skills). It is for people who run many chats, agents, subagents or scheduled tasks at once and need to decide which model each one gets: the newest flagship, the daily driver, the mid model, the small one, or no model at all. Without it, two things go wrong. Everything stays on the top model, and a watcher that says "nothing new" every 20 minutes eats the plan. Or everything moves to the cheap model, and the chats that touch money or customers start making mistakes.

With the skill, the agent reads the plan meters and each session's current model, then checks today's prices and plan rules on the official pages, not from memory. It scores every chat 1-10 by what a mistake there costs and maps that score, plus how often the chat runs, to a model. It flags the traps: a flagship that is free only until a date, usage credits switched off, scheduled tasks that ignore your model choice. And it only downgrades a chat after checking it on that chat's real recent work, with a rollback rule written down.

## Cheap by default, climbs by itself

The default for a cost-conscious owner: every chat that is not a supervisor runs on the mid model at low or medium effort, and its instructions carry one escalation rule. Bulk reading goes to a small-model subagent. A hard step (a plan across many files, a bug after 2 failed tries, work for a paying client) goes to a top-model subagent at high effort for that one step, then back to base. Before anything irreversible (a customer message, ad spend, a deploy, a publish, a delete), a short top-model check of the exact action. Adaptive thinking already scales thinking per turn; a session cannot switch its own model, so the climb is done by delegation and reported ("checked with the strong model").

## See the difference

> **Owner:** "A new flagship came out yesterday. Which chat gets it, which keeps Opus, which goes cheaper? Save without errors."

**Without the skill:** it gives a sensible split (flagship for the supervisor, the daily driver for chats that touch money, small models for watchers), but it moves the design chats down without testing them, sets no date to move the supervisor off the flagship before its free window ends, and promises to "confirm" scheduled tasks were switched without noticing they may have no model setting at all.

**With the skill:** the same split, plus: scores per chat by the cost of a mistake, design chats stay put until 2-3 of their real tasks pass on the cheaper model, a reminder the day before the flagship window closes (credits are off, so the supervisor would stall mid-task), a rollback rule ("2 pieces sent back in a week, move it back up"), and an honest "scheduled tasks start on the app default; here is the workaround".

**Skip it if** you are picking a model inside API code (use the `claude-api` skill) or for one single task.

**Status:** new. Built for a real fleet of 20+ Claude desktop chats the day a new flagship model launched. Tested with and without the skill in two rounds of 3 cases. Round 1 (who gets which model, "move everything to the small model", "switch my scheduled tasks and confirm"): 6/6 with it, 3.5/6 without. Round 2 ("save, Opus is expensive too, climb effort only when needed", a cheap chat about to send a price to a customer, two failed fixes on a live app): 6/6 with it, 4/6 without; without it, bulk reading stayed on the expensive path, the climb was not reported, and it implied a chat can switch its own model. Plain instructions, no scripts, no network calls. MIT. Issues and suggestions welcome.

## Install

With the [skills CLI](https://skills.sh):

```bash
npx skills add RabbitDesigns22/model-per-chat
```

Or manually: copy `skills/model-per-chat/` into `~/.claude/skills/` for all your projects, or into `.claude/skills/` for one project.

## Works well with

- [`one-pass`](https://github.com/RabbitDesigns22/one-pass): fewer redo turns, which saves more than any model switch.
- [`session-watchdog`](https://github.com/RabbitDesigns22/session-watchdog): a watcher that should run on the cheapest model that works.

## Español

**Un modelo para cada chat.** Cuando tienes muchos chats, agentes o tareas programadas, la skill decide cuál lleva el modelo más fuerte, cuál el normal y cuál uno barato o ninguno. Primero lee tus límites y el modelo de cada chat, y revisa las reglas de tu plan en las páginas oficiales. Después le pone a cada chat una nota del 1 al 10 según cuánto cuesta un error ahí. Avisa de las trampas, como un modelo gratis solo hasta cierta fecha o tareas programadas que no respetan el modelo. Solo baja un chat a un modelo más barato después de probarlo con su trabajo real. Por defecto deja los chats en un modelo barato con poco esfuerzo, y para el paso difícil o el que no tiene vuelta atrás (mandar a un cliente, gastar en anuncios, subir una app) le pasa ese paso al modelo fuerte y regresa.

Instalar: `npx skills add RabbitDesigns22/model-per-chat`

## More skills from the same setup

These come from one real workflow: a small agency run with 15+ Claude Code sessions in parallel. Each skill fixes one problem that kept happening there.

| Skill | What it fixes |
|---|---|
| [one-pass](https://github.com/RabbitDesigns22/one-pass) | get it right the first time; stop paying tokens for rework |
| [hora-exacta](https://github.com/RabbitDesigns22/hora-exacta) | real date and time in every message, so nothing lands on the wrong day |
| [hands-on](https://github.com/RabbitDesigns22/hands-on) | the agent does what it can and only asks you for codes, keys and payments |
| [easy-for-them](https://github.com/RabbitDesigns22/easy-for-them) | steps people actually follow: direct link, one action per line |
| [one-answer](https://github.com/RabbitDesigns22/one-answer) | one thought-through answer instead of plan, fix, new plan |
| [never-twice](https://github.com/RabbitDesigns22/never-twice) | a shared error log so no session repeats a known mistake |
| [sort-me-out](https://github.com/RabbitDesigns22/sort-me-out) | one ordered list of what is urgent, waiting and forgotten |
| [in-the-loop](https://github.com/RabbitDesigns22/in-the-loop) | a live board so parallel sessions know what the others changed |
| [study-first](https://github.com/RabbitDesigns22/study-first) | reads everything in full and answers point by point, no invented details |
| [session-watchdog](https://github.com/RabbitDesigns22/session-watchdog) | wakes the sessions that got stuck mid-task, leaves the rest alone |
| [skill-harvest](https://github.com/RabbitDesigns22/skill-harvest) | reads your sessions and finds the skills you are missing |
| [small-budget-ads](https://github.com/RabbitDesigns22/small-budget-ads) | Meta Ads rules for 1-5 USD a day ad sets |
| [whatsapp-closer](https://github.com/RabbitDesigns22/whatsapp-closer) | evidence-ranked rules for a WhatsApp sales assistant that closes |
| [whatsapp-number-onboarding](https://github.com/RabbitDesigns22/whatsapp-number-onboarding) | connect a number to the WhatsApp Cloud API in the safe order |
| [proposal-pdf](https://github.com/RabbitDesigns22/proposal-pdf) | short PDF proposals with every price checked against its source |
| [published-vs-planned](https://github.com/RabbitDesigns22/published-vs-planned) | says "published" only for what it saw live on the public page, Facebook and Instagram checked apart |
| [source-every-fact](https://github.com/RabbitDesigns22/source-every-fact) | every price, phone and name on a post traced to its source, nothing added |
| [only-what-was-asked](https://github.com/RabbitDesigns22/only-what-was-asked) | "pause only Google Ads" pauses only Google Ads, and proves the rest did not move |
| [model-per-chat](https://github.com/RabbitDesigns22/model-per-chat) | the right model for each chat: top model where mistakes cost money, cheap or none where it only runs often |
| [music-by-niche](https://github.com/RabbitDesigns22/music-by-niche) | music that matches the client's business, never repeated for a client or across clients in 3 weeks |
| [full-power](https://github.com/RabbitDesigns22/full-power) | the best the model can do on high-stakes work: strongest model only for the hard step, best-of-N with a judge, fresh-eyes verifier |

If one of them saved you a redo, a star helps other people find it.

## License

MIT. Made by [Rabbit Designs](https://github.com/RabbitDesigns22).
