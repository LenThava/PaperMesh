# Pflichtenheft

PaperMesh - Literature Similarity Map for JabRef

Working draft, revised 14 October 2026 after Giovanni's feedback. This is still tentative: the window layout, similarity rule and limits below are proposals for the team to try and agree before the final version on 27 October. They are not implemented features.

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

We will use JavaFX and Java 21, as specified for the course. The existing local course checkout is based on JabRef 5.15, at revision `c705647a4e2c37ae9f608cdd959d98c88a0fde44`; its development build displays version `100.0.0`. This is a candidate baseline, not a confirmed shared team repository.

The aim is to run on a computer that already runs the course JabRef version, without a dedicated graphics card or extra hardware. We propose a limit of 100 input entries to keep the first version manageable. We have not measured performance yet and are not promising zero extra CPU or memory use.

### 2.5 Assumptions and dependencies

PaperMesh depends on access to JabRef's entry selection, group membership and entry editor. The shared codebase must be chosen and its build checked before integration. There is no dependency on an external API.

## 3. Requirements

### Graph input and opening (UC1)

- /F10/ With a library active, JabRef MUST offer Tools → PaperMesh, opening the separate non-modal graph window with the input selector and five buttons described in 2.2.
- /F11/ Generate MUST use either the explicitly highlighted table entries or all members JabRef associates with the selected group, according to the input selector. A table selection MUST NOT override group input.
- /F12/ The graph MUST contain exactly one node per input entry and retain the source-library association. Generate again MUST replace the graph with the newly read input and clear its node selection.
- /F13/ Empty input or input above the proposed 100-entry limit MUST show a message and leave any previous graph unchanged. It MUST NOT silently drop entries.

### Similarity and explanations (UC2)

- /F20/ Comparison MUST use only the title and keyword terms. Missing fields contribute no terms; repeated terms count once.
- /F21/ Proposed rule: lowercase both fields, split at characters other than letters or numbers, and remove these common English words: `a, an, and, as, at, by, for, from, in, is, it, of, on, or, the, to, with`. Compare the remaining term sets using shared terms divided by the size of their union (Jaccard similarity). If either set is empty, the score is 0. A link is drawn for a score of at least 0.25. No other link filter is proposed yet.
- /F22/ Clicking a link MUST show the two entries, their shared terms in alphabetical order and the score rounded to two decimal places in the details area. The displayed explanation MUST come from the compared metadata.

### Entry navigation (UC3)

- /F30/ Selecting a node and clicking Open entry MUST activate its source library, select the corresponding entry in JabRef's main table and open its entry editor.
- /F31/ Navigation MUST identify the actual entry, not look it up by title alone. If the source library is closed or the entry was removed, it MUST show a message instead of opening another entry.

### Group creation (UC4)

- /F40/ Create group MUST request a name and create a new static group at the root of the source library, containing exactly the selected nodes' entries.
- /F41/ The name MUST be trimmed of leading/trailing whitespace. Blank names, names already used in that library (ignoring case), a closed source library or removed selected entries MUST prevent creation and show a message. Cancel MUST create no group. There MUST be no partial group on failure.

### Safe exploration (UC1–UC5)

- /F50/ Generating, exploring and navigating the graph MUST NOT change bibliographic fields such as title, keywords, author or year. Group creation may change group structure and membership only. Entries with no usable terms MUST stay visible without links.
- /F60/ Users MUST be able to pan by dragging the background, zoom in/out by scrolling the mouse wheel up/down and use Fit view to show the whole graph. Clicking nodes and Ctrl/Cmd-clicking MUST support single and multiple selection. Open entry MUST be disabled unless exactly one node is selected; Create group MUST be disabled when none is selected. Close MUST close only PaperMesh, not JabRef.
- /F61/ PaperMesh SHOULD provide case-insensitive substring search over title, citation key and keywords. It should highlight matches and show their count, without hiding nodes or changing the node selection. Empty search clears the highlights; no matches shows a count of 0.

## 4. Acceptance tests

These tests assume the proposed interface and comparison rule above are retained. We will update the tests if the team changes either. SHOULD checks are optional, not core acceptance gates.

### Test data

Create a saved test library with these entries and no other entries:

| Citation key | Title | Keywords |
| --- | --- | --- |
| pmA | Graph search | networks |
| pmB | Graph search | mapping |
| pmC | Protein folding | biology |
| pmD | empty field | empty field |

