# craft-badge-poc

Responsible-disclosure proof-of-concept for a PII exposure at Craft Conference 2026.

## The issue
The QR printed on attendee badges encodes the bare Tito ticket ID (e.g. `ti_…`).
That ID maps to `https://ti.to/tickets/<id>.json`, which returns the attendee's
full registration record — name, employer, work + personal email, mobile number,
and demographic answers — with **no authentication**. The endpoint also has no
CORS allowance, so it can't be read cross-origin via `fetch()`, but a top-level
browser navigation to the URL renders the data directly.

So a clear photo or scan of a badge QR + knowledge of the URL pattern is enough
to read that attendee's personal data. It is **not** a bulk leak: the IDs are
high-entropy and unguessable, so attendees can't be enumerated.

## This PoC
`index.html` is a static page (camera QR scanner) that demonstrates the chain.
It is **deliberately gated**: it only reveals data for the author's own demo
badge and refuses every other ID, so it cannot be used against other attendees.

Reported privately to the organisers. Found on my own ticket only.
