# Quickdevelopers PS4

Made by Quickdeveloper

Browser host build for PS4 firmware 14.00, based on the public
silverps4hen host (HEAD a265f79) with our safety fixes applied
— see FIXES_AUDIT.md for the verified list, including the
hard-disabled unfinished-trigger route.

Status: chassis + fixes in place. The payload slot
(site/payload.bin) holds our GoldHEN 14.00 build — 293,555
bytes, SHA-256 e6627458…, verified byte-for-byte in the repo
and on the live site. Note: site/goldhen.bin is NOT that
build; it is the chassis's misnamed ≤13.52 stock HEN. The
exploit chain itself is still incomplete — see
CHAIN_STATUS.md for exactly what is missing.
