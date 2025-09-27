# Solution Document

## Bug Investigation & Fixes

### Issues Identified

1. **Caching Issue** – The cache `Map` did not use a fully unique key, leading to cache contamination and inconsistencies.
2. **Performance Issue** – Batch processing did not leverage parallel execution, resulting in slower processing times.
3. **Validation Issue** – Preprocessing logic was incorrectly placed, causing untrimmed inputs to reach the validation function.

## Solutions Implemented

1. **Cache Fix** – Updated the logic to generate a unique cache key by combining `country` and `regulatoryId`, ensuring consistent and isolated cache entries.
2. **Performance Fix** – Refactored batch logic to correctly use parallel processing. Added timing measurements and enhanced return values to include performance statistics.
3. **Validation Fix** – Moved preprocessing logic to execute before passing inputs to the validation function, preventing untrimmed data from being processed.

---

## Support Ticket Analysis

### Ticket Prioritization

1. **Ticket #2847** – _Highest priority_
   - Business-critical. The client cannot submit regulatory reports on time. Immediate action required.
2. **Ticket #2848** – _Medium priority_
   - Important but not regulatory-urgent. Affects pricing updates for the Swiss market. Should be resolved immediately after Ticket #2847.
3. **Ticket #2789** – _Lowest priority_
   - Feature request. Current workflows remain possible through manual updates. Can be planned for the next sprint.

---

### Root Cause Analysis

#### Ticket #2847 – CSV Export Missing Records

- Reproduce the issue in a development environment with identical steps.
- Compare exported CSV data against database entries for the same filters.
- Query the database directly to confirm "Active" trials exist between September 16–30.
- Inspect whether filters, timezone mismatches, or status flags exclude records incorrectly.
- Compare database queries between CSV export logic and UI display logic.
- Review logs and recent code deployments.
- Investigate potential caching mismatches.

#### Ticket #2848 – Swiss Pricing Update

- Reproduce the issue in a development environment.
- Inspect validation logic to confirm whether the system enforces a strict 10% price increase cap (e.g., 150 to 165.50 is +10.33%).
- Verify if new constraints were recently introduced.
- Check for misinterpretation between decimal formats (`160.50` vs `160,50`).
- Compare pricing constraints applied to Switzerland with those for other countries.
- Review logs and code changes from the previous month.

#### Ticket #2789 – Bulk Update Feature Request

- Gather more information from stakeholders about requirements and expectations.
- Check whether a bulk update API already exists.
- Review potential database impact and role-based access considerations.

---

### Proposed Solutions

#### Ticket #2847

- Short-term workaround: Allow manual CSV export by extending the query range.
- Long-term fix: Align export query logic with the UI’s query logic.
- Validate with QA to ensure consistency across all data export methods.

#### Ticket #2848

- Configure the percentage threshold if necessary

#### Ticket #2789

- Implement a bulk update API (if not already available).
- Optimize backend logic with batch database queries.
- Provide a UI for selecting multiple entries and applying updates.
- Add role-based access controls if applicable

---

### Implementation Estimates

- **Ticket #2847** – 8–12 hours (investigation, fix, deployment, QA)
- **Ticket #2848** – 6–10 hours (investigation, fix, deployment, QA)
- **Ticket #2789** – 6–8 days (requirements gathering, design, backend + frontend implementation, deployment, QA)

---

## Overall Assessment

The overall assessment is that regulatory compliance-related tickets (such as data exports) must always take precedence, as they directly impact deadlines and reporting obligations. Issues related to validation thresholds (pricing updates) should follow, while feature requests can be scheduled for upcoming sprints.

To prevent similar issues, we recommend:

- Strengthening regression testing for export logic, validation rules, and pricing thresholds.
- Introduce structured logging for future troubleshooting
- Documenting business rules clearly (e.g., pricing thresholds per market).
- Establishing proactive client communication when critical workflows are blocked.
