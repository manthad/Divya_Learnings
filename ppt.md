# Frugal AI Mentoring — Cycle 1 Check-in
## Speaker Notes & Facilitation Guide

**Session length:** 60 minutes
**Format:** Online video call
**Attendees:** Dhan, Camra, Priti (mentees) + mentoring team
**Tone reminder:** Warm, low-pressure, encouraging. This is a re-engagement call, not an assessment. Never name missed deadlines or who submitted what. The goal is continuity and a sense of achievement — not pressure.

**Time budget at a glance:**

| Block | Slides | Time |
|---|---|---|
| Opening | 1–3 | 8 min |
| Week 1 recap | 4–6 | 7 min |
| Week 2 recap | 7–9 | 9 min |
| Live exercise | 10–11 | 14 min |
| Close & diagnostic | 12–14 | 17 min |
| Buffer | — | 5 min |

Keep one eye on the clock. If you are running behind, the recap (slides 4–9) is where to compress — the exercise and the diagnostic conversation are the parts that must not be cut.

---

## Slide 1 — Title
**Time: 1 min**

Open warmly. Thank them for making the time — acknowledge that everyone is fitting this around a full day job.

Set the frame in one sentence: *"This isn't a test or a lecture — it's a chance to catch our breath together, make sure the first two weeks have landed, and hear from you about what would help most going forward."*

Don't dwell here. Move on while the room is still settling.

---

## Slide 2 — What this session is
**Time: 1.5 min**

Walk the three cards left to right. Keep it light.

- *Catch our breath* — "No quiz. If something hasn't clicked yet, today is for fixing that, not flagging it."
- *Make sense of it* — "We'll go back over Weeks 1 and 2 at a high level, in plain language."
- *Hear from you* — "The last part of the call is genuinely yours. We want your input on how the rest of this runs."

Land the closing line: everything today, they have already seen — this is just letting it settle.

---

## Slide 3 — Powerful tools, fragile ground (the "why")
**Time: 5.5 min**

This is the most important slide of the opening. It is the one Balaji specifically asked us to keep reinforcing. Take your time.

Talking points:
- Today's commercial AI is genuinely impressive — say so plainly, don't undersell it.
- But for a public institution it has three problems: it's expensive and the price keeps climbing, it's hosted on someone else's infrastructure (usually overseas), and it changes on a schedule HEC doesn't control.
- The real question for a national regulator isn't *"can we use AI"* — it's *"can we use it on our own terms."*
- That's what Frugal AI means: AI capability HEC can understand, adapt, govern, and sustain locally.

Walk the three pillars on the right: affordable, locally controlled, sustainable.

**Optional engagement prompt:** ask one mentee — "Has anyone seen the cost of an AI subscription and thought, that doesn't scale for us?" Let it breathe for a moment, but don't force a long discussion.

---

## Slide 4 — Three tools, one workflow (Week 1)
**Time: 2.5 min**

Plain language only. No commands, no jargon.

- **Git** — a safety net for your files; it remembers every change.
- **GitHub** — the shared place that work lives; visible to the team, backed up.
- **Claude Code** — a capable assistant that reads your files, suggests changes, and explains itself.

Land the bottom band: together they let you change something, save it, share it, and keep a record of *why* — without losing track.

Keep moving — this is recap, not first teaching.

---

## Slide 5 — The one habit worth keeping (Week 1)
**Time: 2 min**

Read the quote aloud, slowly: *"Every time you send something somewhere, know what you sent and where it went."*

This is the single habit from Week 1 worth carrying forward. Walk the three "in practice" lines: before you paste, before you push, when in doubt use sample or public data.

Bridge sentence: *"This habit is exactly what Week 2's sovereignty work builds on — so hold onto it."*

---

## Slide 6 — Where you should be (Week 1 outcomes)
**Time: 2.5 min**

Tone is critical here. Read the intro line out loud: *"No need to say anything out loud — just quietly notice where you are."* This lets anyone who is behind check their own list without being put on the spot.

Read the five "I can…" statements at a calm pace, giving a beat after each.

Close with the soft note: *"If a few of these are still 'not yet' — that is exactly why we've given Week 2 more room. No pressure."*

Do **not** ask anyone to report their status.

---

## Slide 7 — What an LLM actually does (Week 2)
**Time: 3 min**

Three sentences, that's the whole mental model:

1. It predicts the next word — over and over, very fast. That's the trick.
2. It learned from a lot of text — patterns from books, websites, code. Not a database of facts.
3. It confabulates when it doesn't know — confident-sounding is not the same as correct.

Spend a moment on point 3 — it's the one that matters most for regulatory work. A plausible wrong answer is more dangerous than an obvious one.

Point to the "Watch this" card: the 3Blue1Brown video, ~8 minutes. *"If you watch one thing this cycle, watch that."*

