# Operations Decision Intelligence Copilot

## Development Plan, Prompting Strategy, and Antigravity Implementation Guide

## 1. Project overview

**Project name:** Operations Decision Intelligence Copilot

**Business domain:** Business Operations & Legal

**Objective:** Transform fragmented project status notes, KPI
spreadsheets, meeting updates, and blocker logs into structured,
evidence-based executive decision briefs.

Operations leaders should be able to understand:

-   Project health and milestone delays.
-   Underperforming business KPIs.
-   Resource bottlenecks.
-   Cross-project dependencies.
-   Strategic options and trade-offs.
-   Recommended decisions and required actions.

### Core functionalities

1.  **Multi-source data ingestion**
    -   Accept multiple Markdown (`.md`) and text (`.txt`) project
        status files.
    -   Accept KPI tracking CSV files.
    -   Support multiple projects and business units.
2.  **Automated risk and dependency analysis**
    -   Detect delayed milestones.
    -   Identify resource bottlenecks.
    -   Identify off-track KPIs.
    -   Detect dependencies between projects.
    -   Assess operational risks and their business impact.
3.  **Executive decision memo generation**
    -   Generate structured executive summaries.
    -   Present strategic alternatives.
    -   Explain trade-offs.
    -   Recommend actions.
    -   Identify decisions requiring leadership approval.
4.  **Action-item extraction**
    -   Extract tasks, owners, and deadlines.
    -   Identify escalations.
    -   Track priority and status.
    -   Distinguish proposed actions from confirmed commitments.

------------------------------------------------------------------------

## 2. Recommended technology stack

  -----------------------------------------------------------------------
  Component               Technology              Responsibility
  ----------------------- ----------------------- -----------------------
  Frontend                Streamlit               Dashboard and user
                                                  interface

  Backend                 Python                  Business logic and
                                                  application services

  LLM                     Gemini API              Information extraction
                                                  and contextual
                                                  reasoning

  Data processing         Pandas                  CSV processing and KPI
                                                  calculations

  AI orchestration        LangGraph (introduce    Multi-stage workflow
                          when useful)            orchestration

  Structured validation   Pydantic                Validate structured LLM
                                                  outputs

  Database                SQLite                  Persist extracted
                                                  records, actions, and
                                                  reports

  Visualization           Plotly                  KPI charts and risk
                                                  visualizations

  Report generation       PDF, Markdown, CSV      Export executive briefs
                                                  and action trackers
  -----------------------------------------------------------------------

### Architecture guidance

Start with a straightforward Python application and modular functions.
Introduce LangGraph when conditional workflows, retries, human approval,
or more complex agent coordination are needed. Do not add a vector
database to the initial version: the first release is primarily a
structured extraction, calculation, and decision-support workflow, not a
semantic-search application.

------------------------------------------------------------------------

## 3. Prompting strategy for Antigravity

Do not ask Antigravity to build the entire application using one giant
implementation prompt. Use **phased prompting with checkpoints**.

The strategy has three prompt levels:

  -----------------------------------------------------------------------
  Prompt type                         Purpose
  ----------------------------------- -----------------------------------
  Master context prompt               Establish project requirements,
                                      architecture, technology choices,
                                      and coding rules

  Phase prompts                       Implement one well-defined module
                                      at a time

  Refinement prompts                  Fix defects, improve behavior, and
                                      integrate modules without rewriting
                                      working code
  -----------------------------------------------------------------------

### Development sequence

1.  Master context and architecture proposal.
2.  Phase 1 -- Project setup and mock data.
3.  Phase 2 -- Multi-source document loader.
4.  Phase 3 -- AI-based structured data extraction.
5.  Phase 4 -- Risk and dependency analyzer.
6.  Phase 5 -- Executive decision memo generator.
7.  Phase 6 -- Action-item extractor.
8.  Phase 7 -- Streamlit dashboard.
9.  Phase 8 -- Integration, testing, and export.

### Checkpoint cycle

For every phase:

1.  Give Antigravity the phase prompt.
2.  Let it inspect the existing repository and implement only that
    phase.
