# Unit Tests: Holidays

## 1. What Is This Folder?

Isolated business logic unit tests for the Holidays domain services and helpers.

## 2. Why Do We Need It?

It provides rapid, deterministic verification of business rules without database or network overhead.

## 3. Main Responsibility

Testing domain algorithms, calculation rules, and state machine workflows for Holidays.

## 4. How It Works

Unit tests instantiate Holidays service classes with mock repositories and mock clients. They verify that business rules are enforced, calculations are accurate, and domain exceptions are raised on invalid states.

## 5. What This Folder Contains

| Item     | Type | Purpose                            | Required/Optional |
|----------|------|------------------------------------|-------------------|
| .gitkeep | File | Directory preservation placeholder | Required          |

## 6. File Responsibilities

| File     | What It Does                                                    | What It Contains        | Input | Output | Called By | Calls |
|----------|-----------------------------------------------------------------|-------------------------|-------|--------|-----------|-------|
| .gitkeep | Preserves the holidays unit tests directory in version control. | Empty placeholder file. | None  | None   | Git VCS   | None  |

## 7. Allowed Responsibilities

Pure unit test functions with mocked dependencies for holidays.

## 8. Boundaries

Must never connect to a real database or make live network calls.

## 9. Connection With Other Layers

- Targets: Holidays services and helpers.

## 10. Data Flow

Mock inputs → Service method → Output assertions / Exception checks.

## 11. Validation Responsibility

Tests business rule validation logic.

## 12. Authentication and Authorization

Tests resource ownership and business authorization rules.

## 13. Error Handling

Asserts that domain exceptions are raised under rule violation conditions.

## 14. Transaction Responsibility

No database transactions.

## 15. Logging and Auditing

Captured by pytest.

## 16. Naming Convention

Files: `test_<service_or_helper>.py`.

## 17. When to Add a New File

When adding or updating business rules in Holidays services or helpers.

## 18. Common Mistakes

Introducing real database calls or unmocked external client calls into unit tests.

## 19. Related Folders

- [Unit Tests Root](../README.md)
- [Holidays Services](../../../modules/holidays/services/README.md)

## 20. Review Checklist

- [ ] Are all external dependencies (repositories, clients) completely mocked?
- [ ] Do tests execute in milliseconds without I/O?
