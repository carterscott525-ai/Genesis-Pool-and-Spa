# Scheduling — Calendly / Google Calendar integration (open item)

**Status: not started — note only.**

`schedule.html`'s `#request-form` currently just collects a *preferred* date/time as free
text fields (`Preferred Date`, `Preferred Time`, `Alternate Date`) and submits via
FormSubmit.co to `contact@genesispoolandspa.com`, plus logs to the Booking Requests sheet
(see [`google-apps-script.md`](google-apps-script.md)). Nothing actually checks
availability or books a real calendar slot — a lead can "request" a date/time that's
already taken, double-booked, or outside working hours, and someone has to manually
reconcile that against the real schedule before confirming.

**Open item: connect a real scheduling tool** (Calendly, Google Calendar's built-in
appointment/booking-page feature, or similar) so `#request-form` becomes an actual
availability-aware booker instead of a "we'll get back to you" request form:

- **Calendly** — embed a Calendly inline widget or redirect to a Calendly booking page in
  place of (or alongside) the current date/time fields. Handles availability, buffer
  time, and confirmation/reminder emails natively; would likely replace the `Preferred
  Date` / `Preferred Time` / `Alternate Date` fields rather than sit next to them.
- **Google Calendar appointment schedule** — Google Calendar has a native "Appointment
  schedule" booking-page feature (Workspace/personal accounts) that could serve the same
  role, and would keep everything inside Google's ecosystem alongside the Booking
  Requests Sheet and Apps Script already in use.
- Either option needs a decision on how it interacts with the existing flow: does it
  *replace* the FormSubmit + Sheet-logging submit path, or does the lead still submit the
  existing contact/property-details form and get handed to Calendly/Google Calendar as a
  second step to actually pick a confirmed slot?

**Open questions**:
- Which tool — Calendly (paid tiers unlock more features, e.g. team routing) vs. Google
  Calendar (free with an existing Google account, less polished embed).
- Whether the existing property/contact-detail fields (address, property type, notes)
  stay as a form Calendly/Google Calendar can collect as custom questions, or remain a
  separate step.
- Whether confirmed bookings should still log to the Booking Requests Sheet (for
  consistency with the [Twilio "reply YES" flow](twilio.md)), which would need
  Calendly/Google Calendar's own Zapier trigger wired into the same Sheet.
