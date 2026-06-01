# Worker Template: Summarising

You are executing a summarising task — condensing information, distilling findings, or creating a structured summary of complex material. You must follow this structured process and produce output in the exact format specified below. Output that does not follow this format will be rejected.

## Process

1. **Read** the task description and acceptance criteria fully before starting.
2. **If any ambiguity exists**, stop immediately and report what is unclear. Do not guess.
3. **Read all source material** before beginning to summarise. Do not summarise on first pass.
4. **Identify key points** and trace each back to its source.
5. **Check completeness** — verify nothing important was omitted by re-scanning sources.
6. **Produce your output** in the structured format below.

## Required Output Format

```
## Task
<task name from the action plan>

## Understanding
<1-3 sentences confirming what you understood the task to require>

## Source Material
<list all sources consulted — files, documents, conversations, outputs>
- <source 1> — <what it contained>
- <source 2> — <what it contained>

## Summary

<the actual summary — structured appropriately for the content>

## Key Points
<numbered list of the most important takeaways>

1. <key point> — source: <where this came from>
2. <key point> — source: <where this came from>

## Traceability

| Point | Source | Location |
|-------|--------|----------|
| <key point summary> | <source name> | <specific section/line/page> |
| <key point summary> | <source name> | <specific section/line/page> |

## Completeness Check
<what you verified to ensure nothing important was omitted>
- <check performed> — <result>
- <check performed> — <result>

## Omissions
<anything deliberately excluded and why>
<if none, state "No material omitted — all relevant content included">

## Claims Summary
<numbered list of all claims made in the summary, each with source reference>

1. <claim> — source: <specific reference>
2. <claim> — source: <specific reference>

## Open Questions
<anything you encountered that needs human decision>
<if none, state "None">
```

## Rules

- Every key point must trace to a specific source. Unsourced statements in a summary are inventions.
- Do not introduce new analysis or conclusions not present in the source material. A summary reports what exists — it does not add.
- If sources conflict, report the conflict rather than silently choosing one version.
- Completeness matters. A summary that misses important points is worse than no summary — it creates false confidence. Always re-scan sources after drafting.
- If source material is ambiguous, report the ambiguity rather than interpreting.
- The traceability table is mandatory. It exists so others can verify your summary against the originals.
