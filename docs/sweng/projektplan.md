# Projektplan

PaperMesh - working revision, 26 October 2026.

Team: Cian Boeniger, Dehlen Thavarajah, Gian Ledergeber and Hrishi Budema.

Supervisor: Giovanni.

This is a rough plan, updated after the feedback on 13 and 22 October. We still need to agree the work division and check the estimates together. The hours are person-hours, not calendar days. Start dates below are planning assumptions, not evidence that tasks have been completed.

## Tasks

| Task | Rough effort | Who | Earliest start / depends on |
| --- | --- | --- | --- |
| Agree the shared course codebase and try reading entries/groups | 6–10 h | Gian, with the team | From 26 Oct; before integration |
| Check the window sketch, comparison rule and graph limit on sample data | 2–4 h | Cian and Dehlen, with the team | From 26 Oct; before finalising requirements |
| Revise the Pflichtenheft and plan using Giovanni's feedback | 4–6 h | Dehlen, with the team | Current revision 26 Oct; remaining team decisions before 27 Oct |
| Prepare title/keyword data, similarity and explanations | 8–12 h | Cian | After entry access and the comparison rule are agreed |
| Build the JavaFX graph, controls and pan/zoom | 12–20 h | Dehlen | After agreeing the input and graph data |
| Add entry navigation and new group creation | 8–12 h | Gian | After the graph prototype and source-library access |
| Add graph search (SHOULD, if time allows) | 2–4 h | Dehlen | After the core graph interactions work |
| Write up the design and prototype | 6–10 h | Hrishi, with the team | Once the first prototype is available |
| Write the test plan, automate the 100/101-entry model check and run acceptance cases | 8–12 h | Hrishi | Sample test data can be prepared now; executable tests after the model/integration exist |
| Fix problems and check graph usability on our computers | 8–12 h | Everyone | After the first tests |
| Prepare user docs, demo and presentation | 6–10 h | Hrishi, with the team | When the main functions work |
| Final review and project submission | 2–4 h | Dehlen, with the team | After testing and documentation |

We will help each other where needed. Search stays optional; it should not delay the required graph, navigation and group features.

## Course dates

- Pflichtenheft and plan: first PR created 9 Oct, feedback 13 and 22 Oct, final version 27 Oct.
- Design/prototype: draft 30 Oct, final 10 Nov.
- Test plan: draft 13 Nov, final 24 Nov.
- Presentation: 1 Dec. Complete project: 15 Dec.

OPEN QUESTION: Does this division fit everyone's availability, and do the estimates make sense once we have tried the proposed integration?

## Current review

The [Pflichtenheft](pflichtenheft.md) and plan are being revised in [PR #1](https://github.com/LenThava/PaperMesh/pull/1), from requirements to project. The initial submission stays in the PR history. The 26 October revision clarifies definitions and separates requirements and acceptance tests in response to the 22 October comments. Tests, including the automated size-limit check, are specified but not yet implemented or executed.

[Course instructions](https://patrickschniderunibas.github.io/software-engineering/project/requirements) · [Course dates](https://patrickschniderunibas.github.io/software-engineering/project/project-summary)
