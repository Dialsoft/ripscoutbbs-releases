# RipScout BBS Sysop Guide

RipScout is BBS software: a RIPscrip graphics BBS that runs as one Windows program. This guide covers
everything you need to run a board with it: installing it, setting it up, letting callers reach it, and
managing it from inside the BBS.

How a board looks is up to you: RipScout comes with **theme packs** (see *Theme packs* below). Dialsoft's own
showcase board, BMX BBS, uses the BMX pack; the **Generic** pack is the recommended starting point.

## 1. Installing

**The easy way: the installer.** Download `RipscoutBBS-Setup.exe` from the latest release and run it.
- It installs into **`C:\RipScoutBBS`** (recommended), or any folder you pick. Avoid Program Files: the board
  saves its data in its own folder.
- Options: a **Windows Firewall rule** (recommended: lets callers in), a **desktop shortcut**, and **start the
  board when Windows starts**.
- It also adds Start Menu shortcuts (the board, the Sysop Guide, the board folder and Uninstall) and an entry in
  Windows' Add or Remove Programs.
- Running it again over an existing board **upgrades** it. A running board is asked to shut down cleanly first,
  and its data is never touched.
- **Uninstalling** removes the program, shortcuts and firewall rule, then asks before deleting the board's data.
  The default is to keep it.
- Windows may show "Windows protected your PC" for a new program: click **More info > Run anyway**.

**Or by hand:** make a folder such as `C:\RipScoutBBS` and put `RipscoutBBS.exe` in it.

### Everything in one folder (moving to another PC)
The board keeps **all** of its data (settings, members, messages, files, high scores, logs) in the
`RipscoutBBS-data` folder next to `RipscoutBBS.exe`, and stores no full paths anywhere. So:
- **Back up** the board by copying its folder (`C:\RipScoutBBS`).
- **Move it to another PC**: shut it down (tray icon > Shut Down), copy the whole folder to the new PC, then run
  the installer there and pick that folder (or just start the EXE in it). Everything comes back as it was. Run the
  Internet Check afterward: the new PC needs its own firewall rule and router port forward.

### Running it
1. Double-click `RipscoutBBS.exe` (or its shortcut). There is no console window. Instead:
   - the **wheel icon** appears in the system tray, next to the clock (Windows may tuck it under the ^ arrow:
     drag it onto the taskbar to keep it visible). Hover over it to see how many callers are online.
   - the **Control Panel** window opens.
3. **Right-click the tray icon** for: Open Control Panel, Open Data Folder, Check for Updates and
   Shut Down RipscoutBBS. Double-clicking the icon also opens the Control Panel.
4. Starting the EXE a second time doesn't start a second board. It just opens the running board's Control Panel.

### First-run setup
The first time it starts, the Control Panel opens with **Welcome to RipScout!**. Fill in:
- **Board name** (the big title on the welcome screen), **your sysop name**, **location** (optional) and a
  **tagline**;
- the **telnet port** callers connect to (23 is standard);
- a **theme pack**: click one of the pictures. **Generic** is recommended and preselected.

Press **Finish setup**. Everything can be changed later in the Settings tab. (If you skip the Control Panel and
sign up in RIP first, the theme pack chooser appears once after your first login as sysop instead.)

### Theme packs
A theme pack is the look a board is dressed in. Every pack has **exactly the same menus, games and features**.
Only these change:

| | Generic (recommended) | BMX |
|---|---|---|
| Welcome screen | A turning globe in a ring of chasing lights, mountains, neon grid | A neon 1980s BMX race bike with spinning mag wheels, dirt-jump hills, a rider |
| Welcome tune | Its own riff | A BMX riff |
| Games room on the menu | **Games** (joystick icon) | **BMX Games** (bike icon) |
| Newsmap on the menu | **Newsmap**: technology and retro-computing news | **BMX Newsmap**: BMX news |
| First file library | Pictures & Videos | BMX Videos & Pictures |

Both welcome screens show **your board's name** as the big title and "Powered by RipScout BBS software".

