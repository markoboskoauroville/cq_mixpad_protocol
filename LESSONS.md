# LESSONS: cq_mixpad_protocol

- **8.11.2026:** Stereo Link in Config › Inputs copies the odd channel onto the even one (EQ, HPF, fader, pan, sends, preset
  label); unlinking does not undo it. Never toggle it in a test.
- MixPad's knobs answer only a slow horizontal drag (`cliclick dd/dm/du`), not the wheel or a fast drag. A MixPad window
  capture came back stale: capture the screen instead.
- The desk never echoes a change to its sender. After each test, reconnect and diff the connect dump against the first one.
- The sniffer must keep its UDP bridges when MixPad reconnects (a new MixPad UDP port every time).
- Claiming "every function decoded" after a first sweep was wrong: keep an explicit not-decoded list next to the codes.
