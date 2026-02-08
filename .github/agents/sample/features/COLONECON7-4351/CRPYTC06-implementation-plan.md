# Implementation Plan: Case, Person & Participant Update Integration

**Branch**: feature/COLONECON7-4351-back-mejoras-integracion-pagare-pendiente-y-garantias | **Date**: 2026-02-04 | **Spec**: [spec.md](./spec.md)
**Input**: Feature specification from /specs/COLONECON7-4351-back-mejoras-integracion-pagare-pendiente-y-garantias/spec.md

**Note**: This template is filled in by the /speckit.plan command. See .specify/templates/commands/plan.md for the execution workflow.

## Summary

Integrate CRPYRC01 library methods for sequential case, person, and participant updates in CRPYTC06-01-CO transaction. Implementation uses early return pattern to avoid nested conditionals, with specific exception handling and unique error codes per operation. Technical approach follows BBVA Elara framework patterns with clean code principles from speckit constitution.

## Technical Context

**Language/Version**: Java 1.8+ (Maven project)  
**Primary Dependencies**: Elara Framework 8.0.11, CRPYRC01 0.4.0, CRPYCC02 0.6.0-SNAPSHOT  
**Storage**: N/A (delegates to library services)  
**Testing**: JUnit 4.13.1, Mockito 3.11.2  
**Target Platform**: BBVA APX Platform (OSGi bundles)  
**Project Type**: Single Maven multi-module project  
**Performance Goals**: Standard transaction processing (<500ms per operation)  
**Constraints**: Must follow Elara transaction lifecycle, OSGi bundle packaging required  
**Scale/Scope**: Single transaction class with 3 library integrations

## Constitution Check

GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.

### Constitution Template Analysis
The project uses the default constitution template. Custom principles must be defined in .specify/memory/constitution.md for this project.

### Code Quality Gates ✅

| Principle | Status | Evidence |
|-----------|--------|----------|
| No Nested Ifs | ✅ PASS | Early return pattern implemented in execute(), all conditional checks at method level |
| Specific Exceptions | ✅ PASS | Using RuntimeException instead of generic Exception |
| Single Responsibility | ✅ PASS | Each update operation extracted to separate method (updateCase, updatePerson, updateParticipant) |
| Clear Naming | ✅ PASS | Method names clearly describe actions (updateCase vs executeUpdateCase) |
| Appropriate Logging | ✅ PASS | INFO for business logic, ERROR for exceptions with message and stack trace |
| Unique Error Codes | ✅ PASS | CRPY00000008 (case), CRPY00000009 (person), CRPY00000010 (participant) |

### Framework Compliance ✅

| Requirement | Status | Implementation |
|-------------|--------|----------------|
| Extend AbstractTransaction | ✅ PASS | CRPYTC0601COTransaction extends AbstractCRPYTC0601COTransaction |
| Use Severity Enum | ✅ PASS | Severity.ENR for errors, Severity.OK for success |
| Use HttpResponseCode | ✅ PASS | HttpResponseCode.HTTP_CODE_204 for success |
| AddAdvice Pattern | ✅ PASS | addAdviceWithDescription() with error code and message |

**Re-evaluation Post-Phase 1**: ✅ ALL GATES PASSED

All constitution principles remain satisfied after design phase:
Data model maintains simplicity (single DTO, sequential flow)
Contracts clearly defined with no hidden complexity
Library interface follows standard patterns
No architectural violations introduced

## Project Structure

### Documentation (this feature)

text
specs/COLONECON7-4351-back-mejoras-integracion-pagare-pendiente-y-garantias/
├── plan.md              # This file (/speckit.plan command output) ✅
├── spec.md              # Feature specification ✅
├── research.md          # Phase 0 output ✅ COMPLETE
├── data-model.md        # Phase 1 output ✅ COMPLETE
├── quickstart.md        # Phase 1 output ✅ COMPLETE
├── contracts/           # Phase 1 output ✅ COMPLETE
│   ├── CRPYRC01-library-contract.md
│   └── CRPYTC06-transaction-contract.md
└── tasks.md             # Phase 2 output (/speckit.tasks command - NOT created by /speckit.plan)

### Source Code (repository root)

text
artifact/
├── transactions/
│   └── CRPYTC06-01-CO/
│       ├── pom.xml
│       ├── README.md
│       └── src/
│           ├── main/
│           │   ├── java/com/bbva/crpy/
│           │   │   ├── AbstractCRPYTC0601COTransaction.java
│           │   │   ├── CRPYTC0601COTransaction.java       # ✅ IMPLEMENTED
│           │   │   └── utils/
│           │   │       ├── Constants.java
│           │   │       ├── Mapper.java
│           │   │       └── Validations.java
│           │   └── resources/
│           │       ├── CRPYTC06-01-CO.xml
│           │       └── multilanguage-ES.properties
│           └── test/
│               ├── java/com/bbva/crpy/
│               │   ├── CRPYTC0601COTransactionTest.java
│               │   └── utils/MockData.java
│               └── resources/log4j.xml
├── libraries/          # CRPYRC01 (external - needs update)
└── dtos/              # CRPYCC02 (external)

.specify/
├── memory/
│   └── constitution.md      # Project constitution template
├── scripts/
│   └── powershell/
│       ├── setup-plan.ps1
│       └── update-agent-context.ps1
└── templates/               # Spec/plan/task templates

.github/
└── agents/                  # Agent prompt definitions

**Structure Decision**: Single Maven multi-module project with OSGi bundle packaging. Main implementation in artifact/transactions/CRPYTC06-01-CO/ module. Uses library-first approach with CRPYRC01 providing business logic services.

## Complexity Tracking

> **No violations detected. All constitution principles followed.**

This feature maintains simplicity by:
Using existing framework patterns (Elara AbstractTransaction)
Delegating business logic to library (CRPYRC01)
Single responsibility per method
Early return pattern eliminating nested conditionals
Standard exception handling with specific types

No additional complexity justification required.