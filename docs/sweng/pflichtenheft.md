# Pflichtenheft

PaperMesh - Literature Similarity Map for JabRef

First draft, 9 October 2026. This is still tentative. We will go through it with the team and Giovanni.

Team: Cian Boeniger, Dehlen Thavarajah, Gian Ledergeber and Hrishi Budema.

Supervisor: Giovanni.

## 1. Introduction

### 1.1 Purpose

This is a first outline of what we want to build.

### 1.2 Scope and goals

We would like to add a graph view to JabRef so users can find related papers and put them into groups. For now, we plan to use titles and keywords from selected entries or a selected group. External APIs and automatic clustering can wait.

### 1.3 Definitions

An entry is a paper in JabRef. In the graph, entries are nodes and connections between them are links.

### 1.5 Overview

The draft covers the idea, requirements, acceptance checks and use cases.

## 2. General description

### 2.1 Integration

PaperMesh will work with JabRef's entries and groups. We still need to choose the shared JabRef codebase and where to open the graph.

### 2.2 Main functions

Show a graph, pan and zoom, inspect a connection, open an entry, and make a group from selected nodes.

### 2.3 Users

Students and researchers using JabRef are the main users. Our team builds it, Giovanni reviews it, and JabRef maintainers may later review the changes.

### 2.4 Constraints

We plan to use JavaFX and the course JabRef environment. We will start with a selection of papers, not a whole large library.

### 2.5 Assumptions

A JabRef library is open. Titles and keywords may be missing. We still need to decide how similarity is calculated and how large the graph can be.

## 3. Requirements

- /F10/ PaperMesh must show one node per selected entry and allow pan and zoom. A selected group can also be used. (UC1)
- /F20/ Links must be based on shared title or keyword terms, and show the terms behind the connection. (UC2)
- /F30/ A node must let the user open or select its entry in JabRef. (UC3)
- /F40/ Users must be able to create a JabRef group from selected nodes. (UC4)
- /F50/ Exploring the graph must not change bibliographic fields. Entries with neither usable title nor keywords must stay visible without links. (UC1–UC3)

## 4. Acceptance checks

- /A10/ Three selected entries give three nodes; pan and zoom work. Empty input shows a message. (/F10/)
- /A20/ Two test entries with the same keyword are linked. The explanation shows that keyword. (/F20/)
- /A30/ Opening a node opens or selects the matching entry, not another one. (/F30/)
- /A40/ A group made from two selected nodes contains exactly those two entries. Cancelling creates nothing. (/F40/)
- /A50/ An entry with neither usable title nor keywords stays visible and unconnected. Exploring the graph leaves bibliographic fields unchanged. (/F50/)

## Appendix A. Use cases

The actor is a JabRef user with an open library. UC2–UC4 also need an open graph.

### UC1: Show the graph

Select entries or a group, open PaperMesh, then pan and zoom.

- Result: The selected entries are shown as nodes.
- Exceptions: Empty input shows a message. Entries with neither usable title nor keywords are shown without links.

### UC2: Inspect a connection

Choose a link and read the shared terms.

- Result: The user sees why the papers are connected.
- Exception: If there are no links, the nodes are still shown.

### UC3: Open a paper

Choose a node and use the open/select action.

- Result: Its entry is opened or selected in JabRef.
- Exception: If the entry was removed, show a message instead.

### UC4: Make a group

Select nodes, choose group creation, enter a name and confirm.

- Result: A new group contains exactly those entries.
- Exceptions: No selection or a blank name needs correcting. Cancelling creates nothing.

## Open questions

- OPEN QUESTION: Is this scope enough, or do we need search too?
- OPEN QUESTION: How should we calculate similarity and limit the graph?
- OPEN QUESTION: Which JabRef codebase and place in the interface should we use?
