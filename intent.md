# Intent: Results-ready notification

Sources: `inputs/01-approved-prd.md` (PRD N-17, approved 2026-09-18), `inputs/02-current-meeting.md` (discovery notes, 2026-09-20). `inputs/03-superseded-request.md` (2026-08-02) is superseded by PRD N-17 and is history only; nothing in this intent is approved on its basis.

## Problem

- **I-1** The clinic receives avoidable calls from patients checking whether their results are ready. PRD: "The fictional clinic receives avoidable calls from patients checking whether results are ready."
- **I-2** These status-only calls are a different problem from patients calling with clinical questions, and the two must not be mixed. Support lead: "count status-only calls separately from clinical questions."

## Proposed outcome

- **I-3** When a doctor has reviewed and released a result, the patient is told that an update is available and is directed to the existing authenticated portal to view it. PRD: "Notify the patient when a result is ready for their review in the existing authenticated portal."
- **I-4** "Ready" means reviewed and released by a doctor. A finished laboratory upload alone is not "ready". Product owner: "'Ready' must not mean that the laboratory upload merely finished."

**How success will be observed**

- **S-1** Fewer results-status (status-only) calls, counted separately from clinical questions and compared against a measured baseline. PRD: "Measure reduction in results-status calls and delivery failures." Support lead: "compare against a measured baseline."
- **S-2** Delivery failures are measured and handled without duplicate patient messages.
- **S-3** No numeric target is approved. PRD: "The baseline and numerical target will be agreed during discovery; none is approved yet." The 90% / one-week target in the superseded request is not adopted.

## Affected users and systems

- **Patients** receive the notification and view the result in the portal.
- **Doctors (reviewing clinicians)**: their review-and-release step is the gate for any notification.
- **Support team**: the main beneficiary of fewer status calls; they classify calls as status-only or clinical.
- **Laboratory**: uploads results. An upload does not trigger a notification by itself.
- **Existing authenticated patient portal**, via the existing portal result route. Engineering lead: "the existing portal path is adequate."
- **Notification delivery and logging**: the channel is not chosen yet (see OPEN-1). Logs must not contain clinical detail.
- **Product, privacy, clinical and engineering owners** hold the open decisions.

## Out of scope

The approved scope is a notification that uses the existing portal result route, with duplicate prevention and delivery-failure handling. Excluded:

- **X-1** Portal redesign (PRD scope).
- **X-2** A new or rebuilt patient dashboard. Engineering lead: "A new patient dashboard is a separate project." The rebuild came only from the superseded request.
- **X-3** Appointment scheduling (PRD scope).
- **X-4** Clinical interpretation of results (PRD scope).
- **X-5** Notifying third parties (PRD scope). The referring doctor is not approved as a recipient and is tracked as OPEN-2, not as approved scope.
- **X-6** Everything only the superseded request asked for: sending results automatically on lab upload, putting test results in an SMS, copying the referring doctor, and a 90% call-reduction target.

## Constraints

- **C-1** A doctor must review and release the result before any notification is eligible (PRD, confirmed by the product owner on 2026-09-20).
- **C-2** The message says only that an update is available and points to the existing authenticated portal. No diagnosis, test name or result, clinical measurement or treatment detail may appear in the message or in notification and operational logs. PRD; privacy owner: "no sensitive content in messages or operational logs."
- **C-3** Access to results is authenticated in the existing portal. No result attachments and no public result link (PRD).
- **C-4** At most one notification per released result version. Delivery failures are handled without sending the patient duplicate messages (PRD).
- **C-5** Only the patient is an approved recipient. No other recipient may be added without a decision by its owners (PRD; clinical owner: "do not infer approval for referring-doctor notifications from the old request").
- **C-6** Consent must be reviewed before a channel is chosen (privacy owner: "Consent and the actual channel are still under review").

## Open questions and owners

- **OPEN-1 Channel** (SMS, email or portal push). Owners: **product owner and privacy owner**, after a consent and delivery review. Not decided. SMS is not assumed, even though the superseded request asked for it.
- **OPEN-2 Referring-doctor notification**: whether the referring doctor may be notified, and on what recipient authorization. Owners: **clinical owner and privacy owner**. "It is not approved by this PRD."
- **OPEN-3 Baseline and numeric target** for the reduction in status calls. To be agreed during discovery. The sources name no owner for this decision, so none is assigned here. Until it is agreed, the target stays `null`.

## Decision record

```json
{
  "notifyWhen": ["doctor-reviewed", "released"],
  "messageMayInclude": ["update-available"],
  "inScope": ["existing-portal-route", "duplicate-prevention", "delivery-failure-handling"],
  "openDecisions": [
    {"topic": "channel", "owners": ["product", "privacy"]},
    {"topic": "referring-doctor-notification", "owners": ["clinical", "privacy"]}
  ],
  "successMeasures": ["status-only-calls", "delivery-failures"],
  "numericTarget": null
}
```
