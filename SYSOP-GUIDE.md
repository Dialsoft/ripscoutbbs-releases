# RipscoutBBS Sysop Guide

This guide covers everything you need to run RipscoutBBS: installing it, letting callers reach it, and
managing it from inside the BBS.

## 1. Installing

1. Make a folder such as `C:\RipscoutBBS` and put `RipscoutBBS.exe` in it.
2. Double-click `RipscoutBBS.exe`. There is no console window. Instead:
   - the **wheel icon** appears in the system tray, next to the clock (Windows may tuck it under the ^ arrow:
     drag it onto the taskbar to keep it visible). Hover over it to see how many callers are online.
   - the **Control Panel** window opens.
3. **Right-click the tray icon** for: Open Control Panel, Open Data Folder, Check for Updates and
   Shut Down RipscoutBBS. Double-clicking the icon also opens the Control Panel.
4. Starting the EXE a second time doesn't start a second board. It just opens the running board's Control Panel.

### The Control Panel
The Control Panel is a window on the server PC (it uses Microsoft Edge's app mode, so it has no browser bars).
It is served to this PC only and protected by a secret key that changes every time the board starts, so
nobody else, on the network or on this PC, can use it. Hover over anything for an explanation.

| Tab | What it does |
|---|---|
| Dashboard | Callers online (with Page and Disconnect), broadcast to everyone, calls today and in total, members, messages, uptime, a 14-day calls chart, and the addresses callers can use |
| Users | Search accounts, change security levels, lock or unlock, reset passwords, reset two-factor (2FA), delete |
| Settings | Every board setting (including the finger port), System News and Board Rules. Saving applies them live |
| Files | The File Libraries: drag files from Windows onto the box to add them, write descriptions, delete, make new libraries, scan folders |
| Networks | FTN echomail (address, hub, passwords, Poll now, AreaFix) and Usenet (news server, login, Check now, Add group), each with a log of recent calls |
| BMX News | The Newsmap feed list and a Refresh button |
| High Scores | Top 10 for every BMX Game |
| Updates | Version, latest on GitHub, Check now, Install now, auto-install on/off |
| Audit | The audit trail: filter by kind, search, and save as CSV (see *Audit Trail* below) |
| Log | Everything logged since startup (also saved in `RipscoutBBS.log`) |

The first time it runs, RipscoutBBS creates a folder named `RipscoutBBS-data` next to the EXE. Everything
the board remembers is in that folder (see section 14). To back up the board, copy that folder.

### Command-line options (optional)

| Option | Example | Meaning |
|---|---|---|
| `-port` | `RipscoutBBS.exe -port 2323` | Listen on a different port (also changeable live from Sysop Admin) |
| `-data` | `RipscoutBBS.exe -data D:\BBSDATA` | Keep the data folder somewhere else |

### Starting it automatically when Windows starts

The simplest way is Task Scheduler: create a task that runs `C:\RipscoutBBS\RipscoutBBS.exe` **At startup**,
with "Run whether user is logged on or not" selected. Set "Start in" to `C:\RipscoutBBS`.

## 2. Letting callers in

1. **Windows Firewall.** Open Command Prompt **as Administrator** and run:
   `netsh advfirewall firewall add rule name="RipscoutBBS" dir=in action=allow protocol=TCP localport=23`
   (Use your port number if you change it.)
2. **From the internet:** if the PC is behind a router, forward TCP port 23 to the PC's local address.
   If the server already has a public IP, the firewall rule is all you need.
3. Some internet providers block port 23. If outside callers can't connect, switch to another port such as
   2323 (Sysop Admin > Board Settings > Telnet port) and forward that port instead.

## 3. Connecting with DialComm

Add a directory entry with **Connection: Telnet**, the server's address, the port, and
**Emulation: RIPscrip**. When you connect, RipscoutBBS sends the standard RIPscrip check (`ESC[!`) and
DialComm answers automatically. If the emulation is set to ANSI, the board shows a text screen explaining
how to switch.

**How the screens work in DialComm:**
- **Click** any button, or press its **underlined letter**.
- **Input boxes** (name, password, email...) work like on a web page: type, then press **Enter** to move
  to the next box. **Esc** cancels. Buttons on screens with input boxes have no letter shortcuts, so typing
  can never press one by accident; click them instead.
- **Lists** (messages, users, nodes) are clickable: click a row to open it.

## 4. Becoming the Sysop

The **first account ever created** becomes the Sysop (security level 100). Call in, choose
**New User Signup**, and the last signup step will say "this account becomes the SYSOP". After that, your
main menu has a yellow-shield **Sysop Admin** button.

## 5. Security levels

| Level | Name | Can do |
|---|---|---|
| 10 | New User | Read and post where areas allow New Users |
| 20 | Validated | Default for new signups |
| 50 | Trusted | Use for areas you want to limit |
| 90 | Co-Sysop | Everything in Sysop Admin except changing a Sysop's account |
| 100 | Sysop | Everything |

Each message area has its own "who can read" and "who can post" level.

## 6. Sysop Admin

Everything is done inside the BBS, in RIPscrip, and **every change takes effect immediately for every
caller**. Nothing needs a restart.

### Users
Click a user to open their account card.
- **Security level:** click one of the radio buttons (or press 1-5).
- **Lock / Unlock:** a locked account can't log in, and anyone using it is disconnected at once.
- **Reset Password:** makes a temporary password such as `kite-4821-moss` and shows it to you. The caller
  logs in with it once and is asked to choose a new one.
- **Edit Email**, and **Delete Account** (the caller's messages stay on the boards).
- You can't demote, lock or delete yourself, and a Co-Sysop can't change a Sysop.

### Board Settings
Click a row (or press its letter) to change it.
| Setting | Notes |
|---|---|
| BBS name, Sysop name, Tagline, Location | Shown on the screens. The tagline appears on the welcome screen |
| Telnet port | The board switches to the new port within a second, and callers already online stay connected. Update your firewall and router to match |
| Max nodes | How many callers at once (1-250) |
| Idle timeout | Minutes of no typing before a caller is hung up |
| New user level | Click to cycle New User / Validated / Trusted |
| Allow new signups | When OFF, the welcome screen shows "Signups Closed" |
| Require email at signup | When OFF, email is optional |
| Authenticator name | The name callers see for this board in Google Authenticator. Blank = the BBS name. Changes apply to new setups only |
| Finger server port | 79 is standard; 0 turns finger off. See *Finger* below |

### Message Areas
**Add Area**, or click an area to rename it, change its description, change its read/post levels
(click to cycle), or delete it with all of its messages. **Echo Tag** shares the area with an FTN network,
and **Newsgroup** mirrors a Usenet newsgroup (see Networks below). An area can be shared with one network;
leave both blank for a local-only area.

### Networks
The **Networks** tile offers two kinds of shared message areas: **Echomail** and **Usenet**.

### Echomail (FTN)
Echomail connects your message areas to the big BBS message networks: **fsxNet**, **FidoNet**, AgoraNet and
other FTN ("FidoNet Technology") networks. A message posted in a shared area here appears on every board in
the network, and theirs appear here. The board uses **binkp**, the standard way FTN boards swap mail over the
internet, and always calls *out* to its hub, so there is nothing to open on your router.

**Getting a node address.** You need an address from the network's coordinator. fsxNet is the friendliest
place to start (free; apply at fsxnet.nz). The reply gives you:
- **your address**, like `21:4/123`
- your **hub's address** (like `21:4/100`) and **internet host** (like `agency.bbs.nz:24556`)
- a **session password**, and sometimes separate packet and AreaFix passwords.

**Setting it up** (Sysop Admin > Echomail, or the Control Panel's Echomail tab). Click any row to change it.

| Setting | What it is |
|---|---|
| This board's address | Your node address, `zone:net/node` |
| Network name | The network's short name, e.g. `fsxnet` (sent with your address) |
| Hub address / Hub internet host | Your uplink. Without a port, 24554 is used |
| Session password | Checked with CRAM-MD5, so the password itself never crosses the internet (when the hub supports it, as nearly all do) |
| Packet password | Written inside each mail packet (up to 8 letters). Blank if your hub uses none |
| AreaFix password | For the hub's AreaFix robot |
| Poll every | Minutes between calls (15 is typical, 5 the minimum) |
| Origin line | The `* Origin:` line under every message you send. Blank = the board name |
| New echoes | Make a message area automatically for each new echo tag, or ignore unknown tags |

Then press **Turn On**. **Poll Now** calls the hub right away, and the screen shows the result.

**Picking echoes.** Press **AreaFix** and type the tags to join, like `+FSX_GEN +FSX_BBS +FSX_RETRO`
(`-TAG` leaves one; `%LIST` asks for the full list). The hub's reply arrives as netmail in the sysop-only
**Netmail** area. New echoes then appear as message areas by themselves. To share an existing area, give it
an echo tag in Message Areas; only posts written after that are sent out, so old local messages don't flood
the network.

**How it works**, if you are curious: new local posts are packed into a mail *packet* (FTS-0001 type 2+) in
`RipscoutBBS-data\ftn\outbound`. On each poll the board logs in to the hub, sends the packets and receives
new mail (ZIP bundles of packets). Each message is filed into the area for its `AREA:` tag. Duplicates (same
MSGID) are skipped, and the routing lines (SEEN-BY, PATH, kludges) are hidden from readers. Messages from the
network show the sender's address next to their name. Files that are not mail (nodelists, .tic files) are kept
in `ftn\bad` for you. **Log** shows what happened on recent calls.

### Usenet newsgroups
Usenet is the original internet-wide discussion system: tens of thousands of *newsgroups* such as `alt.bbs`,
`comp.misc` or `rec.bicycles.misc`. RipscoutBBS reads them from a **news server** with NNTP and shows each
newsgroup as a message area. Replies and new posts written here go out to Usenet on the next check.

**Getting a news server.** **Eternal September** (eternal-september.org) is free: register with your email
and you get a login for `news.eternal-september.org`. Your internet provider or a paid Usenet service work too.

**Setting it up** (Sysop Admin > Networks > Usenet, or the Control Panel's Networks tab). Click a row to change it.

| Setting | What it is |
|---|---|
| News server | Host name, e.g. `news.eternal-september.org` |
| Port / Encryption | 119 for plain NNTP, or 563 with encryption (TLS) on. Switching encryption switches the port too |
| Login name / Password | Your news server account (blank if the server needs no login) |
| Check every | Minutes between checks (30 is plenty for most groups) |
| Most per group | The most new articles fetched from one group per check |
| Poster address | Posts go out `From: handle@this-domain`. The default, `boardname.invalid`, is the Usenet way to keep spammers from harvesting your callers' addresses |

Press **Turn On**, then **Add Group** for each newsgroup. The first check brings in a group's newest 50
articles; after that, everything new. **Check Now** checks right away. Articles show "via Usenet" next to the
sender's name. MIME is handled (accented letters, quoted-printable and base64 text, and multipart messages,
where the plain-text part is shown). Attachments and binary groups are not downloaded.

### Finger
RipscoutBBS answers **finger**, the classic internet "who's on?" service, on port 79:
- `finger @yourboard.com` lists the callers online: node, name, what they're doing, how long they've been on.
- `finger marc@yourboard.com` shows a member: level, location, member since, calls, posts, last call, whether
  they have unread mail, and their **plan**, a line each caller writes in Your Settings > Finger Plan.

Finger never shows IP or email addresses. Change the port, or set it to 0 to turn finger off, in Board Settings.
Callers outside your network need port 79 forwarded to the server, like the telnet port.

### Audit Trail
A permanent record of who did what. Each entry has the time, the kind, who did it (a member, "Control
Panel" or "System"), the node and IP address when it came from a caller, what happened and the details.

| Kind | What it records |
|---|---|
| Logins | Logins and logoffs (with time online), failed logins (wrong password or no such name), new signups |
| Security | Password changes and resets, two-factor on/off/reset, backup-code sign-ins, failed two-factor codes, locked and deleted accounts |
| Sysop | Every settings change with its old and new value (passwords show only "changed"), security levels, disconnects, pages, broadcasts |
| Files | Uploads, downloads, deleted files, new libraries |
| Messages | Deleted messages, email and message areas |
| Network | Chat links connecting or refused, echomail and Usenet failures |
| System | Board start-up, update installs, rejected updates |

In RIP: **Sysop Admin > Audit Trail** lists it newest first. Click a kind to filter, **Search** for a name, IP
address or word, and click an entry for all of its details. In the Control Panel: the **Audit** tab, with the
same filters and **Save as CSV** for Excel. The trail is kept in `audit.log` in the data folder (it is only
ever added to, and moves to `audit-old.log` at 20 MB).

### Nodes & Broadcast
Shows every connected node with its address and what it's doing. Click a node to **disconnect** it (the
caller sees a short goodbye notice first). **Broadcast Message** pops a notice up on every caller's
screen, wherever they are in the board.

### Statistics
A bar chart of calls per day for the last 14 days, a pie chart of members by level, totals and uptime.

### News & Rules
**System News** is shown to every caller after login. **Board Rules** are shown on the last signup step.
Both open in the full-screen editor: each line is one paragraph, and paragraphs wrap automatically on screen.
Click **Save** to publish immediately.

### Updates
RipscoutBBS keeps itself up to date from its download page on GitHub (`Dialsoft/ripscoutbbs-releases`).
- It checks every 6 hours (and one minute after it starts).
- A newer version is downloaded in the background and checked against its published SHA-256 checksum.
  A download that doesn't match is thrown away and never run.
- With **Auto-install ON** (the default), the update is installed the next time **nobody is online**.
  The board restarts itself in the same window within a few seconds, so callers are never dropped.
- **Install Now** installs right away: everyone online gets a 30-second warning pop-up, then the board restarts.
- **Check Now** checks immediately. **Auto-install On/Off** switches the automatic install (live, like every setting).
- During an update, the old EXE is briefly kept as `RipscoutBBS.old.exe` and then deleted automatically.

### Content Editor
**Sysop Admin > Content Editor** changes how the board looks and what its menus do, live, in RIP. It's saved
in `content.json` in the data folder.

- **Menus**: click a menu to edit it. You can:
  - **Add Item**: give it a label, then choose what it opens. That can be a *module* (Message Boards,
    Teleconference, Who's Online, any single BMX game, the Newsmap, Your Settings, Sysop Admin, Goodbye...) or
    *another menu*.
  - Click an item to change its label, hotkey, icon, what it opens, or who can see it (minimum level), move it
    up or down, or delete it.
  - **Title**, **Caption**, **Background**, **Side Panel** (the clock and who's-online panel) and **Try It**
    (opens the menu as a caller sees it).
  - **Create a new menu**, then add an item on another menu that opens it (a submenu).
  - Safety: the main menu always keeps its Sysop Admin item, and on any menu, staff can press **%** to open
    Sysop Admin.
- **Icons**: the library. Built-in icons are vector drawings. **Draw a New Icon** opens the pixel editor:
  pick a color on the right, click squares on the big grid, and watch the actual-size and on-a-button previews.
  Save, Fill All, Clear, Rename (menus follow the new name) and Delete are there too.
- **Backgrounds**: click one to edit it with a live preview: kind (pattern, custom, bands, stars, grid, dirt,
  solid), both colors, the pattern number, or, for *custom*, click the 8x8 squares to make your own texture.
  **Make Board Default** sets the background for every screen that doesn't choose its own.

### Chat Links (linked Teleconference)
Link your Teleconference with other boards running RipscoutBBS, so callers on all of them chat in one room.

1. **Sysop Admin > Chat Links**: set this board's **Tag** (short name, e.g. RIPSCOUT) and **Port**
   (2324 suggested; allow it in Windows Firewall and forward it on your router).
2. **Add Link**: enter the *other* board's tag, its address, its link port, and a **link password**. The other
   sysop adds your board the same way, with the same password.
3. Both boards keep trying to connect every 30 seconds. The list shows **LINKED** when it's up, and the
   Control Panel dashboard shows it too.

In Teleconference, the room list shows everyone with a **line number**: `03` is a caller here, `A07` is line
7 on linked board A (the first link in your list), `B02` on board B, and so on. To send a private message,
**click a name** for a pop-up, or type `/03 message` or `/A07 message`. It arrives as a pop-up on the other
caller's screen.

Security: the password never crosses the network (each side proves it knows the password with an HMAC
challenge), and everything on the link is encrypted with AES-256-GCM. Messages travel one hop, so to join
three boards, link each pair.

## 7. Stats

**BBS Stats** on the main menu shows the board by the numbers: calls, members, messages, files, downloads
and words typed, a chart of calls over the last 14 days, the busiest hours of the day, and records like the
busiest moment ever, time spent online and the newest member. **Leaderboards** shows the top 5 for calls,
posts, words typed, time online, downloads and uploads.

**My Stats** (from BBS Stats) shows a caller everything about themselves: logins, time online, longest visit,
calling streak (days in a row) and best streak, posts, words typed (in posts, email and chat), chat lines,
email sent and received, uploads and downloads, games played and their favorite, best high-score placing,
Word Duels won and lost, their rank by calls, charts of when they call (hour and weekday), and 10
**badges** to earn (Regular, Night Owl, Wordsmith, Chatterbox, Collector, Contributor, Pen Pal, Champion,
Duelist and Veteran; click one to see how).

Sysops can see any member's stats with the **Stats** button on their account in Sysop Admin > Users.
Logins and posts count from the start; the newer counters (time online, words, transfers and so on) count
from the first visit after version 1.9.0.

## 8. Two-factor sign-in (Google Authenticator)

Any caller can protect their account with a second step at login: a 6-digit code from an authenticator app on
their phone. Google Authenticator is free; Microsoft Authenticator, Authy, 1Password and the like work too.

**Turning it on** (Your Settings > Two-Factor):
1. The board draws a **QR code** right on the RIP screen.
2. The caller opens the app, taps **+**, picks **Scan a QR code** and points the phone at the screen.
   A setup key is shown too, for typing in by hand if the camera won't cooperate.
3. They type the 6-digit code the app shows, and two-factor is on.
4. The board shows **8 backup codes**, once. Each works one time in place of the app (lost or dead phone).

**Logging in** then asks for the password, then the code. Codes change every 30 seconds; the board accepts the
current one and the ones just before and after (phone clocks drift), and never accepts the same code twice.
After 3 wrong codes the login stops.

**For the caller** (Your Settings > Two-Factor, once it is on), each needing a current code or a backup code:
**New Phone** (set it up again; the old phone stops working once the new one is confirmed), **New Backup Codes**,
and **Turn Off**.

**For you, the reset tool:** if a caller loses their phone *and* their backup codes, open their account in
Sysop Admin > Users and press **Reset 2FA**, or press **Reset 2FA** next to them on the Control Panel's Users
tab. Their next login needs only the password, and they can set up a new phone. Make sure it is really them
first (the log records every reset).

**The name in the app:** Board Settings > **Authenticator name** (or the Control Panel's Settings tab) sets what
the board is called in callers' authenticator apps, e.g. "Ripscout BMX". Blank means the BBS name. Phones that
are already set up keep the name they were given.

The secret key behind each caller's codes is kept in `users.json`, so protect your backups of the data folder
like you would a password list. Backup codes are stored only as salted hashes.

## 9. Sound

RipscoutBBS uses the two kinds of sound BBSes had:
- **RIPscrip sound effects**, built into RIPscrip 1.54: $BEEP$, $BLIP$, $ALARM$, $PHASER$, $REVPHASER$ and $MUSIC$.
  The games use them (a blip when you jump or hit something, a phaser zap when you crash, Turbo Lap's start
  lights), and so do pages (beep) and sysop broadcasts (alarm). In DialComm they follow the **Bell** setting.
- **ANSI music**, tunes in the GW-BASIC "PLAY" notation: a riff on the spinning-wheel welcome screen, a chime
  for new mail, a fanfare for game invitations and high scores, and a few jingles in the games. In DialComm,
  turn on **ANSI Music** to hear them (the status line offers it the first time a tune arrives).
  Because most terminals keep ANSI music off until asked, the board reminds callers: a line on the welcome
  screen, a one-time notification on each caller's first visit, and a note on the Showcase Sound page.

Each caller can turn the board's sounds off or on in **Your Settings > Sound**. The **RIP Showcase** has a
Sound page (page 8) with a button for every effect and a few tunes.

## 10. Email and notifications

**Email** on the main menu is private mail between members: a mailbox (blue = unread), Read, Reply (with the
letter quoted), Delete and Write a Letter. Letters are kept in `mail.json`.

**Notifications** pop up on any screen, top right, with a beep:
- a **new letter** ("New email from Marc"),
- a **game invitation** (a Word Duel challenge, see BMX Games),
- **pages** from other callers and **broadcasts** from the sysop,
- at login, "You have mail" when letters are waiting.

A notification goes away after a few seconds or when the caller presses a key, and the screen underneath comes
back exactly as it was. When the caller is at a menu, **clicking** the notification opens what it is about
(the mailbox, or the game) and then returns to the same screen. During a real-time game, notifications wait
until the game is over.

## 11. File Libraries

**File Libraries** on the main menu is the download (and upload) section. Callers pick a library, click a file
to see its details and description, and press **Download (Zmodem)**. DialComm (and most terminals) start
receiving automatically: no menus, no protocol picking. **Upload a File** works the other way: the caller
writes a one-line description, and DialComm notices the board is ready and asks which file to send (other
terminals: start a Zmodem upload; in DialComm that is Transfer > Send File, Ctrl+Shift+U). **Search All Files** finds files by name or description in every library.

- The board starts with three libraries: BMX Videos & Pictures, RIPscrip Art & Icons, Terminals & Utilities.
- Each library is a folder in `RipscoutBBS-data\files\`. Copy files into a folder yourself and press
  **Scan Folders** (in RIP or the Control Panel) to list them. Or drag files onto the Control Panel's
  **Files** tab.
- Staff (Co-Sysop and up) can make **New Area**s, edit descriptions and delete files from RIP.
- **Max upload size** is in Board Settings (default 50 MB). An upload that is cut off is thrown away.
- Each file shows a download-time estimate (33.6k modem vs broadband) and a download count.

## 12. BMX Games

From the main menu, **BMX Games** offers seven games played right in the terminal:
- **Dirt Jumper** (side-scroller): SPACE or Up jumps. Press T or Down in the air for a tabletop trick (+100).
  The track speeds up the longer you ride. You get 3 lives.
- **Cone Blaster** (shooter): Left/Right or A/D rides, and SPACE or Up squirts water. Cones are worth 10 and
  tires 25. Getting hit costs a life.
- **Spoke Breaker** (brick breaker): Left/Right or A/D moves the handlebars, and SPACE launches the wheel. Where
  the wheel hits the bars sets its angle. Clear the wall for +500 and a faster next level.
- **Block Drop** (falling blocks): Left/Right moves, Up or W rotates, Down or S soft-drops, and SPACE hard-drops.
  Lines score 40/100/300/1200 times the level, and the blocks fall faster every 10 lines.
- **Turbo Lap** (3-D road racer, arcade style): the car speeds up by itself. Left/Right or A/D steers and
  Down, S or SPACE brakes. Curves push you to the outside, and the grass slows you down. You have 60
  seconds; each finished lap adds 40 more. Passing a car scores 200, hitting one costs time. The road's
  rushing stripes are pure palette cycling: the road is drawn once and only 9 colors change per frame.
  A race sends about 10-15 KB per second, fine for any internet connection.
- **Word Wheelie** (word game): guess the hidden 5-letter word in 6 tries, typing or clicking the on-screen
  keys. Green = right letter in the right spot, gold = in the word but elsewhere, gray = not in the word.
  - **Run**: solve word after word. Each word scores 100 per unused guess, plus 100. The first miss ends the run.
  - **Duel**: pick a caller who is online. They get a notification ("Marc challenges you to Word Wheelie!") and
    click it to play the same word. When both have played, both get a notification saying who won.
- **Solitaire** (Klondike, draw one): click a card, then click where it goes. Click a card twice to send it to
  its foundation. Click the deck to turn a card (or turn the pile back over). D draws, U undoes, A sends
  everything it can to the foundations, N deals again. Classic scoring, plus a time bonus when you win, and the
  bouncing-cards victory lap.
- Each game keeps a top-10 high score table, shown on the BMX Games screen. Q or Esc quits a game (Esc in
  Word Wheelie and Solitaire, where Q is a letter or a key you might want).

## 13. BMX Newsmap

**BMX Newsmap** on the main menu shows current BMX news as a treemap. Each story is a tile: newer stories get
bigger tiles, the color is the category (Racing, Freestyle, Bikes & Gear, Events & Video, Other), and bright
tiles are from the last 24 hours. Click a tile to read the summary. The story page has a **QR code**: scan it
with a phone camera to open the full article.

- The board reads the news feeds every 30 minutes (and when someone clicks **Refresh**).
- **Sysop Admin > News & Rules > BMX News Feeds** edits the feed list: one RSS or Atom link per line.
  WordPress sites usually have their feed at `/feed/`. Save, and the map refreshes right away. Erase every line
  to go back to the default list.
- A feed that fails is skipped and logged in the console, and the rest still show. The status line under the
  map says how many feeds answered.

## 14. The data folder

| File | Contents |
|---|---|
| `config.json` | Board settings |
| `users.json` | Accounts. Passwords are stored only as salted PBKDF2-SHA256 hashes (210,000 rounds), so nobody (including you) can read them |
| `boards.json` | Message areas and messages |
| `stats.json` | Calls per day and the BMX Games high scores |
| `content.json` | Menus, icons and backgrounds made in the Content Editor |
| `files.json`, `files\` | The File Libraries list and the files themselves (one folder per library) |
| `mail.json` | Email between members |
| `audit.log` | The audit trail (one entry per line; `audit-old.log` holds older ones) |
| `usenet\seen.json` | Usenet: Message-IDs already filed (duplicate check) |
| `ftn\` | Echomail: `outbound` (packets waiting to go), `inbound` (just received), `bad` (files that were not mail), `seen.json` (duplicate check) |
| `RipscoutBBS.log` | The same log lines shown in the console |

Files are saved safely: each one is written to a temporary file and then swapped in, so a power cut can't
leave a half-written file behind. To restore a backup, close the board, copy the folder back and start it again.

## 15. Troubleshooting

| Problem | Fix |
|---|---|
| The Control Panel's top-right badge says "NOT listening" | Another program (often a Windows telnet server) is using the port. Stop it, or change the port in Settings |
| I can't see the tray icon | Click the ^ arrow next to the clock. Drag the wheel icon onto the taskbar to keep it visible |
| Callers see plain text instead of graphics | Their terminal isn't in RIPscrip mode. In DialComm, set the entry's Emulation to RIPscrip |
| Outside callers can't connect but local ones can | Check the firewall rule and router forwarding; try a port other than 23 |
| Forgot the Sysop password | Close the board. In `users.json`, set another staff account's `"level"` to 100, log in as that account and reset the Sysop password. If there is no other staff account, rename `users.json` to start a fresh user list (the boards and settings are kept) |
| "All lines are busy" | Raise Max nodes in Board Settings |
| Echomail says "cannot reach the hub" | Check the hub host and port with the hub's sysop. Some networks use a port other than 24554 |
| Echomail says "hub refused the password" | Re-type the session password exactly as the hub's sysop gave it (it is case-sensitive) |
| An echo arrives but no area appears | New echoes is set to "ignore": give an area that echo tag, or change the setting |
| A caller's authenticator codes never work | Their phone's clock is off. Turn on automatic date and time on the phone. Or reset their 2FA |
| Usenet says "login refused" | Check the login name and password with your news provider. Some servers need encryption (port 563) |
| Usenet posts are refused | Some free servers only allow posting after you confirm your account by email, or limit posts per day |
| `finger` from outside gets no answer | Forward port 79 to the server and allow it in Windows Firewall. Some internet providers block port 79 |
| No music in DialComm | Turn on ANSI Music (click the "Music Off" note on the status line, or the ribbon). Sound effects follow the Bell setting |
| A download doesn't start | The terminal needs Zmodem auto-download turned on (DialComm has it on by default) |
