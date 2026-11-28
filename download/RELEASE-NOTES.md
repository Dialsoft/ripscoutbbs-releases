**RipScout BBS 2.10.0: STS-style Teleconference**

- **Line tags, STS style:** every chat line starts with the speaker's tag, `72#4[T1]:MaxMouse) hello`: the board's **station number**, `#` the caller's line, and the channel in brackets. `< >` marks the sysop and co-sysops, `[ ]` members.
- **Station number:** a new setting (Control Panel > Settings). Callers on linked boards show their own board's station number, so give every board on a link network its own number.
- **Channels:** everyone starts on T1; `/T2` tunes to channel 2 (1-99). Each channel is its own room, across linked boards too. `/T` shows your channel and `/who` lists everyone on every channel.
- **Private messages** by station code: `/#4 message` (line 4 here) or `/72#4 message` (line 4 on station 72). The older `/A07` still works.
- Linked boards running an older version still chat fine; they show with a letter (A07) instead of a station number.
