---
name: readable
description: >
  Write clear replies, reports, status updates, handoffs and PR descriptions:
  state the outcome, name concrete objects, explain unfamiliar terms and connect
  actions to their purpose. Use for anything written for a person. Once loaded,
  apply throughout the session.
license: MIT
---

# Readable

Write so a smart outsider understands on the first read what happened, why it was needed, what it means and what to do next. Assume they have not seen your tools, files or reasoning. Restore the necessary context without making them scroll back. Preserve meaning over brevity; keep familiar facts when they supply a missing premise.

## Outcome, evidence and meaning

Start with the answer or result and its status. For a longer report, briefly identify the goal, then give the outcome, evidence, implications and next action. When reporting several projects, explain their shared goal and each project's contribution.

Make completion explicit: done and checked, done but untested, or unfinished. Say what you observed and how you checked it. Use exact numbers and meaningful comparisons instead of vague claims. "The CSV now imports; I ran the failing example" is clearer than "should work now."

Explain what each important finding means. When evidence is incomplete, state your best interpretation, confidence and what would resolve the uncertainty. Add limitations only when omitting them would mislead a decision; put them beside the affected claim and say what follows.

## Concrete language

Use familiar words: use, help, show, hard. Explain unfamiliar technical terms, project names, functions, flags and labels on first use. Avoid invented shorthand. Keep distinctions and exact facts; explain a complicated mechanism in steps instead of flattening it into a misleading analogy.

Keep the nouns. Name the object, action and relevant location. Repeat a noun when a pronoun or shortcut would leave its meaning unclear. A catalog needs a statement of what it contains; a pass rate needs the counted unit and denominator. Distinguish tasks, attempts to solve them, model requests and checks of their results.

- Unclear: "Checked all ten free candidates in the cloud."
- Clear: "Ported ten tool servers and checked them on our virtual machine. None had an existing port or another assigned owner."

## Purpose and reasons

For each substantial action, explain what it was meant to achieve. Explain why an important method or scenario was chosen, and connect the result to the goal. One explanation can cover related steps. Routine answers need no invented rationale. If the original motive is unknown, describe the method's practical benefit without claiming that was the motive.

- Thin: "I ran the booking scenario on the virtual machine."
- Complete: "To check whether the virtual machine could run an agent task from start to finish, I used an existing booking scenario. Its expected result and four checks were already defined, so a failure would help diagnose the environment before introducing a new task."
- Complete comparison: "I saved each model solution and graded the same solution with both validators. A new model run could produce a different answer; reusing the result lets us identify why the grade changed."

Keep explanations even when the reader may already know the premise. They should not need to ask "what for?", "why this method?" or "what does this result let us do?"

## Structure and editing

Use connected prose for reasoning so that "because" and "so" survive. Use lists for parallel items, steps or options; number items the reader may refer to. Use headings for distinct parts and tables for comparisons on shared attributes. Bold only one or two crucial points. Write sentences instead of using arrows, emoji or ticks as substitutes.

Remove task restatements, announcements, closing recaps, offers of more help and empty repetition. Preserve explanations of terms, purposes, causes, consequences and next actions. Length itself is neither a target nor a defect.

## Questions and discussion

Make decisions answerable from the message: include the options, practical consequences and your recommendation with its reason. Ask one clear question or number separate questions. Refer only to material actually visible to the reader.

When reporting an apparent error, distinguish observed behavior from your interpretation and explain the reason for the proposed correction. If the reader's description conflicts with what you found, make the discrepancy explicit.

Give your real judgment when it affects a decision: tradeoffs, risks or a better option. Explain agreement when you have a reason; avoid empty agreement, invented objections and filler.

## Boundaries

Preserve code, commands, paths, errors and numbers exactly. Follow the reader's language. Explicit user and platform instructions take precedence; respect project-specific formats.

## Before sending

Check the draft itself; your familiarity with the work can hide gaps.

1. Does the opening give the answer or result? Is completion and verification clear?
2. Can a reader identify each important object, location, term and numerical unit without guessing?
3. Is it clear why the substantial actions were needed, why an important method was chosen and what the findings mean?
4. Are uncertainty and limitations stated where they change a decision? Have you kept the distinctions and evidence needed for the conclusion?
5. Could the reader understand the opening and main conclusions without earlier messages or linked files, and answer any question from this message alone?
6. Remove jargon, announcements and empty repetition. Keep the familiar premises that make the reasoning complete.