3.  Run the application or module tests.
4.  Inspect the changed files and important code paths.
5.  Ask Antigravity to explain its implementation and assumptions.
6.  Fix issues with targeted prompts.
7.  Approve the phase before moving to the next one.

Do not send all phase prompts in sequence without reviewing the results.
Do not assume a feature is complete merely because the agent says it is
complete; verify it with tests and actual execution.

### Targeted correction prompt pattern

When a defect is found, describe the observed behavior, expected
behavior, and scope of the fix. For example:

> During testing, the KPI analyzer incorrectly identifies a KPI with an
> actual value greater than its target as off-track. Inspect the
> comparison logic, identify the root cause, fix it, and add a
> regression test. Do not modify unrelated modules.

### Persistent project context

Maintain a `PROJECT_CONTEXT.md` file in the repository. Ask Antigravity
to update it at the end of every completed phase.

It should record:

-   Project overview and requirements.
-   Agreed architecture and technology stack.
-   Current folder structure.
-   Module responsibilities and interfaces.
-   Implemented features.
-   Pending features.
-   Important development decisions.
-   Known bugs and limitations.
-   Commands for setup, execution, and testing.

When starting a fresh Antigravity conversation, first ask it to read
`PROJECT_CONTEXT.md` and inspect the repository before continuing.

------------------------------------------------------------------------

## 4. Master context prompt

**Send this prompt first. Its purpose is to establish context and
request an architecture proposal---not to start coding.**

``` text
# Project: Operations Decision Intelligence Copilot

## Project Overview

Build an AI-powered Operations Decision Intelligence Copilot for business operations and legal teams.

The objective is to transform fragmented project status notes, KPI spreadsheets, meeting updates, and blocker logs into structured, evidence-based executive decision briefs.

The application should help leadership understand:
- Project health and milestone delays.
- Underperforming business KPIs.
- Resource bottlenecks.
- Cross-project dependencies.
- Strategic options and trade-offs.
- Recommended decisions and required actions.

## Core Functionalities

1. Multi-source data ingestion:
   - Accept multiple Markdown (.md) and text (.txt) project status files.
   - Accept KPI tracking CSV files.
   - Support multiple projects and business units.

2. Automated risk and dependency analysis:
   - Detect delayed milestones.
   - Identify resource bottlenecks.
   - Identify off-track KPIs.
   - Detect dependencies between projects.
   - Assess operational risks and their business impact.

3. Executive decision memo generation:
   - Generate structured executive summaries.
   - Present strategic alternatives.
   - Explain trade-offs.
   - Recommend actions.
   - Identify decisions requiring leadership approval.

4. Action-item extraction:
   - Extract tasks, owners, and deadlines.
   - Identify escalations.
   - Track priority and status.
   - Distinguish proposed actions from confirmed commitments.

## Technology Stack

- Frontend: Streamlit
- Backend: Python
- LLM: Gemini API
- Data processing: Pandas
- AI orchestration: LangGraph (introduce when needed)
- Structured validation: Pydantic
- Database: SQLite
- Visualization: Plotly
- Report generation: PDF, Markdown, and CSV

## Architecture Principles

- Separate frontend, backend, AI logic, and data processing.
- Use modular and reusable Python components.
- Keep deterministic calculations separate from LLM reasoning.
- Use structured LLM outputs validated through Pydantic.
- Never allow the LLM to invent missing data.
- Maintain traceability between generated findings and their source documents.
- Handle missing, inconsistent, and conflicting data.
- Keep the architecture extensible.

## Development Rules

1. Do not attempt to implement the entire application immediately.
2. Work in clearly defined development phases.
3. Implement only the phase explicitly requested.
4. Before modifying existing files, inspect the current project structure and understand existing implementations.
5. Reuse existing components wherever possible.
6. Do not rewrite working modules unnecessarily.
7. Avoid unnecessary dependencies and overengineering.
8. Write readable, maintainable, properly organized code.
9. Add error handling and input validation.
10. Test every module before considering it complete.
11. Do not use hardcoded mock outputs in actual application functionality.
12. Never expose API keys or secrets in source code.

## Important Instruction

For now, DO NOT implement the application.

First:
1. Analyze the requirements.
2. Propose a clean project folder structure.
3. Explain the responsibility of each major module.
4. Explain how data will flow between modules.
5. Identify potential technical challenges.
6. Present the proposed implementation sequence.

Wait for my approval before starting Phase 1.

Treat this project specification as the persistent context for subsequent development tasks.
```

