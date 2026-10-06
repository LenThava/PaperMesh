# Pflichtenheft — PaperMesh

Status: working template, not a completed submission. The group must fill in and review the sections below.

Structure based on the [course Pflichtenheft template](https://raw.githubusercontent.com/PatrickSchniderUnibas/software-engineering/main/docs/project/templates/pflichtenheft-template.md).

## 1. Introduction

### 1.1 Purpose

[Describe the purpose of this specification and who will read it.]

### 1.2 Scope and goals

[Describe PaperMesh's goals, required features and features outside the agreed scope.]

### 1.3 Definitions

[Explain terms such as entry, node, edge, similarity score and group.]

### 1.4 Referenced documents

- [PaperMesh project proposal](https://docs.google.com/document/d/1McPw3q44qVgxCBKDS9bsi2TjOvbiyJpUuNvl4MF80sc/edit)
- [Course requirements instructions](https://patrickschniderunibas.github.io/software-engineering/project/requirements)

[Add any other documents or issues used in the specification.]

### 1.5 Overview

[Explain the structure of this document.]

## 2. General description

### 2.1 Integration

[Describe how PaperMesh will work with JabRef entries, selection, groups and the user interface.]

OPEN QUESTION: Which shared JabRef repository and revision will the team use?

### 2.2 Main functions

[Summarise the agreed main functions.]

### 2.3 User profiles

[Describe the intended users and their expected knowledge.]

### 2.4 Constraints

[Specify the chosen Java/JabRef environment, supported platforms and any relevant limits.]

### 2.5 Assumptions and dependencies

[Document assumptions about available metadata, entry counts and external dependencies.]

OPEN QUESTION: What behaviour is required when entries have no usable title or keywords?

## 3. Individual requirements

[Derive the functional requirements from the use cases in Appendix A. Give each requirement an identifier and state a clear, testable behaviour.]

Suggested sentence form: "When [condition], PaperMesh shall [observable behaviour]."

- /F10/ [First functional requirement.]
- /F20/ [Second functional requirement.]

## 4. Acceptance criteria

[Explain how each agreed requirement will be checked. Reference the corresponding requirement identifiers.]

- /A10/ [Acceptance check for /F10/.]
- /A20/ [Acceptance check for /F20/.]

## Appendix A. Use cases

### Use case 1: [Name]

- Actors: [Who uses the function?]
- Preconditions: [What must be true before the action?]
- Normal flow:
  1. [User action.]
  2. [System response.]
- Successful outcome: [Result.]
- Exception: [Problem or missing input.]
- Exception flow: [Expected response and resulting state.]

[Repeat for the other use cases.]

## Open questions

- OPEN QUESTION: Who is the assigned supervisor/reviewer?
- OPEN QUESTION: Which features belong to the required scope and which are optional?
- OPEN QUESTION: Which repository and revision will be the shared JabRef codebase?
