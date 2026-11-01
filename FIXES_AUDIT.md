# Fixes audit — Quickdevelopers PS4 web build vs the disc build

Made by Quickdeveloper

The disc's test2 build cut three things out of the leaked
material. This audit confirms the web build carries the same
posture. Checked 2026-11-01 against the chassis copied from
silverps4hen HEAD a265f79.

## 1. Author test-server callback — ABSENT ✔

Disc fix: Boot.io() POSTed to the author's test server
(tester.f5rvoice.com:1992); test2 reduced it to a null return
and overwrote the endpoint string.

Web check: `grep -ri f5rvoice` over the whole tree = 0 hits.
`grep` for absolute http(s) URLs in all .js/.html = 0 hits.
Every fetch in the host is a same-origin relative path.
There is no callback to remove because the web host never
had one — verified, not assumed.

## 2. Log upload — DISABLED in 14.00 mode ✔

Disc fix: the Boot flow's server reporting was cut entirely;
the shipped disc uploads no logs in any mode.

Web check: jb.js `post()` opens with the HX1400_POST_DISABLED
guard — when `hx1400=1` it returns before the `POST /t`
XMLHttpRequest is constructed. On-page logging still works
(that is the ?log=1 console the tester reads); nothing leaves
the console. This matches the disc's posture: telemetry is
local, read by a human, never transmitted.

## 3. Payload — BYTE-VERIFIED ✔

site/payload.bin SHA-256
29ec095fbede651e29954de43411599093138791e62105aaee6fdced58edec06
= the original disc's hen.bin, byte-for-byte (the same value
proven during the Bonk teardown). When stage 5 swaps in our
GoldHEN, the replacement is goldhen_1400.bin, SHA-256
e66274586b895479eeedcbf69a5e694a72216f7ea0e734b1f6d55e4dd0c4b355,
itself diff-verified against the pristine GoldHEN core
(213 bytes, 100% inside the recorded write ranges).

## 4. Kernel patch bytes — DISC-VERIFIED ✔

The PKB carried by this lineage hashes to 165d2ad9… — the
value re-derived from the original disc's own templates
during the 2026-10-31 one-byte investigation. Our documents
were corrected to the disc then; the web build inherits the
disc's bytes, not a transcription.

## 5. Unfinished trigger route — DISABLED in this build ✔

The public kevent stage fires the real -23 kevent trigger
and then aborts at three TODO stubs. In the raw chassis it is
one query parameter away (&kv=1). In site/jb.js the route is
hard-disabled (`false && params.get("kv")`) with a [qd]
comment, so the deployed page cannot reach it until our
completed stage-3 chain replaces the stub.

## 6. Residual carried knowingly

patch_+0x15C at the 13.52 donor value (0x76C270) — the same
single open risk as the disc build, disclosed in its release
notes and in BUILD_PLAN.md here. Nothing else from the disc's
fix list is outstanding.