**Checkpoint:** Review the proposed architecture and folder structure.
Resolve disagreements before approving Phase 1.

------------------------------------------------------------------------

## 5. Phase-by-phase implementation prompts

Send only the prompt for the current phase. After implementation, test
and review it before proceeding.

### Phase 1 -- Project setup and mock data

**Objective:** Establish the repository structure and create a realistic
dataset.

``` text
We are starting Phase 1 of the Operations Decision Intelligence Copilot.

Refer to the previously established project requirements.

### Objective
Set up the project structure and create realistic mock business data.

### Tasks

1. Create the agreed folder structure.
2. Set up the Python dependency requirements file.
3. Create 5–6 realistic project status documents using Markdown and TXT.
4. Include multiple business units such as Finance, Sales, IT, and Operations.
5. Create a KPI CSV containing multiple KPIs across these business units.
6. Include realistic operational scenarios:
   - Delayed milestones.
   - Resource shortages.
   - Dependencies between projects.
   - Off-track KPIs.
   - Missing information.
   - Conflicting weekly status updates.
7. Ensure the dataset is interconnected and realistic.

### Constraints
- Do not implement AI processing yet.
- Do not create the dashboard yet.
- Keep mock data separate from application source code.

### Completion criteria
- All files are created.
- CSV columns are consistent.
- Mock data represents realistic business scenarios.
- Provide a summary of created files and how to run the project.

Update PROJECT_CONTEXT.md with the completed work and any decisions.

After completion, wait for my approval.
```

**Review checklist:**

-   Are there 5--6 useful status documents?
-   Do they cover multiple business units?
-   Are there genuine cross-project dependencies?
-   Does the KPI CSV contain targets, actual values, units, and
    reporting periods where appropriate?
-   Are missing and conflicting details represented deliberately?
-   Is the mock data kept separate from application code?

### Phase 2 -- Multi-source document loader

**Objective:** Read the supplied documents and KPI spreadsheets
reliably.

``` text
Implement Phase 2: Multi-source Data Loader.

First inspect the existing project structure and Phase 1 files.

### Requirements

1. Implement a document loader that accepts multiple .md and .txt files.
2. Implement a CSV loader using Pandas.
3. Support batch processing of files.
4. Handle empty files, invalid formats, missing columns, and encoding issues.
5. Preserve source filename and document metadata.
6. Return normalized raw content that subsequent modules can process.

### Expected output

The loader should produce a structured collection containing:
- Source filename.
- File type.
- Raw content.
- Associated metadata.

### Constraints
- Do not implement LLM extraction yet.
- Do not implement risk analysis.
- Do not modify mock data unnecessarily.
- Keep loaders modular and reusable.

### Testing
Test with all Phase 1 sample files and include invalid-file scenarios.

Update PROJECT_CONTEXT.md.

Explain how to execute and test this module. Wait for approval before proceeding.
```

**Review checklist:**

-   Can multiple `.md` and `.txt` files be loaded in one run?
-   Can the KPI CSV be loaded and validated?
-   Are unsupported, empty, and malformed files handled?
-   Is the source filename retained?
-   Are errors reported clearly without crashing the whole batch?

### Phase 3 -- AI-based structured data extraction

**Objective:** Convert unstructured project notes into validated project
records.

