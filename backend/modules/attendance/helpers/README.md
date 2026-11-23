# Attendance Helpers

## 1. What Is This Folder?

The helpers layer contains stateless utility functions and domain-specific calculation algorithms for Attendance.

## 2. Why Do We Need It?

It keeps service classes focused on business workflows by isolating pure algorithmic calculations into testable utility functions.

## 3. Main Responsibility

Executing mathematical, datetime, and formatting calculations specific to attendance rules.

## 4. How It Works

`attendance_helper.py` provides pure, side-effect-free helper functions. They accept raw inputs (e.g. punch timestamps, shift start times) and compute working durations, break times, overtime intervals, and grace period compliance.

## 5. What This Folder Contains

| Item                 | Type | Purpose                                                                      | Required/Optional |
|----------------------|------|------------------------------------------------------------------------------|-------------------|
| __init__.py          | File | Package initialization marker                                                | Required          |
| attendance_helper.py | File | Stateless calculation utilities for working hours, overtime, and punctuality | Required          |

## 6. File Responsibilities

| File                 | What It Does                                           | What It Contains       | Input                        | Output                         | Called By                   | Calls           |
|----------------------|--------------------------------------------------------|------------------------|------------------------------|--------------------------------|-----------------------------|-----------------|
| __init__.py          | Initializes the helpers package.                       | Empty package marker.  | None                         | None                           | Python runtime              | None            |
| attendance_helper.py | Calculates work duration, grace windows, and overtime. | Pure helper functions. | Timestamps, shift parameters | Calculated durations, booleans | ClockService, ReportService | Python datetime |

## 7. Allowed Responsibilities

- Pure calculations on timestamps, time deltas, and intervals.
- Verifying whether a punch timestamp falls within an allowed grace window.
- Aggregating daily punch pairs into total productive hours.

## 8. Boundaries

- Must not access databases or execute queries.
- Must not make HTTP calls or import client classes.
- Must not maintain mutable global state.

## 9. Connection With Other Layers

- Called by: Attendance services (`backend/modules/attendance/services`).
- Calls: Standard library utilities (datetime, math).
- Allowed: Pure utility function calls.
- Prohibited: Calls to database, network, or external services.

## 10. Data Flow

Enters: Timestamps, integers, configuration parameters.
Leaves: Calculated hours, boolean flags, formatted duration strings.

## 11. Validation Responsibility

Validates mathematical and datetime integrity (e.g. clock-out is after clock-in).

## 12. Authentication and Authorization

No security decisions.

## 13. Error Handling

Raises standard Python ValueError on invalid mathematical or datetime inputs.

## 14. Transaction Responsibility

No transaction responsibility.

## 15. Logging and Auditing

Helpers do not perform logging or auditing.

## 16. Naming Convention

Files: `attendance_helper.py`. Functions: `calculate_work_duration`, `is_within_grace_period`.

## 17. When to Add a New File

When a new complex mathematical or calculation algorithm is needed in Attendance.

## 18. Common Mistakes

- Hiding business workflow decisions or database queries inside helper functions.
- Relying on system clock (`datetime.now()`) directly inside pure functions instead of passing timestamps as arguments.

## 19. Related Folders

- [Attendance Module Root](../README.md)
- [Attendance Services](../services/README.md)
- [Shared Helpers](../../../shared/helpers/README.md)

## 20. Review Checklist

- [ ] Are all helper functions strictly stateless and pure?
- [ ] Are there zero database or network calls in helper files?
- [ ] Are edge cases (e.g. midnight shift crossovers) accounted for?
