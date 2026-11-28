**RipScout BBS 2.9.0: Caller locations and Finger**

- **Caller locations** (only you see them): where each call comes from, worked out from the caller's internet address.
  - In the **Log** ("call from 203.0.113.9 (Wharton, New Jersey, US)"), the Dashboard's **Callers online**, **Users** (*Last called from*), **Audit** (a *Where* column, also in the CSV export) and **Sysop Admin > Nodes** in RIP.
  - A member logging in from a **country they've never called from** gets a security entry in the Audit trail.
  - Each new address is looked up once (ip-api.com, ipwho.is as a backup) and remembered for 30 days. Addresses on your own network are never sent. Turn it off in Settings > **Caller locations**.
- **Finger app** on the Main Menu: callers can finger anywhere on the internet (`@host`, `name@host`, `/W`), a member of this board, or see who's on here. Long answers scroll. It always uses port 79, and only staff may finger addresses on the board's own network.
- **Finger server** (RFC 1288, port 79): replies are now cleaned of control characters and every line ends with CRLF, as the RFC asks; forwarding requests are politely refused.
