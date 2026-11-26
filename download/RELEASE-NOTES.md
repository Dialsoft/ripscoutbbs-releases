**RipscoutBBS 1.9.0: Stats and the audit trail**

- **BBS Stats** (main menu): the board by the numbers. Calls, members, messages, files, downloads and words typed; calls over the last 14 days; the busiest hours; records like the busiest moment ever.
- **Leaderboards:** the top 5 for calls, posts, words typed, time online, downloads and uploads.
- **My Stats:** every caller's own numbers.
  - Logins, time online and longest visit, plus calling streaks.
  - Words typed (posts, email and chat), chat lines, email sent and received.
  - Uploads and downloads, games played, best high-score placing, Word Duels won and lost.
  - Charts of when they call, and **10 badges** to earn.
- Sysops can open any member's stats from Sysop Admin > Users.
- **Audit trail** (Sysop Admin > Audit Trail, and the Control Panel's new **Audit** tab):
  - Records logins and logoffs, failed logins, signups, and security events (passwords, two-factor, locks, deletions).
  - Records sysop actions, and **every settings change with its old and new value** (passwords show only "changed").
  - Also covers uploads, downloads and deletions, and network and system events.
  - Filter by kind and search in both places; the Control Panel can save it as a CSV file.
  - Kept in audit.log in the data folder.
- Fixed: a few counts said "entrys" and "guesss".

The newer counters (time online, words, transfers and so on) start counting on the first visit after this update.

If you're running 1.8.1, this update installs itself the next time nobody is online.
