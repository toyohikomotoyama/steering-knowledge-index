Date: 2026-05-18
Related: [[2026-02-03-download-confirmation-change]]
Status: waiting for confirmation

# Account data export change

Release planned for May 29. Reviewed the request: three UI changes and one export-condition change.

## TODO
- [x] inspect current UI
- [x] inspect impact on export flow
- [ ] waiting for answers on two specification questions
- [ ] implement
- [ ] test
- [ ] real-device verification
- [ ] update docs

## Current behavior

The request says "confirm the target count on the export confirmation screen before executing."

There is no export confirmation screen in the current UI.

This wording looks similar to the February download change.

Check [[2026-02-03-download-confirmation-change]].

That request also assumed a confirmation screen existed, but it did not, and implementation had to wait for an external answer.

There are 11 days until release this time. If we ask late, the real-device verification window may get compressed, so ask now.

Questions:
1. should a new confirmation screen be added this time as well?
2. does "target count" mean the number currently shown in the UI, or the number re-evaluated by the server?

Requested answer by May 21.

## Check export flow

The current API receives search conditions and re-selects the export target on the server.

It does not simply receive the count displayed in the UI.

So if the confirmation screen only shows the client-side count, the displayed number may differ from the final export count depending on conditions.

The second question is necessary. Do not implement the UI before the answer.

## Current state

Investigation is complete for what can be determined from the code.

Waiting on two external answers.

If there is no answer by May 21, recheck the schedule because this will affect the planned May 25 real-device verification.
