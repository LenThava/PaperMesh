# PaperMesh project plan

Initial draft, 9 October 2026. This is a tentative plan, not a fixed commitment. We will review it together and with Giovanni after the first feedback.

Team: Cian Boeniger, Len Thava, Gian Ledergeber and Hrishi Budema.

Supervisor: Giovanni.

## Tasks

The estimates below are person-hours of effort, not continuous calendar time. Dates are proposed earliest starts and depend on the previous tasks and our availability. The responsible person coordinates the task; we will help each other where needed.

| ID | Task | Estimated effort | Responsible person | Earliest start / dependency |
| --- | --- | --- | --- | --- |
| T01 | Agree the shared JabRef codebase and complete team access | 4 h | Gian | 12 Oct; team decision and Gian's GitHub username |
| T02 | Review and update the Pflichtenheft and plan after feedback | 4 h | Len | 13 Oct; supervisor discussion, final version by 27 Oct |
| T03 | Read selected JabRef entries/groups and identify integration points | 6 h | Gian | 13 Oct; T01 |
| T04 | Implement metadata normalisation, similarity scoring and unit tests | 12 h | Cian | 20 Oct; T03 and initial scope agreement |
| T05 | Build a small graph model and JavaFX prototype | 10 h | Len | 20 Oct; T03, agreed input/output model with Cian |
| T06 | Document the technical design and prototype | 6 h | Hrishi, with team input | 27 Oct; T04/T05 prototype, first version by 30 Oct |
| T07 | Connect real entries; add pan, zoom and link explanations | 12 h | Len | 3 Nov; T04/T05 |
| T08 | Add entry navigation, node selection and group creation | 10 h | Gian | 10 Nov; T07 |
| T09 | Add threshold/link limits and missing-metadata handling | 6 h | Cian, with Len | 10 Nov; T04/T07 |
| T10 | Write the test plan and add automated checks | 8 h | Hrishi | 10 Nov; T04/T05, first test-plan version by 13 Nov |
| T11 | Run integration and acceptance tests | 10 h | Hrishi, with team support | 17 Nov; T08/T09/T10 |
| T12 | Fix issues and improve usability | 6 h | Len, with team support | 24 Nov; T11 |
| T13 | Finish user documentation, demo data and presentation | 8 h | Hrishi, with all team members | 24 Nov; working core features, presentation by 1 Dec |
| T14 | Final review and prepare the complete project submission | 4 h | Len, with all team members | 2 Dec; T12/T13, final submission by 15 Dec |

These rough estimates total 106 person-hours across the team. They are a starting point and may change once we understand the JabRef code better. If time is short, we will prioritise the core features and discuss scope changes with Giovanni rather than silently drop requirements.

## Tentative work division

- Cian: metadata, similarity calculation and related tests.
- Len: graph interface and coordination of the requirements draft.
- Gian: integration with JabRef, entry navigation and groups.
- Hrishi: testing, documentation and demo preparation.

Gian is a full team member. His repository access is still pending; this does not change his role in the plan. The team should confirm this division and the effort estimates together.

## Course milestones

- 9 Oct: first Pflichtenheft and project-plan submission.
- 13 Oct: discussion with Giovanni.
- 27 Oct: final revised Pflichtenheft and project plan.
- 30 Oct: first design/prototype submission; discussion on 3 Nov and final version on 10 Nov.
- 13 Nov: first test-plan submission; discussion on 17 Nov and final version on 24 Nov.
- 1 Dec: presentation.
- 15 Dec: complete project submission.

## Open questions

- OPEN QUESTION: Does the work division fit everyone's availability?
- OPEN QUESTION: Is the proposed core scope and effort realistic after the first code exploration?
- OPEN QUESTION: Which JabRef codebase will be shared, and when can Gian receive repository access?

## Submission workflow

The [Pflichtenheft](pflichtenheft.md) and this plan belong in `docs/sweng` on `requirements`. The group submits one pull request from `requirements` into `project`, with Giovanni as reviewer. The PR stays open for feedback; this initial draft is not the final version.

[Course requirements](https://patrickschniderunibas.github.io/software-engineering/project/requirements) · [Course timetable](https://patrickschniderunibas.github.io/software-engineering/project/project-summary)