``` text
Implement Phase 3: AI-based Structured Data Extraction.

Inspect the existing loader and reuse it.

### Objective
Convert unstructured project status notes into structured project records using the Gemini API.

### Extract the following fields

- Project name.
- Business unit.
- Reporting period.
- Project owner.
- Overall status.
- Milestones.
- Milestone deadlines and statuses.
- Blockers.
- Dependencies.
- Resource constraints.
- Expected completion date.
- Risks.
- Action items.

### Technical requirements

1. Use Gemini API for information extraction.
2. Use Pydantic models to define and validate structured outputs.
3. Handle missing or ambiguous information.
4. Never invent missing owners, dates, or statuses.
5. Preserve source document references for extracted information.
6. Handle API errors and invalid model responses.
7. Separate prompts, schemas, and extraction logic into appropriate modules.

### Expected output
A validated structured representation of each project.

### Constraints
- Do not implement the risk analyzer yet.
- Do not generate executive memos yet.
- Do not build the frontend.

Test extraction against all mock documents.

Update PROJECT_CONTEXT.md and wait for approval.
```

**Review checklist:**

-   Are all required fields represented in the schema?
-   Are unknown values left null or explicitly marked unknown?
-   Are dates and statuses normalized consistently?
-   Can invalid LLM responses be detected and handled?
-   Can extracted fields be traced to their source documents?
-   Are API credentials read from environment variables?

### Phase 4 -- Risk and dependency analyzer

**Objective:** Identify operational risks using deterministic
calculations and contextual reasoning.

``` text
Implement Phase 4: Automated Risk and Dependency Analyzer.

Use the validated structured project data from Phase 3 and KPI CSV data.

### Implement three components

1. Milestone Analyzer:
   - Identify overdue milestones.
   - Calculate delay duration.
   - Identify downstream milestone impact.

2. KPI Analyzer:
   - Calculate actual versus target deviations.
   - Identify off-track KPIs.
   - Support configurable thresholds.
   - Group results by business unit.

3. Dependency and Resource Analyzer:
   - Identify cross-project dependencies.
   - Detect shared resource bottlenecks.
   - Identify risks that could affect multiple projects.

### Risk assessment

Assign Low, Medium, High, or Critical severity based on configurable rules.

For every risk, provide:
- Risk description.
- Source evidence.
- Related project or KPI.
- Business impact.
- Severity.
- Potential mitigation.

### Technical requirements
- Use Python for deterministic calculations.
- Use LLM reasoning only where contextual interpretation is necessary.
- Avoid duplicate risk records.
- Preserve source traceability.

### Testing
Test delayed milestones, off-track KPIs, shared resources, and dependent projects.

Do not implement memo generation or frontend yet.

Update PROJECT_CONTEXT.md and wait for approval.
```

**Review checklist:**

-   Are calculations performed in Python rather than delegated to the
    LLM?
-   Are KPI thresholds configurable and appropriate to each KPI?
-   Are higher-is-better and lower-is-better KPIs handled correctly?
-   Are overdue milestones calculated using a clearly defined reporting
    date?
-   Are dependencies supported by evidence?
-   Are risks deduplicated and traceable?
-   Are severity rules documented?

### Phase 5 -- Executive decision memo generator

**Objective:** Generate an evidence-backed executive brief.

``` text
Implement Phase 5: AI-powered Executive Decision Memo Generator.

Use the existing structured project data and risk analysis outputs.

### Objective
Generate an executive-ready decision brief using the Gemini API.

### Memo sections

1. Executive Summary.
2. Overall Portfolio Health.
3. Key Operational Findings.
4. KPI Performance Summary.
5. Critical Risks and Dependencies.
6. Strategic Options.
7. Trade-offs and Business Implications.
8. Recommended Actions.
9. Leadership Decisions Required.

### Requirements

- Generate concise, professional executive language.
- Present at least two strategic alternatives when sufficient evidence exists.
- Explain advantages and disadvantages of each alternative.
- Provide evidence-backed recommendations.
- Reference the relevant project or KPI source.
- Explicitly identify missing information.
- Distinguish facts from assumptions and recommendations.
- Never fabricate business metrics or decisions.

Use structured output and Pydantic validation.

The memo must be generated from actual analysis results, not hardcoded text.

Do not build the frontend yet.

Test the generated memo using all mock projects.

Update PROJECT_CONTEXT.md and wait for approval.
```

**Review checklist:**

