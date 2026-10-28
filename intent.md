# Intent: Results-ready notification

Sources: `inputs/01-approved-prd.md` (PRD N-17, approved 2026-09-18), `inputs/02-current-meeting.md` (discovery notes, 2026-09-20). `inputs/03-superseded-request.md` is history only and approves nothing.

## Problem

The fictional clinic receives avoidable calls from patients who only want to know whether their results are ready. Patients have no proactive signal, so they phone the clinic to check.

## Proposed outcome

**I-1.** A patient is notified when a result is ready for their review in the existing authenticated portal.

**I-2.** A notification is eligible only after a doctor has reviewed and released the result. A finished laboratory upload alone does not make a result "ready".

**I-3.** The message says only that an update is available and directs the patient to the existing authenticated portal. It contains no diagnosis, test name, test result, clinical measurement or treatment detail, and none of these appear in notification logs.

**How success will be observed:**

- Results-status calls fall, measured against a baseline. The support lead counts status-only calls separately from clinical questions.
- Delivery failures are tracked and handled without sending duplicate messages to the patient.
- The baseline and any numerical target are still to be agreed in discovery. None is approved, so none is stated here.

## Affected users and systems

- **Patients** who receive the notification and sign in to the portal.
- **Clinic support staff** who take the status-check calls and measure them.
- **Doctors**, whose review and release is the gate for every notification.
- **Existing authenticated patient portal** and its current result route, which is used as is. Results are not attached and no public result link is introduced.
- **Notification delivery**, which must avoid duplicates for the same released result version and handle delivery failures.

## Out of scope

- Portal redesign and a new patient dashboard (a separate project, per the engineering lead).
- Appointment scheduling.
- Clinical interpretation of results.
- Notifying third parties. Referring-doctor notification is not approved and is tracked as an open question (OPEN-2).
- Everything that exists only in the superseded early request: sending every result at upload time, putting the result in an SMS, copying the referring doctor, rebuilding the dashboard and the 90% call-reduction target.

## Constraints

- **C-1.** A doctor must review and release a result before any notification is eligible.
- **C-2.** No clinical detail in messages or operational logs.
- **C-3.** Access is authenticated in the existing portal: no attached results and no public result link.
- **C-4.** No duplicate notification for the same released result version, including when delivery fails.
- **C-5.** Use the existing portal result route.

## Open questions

- **OPEN-1. Channel** (SMS, email or portal push). Owners: product owner and privacy owner, after consent and delivery review. Not decided.
- **OPEN-2. Referring-doctor notification.** Owners: clinical owner and privacy owner, who decide whether the recipient is authorized. Not approved by the PRD and not inferred from the old request. It is decided separately from the patient notification.

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
