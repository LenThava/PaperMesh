# Pflichtenheft — PaperMesh

Initial draft, 9 October 2026. We will review the scope and open questions with Giovanni. This is not the final specification.

Team: Cian Boeniger, Len Thava, Gian Ledergeber and Hrishi Budema.

Supervisor: Giovanni.

## 1. Introduction

### 1.1 Purpose

This document describes the first proposed version of PaperMesh. It gives our team and supervisor a shared basis for implementation and acceptance testing.

### 1.2 Scope and goals

PaperMesh adds a literature similarity map to JabRef. It should help users see relationships between papers already in their library, inspect these relationships and organise related entries.

For the first version, we propose using titles and keywords only. The user starts with selected entries or a selected JabRef group. The map supports pan, zoom, similarity explanations, opening entries and creating a group from selected nodes.

External APIs, full-text analysis, AI-based recommendations, automatic topic clustering and graph search are outside this initial scope. We can discuss these as later additions rather than promise them now.

### 1.3 Definitions

- Entry: one bibliographic record in JabRef.
- Node: an entry shown in the map.
- Link: a connection between two entries with shared metadata terms.
- Similarity score: a value between 0 and 1 describing the overlap of these terms.
- Group: a JabRef collection of entries.

### 1.4 Referenced documents

- [Project proposal](https://docs.google.com/document/d/1McPw3q44qVgxCBKDS9bsi2TjOvbiyJpUuNvl4MF80sc/edit)
- [Course requirements instructions](https://patrickschniderunibas.github.io/software-engineering/project/requirements)
- [Course Pflichtenheft template](https://raw.githubusercontent.com/PatrickSchniderUnibas/software-engineering/main/docs/project/templates/pflichtenheft-template.md)
- [Project plan](projektplan.md)

### 1.5 Overview

Section 2 describes the context and assumptions. Section 3 lists the proposed requirements, section 4 explains how to check them, and Appendix A describes the user workflows from which they were derived.

## 2. General description

### 2.1 Integration

PaperMesh will be part of JabRef, using its existing entries, selection, entry editor and groups. The interface will use JavaFX. It will read local metadata; generating or exploring the map must not change that metadata. Creating a group is an explicit user action.

The shared JabRef codebase and exact menu or toolbar entry point are still to be agreed. The current repository contains project documentation, not the JabRef implementation.

### 2.2 Main functions

Generate a map, inspect a connection, navigate to an entry and create a group from selected nodes. Pan, zoom and a similarity threshold help users explore the map.

### 2.3 User profiles

The main users are students and researchers who already use JabRef. They should not need programming knowledge. Other stakeholders are JabRef users and maintainers, our project team, and Giovanni as supervisor and reviewer.

### 2.4 Constraints

Development will use the course JabRef version, Java 21 and JavaFX. We propose a maximum of 100 entries per map and five links per node to keep the first version manageable. These limits are proposals for the supervisor discussion, not measured performance claims.

### 2.5 Assumptions and dependencies

The user has an open JabRef library. Some entries may have missing metadata. The similarity score measures word overlap, not whether papers really discuss the same topic. No internet connection or external API is required for the proposed core features.

Our initial scoring proposal is simple: use unique lowercase words from title and keywords, with punctuation separating words. The score is the number of shared words divided by the number of words in their combined set. An empty combined set gives a score of 0. The proposed default threshold is 0.2. We will check this approach with Giovanni before finalising it.

## 3. Individual requirements

- /F10/ The system must let the user generate a map from selected entries or one selected group. It must show one node per entry for inputs of 1–100 entries. An empty input or more than 100 entries must produce a clear message without generating a partial map. (UC1)
- /F11/ The system must keep entries with neither usable title nor keywords visible as unconnected nodes. A missing title must use the citation key as its label, or "Untitled entry" if that is also missing; available keywords can still be used for similarity. (UC1)
- /F12/ Generating, filtering or exploring a map must leave the original bibliographic metadata unchanged. (UC1–UC3)
- /F20/ The system must calculate similarity from title and keyword terms using the initial rule in section 2.5, and keep the score and shared terms available for each displayed link. (UC2)
- /F21/ The system must let the user adjust the threshold between 0 and 1. It must show only links with a positive score at least equal to that threshold, with at most five links per node and higher-scoring connections prioritised. (UC2)
- /F30/ The system must let the user pan and zoom the map without changing its entries. (UC1)
- /F31/ The system must let the user inspect a link's score and shared terms, for example through a tooltip or detail panel. If there are no qualifying links, the nodes must still be shown. (UC2)
- /F40/ The system must let the user open or select the corresponding JabRef entry from a node. If that entry has been removed from the library, it must show a message instead of opening another entry. (UC3)
- /F50/ The system must let the user create a new JabRef group containing exactly the entries represented by the selected nodes. (UC4)
- /F51/ Group creation must require at least one selected node and a non-empty, unused group name. Invalid input or cancellation must not create a group or change existing groups. (UC4)

## 4. Acceptance criteria

- /A10/ With a library containing three test entries, generating a map from those entries or their group shows exactly three nodes. One entry shows one node. Zero entries or 101 entries produces a message rather than a partial map. Checks /F10/.
- /A20/ For term sets {graph, networks, map} and {graph, models, map}, the displayed similarity is 0.5 and the explanation contains "graph" and "map". A threshold of 0.6 hides that link. A third entry with no shared terms has no link to them. Checks /F20/, /F21/ and /F31/.
- /A21/ A test collection with more than five possible connections per entry never displays more than five links on one node. Pan and zoom work, and a map with no qualifying links still shows its nodes. Checks /F21/, /F30/ and /F31/.
- /A30/ Opening a node selects or opens the correct entry in JabRef. Removing that entry before opening it produces a clear message. Checks /F40/.
- /A40/ Selecting three nodes and creating a group with a new name creates a group with exactly those three entries. No selection, an empty or duplicate name, and cancellation leave the groups unchanged. Checks /F50/ and /F51/.
- /A50/ An entry with neither usable title nor keywords is shown using its citation key or fallback label and remains unconnected. An entry without a title but with keywords can still connect through those keywords. Comparing the library before and after map generation, filtering and navigation confirms that its bibliographic fields have not changed. Checks /F11/ and /F12/.

## Appendix A. Use cases

### UC1: Generate and explore a map

- Actor: JabRef user.
- Preconditions: A library is open and the user has selected entries or a group.
- Normal flow: 1. The user starts PaperMesh. 2. PaperMesh reads the selection and calculates connections. 3. The map appears. 4. The user pans or zooms it.
- Successful outcome: The chosen entries are visible without changing their metadata.
- Exceptions: Empty input or more than 100 entries gives a message and asks the user to choose a valid input. Entries with neither usable title nor keywords remain unconnected; a missing title uses the fallback label. A valid input with no similarities still shows its nodes.

### UC2: Understand a connection

- Actor: JabRef user.
- Preconditions: A map is open.
- Normal flow: 1. The user inspects a link. 2. PaperMesh shows its score and shared terms. 3. The user changes the threshold to show fewer or more connections.
- Successful outcome: The user can see why a displayed connection exists.
- Exceptions: If the new threshold leaves no links, the nodes remain visible. A value outside 0–1 is rejected without changing the current threshold.

### UC3: Open a paper in JabRef

- Actor: JabRef user.
- Preconditions: A map containing the paper's node is open.
- Normal flow: 1. The user chooses the node's open/select action. 2. JabRef selects or opens the corresponding entry.
- Successful outcome: The user reaches the correct entry without metadata changes.
- Exception: If the entry no longer exists, PaperMesh shows a message and opens no entry.

### UC4: Create a group from the map

- Actor: JabRef user.
- Preconditions: A map is open and its entries still exist in the active library.
- Normal flow: 1. The user selects related nodes. 2. The user chooses group creation and enters a name. 3. PaperMesh creates the group with exactly those entries.
- Successful outcome: The new group is available in JabRef.
- Exceptions: No selection, a blank name or an existing name prompts the user to correct the input; no group is created. Cancellation leaves all groups unchanged.

## Open questions

- OPEN QUESTION: Does this smaller, local-data-only scope meet the project expectations, or should graph search or other proposal features be required?
- OPEN QUESTION: Is the proposed word-overlap score useful enough? Should we remove common words or handle different languages differently?
- OPEN QUESTION: Are the proposed 100-entry limit, five-link limit and default threshold suitable?
- OPEN QUESTION: Which shared JabRef repository and revision should we use, and where should PaperMesh be opened in the interface?
- OPEN QUESTION: Should the map refresh when the library changes, or should users generate a new map in the first version?
