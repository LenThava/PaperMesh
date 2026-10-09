# Pflichtenheft

PaperMesh - Literature Similarity Map for JabRef

First draft, 9 October 2026. The scope and plan are tentative. We will revise them after discussing the open questions with Giovanni.

Team: Cian Boeniger, Dehlen Thavarajah, Gian Ledergeber and Hrishi Budema.

Supervisor: Giovanni.

## 1. Introduction

### 1.1 Purpose

This document describes what we would like to build and gives our team and Giovanni a basis for discussion.

### 1.2 Scope and goals

PaperMesh will add a graph view to JabRef. Users should be able to explore related papers from their existing library and organise them into groups.

We plan to start with titles and keywords from selected entries or a selected group. External APIs and automatic topic clustering are not part of the first draft.

### 1.3 Definitions

- Entry: a paper stored in JabRef.
- Node: an entry shown in the graph.
- Link: a connection between related entries.
- Group: a collection of entries in JabRef.

### 1.4 Referenced documents

- [Project proposal](https://docs.google.com/document/d/1McPw3q44qVgxCBKDS9bsi2TjOvbiyJpUuNvl4MF80sc/edit)
- [Course instructions and template](https://patrickschniderunibas.github.io/software-engineering/project/requirements)
- [Project plan](projektplan.md)

### 1.5 Overview

Section 2 describes the context, section 3 the requirements, and section 4 the acceptance criteria. The use cases are in Appendix A.

## 2. General description

### 2.1 Integration

The graph will use JabRef's existing entries and groups. The shared JabRef codebase and the place from which users open PaperMesh are still to be agreed.

### 2.2 Main functions

Generate a graph, inspect connections, open an entry in JabRef, and create a group from selected nodes. Users should also be able to pan and zoom.

### 2.3 User profiles

The main users are students and researchers who already use JabRef. They should not need programming knowledge. The team, Giovanni and JabRef maintainers are also affected by the extension.

### 2.4 Constraints

We will use the course JabRef environment and plan to build the graph interface with JavaFX. We will start with a limited selection of entries rather than the whole library.

### 2.5 Assumptions and dependencies

The user has an open JabRef library. Similarity will initially use the available title and keyword data. The calculation and graph limits still need discussion. Missing metadata must not cause a crash.

## 3. Individual requirements

- /F10/ The system must generate a graph from selected entries or a selected group, with one node per entry. (UC1)
- /F20/ The system must support connections based on shared title or keyword terms and show the terms explaining a displayed connection. (UC2)
- /F30/ The system must let the user pan and zoom the graph. (UC1)
- /F40/ The system must let the user open or select the corresponding entry in JabRef. (UC3)
- /F50/ The system must let the user create a JabRef group containing exactly the entries represented by the selected nodes. (UC4)
- /F60/ Viewing the graph must not change the original bibliographic fields. Entries with neither usable title nor keywords must remain visible without invented connections. (UC1–UC3)

## 4. Acceptance criteria

- /A10/ Selecting three entries and generating a graph shows three nodes. The graph can be panned and zoomed. Empty input gives a message. Checks /F10/ and /F30/.
- /A20/ Two test entries with a shared keyword produce an explainable connection. The explanation shows a term actually present in both entries. Checks /F20/.
- /A30/ Opening a node selects or opens the correct JabRef entry. Checks /F40/.
- /A40/ Selecting two nodes and creating a group adds exactly those two entries. Cancelling creates no group. Checks /F50/.
- /A50/ An entry with no usable title or keywords remains visible and unconnected. Bibliographic fields are unchanged after exploring the graph. Checks /F60/.

## Appendix A. Use cases

### UC1: Generate a graph

- Actor: JabRef user.
- Preconditions: A library is open.
- Flow: Select entries or a group, start PaperMesh, then explore the graph with pan and zoom.
- Success: The selected entries are visible.
- Exceptions: Empty input gives a message. Entries without usable metadata remain visible and unconnected.

### UC2: Inspect a connection

- Actor: JabRef user.
- Preconditions: A graph is open.
- Flow: Inspect a link and read the shared terms explaining it.
- Success: The user can see why the papers are connected.
- Exception: If there are no connections, the graph still shows its nodes.

### UC3: Open an entry

- Actor: JabRef user.
- Preconditions: A graph is open.
- Flow: Choose a node and use its open/select action.
- Success: The corresponding entry is opened or selected in JabRef.
- Exception: If the entry no longer exists, show a message and do not open another entry.

### UC4: Create a group

- Actor: JabRef user.
- Preconditions: A graph is open.
- Flow: Select nodes, choose group creation and enter a group name.
- Success: A new group contains exactly the selected entries.
- Exceptions: No selected nodes or a blank name prompts the user to correct the input. Cancelling creates no group.

## Open questions

- OPEN QUESTION: Is this initial scope enough, or should graph search be included?
- OPEN QUESTION: Which similarity calculation should we use?
- OPEN QUESTION: How many entries and links should we display?
- OPEN QUESTION: Which JabRef codebase and interface entry point should we use?
