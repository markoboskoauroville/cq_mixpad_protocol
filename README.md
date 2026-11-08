# cq_mixpad_protocol

The database of codes for the **Allen & Heath CQ** mixers (decoded on a CQ-12T): every message that CQ MixPad sends
to the desk and the desk sends back, found by sniffing the real MixPad. Any app that wants to control a CQ can take its
knowledge from here.

- **[protocol.json](protocol.json)**: machine-readable: transport ports, message form, channel numbers, value scales,
  every decoded parameter, and the list of what is not decoded.
- **[PROTOCOL.md](PROTOCOL.md)**: the same in prose and tables, with the method.
- **[tools/](tools/)**: the sniffer and helpers used to decode it.
  - `cq_mitm.py DESK_IP --local` man-in-the-middle between CQ MixPad (Mac, "Other…" → 127.0.0.1) and the desk; records
    every packet, can inject messages on MixPad's own session (port 51399).
  - `cq_tour.py` shows every message under the click (from an `actions.log`) it followed.
  - `cq_dump.py` extracts the desk's full state dump at connect and diffs two captures.
  - `cq_inject.sh "f7 0c 0c 0d 00 00 56 5e"` sends hex messages through a running sniffer.

## At a glance
- TCP 51326 (one client at a time): `F7 BB BB PP CH SS VL VH` messages, `7F ...` frames; UDP stream with meters and the
  31-band RTA. TCP 51325 is the documented MIDI port (faders, mutes, scenes).
- dB values are `(raw − 0x8000)/256`. Frequencies and times are logarithmic and **not yet calibrated**.
- The desk never echoes a change to the client that made it; read the true state from the connect dump.
- **Danger:** Stereo Link copies the odd channel onto the even one and unlinking does not undo it.

## Status (8.11.2026)
Decoded: inputs (source, gain, trim, polarity, HPF, gate, 4-band PEQ, compressor, AutoGain, names, colours), mix
(levels, sends, pans, mutes, mute groups, DCAs), outputs, FX1/FX2, AMM, USB routing, SD transport and arming, RTA.
Not decoded yet: see `not_decoded` in protocol.json. Left out on purpose: +48 V, sample rate, scene/Library recall, network.

## Use it, and add to it
Unofficial and independent: not made or endorsed by Allen & Heath. Use it only on your own mixer and network. Add a
parameter the same way it was found: one click, one log line, diff the traffic, put the value back, check with
`cq_dump.py`. MIT licence, see [LICENSE](LICENSE).

Used by: [CQ_MIXPAD_MANTRA](https://github.com/markoboskoauroville/CQ_MIXPAD_MANTRA) (Android app, Sonnet mixing assistant).