-   Does the memo reflect the actual analysis results?
-   Are facts separated from assumptions and recommendations?
-   Are options and trade-offs meaningful?
-   Are recommendations supported by evidence?
-   Are missing facts identified rather than invented?
-   Are leadership approvals clearly distinguished from proposed
    actions?

### Phase 6 -- Action-item extractor

**Objective:** Produce a review-ready action register.

``` text
Implement Phase 6: Action Item Extraction and Tracking.

Use project notes, extracted structured records, risk analysis, and memo recommendations.

### Requirements

Extract:
- Action description.
- Related project.
- Responsible owner.
- Deadline.
- Priority.
- Current status.
- Escalation requirement.
- Escalation reason.
- Source reference.

### Additional functionality

1. Detect duplicate action items.
2. Identify overdue actions.
3. Flag missing owners and deadlines.
4. Separate proposed actions from confirmed commitments.
5. Generate a structured action-item table.
6. Allow action records to be stored in SQLite.

Do not invent missing information.

Test with all mock data and verify that recommendations are not incorrectly marked as approved tasks.

Do not implement the frontend yet.

Update PROJECT_CONTEXT.md and wait for approval.
```

**Review checklist:**

-   Are owners and deadlines extracted only when supported by evidence?
-   Are proposed actions distinguished from confirmed commitments?
-   Are duplicate actions handled?
-   Are overdue actions calculated consistently?
-   Are escalation reasons understandable?
-   Are records persisted and retrievable from SQLite?

### Phase 7 -- Streamlit dashboard

**Objective:** Build the user-facing application and connect the
completed backend modules.

``` text
Implement Phase 7: Streamlit Frontend.

Inspect all existing backend modules and integrate them without rewriting working logic.

### Design requirements

Build a professional, modern, light-themed enterprise dashboard.

### Pages

1. Overview Dashboard
   - Total initiatives.
   - On-track projects.
   - Delayed projects.
   - Critical risks.
   - Off-track KPIs.

2. Project Analysis
   - Project details.
   - Milestone progress.
   - Blockers.
   - Dependencies.

3. KPI Monitoring
   - Target versus actual charts.
   - Business-unit filtering.
   - KPI deviation indicators.

4. Risk Intelligence
   - Risk register.
   - Severity indicators.
   - Dependency visualization.

5. Executive Brief
   - Generate memo button.
   - Memo preview.
   - Strategic options and recommendations.

6. Action Tracker
   - Action-item table.
   - Owner and deadline.
   - Priority and escalation status.

### Interactions

- Upload multiple project documents.
- Upload KPI CSV files.
- Trigger analysis.
- Generate executive memos.
- View and filter results.

Use Plotly for charts.

Ensure the interface is responsive and visually consistent.

Do not duplicate backend logic inside Streamlit.

Update PROJECT_CONTEXT.md and wait for approval.
```

**Review checklist:**

-   Can users upload multiple supported files?
-   Does the UI invoke the existing backend services?
-   Are loading, success, and error states clear?
-   Do dashboard numbers match backend results?
-   Do filters work correctly?
-   Are memo and action-item views readable?
-   Is the interface usable at different screen sizes?

### Phase 8 -- Integration, testing, and export

**Objective:** Verify the complete application and deliver export
functionality.

``` text
Implement Phase 8: Final Integration, Testing and Export.

### Tasks

1. Verify the complete workflow from document upload to executive memo generation.
2. Test all modules together.
3. Resolve integration errors.
4. Verify KPI calculations and risk classifications.
5. Test conflicting project updates and missing information.
6. Verify that generated recommendations have supporting evidence.
7. Implement PDF memo export.
8. Implement Markdown memo export.
9. Implement CSV action-item export.
10. Improve error messages and loading indicators.
11. Ensure API keys are managed through environment variables.

### Final acceptance criteria

A user should be able to:

1. Upload project status documents and KPI CSV files.
2. Trigger automated analysis.
3. View project and KPI performance.
4. Identify risks and dependencies.
5. Generate an executive decision memo.
6. Review strategic options and recommendations.
7. View extracted action items.
8. Export the memo and action tracker.

Provide a final project summary, setup instructions, environment configuration, and testing results.

Do not claim any functionality is complete unless it has actually been implemented and tested.

Update PROJECT_CONTEXT.md with the final architecture, implementation status, known limitations, and run instructions.
```