**Switching packs** is live: in the Control Panel's **Settings** tab (click a pack), or in RIP under
**Sysop Admin > Board Settings > Theme pack** (with a Preview button for each). Callers see it on their next
screen. Menu items and the first file library that still have the old pack's names are renamed to the new
pack's; anything you renamed yourself is left alone. The news feeds follow the pack unless you've set your own.

Boards made before theme packs existed (2.1 and older) keep the BMX look automatically.

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
| News Feeds | The Newsmap feed list (the theme pack's defaults until you change it) and a Refresh button |
| High Scores | Top 10 for every game |
| Updates | Version, latest on GitHub, the update setting (Ask me first / Automatic / Off), What's new, Check now, Install now |
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

### Update progress

While an update is under way, a progress bar runs across the top of the Control Panel on every tab: the
download (percent and MB), the checksum check, the countdown while callers are warned, installing, and
restarting. When the board comes back as the new version, the Control Panel reloads itself.

## 2. Letting callers in

### Internet Check: are you live?
The top of the Control Panel's **Dashboard** says whether callers on the internet can reach the board: a big
green **"All tests good - live on the internet"**, or a red **"Needs attention"** with what to fix. The tray
icon's tooltip shows it too. The tests:

| Test | What it checks |
|---|---|
| Answering calls | The board is listening on its telnet port |
| Windows Firewall | An inbound rule lets callers reach RipscoutBBS.exe. If not, **Fix Firewall** adds one (Windows asks you to approve) |
| Internet connection | This PC is online, and its public address |
| Reachable from the internet | An outside checker calls your public address on the telnet port |
| It's your board answering | That call reached **this** board (it says a one-time code and the board answers with its name), not another device your router forwards to |
| Chat link / Finger port | Those ports are reachable too, when you use them |
| Host name | When you've entered a **Public host name** (Settings), it points at this board |

Each failed test says how to fix it, including your PC's local address for the router's port-forward page. The
check runs 20 seconds after the board starts, every 6 hours, after you change ports in Settings, and when you
press **Run Check**. The checker's test calls are never counted as callers.

The outside checker is a small Dialsoft service. It only ever calls back the address that asked it, so it
can't be used to probe anyone else's computer.


1. **Windows Firewall.** The installer adds the rule. Without the installer, press **Fix Firewall** in the
   Internet Check, which adds the same rule for RipscoutBBS.exe on any port.
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
| Two-factor sign-in | Who must use an authenticator app (Optional / Required for sysops / Required for everyone), and the name callers see for this board in the app (blank = the BBS name; changes apply to new setups only) |
| Menu button lettering | How the words on menu buttons are drawn: **Crisp** (the sharp screen font, easiest to read, the default) or **Classic** (thin drawn letters with a shadow, the look before 2.7.0). Control Panel > Settings; live |
| Finger server port | 79 is standard; 0 turns finger off. See *Finger* below |
| Theme pack | How the board looks: Generic or BMX, with a preview of each. See *Theme packs* above |

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
New versions of RipscoutBBS are published on its download page on GitHub (`Dialsoft/ripscoutbbs-releases`).
**You decide what happens when one comes out** (Sysop Admin > Updates, or the Control Panel's Updates tab):

| Setting | What it does |
|---|---|
| **Ask me first** (the default) | Every 6 hours the board looks for a new version. When it finds one, it downloads it and checks it against its published SHA-256 checksum, then **waits for you**. Staff who are online get a notification ("Update 2.1.0 is ready"), and staff logging in later get it too. You read what changes, then press **Install Now** when it suits you, or never. |
| **Automatic** | As above, but it installs by itself the next time **nobody is online**. The board restarts in the same window within a few seconds, so callers are never dropped. |
| **Off** | The board never contacts GitHub on its own. **Check Now** still works whenever you press it. |

**What changes:** **What's New** (in RIP) and the **What's new** box (Control Panel) show the release notes of
every version newer than yours, newest first, so you can read exactly what an update brings before installing.
If you skip versions, the notes of all of them are there.

- **Install Now** installs the downloaded version. It offers What's New first. Everyone online gets the
  **caller notice** (below) as a notification on whatever screen they're on. When the warning time is up, they
  see "Installing version ... now. Please call back in a minute!" and are disconnected while the board restarts.
- **Caller notice** (Sysop Admin > Updates > Caller Notice, or the *Caller notice* box on the Control Panel's
  Updates tab): the message callers get before an update, and the **warning time** (5-600 seconds, default 30).
  The default message is *"The BBS is installing new version {version}. You'll be disconnected in {seconds}
  seconds - please call back in a minute!"*. In your own message, `{version}` becomes the new version,
  `{seconds}` the warning time, `{board}` your BBS name and `{current}` the version running now. Both screens
  show exactly what callers will see. Changes apply from the next update on, with no restart. **Use Default**
  goes back to the built-in message.
- A download that doesn't match its checksum is thrown away and never run (and recorded in the audit trail).
- During an update, the old EXE is briefly kept as `RipscoutBBS.old.exe` and then deleted automatically.
- **How the switch-over works:** the old copy starts the new one and quits. The new copy waits for the old
  one to be gone before it opens the telnet port. If the old copy is still running after 15 seconds, the
  new copy closes it, so you never end up with two boards running. The log says which happened ("the old copy
  exited normally" or "...did not exit within 15 seconds, so it was closed"), then "Update finished: now
  running version X".
- **If the Control Panel had to move** (updating from version 2.1.0 or older, which didn't remember the panel's
  address), the new version opens a fresh Control Panel window by itself. Close the old one that says "BBS not
  responding".
- **The Control Panel stays open through an update.** It keeps the same address, so while the board
  restarts it shows *Not responding - restarting? Reconnecting...*, then picks up by itself and says
  *Update finished: now running version X*. No need to reopen it.
- If a newer version comes out after one was already downloaded, the older download is thrown away and the
  newest one is fetched, so Install Now always installs the latest.
- Boards updated from 1.9 or older start on **Ask me first**, whatever they had before.

### Content Editor
**Sysop Admin > Content Editor** changes how the board looks and what its menus do, live, in RIP. It's saved
in `content.json` in the data folder.

- **Menus**: click a menu to edit it. You can:
  - **Add Item**: give it a label, then choose what it opens. That can be a *module* (Message Boards,
    Teleconference, Who's Online, any single game, the Newsmap, Your Settings, Sysop Admin, Goodbye...) or
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

**Line tags, STS style.** Every chat line starts with the speaker's tag:

    72#4[T1]:MaxMouse) hello everybody
    72#2<T1>:Marc) welcome!

`72` is the board's **station number** (Control Panel > Settings > *Station number*), `#4` the caller's line,
and `T1` the channel. The brackets show rank: `< >` for the sysop and co-sysops, `[ ]` for members. Callers on
linked boards show their own board's station number (`12#1[T1]:Bob)`), so give every board on a link network
its own number. A linked board running a version before 2.10 shows as a letter instead (`A07`).

**Channels.** Everyone starts on channel T1. `/T2` tunes to channel 2 (1-99); each channel is its own room,
across linked boards too. `/T` says which channel you're on, and `/who` lists everyone on every channel.

**Private messages.** Click a name in the room list for a pop-up, or type `/#4 message` (line 4 here),
`/72#4 message` (line 4 on station 72) or the older `/A07 message`. It arrives as a pop-up on the other
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

**At signup:** the last step of New User Signup offers it right away (the QR code below), with **Skip for
now**. Members who skip can turn it on later.

**Who must use it** (Board Settings > **Two-factor sign-in** in RIP, or the Control Panel's Settings tab), live:
- **Optional** (the default): offered at signup, members can skip it.
- **Required for sysops**: every sysop account must have it. A sysop without it is walked through setup right
  after logging in.
- **Required for everyone**: every member must have it. Anyone without it is walked through setup after
  logging in; if they leave without finishing, they are told why and logged off. Members without a
  smartphone can't get in, so only choose this if all your callers have one.

**Turning it on** (Your Settings > Two-Factor, at signup, or when required):
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

**The name in the app:** Board Settings > **Two-factor sign-in** > **Change Name** (or the Control Panel's
Settings tab) sets what
the board is called in callers' authenticator apps, e.g. "Retro Hub BBS". Blank means the BBS name. Phones that
are already set up keep the name they were given.

The secret key behind each caller's codes is kept in `users.json`, so protect your backups of the data folder
like you would a password list. Backup codes are stored only as salted hashes.

## 9. Sound

RipscoutBBS uses the two kinds of sound BBSes had:
- **RIPscrip sound effects**, built into RIPscrip 1.54: $BEEP$, $BLIP$, $ALARM$, $PHASER$, $REVPHASER$ and $MUSIC$.
  The games use them (a blip when you jump or hit something, a phaser zap when you crash, Turbo Lap's start
  lights), and so do pages (beep) and sysop broadcasts (alarm). In DialComm they follow the **Bell** setting.
- **ANSI music**, tunes in the GW-BASIC "PLAY" notation: a riff on the welcome screen, a chime
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
- a **game invitation** (a Word Duel challenge, see Games),
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

- The board starts with three libraries: Pictures & Videos (BMX Videos & Pictures with the BMX pack), RIPscrip Art & Icons, Terminals & Utilities.
- Each library is a folder in `RipscoutBBS-data\files\`. Copy files into a folder yourself and press
  **Scan Folders** (in RIP or the Control Panel) to list them. Or drag files onto the Control Panel's
  **Files** tab.
- Staff (Co-Sysop and up) can make **New Area**s, edit descriptions and delete files from RIP.
- **Max upload size** is in Board Settings (default 50 MB). An upload that is cut off is thrown away.
- Each file shows a download-time estimate (33.6k modem vs broadband) and a download count.

## 12. Games

From the main menu, **Games** (**BMX Games** with the BMX pack) offers seven games played right in the terminal:
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
- **Solitaire** (Klondike, draw one): drag a card where it goes, or click a card, then click where it goes. In DialComm 1.9.6 or newer the card follows the mouse as a see-through ghost and the pile under it turns gray. Click a card twice to send it to
  its foundation. Click the deck to turn a card (or turn the pile back over). D draws, U undoes, A sends
  everything it can to the foundations, N deals again. Classic scoring, plus a time bonus when you win, and the
  bouncing-cards victory lap.
- Each game keeps a top-10 high score table, shown on the games screen. Q or Esc quits a game (Esc in
  Word Wheelie and Solitaire, where Q is a letter or a key you might want).

## RIP Art Gallery

**RIP Showcase > Art Gallery** shows full-screen RIPscrip pictures:
- **Three built-in, animated pieces** drawn by RipScout: *Neon Skyline* (windows switch on and off, a
  flickering neon sign, shimmering water), *Ocean Sunset* (a striped sun, glittering sea, palm trees) and
  *Aurora* (northern lights rippling over mountains and a lake). The motion is palette cycling, a few dozen
  bytes per frame.
- **Every .RIP file in your file libraries** that the caller may download. To add art, upload .RIP files to a
  library (for example RIPscrip Art & Icons), or copy them into its folder and use Scan Folders. Title, artist
  and group come from the file's SAUCE record (the info block 1990s art tools added), otherwise from the file
  name and uploader.
- Viewing: click a picture in the list. Then click or press Space/N for the next one, P for the previous one,
  and Q or Esc to go back to the list.
- Uploaded pictures are cleaned before they're shown: commands that would make the terminal type to the board,
  ask about files or start a transfer are removed, and clickable areas are cleared. A picture can only draw.
- The same picture in two libraries (same file name and size) is listed once.

### Input boxes

Every screen with more than one input box (Login, Change Password, Email, posting a message, Your Details)
works like any Windows dialog: **Tab** or **Down** goes to the next box, **Shift+Tab** or **Up** to the
previous one, **clicking** a box puts the cursor there, and what's already typed stays put. **Enter** moves on,
or submits when it's the last box or every box is filled in; the screen's main button (Login, Save...) submits
from anywhere; **Esc** or **Cancel** cancels. In the signup wizard, Shift+Tab goes back a step.

### The classic RIP art pack (one click)

Control Panel > **Files** > **Add the classic RIP art pack** (also a checkbox in the first-run setup) adds 88
classic RIPscrip pictures from the 1990s art groups (ACiD, iCE, Fire and others) to a new file library called
**Classic RIP Art**. Each gets a description like *Negative Infinity by Wildcat (ACiD Productions, 1996)*.
They appear in the Art Gallery right away, and members can download them.

- The art belongs to its artists, so it isn't inside RipScout. When you press the button the board fetches
  each file from the **16colo.rs** art archive, one at a time with a short pause (about 3.3 MB, a minute or so).
  A bar shows progress.
- Every file is checked against a fingerprint (SHA-256) that RipScout carries, so only the genuine file is
  ever added. A damaged or changed file is refused.
- Press it again any time: pictures already there are skipped, so it only fills in what's missing (for
  example after an internet hiccup, or if you deleted some).
- Only the sysop and co-sysops can upload to the Classic RIP Art library. Move or delete pieces like any
  other file.
- Every member can open the gallery, so pieces with slurs, profanity or nudity are left out of the pack (7 of
  the originals). You can still add any art you like by hand, like any other file.
- For more, browse the 1990s artpacks at 16colo.rs and add what you like to your own board.

## Live Camera

Show a network camera to callers as **live RIPscrip video**. Set it up on the Control Panel's **Camera** tab;
while it's turned on, **Live Camera** appears on the Main Menu for members allowed to watch.

| Setting | What it does |
|---|---|
| Turned on | Shows Live Camera on the Main Menu. Turning it off takes it off the menu and returns anyone watching to the menu. |
| Name | Shown at the top of the screen, e.g. "BMX track". |
| Camera type | **MOBOTIX** (Q24, M24, Q25, M16 ...: the board asks for `/control/faststream.jpg`), **Axis** (`/axis-cgi/mjpg/video.cgi`), or **Other camera**: paste its MJPEG address or a snapshot address (a still picture fetched again and again). |
| Camera address | The camera's IP address or name, as you type it into a browser (add `:port` if it isn't 80). |
| User name / Password | The camera's login. Basic and Digest logins both work. Best: a camera user that can only view (MOBOTIX: Admin Menu > Users and Passwords, group "guests"). The password stays in the board's settings file and is never shown on the Control Panel. |
| Who can watch | The lowest member level that sees it (every member, validated, trusted, co-sysops, or only you). |
| Picture | **Standard** (blocks of 4 x 3 pixels, about 50-150 KB a second) or **Sharp** (2 x 2, about 3 times the detail and data). Callers can switch with **S** while watching. |
| Frames a second | The most pictures a second taken from the camera (1-15, default 8). Slow connections get fewer automatically. |

Press **Test** to connect for 30 seconds and see a preview of exactly what callers will see. All settings are
live.

How it works: the board talks to the camera only while someone is watching, and everyone shares that one
connection. Every few seconds it picks the best 14 of the EGA's 64 colors for the scene (black and white stay
for the screen's text) and only switches when the new set is clearly better. Each frame is dithered with a
steady pattern and only the parts of the picture that changed are sent, so a still scene costs almost nothing.
Callers press **Q** (or Esc) to leave. Watching counts as activity (the idle timer doesn't hang up on them),
and every viewer is logged in the Audit tab.

If the picture doesn't come: check the address works in a web browser on the board's PC, that the user name
and password are right, and (MOBOTIX) that the camera allows the user to see the live image. The status line
next to Test says what went wrong.

## Caller locations

The board works out roughly where each call comes from, using the caller's internet address. Only you see it:

- **Log:** `Node 2: call from 203.0.113.9 (Wharton, New Jersey, US)`.
- **Dashboard > Callers online:** a *Calling from* column.
- **Users:** *Last called from* for every member (hover for the address).
- **Audit:** a *Where* column on every entry with an address (and in the CSV export). A member logging in
  from a country they have never called from before gets a **security** entry ("Logged in from a new
  country"), which can mean someone else has their password.
- **Sysop Admin > Nodes** in RIP: *Calling from* next to each caller's address.

Each new address is sent once to a free IP-location service (ip-api.com, or ipwho.is as a backup) and the
answer is kept for 30 days in `geo.json`. Addresses on your own network show as *Local network* and are never
sent anywhere. Locations are approximate: usually the city or the provider's nearest hub, and a VPN shows the
VPN's location. Turn it off with **Settings > Caller locations > Off** (live).

## Finger

**Server.** The board answers finger (RFC 1288) on port 79, set in Settings (0 turns it off; live):
`finger @yourbbs.com` lists who is online, `finger name@yourbbs.com` describes one member (level, location,
member since, calls, posts, last call, unread mail, and their plan), and `/W` (verbose) is accepted. Requests
to forward a query to another host (`name@host@yourbbs.com`) are politely refused, as the RFC recommends.
Replies never include email or IP addresses, are cleaned of control characters, and end every line with CRLF.

**Client.** **Finger** on the Main Menu lets callers finger anywhere on the internet: `@host` (who's on),
`name@host`, `/W name@host`, a member's name (this board), or nothing (who's on here). **This board** shows
who's online here. Long answers scroll with Up/Down and PgUp/PgDn. Finger always uses port 79, and only you and
co-sysops may finger addresses on the board's own network. Every query is written to the Log.

## Texts: the board's own messages

Every message the board says to callers in its own words can be changed: the login prompt and login errors,
the "Welcome back" title, the new-member welcome, the goodbye screen, the disconnected-by-sysop, idle-timeout
and lines-busy messages, what callers without RIPscrip are told, the Teleconference join and leave lines,
everyday confirmations (page sent, message posted, upload received, file missing) and the last "Installing
version ... now" message during an update.

- **Where:** the Control Panel's **Texts** tab (edit a box and click away: saved), or in RIP
  **Sysop Admin > News & Rules > Texts** (click a message, then Edit). Changed messages are marked.
- **Fill-ins:** `{board}` the BBS name, `{sysop}` your name, `{name}` the caller's name, `{node}` their line,
  plus extras listed under some messages (for example `{level}`, `{line}`, `{version}`, `{minutes}`).
- **Live:** callers see the change from their next screen on. **Built-in** (or an empty box) puts the original
  back. Each message has a length limit so it still fits on its screen; long titles are drawn smaller.
- System News and Board Rules are on the Settings tab, and the warning before an update is the Caller notice
  on the Updates tab. Every change is recorded in the audit trail.

## 13. Newsmap

**Newsmap** on the main menu (**BMX Newsmap** with the BMX pack) shows current news as a treemap. Each story is
a tile: newer stories get bigger tiles, the color is the topic, and bright
tiles are from the last 24 hours. Click a tile to read the summary. The story page has a **QR code**: scan it
with a phone camera to open the full article.

- The board reads the news feeds every 30 minutes (and when someone clicks **Refresh**).
- What the news is about comes from the theme pack. Generic reads technology and retro-computing sources
  (topics: Tech & Gadgets, Retro & BBS, Games, Science & Space, Other); BMX reads BMX sites (Racing,
  Freestyle, Bikes & Gear, Events & Video, Other).
- **Sysop Admin > News & Rules > News Feeds** (or the Control Panel's News Feeds tab) edits the feed list: one
  RSS or Atom link per line. WordPress sites usually have their feed at `/feed/`. Save, and the map refreshes
  right away. Erase every line to go back to the theme pack's default list.
- A feed that fails is skipped and logged in the console, and the rest still show. The status line under the
  map says how many feeds answered.

## 14. The data folder

| File | Contents |
|---|---|
| `config.json` | Board settings |
| `users.json` | Accounts. Passwords are stored only as salted PBKDF2-SHA256 hashes (210,000 rounds), so nobody (including you) can read them |
| `boards.json` | Message areas and messages |
| `stats.json` | Calls per day and the games' high scores |
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