---

## Slide 8 — Prompting is a skill (Week 2)
**Time: 3 min**

The four patterns: define the role, shape the output, allow "I don't know", ask for citations.

Then walk the before/after on the right — this is the slide that makes it concrete:
- *Before:* "Summarise this policy." — vague, you'll get something generic.
- *After:* role defined, output shaped to three bullets, permission to flag uncertainty.

Tell them: this exact "after" prompt is essentially what they'll use in the exercise in two slides' time — so this is a preview.

---

## Slide 9 — Sovereignty in one diagram (Week 2)
**Time: 3 min**

Walk the diagram as a story, following the arrows:
- You ask Claude Code to read a document on your laptop.
- It goes through HEC's gateway, crosses the boundary over HTTPS to Anthropic's API — by default in the US.
- The response comes back.
- You paste the summary into Outlook → it reaches the director's mailbox.
- Logs exist at every step — email logs and Anthropic logs are their own data flows.

Point at the dashed boundary line. Land the bottom band: *"Every arrow that crosses that line is a moment your data leaves your control."*

Tie it back: this is the trace from the Week 2 reading and Assignment 2. The point isn't that the tool is unsafe — it's that you don't *know* whether it's safe for a task until you've done the trace.

---

## Slide 10 — A small exercise (live activity)
**Time: 9 min total — ~2 min to set up, ~5 min to work, ~2 min buffer**

This is the "sense of achievement" moment. Make it feel easy and safe.

Set it up:
- Open Claude Code. Pick any short **public** HEC document — a press note, a public guideline, a webpage.
- Ask it to summarise in three bullets — but with a system prompt that (1) defines the role, (2) asks for exactly three bullets, (3) invites it to say "I don't know" if unsure.

Stress the guardrails on the right:
- **Public document only.** No HEC personal or internal data. (Say this clearly — it's the Week 0 data rule in action.)
- If Claude Code isn't working yet, watch a neighbour — they'll do it themselves later, no problem.
- There is no wrong output. The interesting part is noticing what changes when the prompt changes.

Then: *"Five minutes on the clock — go."* Stay on the call, mute, let them work. Offer help if someone is stuck. Keep it relaxed — if it runs to six minutes, that's fine.

---

## Slide 11 — Let's hear what came back
**Time: 5 min**

Bring them back. Go around all three mentees, briefly:
- Read your three bullets aloud — just the output.
- One thing you tried in the prompt — a line you wrote, a constraint you added.
- One thing you'd change next time — or a question that came up.

For each person, comment positively on **one** prompting choice — keep it genuine and specific. This is where the small win lands. Don't critique; reinforce.

If someone didn't manage to produce output, that's fine — ask what they would try, and move on warmly.

---

## Slide 12 — A small adjustment to the schedule
**Time: 2 min**

State the Week 2 extension plainly and neutrally — no blame, no apology, just a fact: *"We're extending Week 2 by one week. Everyone is balancing this with a real job — we'd rather you finish confident than rushed."*

Walk "what stays the same": we're here, email anytime; the materials don't change; no penalty for going at your own pace.

Mention that Cycles 2 and 3 will be shaped around what they actually want — which leads straight into the next slide. Do **not** go into Week 3+ content or the original roadmap; that's deliberately not in this deck.

---

## Slide 13 — Three questions for you
**Time: 13 min**

This is the diagnostic core of the call — the real reason for the session per the May 12 plan. Slow down. Let silences sit.

Take the three questions in any order they like:
1. What would you most like to be able to do by the end of this training programme?
2. Where in Weeks 1 or 2 did you get stuck — even briefly?
3. What's one thing we could change about how we're running this that would help?

Facilitation tips:
- Ask, then **wait.** Don't fill the silence. Government staff may be cautious about speaking first.
- Go person by person if the group is quiet — invite each by name, gently.
- For Q1, push softly for something concrete: "Build X", "understand Y" beats a tidy general answer.
- For Q2 and Q3, make clear there are no wrong answers — tooling, pacing, format, what you ask of them — all fair game.
- **Take notes.** This is the input that restructures Cycles 2 and 3. Capture it carefully.

If the conversation is good and running long, let it — this is the most valuable part of the hour.

---

## Slide 14 — Thank you
**Time: 1 min**

Close warmly. Thank them again for their honesty.

Two clear next steps:
- A short follow-up email this week summarising what we heard from them.
- No new homework today — just the extra week to finish what they've started.

Final line: small steps, taken together, are the whole point of this. End on time.

---

## After the call — for the mentoring team

- Send the promised follow-up email within a few days, summarising what was heard (Ravi's action).
- Use the Q13 answers to decide how Cycles 2 and 3 are restructured. Weeks 3 and 4 remain in abeyance until then.
- If only one mentee had attended, the plan was to reschedule — note attendance for the team.