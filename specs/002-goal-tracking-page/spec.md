# Feature Specification: Goal Tracking Page (Doit)

**Feature Branch**: `002-goal-tracking-page`  
**Created**: 2026-02-09  
**Status**: Draft  
**Input**: User description: "goal tracking webapp 'doit' with two columns (current goals with days left, completed goals), each goal has checkbox to move to completed or delete; add new goals via button opening modal form with title and end date fields; goals within 3 days of end date highlighted; modern light theme with fun pastel colors."

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Track goals at a glance (Priority: P1)

As a user, I can see my current goals and completed goals in separate columns with days left so I can quickly understand what needs attention.

**Why this priority**: This is the core value of the page: quick status visibility for goals.

**Independent Test**: Seed a few goals with different end dates and statuses and confirm the two-column view and days-left values render correctly.

**Acceptance Scenarios**:

1. **Given** at least one current goal and one completed goal, **When** the page loads, **Then** the current goals appear in the left column with days left and completed goals appear in the right column.
2. **Given** a current goal with an end date 5 days away, **When** the page loads, **Then** the goal shows "5 days left" (or equivalent singular/plural) in the current goals column.

---

### User Story 2 - Add a new goal (Priority: P2)

As a user, I can add a goal using a modal form with a title and end date so I can grow my list of goals.

**Why this priority**: Creating goals is essential for continued use after the initial list is set up.

**Independent Test**: Open the modal, enter a valid title and end date, submit, and confirm the goal appears in the current goals list.

**Acceptance Scenarios**:

1. **Given** the add goal button is available, **When** I click it, **Then** a modal opens with title and end date fields and a clear way to submit or cancel.
2. **Given** a valid title and end date, **When** I submit the form, **Then** the new goal appears in the current goals column with the correct days-left value.

---

### User Story 3 - Update goal status or remove goals (Priority: P3)

As a user, I can mark a goal completed with a checkbox or delete it so I can keep my lists accurate.

**Why this priority**: Keeping the list clean and accurate improves trust and usability, but can follow the core viewing and adding flows.

**Independent Test**: Toggle a goal to completed and delete another goal, verifying list movement and removal.

**Acceptance Scenarios**:

1. **Given** a current goal, **When** I check its completion checkbox, **Then** the goal moves from current goals to completed goals.
2. **Given** any goal in either column, **When** I select delete, **Then** the goal is removed from the list.

### Edge Cases

- No goals exist: both columns show an empty state that explains how to add a goal.
- End date is today: days-left displays as "0 days left" (or "Due today") and is highlighted as near-due.
- End date is in the past: goal is flagged as overdue and remains in current goals until completed or deleted.
- Invalid or missing input: title or end date is missing, and the form blocks submission with a clear message.
- Multiple goals share the same end date: all show the same days-left value and highlight rule consistently.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: System MUST present two columns: current goals and completed goals.
- **FR-002**: System MUST display days left for each current goal based on its end date and the user's local date.
- **FR-003**: Users MUST be able to mark a current goal as completed via a checkbox, moving it to the completed column.
- **FR-004**: Users MUST be able to delete any goal from either column.
- **FR-005**: System MUST provide an add-goal button that opens a modal form with required title and end date fields.
- **FR-006**: System MUST validate that title and end date are provided before allowing submission.
- **FR-007**: System MUST highlight current goals that are within 3 days of their end date (inclusive).
- **FR-008**: System MUST visually distinguish overdue current goals from upcoming goals.
- **FR-009**: System MUST use a modern light theme with fun pastel colors across the page.

### Acceptance Coverage

The acceptance scenarios in User Stories 1-3 and the Edge Cases section define the acceptance criteria for the functional requirements above.

### Key Entities *(include if feature involves data)*

- **Goal**: Represents a single tracked objective with a title, end date, status (current or completed), and derived days-left value.

### Assumptions

- Days-left values are calculated using the user's local calendar date, not time of day.
- Goals remain in the current column even when overdue until completed or deleted.
- Deleting a goal does not require a confirmation step unless added later.

### Dependencies

- None identified for this feature.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: At least 90% of test users can add a goal and see it in the current goals list without assistance.
- **SC-002**: At least 90% of test users can mark a goal completed and see it move columns on the first attempt.
- **SC-003**: Near-due highlighting correctly applies to 100% of goals with end dates within 3 days in a verification set.
- **SC-004**: Users can complete the add-goal flow in under 1 minute in 95% of observed sessions.
