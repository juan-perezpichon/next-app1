<!--
Sync Impact Report
- Version change: N/A (template) -> 1.0.0
- Modified principles: PRINCIPLE_1_NAME -> Documentation-First Requirements Intake; PRINCIPLE_2_NAME -> Repository Comprehension Before Changes; PRINCIPLE_3_NAME -> Generated Code Integrity; PRINCIPLE_4_NAME -> Dependency Governance; PRINCIPLE_5_NAME -> Secure Runtime Discipline
- Added sections: None (template placeholders resolved)
- Removed sections: None
- Templates requiring updates: ✅ .specify/templates/plan-template.md; ✅ .specify/templates/spec-template.md; ✅ .specify/templates/tasks-template.md; ⚠ .specify/templates/commands/*.md (not found)
- Follow-up TODOs: TODO(RATIFICATION_DATE): original adoption date not found in repo
-->
# next-app1 Constitution

## Core Principles

### Documentation-First Requirements Intake
MUST locate local APX instructions at .github/instructions/apx_style_guide.instructions.md when APX work is in scope. If missing, MUST retrieve the authoritative style guide, APX architecture docs, and APX secure development guide via approved tooling and read each document in full before design or coding. Rationale: APX compliance depends on current, authoritative guidance.

### Repository Comprehension Before Changes
MUST read README.md and the relevant component files before modifying code. If instructions require reading the entire repo, do so and document scope in the plan. Rationale: avoids misaligned changes and hidden constraints.

### Generated Code Integrity
PROHIBITED from editing APX-generated artifacts (transaction abstract classes, IDE generated configs). Only implementation classes or files marked for customization (e.g., *-app, *Impl) may be changed. Rationale: generated code is overwritten by the framework.

### Dependency Governance
MUST declare dependencies only in the module that uses them, never in parent POMs, and MUST NOT mark dependencies as optional. Use APX CLI to remove dependencies instead of editing pom.xml manually. Runtime Import-Package ordering and inclusion of trailing * MUST be preserved. Rationale: keeps OSGi metadata consistent and deployable.

### Secure Runtime Discipline
MUST follow APX security and runtime rules: use bind variables, avoid DB-specific SQL, never access DataSource directly, never manage transactions or threads manually, avoid System.* logging, and do not catch generic Exception. Only approved exception types may be thrown from application code. Rationale: ensures security and platform stability.

## APX Security and Runtime Constraints

- Data access uses APX JdbcUtils or approved connectors only; direct DataSource/Connection access is prohibited.
- SQL uses bind variables and schema-qualified tables; vendor-specific SQL constructs are prohibited.
- Error handling uses APX advice/error model; do not throw RuntimeException or catch Exception.
- Logging uses framework logger with appropriate levels; System.out/System.err are prohibited.
- Transactions and threads are framework-managed; manual commit/rollback or thread creation is prohibited.
- External calls use approved connectors with architecture validation where required.

## Workflow and Compliance

- Step 1: Requirements analysis must verify local instructions and fetch external APX docs if missing.
- Step 2: Read APX architecture docs and the secure development guide in full.
- Step 3: Read README.md and relevant repo files (or the entire repo if required).
- Step 4: Implement changes following APX standards and the constraints above.
- Step 5: Record compliance checks in plans/specs/tasks and validate before merge.

## Governance

This constitution supersedes conflicting practices. Amendments require a documented rationale and version bump following semantic versioning. All plans, specs, and tasks must include a constitution compliance check, and reviews must verify compliance before merge.

**Version**: 1.0.0 | **Ratified**: TODO(RATIFICATION_DATE): original adoption date not found in repo | **Last Amended**: 2026-02-08
