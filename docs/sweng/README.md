# PaperMesh project documentation

Status: the first Pflichtenheft and plan were submitted in [PR #1](https://github.com/LenThava/PaperMesh/pull/1) on 9 October. The 26 October revision addresses Giovanni's 22 October feedback with clearer definitions, smaller interaction requirements and separate acceptance tests. The selected development codebase is the private [LenThava/jabref_hs26](https://github.com/LenThava/jabref_hs26) fork, initially at main revision `c705647a4e2c37ae9f608cdd959d98c88a0fde44`. Final team agreement on the interface, calculation and limits is still open.

## Workflow

- `main` contains the project overview.
- `project` is the documentation review target in this repository. Implementation will use the selected private course fork; its project branches have not been prepared as part of this documentation update.
- `requirements` was created from `project` and contains the working Pflichtenheft and project plan.
- Review the requirements documents through a pull request from `requirements` into `project`, with the assigned supervisor as reviewer.

## Working documents

These links point to the drafts on the `requirements` branch:

- [Pflichtenheft](https://github.com/LenThava/PaperMesh/blob/requirements/docs/sweng/pflichtenheft.md)
- [Project plan](https://github.com/LenThava/PaperMesh/blob/requirements/docs/sweng/projektplan.md)
- [Project proposal](https://docs.google.com/document/d/1McPw3q44qVgxCBKDS9bsi2TjOvbiyJpUuNvl4MF80sc/edit)

## Decisions and open points

- Codebase decision: use the private LenThava/jabref_hs26 fork at the initial baseline recorded above.
- Supervisor: Giovanni ([Giovvvvva](https://github.com/Giovvvvva)).
- OPEN QUESTION: Can every group member access and build the selected private course fork?
- Agree the proposed interface, comparison rule, work division and estimates before the final Pflichtenheft version on 27 October.

This public repository contains documentation, not private JabRef source. Choosing the codebase does not complete team-access, build or integration setup.

## Course instructions

- [Group repository setup](https://patrickschniderunibas.github.io/software-engineering/project/group-repository)
- [Pflichtenheft and project plan](https://patrickschniderunibas.github.io/software-engineering/project/requirements)
