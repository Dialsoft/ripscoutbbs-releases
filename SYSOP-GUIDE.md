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
| Users | Search accounts, change security levels, lock or unlock, reset passwords, delete |
| Settings | Every board setting, including System News and Board Rules. Saving applies them live |
| Files | The File Libraries: drag files from Windows onto the box to add them, write descriptions, delete, make new libraries, scan folders |
| Echomail | FTN echomail setup (address, hub, passwords, poll time), Poll now, AreaFix, and the log of recent calls to the hub |
| BMX News | The Newsmap feed list and a Refresh button |
| High Scores | Top 10 for every BMX Game |
| Updates | Version, latest on GitHub, Check now, Install now, auto-install on/off |
| Log | Everything logged since startup (also saved in `RipscoutBBS.log`) |

The first time it runs, RipscoutBBS creates a folder named `RipscoutBBS-data` next to the EXE. Everything
the board remembers is in that folder (see section 10). To back up the board, copy that folder.

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

### Message Areas
**Add Area**, or click an area to rename it, change its description, change its read/post levels
(click to cycle), or delete it with all of its messages. **Echo Tag** shares the area with an FTN network
(see Echomail below); leave it blank for a local-only area.

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

## 7. File Libraries

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

## 8. BMX Games

From the main menu, **BMX Games** offers five real-time games played right in the terminal:
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
- Each game keeps a top-10 high score table, shown on the BMX Games screen. Q or Esc quits a game.

## 9. BMX Newsmap

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

## 10. The data folder

| File | Contents |
|---|---|
| `config.json` | Board settings |
| `users.json` | Accounts. Passwords are stored only as salted PBKDF2-SHA256 hashes (210,000 rounds), so nobody (including you) can read them |
| `boards.json` | Message areas and messages |
| `stats.json` | Calls per day and the BMX Games high scores |
| `content.json` | Menus, icons and backgrounds made in the Content Editor |
| `files.json`, `files\` | The File Libraries list and the files themselves (one folder per library) |
| `ftn\` | Echomail: `outbound` (packets waiting to go), `inbound` (just received), `bad` (files that were not mail), `seen.json` (duplicate check) |
| `RipscoutBBS.log` | The same log lines shown in the console |

Files are saved safely: each one is written to a temporary file and then swapped in, so a power cut can't
leave a half-written file behind. To restore a backup, close the board, copy the folder back and start it again.

## 11. Troubleshooting

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
| A download doesn't start | The terminal needs Zmodem auto-download turned on (DialComm has it on by default) |