For pmD, both fields must actually be empty, not contain the text "empty field". Set author to **Test Author** and year to **2020** for all four entries, so the preservation checks include non-empty fields. Create an existing static group **Graph papers** containing pmA and pmB. No group called **Mesh selection** should exist.

Use a fresh copy of this library for each test; clear JabRef's search and select All entries in its groups sidebar unless the test says otherwise. Open PaperMesh before testing, except A10 which checks opening itself. "Generate from" means highlight the named table rows, choose Selected entries and click Generate. Temporary test libraries are separate copies, not additions to the four-entry fixture.

- /A10/ Highlight pmA, pmC and pmD in the main table. Open Tools → PaperMesh, choose Selected entries and click Generate. Exactly those three labelled nodes appear in a separate window. The main JabRef window remains usable. Choose Close: JabRef remains open. (/F10/, /F11/, /F12/, /F60/)
- /A11/ Select Graph papers in the groups sidebar and highlight only pmA in the visible table. Choose Selected group and Generate: exactly pmA and pmB appear. Switch to Selected entries, keep only pmA highlighted and Generate again: only pmA remains, with no selected nodes. (/F11/, /F12/)
- /A12/ Generate all four fixture entries, then clear the table selection and Generate with Selected entries. A message appears and the same graph remains. In a temporary library, select 101 entries and Generate: it is refused without truncation. Select exactly 100 instead and Generate: exactly 100 nodes appear. (/F13/)
- /A20/ Generate from all four test entries. There is exactly one link, pmA–pmB. Click it: details show pmA, pmB, shared terms **graph, search**, and score **0.50** (two shared terms out of four different terms). pmC and pmD have no links. (/F20/, /F21/, /F22/, /F50/)
- /A21/ In a separate temporary library, create an entry with empty title and keywords **GRAPH; graph**, and another with empty title and keywords **graph**. Generate from both: one link appears, with the single shared term **graph** and score **1.00**. This checks keywords alone, case handling and duplicates. (/F20/, /F21/, /F22/)
- /A22/ Test the boundary in a temporary library: entry X has empty title and keyword **graph**; entry Y has title **The graph search networks maps** and empty keywords. Their score is **0.25**, so they are linked. Add **extra** to Y's title and generate again: the score is **0.20**, and no link appears. This also checks that **the** is ignored. (/F21/)
- /A30/ Generate from pmA and pmB, which share a title. Open entry is disabled with zero or two selected nodes. Select only pmB and click Open entry: JabRef selects pmB and its editor shows citation key pmB, not pmA. Delete pmB without regenerating and try again: a message appears and no substitute entry is opened. On a fresh copy with pmB present, generate again, close the source library and try Open entry: a message appears and no entry opens. (/F30/, /F31/, /F60/)
- /A40/ Generate all four entries. Select pmA, then Ctrl/Cmd-click pmC. Create group → **Mesh selection** → Create: a root static group contains exactly pmA and pmC. Try an empty name, spaces only and then **mesh selection**: each shows a message and creates nothing. Enter a new name and Cancel: nothing is created. Clear node selection: Create group is disabled. On fresh copies, separately delete a selected entry or close the source library before confirmation: each shows a message with no partial group. (/F40/, /F41/, /F60/)
- /A50/ Record all four entries' title, keywords, author and year fields. Generate the graph, inspect links, pan/zoom and open an entry. Those fields remain identical to the recorded values. Create a group: membership may change, but the recorded fields do not. pmD remains visible and unconnected. (/F50/)
- /A60/ Generate all four nodes and select pmA. Drag empty background: node positions move on screen but pmA stays selected. Scroll up over a node: its displayed size increases; scroll down: it decreases. Fit view places all four nodes within the canvas. Click pmA, Ctrl/Cmd-click pmC, then click pmA without a modifier: the selection counts are 1, 2, 1. Clicking empty background gives 0. (/F60/)
- /A61/ If search is implemented, generate all four entries and select pmC. Search **GRAPH**: exactly pmA and pmB are highlighted and the match count is 2; pmC stays selected and all nodes remain visible. Search **pmC** and **biology** separately: only pmC matches each. Search **zzzz**: count 0. Clear search: no search highlights remain. (/F61/, SHOULD)

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
- OPEN QUESTION: Which shared course repository and revision will we use? The local 5.15-based snapshot is only a candidate, and private course source must not be copied into this public documentation repository.
- OPEN QUESTION: Does the bounded graph stay usable on the team's existing computers? We will measure this after the prototype, rather than invent a hardware or timing guarantee.
