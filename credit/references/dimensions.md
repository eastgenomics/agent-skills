# CRediT Dimension Heuristics

Reference for the `/credit` skill. Each dimension has evidence signals and a default rating when evidence is absent. Always err toward Human-led when uncertain.

---

## 1. Conceptualisation

**Key question:** Who defined what the work is for and what question it answers?

**Human-led signals:**
- Human initiated the core research question or task unprompted
- Human defined the scope and goals before Claude responded
- Human identified the specific problem to solve from domain experience

**Collaborative signals:**
- The question emerged through back-and-forth dialogue
- Claude refined or sharpened a vague human prompt into a concrete question

**AI-led signals:**
- Claude proposed the entire research direction with no prior human framing
- Human simply said "do something useful" with no further direction

**Default when uncertain: Human-led** — someone had to start the conversation.

---

## 2. Methodology

**Key question:** Who designed the analytical approach and made the key decisions?

**Human-led signals:**
- Human specified the analytical approach explicitly before Claude began
- Human rejected or substantially redirected Claude's proposed methods
- Human chose between competing approaches based on domain knowledge

**Collaborative signals:**
- Human approved or redirected methods at key decision points
- Human scoped or constrained the approach (e.g. "v2 only", "protein level not genomic", "include INDELs")
- Human accepted Claude's proposals after asking clarifying questions

**AI-led signals:**
- Claude proposed all methods; human accepted without questioning or redirecting
- No methodological decisions were made or discussed explicitly

**Default when uncertain: Collaborative**

---

## 3. Software & Execution

**Key question:** Who wrote the code, ran the queries, and processed the data?

**Human-led signals:**
- Human wrote or substantially modified code
- Human ran scripts themselves
- Human set up the environment and data processing pipeline

**Collaborative signals:**
- Human provided existing code that Claude adapted
- Human ran scripts but Claude wrote them

**AI-led signals:**
- Claude wrote all scripts from scratch
- Claude ran all API queries, processed all data, generated all outputs

**Default when uncertain: AI-led** — in Claude Code sessions, Claude almost always writes and runs the code.

---

## 4. Analysis & Interpretation

**Key question:** Who drew meaning from the results and validated conclusions?

**Human-led signals:**
- Human identified key findings that Claude missed or hadn't highlighted
- Human corrected AI interpretations
- Human brought analytical insight not derivable from the raw outputs
- Human directed follow-up analysis to probe specific questions

**Collaborative signals:**
- Human asked follow-up questions that shaped the interpretation
- Human identified missing context or references that changed the conclusions
- Human approved conclusions after interrogating specific results

**AI-led signals:**
- Claude generated all analysis and interpretation; human accepted conclusions without substantive challenge

**Counterfactual test:** Would the analysis have been substantially different if the human had only said "go"? If no, lean AI-led regardless of how the human used the output afterwards.

**Default when uncertain: Collaborative**

---

## 5. Domain Knowledge

**Key question:** Who supplied the clinical/scientific expertise and identified key references?

**Human-led signals:**
- Human identified specialist references the AI couldn't have prioritised in context (e.g. specific national guidelines)
- Human corrected AI on clinical or scientific matters
- Human defined the clinical use case or patient safety context
- Human knew to probe specific real-world examples that were clinically meaningful
- Human identified a gap in the analysis based on domain expertise

**Collaborative signals:**
- Human provided context that AI used to refine its outputs
- AI provided general knowledge that human validated against their domain experience

**AI-led signals:**
- All domain knowledge came from AI training data; human provided no specialist input

**Default when uncertain: Human-led** — domain knowledge is the dimension most resistant to AI substitution, and the most important for clinical validity.

---

## 6. Documentation

**Key question:** Who produced the prose?

**Human-led signals:**
- Human wrote substantial prose themselves
- Human drafted sections that Claude then edited

**Collaborative signals:**
- Human provided detailed outlines that Claude expanded
- Human wrote key sentences that Claude wove into larger text

**AI-led signals:**
- Claude generated all prose from prompts
- Human's contribution to text was limited to reviewing and approving

**Default when uncertain: AI-led** — in Claude Code sessions, Claude almost always writes all text.

---

## 7. Review & Editing

**Key question:** Who reviewed outputs, directed revisions, and approved content?

**Human-led signals:**
- Human directed multiple rounds of revision with specific instructions
- Human rejected or rewrote sections
- Human approved the final version explicitly
- Human identified errors or gaps that required correction

**Collaborative signals:**
- Human reviewed and gave general feedback; Claude implemented changes

**AI-led signals:**
- Human accepted all output without substantive review or revision requests

**Default when uncertain: Human-led** — if the human has invoked `/credit`, they reviewed enough to know the work is done.

---

## 8. Test Design & Validation

**Key question:** Who designed the tests, set acceptance criteria, and determined fitness for purpose?

This dimension has **two sub-components**. Note both in the evidence:

### 8a. Test design (who decided *what* to test)

**Human-led signals:**
- Human specified which edge cases to probe by name or category
- Human set an explicit acceptance threshold before full execution (e.g. ">95% must pass", "test with a small number first")
- Human directed a pre-run validation protocol before committing to full analysis
- Human identified specific real-world examples to test based on domain knowledge
- Human asked "what happens with X?" type questions that shaped the test suite
- Human identified variant types, edge cases, or scenarios that AI had not considered

**Collaborative signals:**
- Human approved AI-proposed test cases with minor additions
- Human added specific cases during testing after seeing initial results

**AI-led signals:**
- Claude proposed all test cases without human direction
- Acceptance criteria were set by AI alone
- Human accepted all test results without questioning any specific case

### 8b. Test execution & result interpretation (who ran the tests and judged pass/fail)

**Human-led signals:**
- Human ran tests themselves and made pass/fail judgements
- Human interrogated specific unexpected results and directed follow-up

**AI-led signals:**
- Claude ran all tests and reported results; human accepted AI's pass/fail assessment

**Collaborative signals:**
- Human directed specific follow-up tests after seeing initial results
- Human questioned unexpected results and asked Claude to investigate

**Default when uncertain: Collaborative** for 8b; Human-led for 8a in clinical contexts.

### Clinical bioinformatics note

Human-led test design is the essential safeguard in clinical pipelines. AI can propose and run tests, but a clinically accountable human must define:
- What "correct" means for this application
- Which edge cases matter for patient safety
- Whether outputs are fit for clinical purpose

This is the dimension where "human in the loop" has the most direct bearing on patient safety. When assessing this dimension, focus on the test *design* — not the test code. A human who says "test with known hotspots before running the full analysis" and "examine TP53 R213 specifically" is providing clinically essential test design, even if AI writes and runs all the code.

Applies equally to **data analyses**: pre-run spot-checks with known examples, validation against expected results, plausibility checks, and the decision that results are fit for clinical reporting all count as test design/validation activity.
