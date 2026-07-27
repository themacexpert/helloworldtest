# Writing Good Documentation

Documentation fails for boring reasons. It is written for the wrong reader, it
explains what the code already says, or it drifts out of date and quietly starts
lying. This page is a practical guide to avoiding those failures.

## Start with the reader, not the feature

Before writing a word, decide who is reading and what they are trying to finish.
The same feature needs very different documents depending on the answer.

| Reader's goal | What they need | What kills it |
| --- | --- | --- |
| Get something working now | A short, copy-pasteable path | Background, caveats, alternatives |
| Understand how it works | Concepts, diagrams, trade-offs | Step-by-step instructions |
| Look up a detail | Exhaustive, scannable reference | Narrative prose |
| Fix something broken | Symptoms mapped to causes | Cheerful happy-path examples |

Mixing these in one document is the single most common documentation mistake. A
tutorial that keeps pausing to explain architecture loses the person trying to
ship, and a reference page padded with encouragement wastes the time of someone
scanning for a parameter name.

## The four kinds of pages

- **Tutorial** — a guided lesson that takes a beginner from nothing to a working
  result. It is allowed to be opinionated and to skip alternatives. Success is
  measured by whether the reader finishes.
- **How-to guide** — a recipe for one specific task, aimed at someone who
  already understands the basics. Success is measured by whether the task gets
  done.
- **Reference** — a complete, accurate description of the surface area:
  functions, flags, fields, endpoints. Success is measured by whether the answer
  is findable and correct.
- **Explanation** — the why. Design decisions, constraints, history, rejected
  alternatives. Success is measured by whether the reader can now make good
  decisions on their own.

When a page feels hard to write, it is usually because it is secretly two of
these fighting each other. Split it.

## Write the first paragraph last

The opening paragraph should tell the reader what the page is and whether they
are in the right place. That is hard to know before you have written the body,
so write it at the end and put it at the top.

A good opener answers three questions in about three sentences: what is this,
who is it for, and what will you have when you are done.

## Structure for scanning

Almost nobody reads documentation start to finish. They scan for the heading
that matches their problem, read that section, and leave.

- Use headings that describe the reader's problem, not your system's
  architecture. "Handling expired tokens" beats "The AuthRefresh subsystem."
- Front-load each paragraph with its conclusion. The first sentence should be
  the one worth reading if they read nothing else.
- Keep procedures as numbered lists and keep each step to a single action. If a
  step has an "and" in it, it is probably two steps.
- Put the code sample above the explanation. Readers copy first and read only if
  it fails.

## Make examples real and complete

An example that cannot be run is a liability. It looks authoritative, and then
it wastes an hour.

Good examples share a few properties. They run as written, with no elided
imports or invented variables. They use realistic values instead of `foo` and
`bar`, because realistic values teach the shape of the data. They show the
expected output, so the reader can tell whether it worked. And they are short
enough to read in one screen.

If an example is too long to include in full, link to a runnable file in the
repository rather than trimming it into something that no longer works.

## Say the unhappy things

The most valuable paragraphs in any document are usually the ones describing
what goes wrong. Rate limits, size caps, error codes, race conditions,
migrations that cannot be reversed. Readers hit these anyway; the only question
is whether they hit them in your docs or in production.

Be specific about limits. "This may be slow for large inputs" tells the reader
nothing. "Requests above roughly 10,000 rows time out at the 30 second gateway
limit" tells them exactly when to change approach.

## Keep it honest over time

Documentation rots faster than code because nothing fails when it goes stale.
A few habits slow the decay:

- Treat docs changes as part of the change that caused them, in the same commit
  or the same pull request. Documentation written a week later is documentation
  written from memory.
- Prefer linking to a single source of truth over restating it. Every duplicated
  fact is a future contradiction.
- Delete aggressively. A page that is wrong is worse than a page that does not
  exist, because the reader trusts it.
- Date anything that is inherently a snapshot, such as benchmark numbers or
  supported version lists, so readers can judge staleness themselves.

## A short checklist

Before publishing, read the page once as the intended reader:

- [ ] Can the reader tell within ten seconds whether this page is for them?
- [ ] Does every example run exactly as written?
- [ ] Is there a single, obvious next step at the end?
- [ ] Have the common failure modes been named, with what to do about each?
- [ ] Is anything here restated from somewhere else that could be linked instead?
- [ ] Would this still be true after the next planned release?
