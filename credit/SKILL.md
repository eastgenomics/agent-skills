---
name: credit
description: Produce an informal human-AI contribution matrix for a coding session to help the user reflect on how they are working with AI. Use after completing a coding task, analysis, or document.
allowed-tools: Read, Glob, Grep, Bash, AskUserQuestion, mcp__atlassian__getConfluencePage, mcp__atlassian__updateConfluencePage
---

The user has invoked `/credit` to generate a contribution matrix for the work just completed.

`$ARGUMENTS` may specify an output target:
- `markdown` — print a formatted table (default)
- `confluence` — add as a section to the current project's Confluence page
- `page <URL or page ID>` — add to a specified Confluence page

---

## Your task

Produce a human-AI contribution matrix using the eight adapted CRediT dimensions defined in `references/dimensions.md`. Assess each dimension based on evidence from the session artifacts and conversation, ask clarifying questions only where genuinely uncertain, then output the matrix.

---

## Step 1 — Gather artifacts

Run these in parallel:

1. **Git log**: `git log --oneline` in the working directory — reveals what was created and committed
2. **Repository files**: use Glob to find .py, .md, .tsv, .sh files; read scripts briefly to understand scope of execution
3. **Confluence page**: if a page URL or ID is known from the session, fetch it with `getConfluencePage` to understand document scope

Review the conversation in context, focusing on key moments: where the human framed or scoped the problem, redirected or rejected an approach, applied domain knowledge, or approved outputs. Note specific examples to carry forward as evidence in Step 2, loosely mapped to the dimensions they bear on (e.g. a scope decision → Methodology; a domain correction → Domain Knowledge; acceptance of all outputs without challenge → Review & Editing).

---

## Step 2 — Assess each dimension

Consult `references/dimensions.md` for the full heuristics for each of the eight dimensions:

1. Conceptualisation
2. Methodology
3. Software & Execution
4. Analysis & Interpretation
5. Domain Knowledge
6. Documentation
7. Review & Editing
8. Test Design & Validation

For each dimension, assign one of three ratings:
- **Human-led** 🧑
- **Collaborative** 🤝
- **AI-led** 🤖

Write a one-sentence evidence note for each. Flag any dimension where you are genuinely uncertain (evidence could support either of two ratings).

**Critical defaults — when in doubt:**
- Test Design & Validation should reflect who *designed* the tests, not who *ran* them

---

## Step 3 — Clarifying questions (only if needed)

If any dimensions are flagged as uncertain, use `AskUserQuestion` to ask **at most three** targeted questions. Choose the dimensions where human input would most change the rating.

Example questions:
- "Did you bring prior domain expertise to the interpretation of results, or did the conclusions emerge primarily from the analysis outputs?"
- "Were the test cases (edge cases, specific variants to examine) chosen by you, or did you accept the ones Claude proposed?"
- "Were any key methodological decisions made before this session began, or did the approach emerge during our conversation?"

Skip this step entirely if all dimensions can be assessed confidently from the artifacts and conversation.

---

## Step 4 — Output the matrix

### If output target is `markdown` (default):

Print the matrix as a formatted table:

```
## Human-AI Contribution Matrix

| Dimension | Rating | Evidence |
|---|---|---|
| Conceptualisation | 🧑 Human-led | ... |
| Methodology | 🤝 Collaborative | ... |
| Software & Execution | 🤖 AI-led | ... |
| Analysis & Interpretation | ... | ... |
| Domain Knowledge | ... | ... |
| Documentation | ... | ... |
| Review & Editing | ... | ... |
| Test Design & Validation | ... | ... |
```

### If output target is `confluence` or `page <ID>`:

1. Identify the Confluence page: use the page ID from `$ARGUMENTS`, or identify it from memory/context if not specified
2. Fetch the current page with `getConfluencePage` to get the current version number and full body
3. Add a new H2 section `🤝 Contribution matrix` to the page body, positioned **after** the Overall Outcome section and **before** the Useful linked documentation section
4. The section should contain:
   - The introductory sentence (see below)
   - The contribution table in ADF format
5. Update the page with `updateConfluencePage`, incrementing the version

**ADF table structure for the matrix:** Use a standard ADF table with three columns (Dimension, Rating, Evidence). The rating cell should contain the emoji and text (e.g., "🧑 Human-led"). See the `create-dev-doc` skill for ADF table patterns.

**Introductory sentence** (use exactly this wording):
> Human-AI contribution profile for this work session, assessed (by the AI, therefore should be reviewed) using an adapted CRediT taxonomy. Intended as feedback to help reflect on how you are working with AI.

Do not add any additional caveat or disclaimer after the table.

---

## Self-assessment rules

1. Do not add a separate caveat or disclaimer after the matrix — the introductory sentence already signals that review is required
2. The Test Design & Validation dimension should reflect who *designed* the tests and set the fitness-for-purpose criteria — not who wrote the test code or executed the runs
3. If the human set an explicit acceptance threshold, named specific cases to investigate, or directed the testing protocol before execution, that is strong evidence for Human-led on dimension 8
