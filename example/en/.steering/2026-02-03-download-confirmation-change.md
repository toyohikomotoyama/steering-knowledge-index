Date: 2026-02-03
Related: [[2026-01-12-legacy-encoding-investigation]]
Status: completed

# Add confirmation before download

Requested change: "show the notice before downloading."

Release planned: Feb 13
Real-device verification available: Feb 10

## TODO
- [x] inspect current flow
- [x] confirm specification
- [x] implement
- [x] test
- [x] Feb 10 real-device verification
- [x] update docs

## Current flow

The button currently calls the download API immediately.

The request says "show the notice on the confirmation screen," but the current UI has no such confirmation screen.

I originally assumed this was only a wording change to an existing screen, but a new UI may be required. Do not add a dialog without confirmation.

Questions to external owner:
- should the "confirmation screen" be newly added?
- should the download start only after OK/Continue?
- should Cancel avoid sending the request entirely?

Response expected Feb 6. Until then, leave this item unimplemented and continue with the other changes.

## Feb 6 response

Add a new confirmation dialog.

"Continue" downloads. "Cancel" does nothing. Do not call the API when the dialog first appears.

Implementation can proceed.

## Implementation

Changed the flow so the existing download function is called from the dialog's Continue action.

Tested that Cancel does not call the API.

Code is complete. Real-device verification is on Feb 10.

## Feb 10 real-device verification

Checked on desktop browser and smartphone.

- dialog appears: OK
- download after Continue: OK
- no request on Cancel: OK

Added the current behavior to `docs/SPEC.md`. Complete.
