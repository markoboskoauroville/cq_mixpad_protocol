# TAKEOVER: cq_mixpad_protocol (8.11.2026)

**What it is:** the public database of the CQ-12T's private protocol. Source of truth for every app of Marko's that talks
to a CQ (CQ_MIXPAD_MANTRA, CQ MixPad Tutorial, DJ Mantra).

**How the data was made:** `tools/cq_mitm.py 192.168.1.62 --local` on the Mac; CQ MixPad for Mac → Other… → 127.0.0.1;
one click at a time (log with its time in `captures/<time>/actions.log`); `tools/cq_tour.py --last N`; the value put back
with `tools/cq_inject.sh`; `tools/cq_dump.py <first> <last>` must show zero differences at the end. The raw captures
(about 80 MB) stay on the Mac in CQ_MIXPAD_MANTRA/captures, not in git.

**Next (the calibration pass):** frequency (HPF, PEQ), time (gate, comp) and ratio/width scales are known at two points
only; drag each knob, read the value on screen and the raw number in the dump, fit the curve, add it to protocol.json
`scales`. Then the untoured parts (Main LR EQ/comp, Out1–6 PEQ, Ip2–10, ST IN, FX inner tabs, meters 9/24).

**Rules:** no +48 V, sample rate, scene/Library recall or network in a test; never Stereo Link; MixPad on the Pixel closed
while sniffing (one client at a time). Keep protocol.json and PROTOCOL.md in step; whoever finds a new code adds it here
first and the apps follow.
