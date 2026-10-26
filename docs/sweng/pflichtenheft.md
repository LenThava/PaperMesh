# Pflichtenheft

PaperMesh - Literature Similarity Map for JabRef

Working draft, revised 26 October 2026 after Giovanni's feedback of 22 October. This document specifies the currently planned behaviour, not implemented or approved features. The codebase has been selected; the interface, similarity calculation and limits still need team confirmation before the final version on 27 October.

Team: Cian Boeniger, Dehlen Thavarajah, Gian Ledergeber and Hrishi Budema.

Supervisor: Giovanni.

## 1. Introduction

### 1.1 Purpose

This document describes what PaperMesh should do, how users would work with it, and how we will check whether it works. It gives our team and Giovanni a common starting point for implementation and review. It is not the technical design.

### 1.2 Scope and goals

PaperMesh will add a graph view to JabRef so users can explore related papers in an existing library and put selected papers into groups. Similarity will use titles and keywords already stored in JabRef.

The described version does not require an external API or online account. OpenAlex, finding new papers online and automatic topic clustering are outside this scope. Search within the graph is a SHOULD feature, not part of the required core.

### 1.3 Definitions

| Term | Meaning |
| --- | --- |
| Entry | One bibliographic record in a JabRef library, not necessarily a downloaded paper or PDF. |
| Selected entries | Rows explicitly highlighted by the user in JabRef's main entry table. |
| Source library | The JabRef library from which the current graph was generated. It remains the target for navigation and new groups even if another library becomes active. |
| Test library | A separate library containing the predefined sample entries pmA–pmD below. Each acceptance test uses a fresh copy unless stated otherwise. |
| JabRef group | A named collection of entries managed in JabRef. Group input means the members of the group selected in JabRef's groups sidebar, not just highlighted table rows. |
| Node | One entry displayed in the graph, labelled with its citation key and title where available. |
| Link | A displayed connection between two entries whose similarity meets the chosen rule. |
| Term | A word or number taken from an entry's title or keywords for comparison. |
| MUST / SHOULD | MUST is intended core behaviour; SHOULD is a desired, optional feature. The proposals in this draft still need team agreement. |

### 1.4 Referenced documents

