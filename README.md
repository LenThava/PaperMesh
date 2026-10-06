# PaperMesh — Literature Similarity Map for JabRef

Software Engineering HS 2026 project proposal.

[Project proposal on Google Docs](https://docs.google.com/document/d/1McPw3q44qVgxCBKDS9bsi2TjOvbiyJpUuNvl4MF80sc/edit)

## Repository status

This repository currently contains PaperMesh project documentation. The team has not yet selected or added the JabRef source code.

- [`project`](https://github.com/LenThava/PaperMesh/tree/project): shared project branch and documentation index.
- [`requirements`](https://github.com/LenThava/PaperMesh/tree/requirements/docs/sweng): working branch for the Pflichtenheft and project plan.
- Project documentation is kept in `docs/sweng`.

The documentation structure is prepared. The complete development setup still requires a shared JabRef codebase and access for every team member.

## Group members

- Cian Boeniger
- Len Thava
- Gian Ledergerber
- Hrishi Budema

## Project description

PaperMesh extends JabRef with an interactive, Obsidian-inspired graph view of the managed bibliography. Its goal is to help researchers discover relationships, topic clusters, and potentially overlooked papers within an existing JabRef library.

Each managed paper is displayed as a node. Edges represent an explainable similarity score calculated from bibliographic metadata already stored in JabRef, such as title, abstract, keywords, authors, and venue. Users can inspect why two papers are connected, search the map, select topic clusters, open the corresponding entries in JabRef, and create JabRef groups from selected nodes or clusters.

The extension will integrate directly with JabRef’s bibliography entries, main table, entry editor, groups, and search/selection functionality. The graph will initially be generated from a selected group or selection of papers, rather than requiring users to analyze their entire library at once.

## Main components

- **JabRef integration layer:** reads the currently selected JabRef entries or group, opens/selects entries from the map, and creates new JabRef groups from selected nodes.
- **Metadata and similarity backend:** extracts and normalises titles and keywords from BibEntry objects. It calculates a transparent similarity score and records the shared terms that explain each graph connection.
- **Graph model and layout:** converts similarity results into nodes and edges, limits the map to approximately 100 entries, and keeps only the strongest connections so that the visualisation remains readable.
- **JavaFX graphical interface:** displays the interactive graph with nodes and links. Users can pan, zoom, inspect tooltips, select papers, and navigate to the corresponding JabRef entry.
- **Group-management actions:** lets users select related papers in the map and create or populate a JabRef group directly from that selection.

The main difficulty will be calculating useful similarity scores from incomplete or inconsistent metadata while keeping the graph readable for larger collections. We will address this through transparent similarity explanations, adjustable thresholds, a limit on the number of displayed links per paper.

## Tentative work division

This is a tentative division of the work and may change during the project:

- **Cian Boeniger:** metadata processing and similarity calculation.
- **Len Thava:** the graph interface.
- **Gian Ledergerber:** integration with JabRef.
- **Hrishi Budema:** testing and documentation.

We will help each other where needed.

## Planned schedule

| Week | Plan |
| --- | --- |
| 1 | Analyze the relevant JabRef code, define the data model and similarity formula, create a UI wireframe, and prove that selected entries/groups can be read. |
| 2 | Implement title/keyword normalization and similarity scoring. Write unit tests and produce related-paper pairs in a text-based prototype. |
| 3 | Build the graph model and JavaFX graph prototype with nodes and edges for test data. |
| 4 | Connect the graph to real JabRef entries. Add pan, zoom, tooltips, and opening/selecting entries from the graph. |
| 5 | Add multi-node selection, group creation, readability limits, and handling for missing metadata or isolated papers. |
| 6 | Complete integration tests, usability improvements, documentation, demo data, bug fixes, and presentation preparation. |

## Time estimate

These estimates are tentative:

- Best case: 4–5 weeks.
- Normal case: 6 weeks.
- Worst case: 7–8 weeks.
