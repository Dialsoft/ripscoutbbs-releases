**RipScout BBS 2.3.1: Updating from older versions**

- **Updating from 2.1.0 or older no longer leaves you with what looks like a dead board.** Those versions start the new copy without saying which copy it replaces, and the way the new copy looked for the old one could miss it. It now finds every other running RipScout and makes sure it's gone (closing it if it hasn't quit within 15 seconds), so only one board is left running.
- **A fresh Control Panel opens after such an update.** Older versions didn't remember the Control Panel's address, so the new copy came up on a different one and your open panel just said "BBS not responding". When that happens now, the new version opens a new panel window by itself. Close the old one.
- Updates from 2.1.1 and newer already worked this way. Nothing else changed.
