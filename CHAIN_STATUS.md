# Chain status — what is complete and what is not

Project: Quickdevelopers PS4 (browser host for PS4 firmware 14.00),
based on the public silverps4hen raw13 host at commit a265f79,
with the Quickdevelopers fixes applied (see FIXES_AUDIT.md).

## Working

- Version-select landing page and the firmware-detect loader.
- The jailbreak page flow for firmwares in its kernel table.
- 14.00 stage 1 (`hx1400_port.js`): builds the browser primitive
  and completes kernel-base discovery, then stops at its own
  milestone message. Verified behaviour; it does not proceed
  further by design.
- The payload slot: `site/payload.bin` holds our GoldHEN 14.00
  build (SHA-256 e6627458…), byte-verified.

## Incomplete — the kernel stage (`hx_kevent.js`)

The file's setup work exists (socket spray, memory arena,
trigger invocation), but the run aborts partway at three
unimplemented placeholder functions, which currently just
raise a TODO error. The run's own final message is
"KEVENT-STAGE1-PARTIAL". The three missing pieces, in order:

1. The scan that locates the victim socket's kernel address
   after the spray.
2. The construction that turns the bug into kernel read/write,
   together with its readback verification.
3. The final hook installation that would hand control to a
   payload stage.

The `&kv=1` route that enters this file is deliberately
disabled in this build until the stage is complete
(see the [qd] note in jb.js).

## Absent — everything after the kernel stage

No code exists yet for the steps that would follow kernel
read/write: applying the kernel patch set, preparing the
payload in memory, and launching it. The patch bytes
themselves are known and disc-verified (FIXES_AUDIT.md §4);
the browser-side machinery that would apply them is not
written.

## Reference

A completed version of this same chain exists in the BD-J
(disc) lineage of the project; its stage layout is documented
in the project's own teardown notes (private archive repo
quickky1/bonk-hen-1400-archive and the goldhen-1400-port
docs). The browser port above is what remains unfinished.
