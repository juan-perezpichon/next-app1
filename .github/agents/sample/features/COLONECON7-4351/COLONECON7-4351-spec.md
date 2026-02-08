# Feature Specification: Case, Person & Participant Update Integration

**Feature ID**: COLONECON7-4351  
**Date**: 2026-02-04  
**Status**: In Progress

## Overview

Enhance the CRPYTC06-01-CO transaction to support sequential updates of case, person, and participant records through integration with CRPYRC01 library services.

## Requirements

### Functional Requirements

1. **Sequential Update Flow**: The transaction must update records in strict sequence:
   - Step 1: Update Case
   - Step 2: Update Person (only if Case update succeeds)
   - Step 3: Update Participant (only if Person update succeeds)

2. **Early Termination**: If any step fails, subsequent steps must not execute and the transaction must terminate with appropriate error handling.

3. **Library Integration**: Use CRPYRC01 library version 0.4.0+ methods:
   - executeUpdateCase(CaseDTO) → Boolean
   - executeUpdatePerson(CaseDTO) → Boolean
   - executeUpdateParticipant(CaseDTO) → Boolean

4. **Error Handling**: Each operation must have:
   - Unique error code (CRPY00000008, CRPY00000009, CRPY00000010)
   - Descriptive error messages
   - Appropriate logging (INFO for business errors, ERROR for exceptions)

5. **Success Response**: Return HTTP 204 (No Content) with Severity.OK when all three updates complete successfully.

### Non-Functional Requirements

1. **Code Quality**: Follow speckit constitution principles:
   - No nested if statements (use early returns)
   - No generic exception catching (use specific exceptions)
   - Single Responsibility Principle (extract methods)
   - Clear method naming and documentation

2. **Maintainability**: 
   - Each update operation isolated in separate method
   - Consistent error handling pattern across all methods
   - Logging at appropriate levels

3. **Framework Compliance**: Follow BBVA Elara framework patterns:
   - Extend AbstractTransaction
   - Use addAdviceWithDescription for errors
   - Set appropriate Severity levels
   - Use HttpResponseCode constants

## Technical Details

### Input
CaseDTO containing case, person, and participant data

### Processing
1. Map input to library format using Mapper.mapperPatchCase()
2. Get CRPYRC01 service library instance
3. Execute sequential updates with early return pattern

### Output
Success: HTTP 204, Severity.OK
Failure: HTTP with error advice, Severity.ENR

## Dependencies

CRPYRC01 library 0.4.0+
Elara Framework 8.0.11
CRPYCC02 0.6.0-SNAPSHOT (for DTOs)

## Constraints

Must maintain backward compatibility with existing executeUpdateCase functionality
Must not block if CRPYRC01 methods don't exist yet (compilation errors acceptable during development)
Transaction timeout limits apply (framework default)

## Success Criteria

[ ] All three update methods integrated
[ ] Early return pattern implemented (no nested ifs)
[ ] Specific exception handling (RuntimeException, not Exception)
[ ] Unique error codes for each operation
[ ] Appropriate logging at each step
[ ] Code passes speckit constitution review
[ ] Unit tests cover all paths (when CRPYRC01 methods available)