------------------------------------------------------------------------

## 6. Suggested project folder structure

The exact structure should be reviewed with Antigravity during the
master-context stage. A possible starting point is:

``` text
operations-copilot/
│
├── app.py
├── requirements.txt
├── .env.example
├── .gitignore
├── PROJECT_CONTEXT.md
│
├── data/
│   ├── status_notes/
│   └── kpi/
│
├── backend/
│   ├── ingestion/
│   │   ├── document_loader.py
│   │   └── csv_loader.py
│   │
│   ├── extraction/
│   │   ├── extractor.py
│   │   └── prompts.py
│   │
│   ├── analysis/
│   │   ├── milestone_analyzer.py
│   │   ├── kpi_analyzer.py
│   │   ├── dependency_analyzer.py
│   │   └── risk_engine.py
│   │
│   ├── memo/
│   │   ├── memo_generator.py
│   │   └── memo_prompts.py
│   │
│   ├── actions/
│   │   └── action_extractor.py
│   │
│   ├── orchestration/
│   │   └── workflow.py
│   │
│   ├── schemas/
│   │   ├── project.py
│   │   ├── risk.py
│   │   ├── action.py
│   │   └── memo.py
│   │
│   ├── database/
│   │   ├── db.py
│   │   └── repositories.py
│   │
│   └── exports/
│       ├── pdf_exporter.py
│       ├── markdown_exporter.py
│       └── csv_exporter.py
│
├── frontend/
│   ├── pages/
│   │   ├── overview.py
│   │   ├── projects.py
│   │   ├── kpis.py
│   │   ├── risks.py
│   │   ├── executive_brief.py
│   │   └── actions.py
│   │
│   └── components/
│
├── tests/
│   ├── test_ingestion.py
│   ├── test_extraction.py
│   ├── test_analysis.py
│   ├── test_memo.py
│   └── test_actions.py
│
└── outputs/
```

Do not create every file as an empty placeholder merely to match this
tree. Add modules when their implementation phase begins.

------------------------------------------------------------------------

## 7. Data model

Use common structured records across the application.

  Entity       Important fields
  ------------ -----------------------------------------------------------
  Project      project_id, name, business_unit, owner, status
  Milestone    milestone_id, project_id, name, deadline, status
  KPI          kpi_id, business_unit, name, target, actual, unit, period
  Blocker      blocker_id, project_id, description, severity
  Dependency   source_project, dependent_project, dependency_type
  Risk         risk_id, description, impact, severity, mitigation
  Action       action_id, description, owner, deadline, priority, status
  Decision     decision_id, options, recommendation, approval_status

SQLite is sufficient for the prototype. Keep schemas explicit and
validate data before persistence.

------------------------------------------------------------------------

## 8. Risk analysis design notes

### Milestone delay analysis

The system should identify:

-   Overdue milestones.
-   Milestones delayed against their planned dates.
-   Not-started milestones approaching their deadlines.
-   Downstream milestones affected by delayed predecessors.

Define the reporting date and delay calculation consistently. A
milestone's status and date fields should be validated before
calculating a delay.

### KPI analysis

For a higher-is-better KPI:

``` text
Deviation = Actual - Target
Relative deviation (%) = ((Actual - Target) / Target) * 100
```

For lower-is-better KPIs, the interpretation must be reversed. Some KPIs
also require ranges or tolerance bands rather than a simple target
comparison.

Store KPI direction and threshold configuration rather than assuming
every KPI follows the same rule.

### Dependency and resource analysis

Look for evidence such as:

-   One project's milestone explicitly depends on another project's
    output.
-   The same team or named resource is assigned to several initiatives.
-   A blocker affects a shared service or resource.
-   A delayed upstream milestone could affect downstream delivery.

Distinguish confirmed dependencies from inferred potential dependencies.
Do not present a suspected resource bottleneck as a verified fact
without supporting evidence.

### Risk record

A useful risk record should include:

-   Risk description.
-   Related project, milestone, or KPI.
-   Evidence and source reference.
-   Business impact.
-   Severity.
-   Confidence or evidence status.
-   Suggested mitigation.

Python should perform deterministic date and KPI calculations. The LLM
can help interpret unstructured context, explain impact, and suggest
mitigation options.

------------------------------------------------------------------------

## 9. Executive memo design

The generated memo should contain:

1.  Executive Summary.
2.  Overall Portfolio Health.
3.  Key Operational Findings.
4.  KPI Performance Summary.
5.  Critical Risks and Dependencies.
6.  Strategic Options.
7.  Trade-offs and Business Implications.
8.  Recommended Actions.
9.  Leadership Decisions Required.

The memo should:

-   Use concise executive language.
-   Present alternatives when the evidence supports them.
-   Explain advantages, disadvantages, and implications.
-   Link findings to source documents or KPI records.
-   Explicitly identify missing information.
-   Distinguish facts, assumptions, and recommendations.
-   Avoid inventing metrics, owners, dates, approvals, or decisions.

The application should support human review and editing before a memo is
finalized. The copilot recommends decisions; it does not make or approve
them.

------------------------------------------------------------------------

## 10. Important test scenarios

Do not test only clean, straightforward examples. The mock dataset
should demonstrate difficult operational cases.

  -----------------------------------------------------------------------
  Scenario                            Expected behavior
  ----------------------------------- -----------------------------------
  Milestone overdue by 10 days        Flag the delayed milestone and
                                      calculate delay

  KPI actual below target             Assess using KPI direction and
                                      configured threshold

  Actual exceeds target for a         Do not incorrectly flag as
  higher-is-better KPI                off-track

  Actual exceeds target for a         Interpret according to KPI
  lower-is-better KPI                 direction

  Same team assigned to several       Flag a potential resource
  projects                            bottleneck when supported by
                                      evidence

  Project B depends on delayed        Identify potential downstream
  Project A                           impact

  Missing project owner               Mark owner as unknown

  Two weekly notes show different     Flag conflicting updates and retain
  statuses                            both sources

  No deadline specified               Do not invent a deadline

  Critical blocker has no mitigation  Flag the need for review or
                                      escalation

  Duplicate action appears in         Detect or consolidate carefully
  multiple notes                      while preserving sources

  Proposed recommendation appears in  Do not automatically mark it as an
  a memo                              approved commitment
  -----------------------------------------------------------------------

------------------------------------------------------------------------

## 11. Final acceptance criteria

The application is ready for demonstration when a user can:

-   Upload multiple project status documents and KPI CSV files.
-   Validate and process the uploaded data.
-   Extract structured project information.
-   View project and KPI performance.
-   Identify milestone delays, risks, blockers, and dependencies.
-   Understand the evidence behind important findings.
-   Generate an executive decision memo.
-   Review strategic options, trade-offs, and recommendations.
-   View extracted action items with owners, deadlines, priorities, and
    escalation information.
-   Distinguish proposed actions from confirmed commitments.
-   Export the memo as PDF and Markdown.
-   Export the action tracker as CSV.

### Final quality principles

1.  Evidence over unsupported AI conclusions.
2.  Deterministic calculations over LLM arithmetic.
3.  Explicit unknowns over invented information.
4.  Modular implementation over a monolithic codebase.
5.  Human approval over automatic executive decisions.
6.  Tested functionality over claims of completion.

------------------------------------------------------------------------

## 12. Recommended starting procedure

1.  Open the project workspace in Antigravity.
2.  Send the **Master Context Prompt** from Section 4.
3.  Review the proposed architecture, module responsibilities, and
    folder structure.
4.  Ask for revisions if necessary.
5.  Approve the architecture.
6.  Send the Phase 1 prompt only.
7.  Review and test the generated mock dataset.
8.  Update and inspect `PROJECT_CONTEXT.md`.
9.  Continue phase by phase, using the checkpoint cycle in Section 3.

**Do not rush into the frontend.** First establish reliable ingestion,
structured extraction, and risk analysis. The dashboard becomes much
easier to build once the backend outputs are stable.