- [Project proposal](https://docs.google.com/document/d/1McPw3q44qVgxCBKDS9bsi2TjOvbiyJpUuNvl4MF80sc/edit)
- [Course instructions and template](https://patrickschniderunibas.github.io/software-engineering/project/requirements)
- [Course JabRef/Java setup](https://patrickschniderunibas.github.io/software-engineering/exercises/jabref-setup)
- [Project plan](projektplan.md)
- [Giovanni's feedback, 13 October](https://github.com/LenThava/PaperMesh/pull/1#issuecomment-6146387203)
- [Giovanni's review, 22 October](https://github.com/LenThava/PaperMesh/pull/1#pullrequestreview-5614301736)

### 1.5 Overview

Section 2 describes how PaperMesh fits into JabRef and the proposed interface. Section 3 lists the numbered requirements. Section 4 gives the corresponding acceptance tests. Appendix A describes the user workflows and their exceptions. The remaining decisions are listed at the end.

## 2. General description

### 2.1 Integration

We propose adding a **Tools → PaperMesh** action to JabRef. It opens a separate, non-modal JavaFX window in the same application, so the main table and entry editor remain usable.

When the user clicks Generate, PaperMesh reads the chosen input from the active JabRef library. The resulting graph remembers that source library. Opening a node returns to its original entry in JabRef; creating a group uses JabRef's existing group management in the same library. Entries are referenced, not copied into a second bibliography.

The graph is a snapshot. Changes in JabRef are picked up by generating it again, rather than updating the graph live.

### 2.2 Functions and proposed interface

The window has an input selector with **Selected entries** and **Selected group**. Group input takes all members, even if only some visible rows are highlighted.

We propose five main buttons: **Generate**, **Fit view**, **Open entry**, **Create group** and **Close**. The centre contains the graph; a details area shows the selected entry or an inspected link. Open entry is enabled for exactly one selected node, and Create group for at least one.

A rough layout, not a finished design:

```text
PaperMesh - [source library]
Input: [Selected entries / Selected group]  [Generate] [Fit view] [Close]
Search (SHOULD): [________________]
+--------------------------------+-----------------------------+
| Graph: nodes and links          | Entry / link details        |
| Drag background to pan         | Shared terms and score      |
| Mouse wheel to zoom            |                             |
+--------------------------------+-----------------------------+
Selected: 0                     [Open entry] [Create group]
```

Clicking a node selects it; Ctrl-click or Cmd-click toggles additional nodes. Clicking empty background clears the selection. Clicking a link shows its explanation without changing the node selection. Create group opens a small name dialog with Create and Cancel.

### 2.3 Users

Students and researchers using JabRef are the main users. They should only need normal JabRef experience, not programming knowledge. Our team builds the extension, Giovanni reviews it, and JabRef maintainers may later review the changes.

### 2.4 Constraints

We will develop PaperMesh in the private course fork [LenThava/jabref_hs26](https://github.com/LenThava/jabref_hs26), selected on 26 October 2026. The initial baseline is its main branch at `c705647a4e2c37ae9f608cdd959d98c88a0fde44`, which matched the course upstream's main branch when checked on that date.

We will use JavaFX and Java 21. This course snapshot is based on JabRef 5.15; its development build displays version `100.0.0`. The public PaperMesh repository remains a documentation/review record. Private course source will not be copied into it.

The aim is to run on a computer that already runs the course JabRef version, without a dedicated graphics card or extra hardware. We propose a limit of 100 input entries to keep the first version manageable. We have not measured performance yet and are not promising zero extra CPU or memory use.

### 2.5 Assumptions and dependencies

PaperMesh depends on access to JabRef's entry selection, group membership and entry editor. The codebase is selected, but team access and a reproducible build of the shared baseline still need checking before integration. There is no dependency on an external API.

## 3. Requirements

### Graph input and opening (UC1)

- /F10/ With a library active, JabRef MUST offer Tools → PaperMesh, opening the separate non-modal graph window with the input selector and five buttons described in 2.2.
- /F11/ Generate MUST use either the explicitly highlighted table entries or all members JabRef associates with the selected group, according to the input selector. A table selection MUST NOT override group input.
- /F12/ The graph MUST contain exactly one node per input entry and retain the source-library association. Generate again MUST replace the graph with the newly read input and clear its node selection.
- /F13/ Empty input or input above the proposed 100-entry limit MUST show a message and leave any previous graph unchanged. It MUST NOT silently drop entries.

### Similarity and explanations (UC2)

F21 defines the calculation used by this working specification and its tests. It is not a selectable alternative or an extra feature. The final team decision on this calculation is still listed under Open questions.

- /F20/ Comparison MUST use only the title and keyword terms. Missing fields contribute no terms; repeated terms count once.
- /F21/ For this draft, PaperMesh MUST lowercase both fields, split at characters other than letters or numbers, and remove these common English words: `a, an, and, as, at, by, for, from, in, is, it, of, on, or, the, to, with`. Compare the remaining term sets using shared terms divided by the size of their union (Jaccard similarity). If either set is empty, the score is 0. A link is drawn for a score of at least 0.25. No additional per-node link filter is specified in this revision.
- /F22/ Clicking a link MUST show the two entries, their shared terms in alphabetical order and the score rounded to two decimal places in the details area. The displayed explanation MUST come from the compared metadata.

### Entry navigation (UC3)

- /F30/ Selecting a node and clicking Open entry MUST activate its source library, select the corresponding entry in JabRef's main table and open its entry editor.
- /F31/ Navigation MUST identify the actual entry, not look it up by title alone. If the source library is closed or the entry was removed, it MUST show a message instead of opening another entry.

### Group creation (UC4)

- /F40/ Create group MUST request a name and create a new static group at the root of the source library, containing exactly the selected nodes' entries.
- /F41/ The name MUST be trimmed of leading/trailing whitespace. Blank names, names already used in that library (ignoring case), a closed source library or removed selected entries MUST prevent creation and show a message. Cancel MUST create no group. There MUST be no partial group on failure.
- /F42/ The source library and selected entries MUST be checked before opening the name dialog and again on Create. If either check fails, PaperMesh MUST show a message without creating a group.

### Safe exploration (UC1–UC5)

- /F50/ Generating, exploring and navigating the graph MUST NOT change bibliographic fields such as title, keywords, author or year. Group creation may change group structure and membership only. Entries with no usable terms MUST stay visible without links.
- /F60/ The graph window MUST provide the following exploration controls. The sub-requirements separate the individual actions:
  - /F60.1/ Dragging empty background MUST pan the graph without changing the node selection.
  - /F60.2/ Scrolling the mouse wheel up/down MUST zoom the graph in/out.
  - /F60.3/ Fit view MUST place every displayed node within the graph canvas.
  - /F60.4/ Clicking a node without a modifier MUST select only that node.
  - /F60.5/ Ctrl-click or Cmd-click MUST toggle that node's selection while keeping the other selected nodes.
  - /F60.6/ Clicking empty background without dragging MUST clear the node selection.
  - /F60.7/ Open entry MUST be enabled for exactly one selected node and disabled otherwise.
  - /F60.8/ Create group MUST be enabled for at least one selected node and disabled when none is selected. Source validity is checked under /F42/.
  - /F60.9/ Close MUST close only PaperMesh, not JabRef.
- /F61/ PaperMesh SHOULD provide case-insensitive substring search over title, citation key and keywords. It should highlight matches and show their count, without hiding nodes or changing the node selection. Empty search clears the highlights; no matches shows a count of 0.

## 4. Acceptance tests

These checks describe expected results for the specification above. They have not been run against an implemented add-on. The optional SHOULD check is not a core acceptance gate.

### Test library and common starting point

Create a separate saved JabRef library with exactly these four sample entries:

| Citation key | Title | Keywords |
| --- | --- | --- |
| pmA | Graph search | networks |
| pmB | Graph search | mapping |
| pmC | Protein folding | biology |
| pmD | leave blank | leave blank |

For pmD, leave both fields empty; do not enter the words "leave blank". Set author to **Test Author** and year to **2020** for all four entries. Add a root static group **Graph papers** containing pmA and pmB. There must be no group called **Mesh selection**.

For each manual test:
1. Open a fresh copy of this test library and make it active.
2. Clear JabRef's search and select All entries in the groups sidebar, unless the test specifies a group.
3. Start with no table rows selected. Open Tools → PaperMesh, except in A10 which tests that action itself.

A14 is a planned automated check with its own generated records. A21 and A22 use separate temporary libraries. Changes made in one test must not carry into another.

### /A10/ Open the graph window

1. Highlight pmA, pmC and pmD in JabRef's main table.
2. Choose Tools → PaperMesh.
3. Choose Selected entries and click Generate.

Expected: a separate window shows exactly those three labelled nodes, the input selector and the five buttons from 2.2. The main JabRef window remains usable. (/F10/, /F11/, /F12/)

### /A11/ Use a group's full membership

1. Select Graph papers in JabRef's groups sidebar.
2. Highlight only pmA in the visible table.
3. In PaperMesh, choose Selected group and click Generate.

Expected: exactly pmA and pmB appear, not just the highlighted row. (/F11/)

### /A12/ Reject empty input

1. Highlight pmA, pmB, pmC and pmD; choose Selected entries and click Generate.
2. Clear the table selection in JabRef.
3. Keep Selected entries and click Generate again.

Expected: a message explains that the input is empty. The previous four-node graph remains unchanged. (/F13/)

### /A13/ Regenerate with a different input

1. Highlight pmA and pmB; choose Selected entries and click Generate.
2. Select the pmB node in the graph.
3. In JabRef, highlight only pmC, then click Generate in PaperMesh.

Expected: the old graph is replaced by exactly one node, pmC, with no nodes selected. (/F12/)

### /A14/ Check the 100/101-entry boundary automatically

Planned unit test for the input validator and graph model, to be implemented during development:
1. Create 100 distinct in-memory entries with unique citation keys and generate the graph model.
2. Assert that the model contains exactly 100 distinct nodes.
3. Add a 101st entry and request generation again.

Expected: the second request is rejected as over the limit, with no truncation and no replacement of the previous model. The test counts model nodes automatically, not circles by eye. A12 separately checks that rejected input produces a UI message and leaves the displayed graph unchanged. This is a planned test, not an existing or passing unit test. (/F12/, /F13/)

### /A20/ Inspect the title-based connection

1. In JabRef's main table, highlight all four sample entries: pmA, pmB, pmC and pmD.
2. In PaperMesh, choose Selected entries and click Generate.
3. Click the link between pmA and pmB.

Expected: four nodes and exactly one link, pmA–pmB. Its details show both citation keys, the shared terms **graph, search** and score **0.50** (two shared terms out of four different terms). pmC and pmD have no links. (/F20/, /F21/, /F22/, /F50/)

### /A21/ Compare keywords without titles

1. In a separate temporary library, create two entries with unique citation keys and empty titles.
2. Give the first keywords **GRAPH; graph** and the second **graph**.
3. Highlight both rows, open PaperMesh, choose Selected entries and click Generate.
4. Click their link.

Expected: two nodes and one link, with the single shared term **graph** and score **1.00**. Case and repeated terms do not change the result. (/F20/, /F21/, /F22/)

### /A22/ Check the similarity threshold

1. In a temporary library, give entry X an empty title and keyword **graph**; give Y title **The graph search networks maps** and empty keywords.
2. Highlight both rows, choose Selected entries and click Generate in PaperMesh.
3. Click the X–Y link to inspect its details.

Expected: X and Y are linked with score **0.25**; **the** is ignored.

4. Add **extra** to Y's title in JabRef, keep both rows highlighted and click Generate again.

Expected: no link appears, because the calculation under /F21/ now gives **0.20**, below the threshold. No score display is expected for an absent link. (/F21/)

### /A30/ Open the correct entry despite duplicate titles

1. Highlight pmA and pmB in JabRef; choose Selected entries and click Generate.
2. Click only the node labelled pmB, then Open entry.

Expected: the source library becomes active, pmB is selected in its table and its entry editor shows citation key pmB, not pmA. (/F30/, /F31/)

### /A31/ Enable Open entry only for one node

1. Generate a graph from highlighted table rows pmA and pmB, using Selected entries.
2. Check Open entry with no nodes selected, then after clicking pmA, then after Ctrl/Cmd-clicking pmB.

Expected, in that order: disabled, enabled, disabled. (/F60.7/)

### /A32/ Handle a deleted entry

1. Generate a graph from highlighted rows pmA and pmB, using Selected entries, and select the pmB node.
2. Delete pmB in JabRef without regenerating the graph.
3. Click Open entry in PaperMesh.

Expected: a message explains that the entry is unavailable; no substitute entry or editor is opened. (/F31/)

### /A33/ Handle a closed source library

1. Generate a graph from highlighted rows pmA and pmB, using Selected entries, and select the pmB node.
2. Close that library in JabRef without closing PaperMesh.
3. Click Open entry.

Expected: a message explains that the source library is unavailable; no entry is opened. (/F31/)

### /A40/ Create a group successfully

1. Highlight all four sample entries; choose Selected entries and click Generate.
2. Click pmA, then Ctrl/Cmd-click pmC.
3. Click Create group, enter **Mesh selection** and confirm with Create.

Expected: a new root static group in the source library contains exactly pmA and pmC. (/F40/)

### /A41/ Reject invalid group names

Run each row below independently with a fresh copy of the test library. First carry out its Additional setup in JabRef, then select All entries and clear search again. Highlight pmA, pmB, pmC and pmD in the table, choose Selected entries and click Generate. Select pmA and pmC in the graph.

| Name to enter | Additional setup | Expected result after Create |
| --- | --- | --- |
| Leave the name field empty | None | Message; no group created |
| Type exactly three space characters | None | Message; no group created |
| mesh selection | First create an existing group named Mesh selection | Message; no additional group or membership change |

Click Create group, enter the row's name and confirm with Create. (/F41/)

### /A42/ Cancel group creation

1. Generate from all four explicitly highlighted table rows, using Selected entries; select pmA and pmC in the graph.
2. Click Create group and enter **Mesh selection**.
3. Click Cancel.

Expected: no new group and no changed memberships. (/F41/)

### /A43/ Do not create a group with a removed entry

1. Generate from all four explicitly highlighted table rows, using Selected entries; select pmA and pmC in the graph.
2. Delete pmC in JabRef without regenerating.
3. Click Create group.

Expected: a message appears before the name dialog; no group is created, including no partial group containing only pmA. (/F41/, /F42/)

### /A44/ Do not create a group in a closed library

1. Generate from all four explicitly highlighted table rows, using Selected entries; select pmA and pmC in the graph.
2. Close the source library in JabRef.
3. Click Create group.

Expected: a message appears before the name dialog; no group is created in another library. (/F41/, /F42/)

### /A45/ Enable Create group only with a selection

1. Generate from highlighted table rows pmA and pmB, using Selected entries.
2. Check Create group with no nodes selected, after clicking pmA, and after clicking empty background.

Expected, in that order: disabled, enabled, disabled. (/F60.6/, /F60.8/)

### /A50/ Preserve bibliographic fields

1. Record the four sample entries' title, keywords, author and year values.
2. Highlight all four rows, choose Selected entries and click Generate.
3. Inspect the pmA–pmB link and pan/zoom. Click the pmA node, then Open entry.
4. Return to PaperMesh, click pmA and Ctrl/Cmd-click pmC. Click Create group, enter **Mesh selection** and confirm with Create.

Expected: the recorded fields remain identical after these actions. Only group structure/membership changes; pmD remains visible and unconnected. (/F50/)

### /A60/ Explore and close the graph

Generate from all four explicitly highlighted table rows using Selected entries. Perform these checks in order:

| Step | Action | Expected result | Requirement |
| --- | --- | --- | --- |
| 1 | Click pmA, then drag empty background | Nodes move on screen; pmA stays selected | /F60.1/ |
| 2 | Scroll up over a node, then down | Its displayed size increases, then decreases | /F60.2/ |
| 3 | Click Fit view | All four nodes fit within the canvas | /F60.3/ |
| 4 | Click pmA, Ctrl/Cmd-click pmC, then click pmA without a modifier | Selection counts are 1, 2, 1 | /F60.4/, /F60.5/ |
| 5 | Click empty background without dragging | Selection count is 0 | /F60.6/ |
| 6 | Click Close | PaperMesh closes; JabRef and the library stay open | /F60.9/ |

### /A61/ Search the graph (SHOULD)

If search is implemented:
1. Highlight all four sample entries; choose Selected entries and click Generate, then select pmC in the graph.
2. Search **GRAPH**: pmA and pmB are highlighted, count 2; pmC stays selected and all nodes remain visible.
3. Search **pmC**, then **biology**: only pmC matches each, count 1.
4. Search **zzzz**: count 0.
5. Clear search: no search highlights remain.

These steps must not change the node selection or hide nodes. (/F61/, SHOULD)

## Appendix A. Use cases

### UC1: Generate and explore a graph

- Actor: JabRef user.
- Preconditions: A library is open and active. For table input, entries are explicitly highlighted; for group input, a group is selected in the sidebar.
- Steps: Open Tools → PaperMesh; choose the input type; click Generate; pan, zoom or use Fit view.
- Success: One node per chosen entry is shown, associated with that source library.
- Exceptions: Empty input or more than 100 entries gives a message and leaves the previous graph unchanged. Missing metadata is allowed; entries with no usable terms remain isolated. The user changes the input and tries Generate again.

### UC2: Inspect a link

- Actor: JabRef user.
- Preconditions: A graph has been generated and has at least one link.
- Steps: Click a link; read the entry identities, shared terms and score in the details area.
- Success: The user can see what the connection is based on.
- Exception: If the graph has no links, it still shows its nodes; there is no link to inspect.

### UC3: Open an entry

- Actor: JabRef user.
- Preconditions: A graph is open, exactly one node is selected, and its source library and entry still exist in JabRef.
- Steps: Click Open entry; continue in the selected entry's editor in JabRef.
- Success: The correct source library, table row and editor are active.
- Exception: If the entry was removed or the library closed after generation, show a message; do not open a substitute entry.

### UC4: Create a group

- Actor: JabRef user.
- Preconditions: A graph is open, at least one node is selected, and the source library and all selected entries still exist.
- Steps: Click Create group; enter a new name; confirm with Create.
- Success: A new root static group contains exactly those entries.
- Exceptions: A blank or existing name shows a message so the user can correct it. Cancel changes nothing. A closed library or removed entry prevents creation; the user must reopen the library or regenerate the graph first.

### UC5: Search the graph (SHOULD)

- Actor: JabRef user.
- Preconditions: A graph is generated and the optional search control is available.
- Steps: Enter part of a title, citation key or keyword; inspect the highlights and match count; clear the text to remove them.
- Success: Matching nodes are highlighted without changing selection or group input.
- Exception: No matches gives a count of 0; the graph remains available.

## Open questions

- OPEN QUESTION: Does the team agree with the separate window, Tools entry point and proposed controls? We will check the existing JabRef action/dialog pattern in a prototype.
- OPEN QUESTION: Is the proposed term rule, 0.25 threshold and 100-entry limit useful on a small real library? Dense graphs may need a further link limit; we have not committed to one yet.
- OPEN QUESTION: Can every group member access and build the selected private fork at the recorded baseline? Repository selection does not yet establish team access or a verified integration setup.
- OPEN QUESTION: Does the bounded graph stay usable on the team's existing computers? We will measure this after the prototype, rather than invent a hardware or timing guarantee.
