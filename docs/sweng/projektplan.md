# Projektplan

PaperMesh - first draft, 9 October 2026.

Team: Cian Boeniger, Dehlen Thavarajah, Gian Ledergeber and Hrishi Budema.

Supervisor: Giovanni.

This is a tentative plan. The tasks, estimates and work division may change after our discussion with Giovanni and a closer look at the JabRef code.

## Tasks

The estimates are rough person-hours, not calendar days. Start dates are proposed earliest starts; dependencies and our availability may shift them.

| Task | Estimated effort | Responsible person | Earliest start / dependency |
| --- | --- | --- | --- |
| Agree the JabRef codebase and try reading selected entries | 6–10 h | Gian | From 12 Oct, after the team chooses a codebase |
| Revise the Pflichtenheft and plan | 2–4 h | Dehlen | After feedback on 13 Oct |
| Normalise title/keyword data and build the similarity backend | 8–12 h | Cian | After entry access is working |
| Build the graph model and JavaFX interface | 12–20 h | Dehlen | After agreeing the data passed to the graph |
| Add entry navigation and group creation | 8–12 h | Gian | After the graph prototype |
| Prepare design/prototype documentation | 6–10 h | Hrishi, with the team | Once the first prototype is available |
| Write the test plan and test the main functions | 8–12 h | Hrishi | Alongside implementation, then after integration |
| Fix issues and improve usability | 8–12 h | Whole team | After the first integration tests |
| Prepare user documentation, demo and presentation | 6–10 h | Hrishi, with the team | Once the core functions are working |
| Final review and complete project submission | 2–4 h | Dehlen, with the team | After testing and documentation |

## Tentative work division

- Cian: metadata and similarity.
- Dehlen: graph interface.
- Gian: JabRef integration and groups.
- Hrishi: testing and documentation.

We will help each other where needed. This is only a starting point for the team's own planning.

## Course milestones

- Pflichtenheft and plan: first submission 9 Oct, discussion 13 Oct, final version 27 Oct.
- Design/prototype: first submission 30 Oct, final version 10 Nov.
- Test plan: first submission 13 Nov, final version 24 Nov.
- Presentation: 1 Dec.
- Complete project submission: 15 Dec.

## Open questions

- OPEN QUESTION: Does this division fit everyone's availability?
- OPEN QUESTION: Are the effort estimates realistic after looking at the code?
- OPEN QUESTION: Which shared JabRef codebase will we use?

## Submission workflow

The [Pflichtenheft](pflichtenheft.md) and this plan are submitted together through one group PR from requirements to project, with Giovanni as reviewer.

[Course instructions](https://patrickschniderunibas.github.io/software-engineering/project/requirements) · [Course timetable](https://patrickschniderunibas.github.io/software-engineering/project/project-summary)
