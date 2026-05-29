# Skill: LLM Council

## What it is
A structured multi-perspective review that runs a question through three specialist reviewer personas, then synthesizes a unified recommendation. Simulates the llm-council pattern (fan-out → peer review → synthesis) within a single response.

## How to invoke
`/skill council <question or decision>`

Best for: architecture decisions, code design choices, debugging approaches, ambiguous tradeoffs, "should I do X or Y" questions.
Not suited for: factual lookups, deterministic tasks with a clear correct answer.

## Council roles

| Role | Focus |
|---|---|
| Skeptic | Risks, edge cases, hidden assumptions, what can go wrong |
| Pragmatist | Implementation cost, complexity, "good enough" threshold |
| Architect | First principles, long-term design quality, what actually matters |
| Chairman | Reads all three, resolves conflicts, delivers the final call |

## How to run the council

When this skill is loaded, run the question through all four roles in sequence:

1. **Skeptic** — 2–3 sentences. Surface the strongest risk or flaw in the default approach.
2. **Pragmatist** — 2–3 sentences. Assess effort vs. payoff. Flag if the question is over-engineered.
3. **Architect** — 2–3 sentences. Evaluate the design from first principles or long-term maintainability.
4. **Chairman** — 3–4 sentences. Weigh the three inputs and deliver a clear recommendation with reasoning. If reviewers agree, confirm and add nuance. If they conflict, name the tradeoff and pick a side.

## Output format

```
[Skeptic] ...
[Pragmatist] ...
[Architect] ...
[Chairman] ...
```

Plain text, no markdown headers. Each reviewer label in square brackets. Telegram-friendly.

## Constraints
- Keep total response under 350 words
- If token budget is tight, cut reviewer verbosity before Chairman
- Chairman must always give a clear recommendation — no "it depends" without a tiebreaker
- Reviewers assess independently; only Chairman synthesises across them
