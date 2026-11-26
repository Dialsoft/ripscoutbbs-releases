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
| BMX News | The Newsmap feed list and a Refresh button |
| High Scores | Top 10 for all four BMX Games |
| Updates | Version, latest on GitHub, Check now, Install now, auto-install on/off |
| Log | Everything logged since startup (also saved in `RipscoutBBS.log`) |

The first time it runs, RipscoutBBS creates a folder named `RipscoutBBS-data` next to the EXE. Everything
the board remembers is in that folder (see section 7). To back up the board, copy that folder.

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
(click to cycle), or delete it with all of its messages.

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

## 7. BMX Games

From the main menu, **BMX Games** offers two real-time games played right in the terminal:
- **Dirt Jumper** (side-scroller): SPACE or Up jumps. Press T or Down in the air for a tabletop trick (+100).
  The track speeds up the longer you ride. You get 3 lives.
- **Cone Blaster** (shooter): Left/Right or A/D rides, and SPACE or Up squirts water. Cones are worth 10 and
  tires 25. Getting hit costs a life.
- **Spoke Breaker** (brick breaker): Left/Right or A/D moves the handlebars, and SPACE launches the wheel. Where
  the wheel hits the bars sets its angle. Clear the wall for +500 and a faster next level.
- **Block Drop** (falling blocks): Left/Right moves, Up or W rotates, Down or S soft-drops, and SPACE hard-drops.
  Lines score 40/100/300/1200 times the level, and the blocks fall faster every 10 lines.
- Each game keeps a top-10 high score table, shown on the BMX Games screen. Q or Esc quits a game.

## 8. BMX Newsmap

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

## 9. The data folder

| File | Contents |
|---|---|
| `config.json` | Board settings |
| `users.json` | Accounts. Passwords are stored only as salted PBKDF2-SHA256 hashes (210,000 rounds), so nobody (including you) can read them |
| `boards.json` | Message areas and messages |
| `stats.json` | Calls per day and the BMX Games high scores |
| `RipscoutBBS.log` | The same log lines shown in the console |

Files are saved safely: each one is written to a temporary file and then swapped in, so a power cut can't
leave a half-written file behind. To restore a backup, close the board, copy the folder back and start it again.

## 10. Troubleshooting

| Problem | Fix |
|---|---|
| The Control Panel's top-right badge says "NOT listening" | Another program (often a Windows telnet server) is using the port. Stop it, or change the port in Settings |
| I can't see the tray icon | Click the ^ arrow next to the clock. Drag the wheel icon onto the taskbar to keep it visible |
| Callers see plain text instead of graphics | Their terminal isn't in RIPscrip mode. In DialComm, set the entry's Emulation to RIPscrip |
| Outside callers can't connect but local ones can | Check the firewall rule and router forwarding; try a port other than 23 |
| Forgot the Sysop password | Close the board. In `users.json`, set another staff account's `"level"` to 100, log in as that account and reset the Sysop password. If there is no other staff account, rename `users.json` to start a fresh user list (the boards and settings are kept) |
| "All lines are busy" | Raise Max nodes in Board Settings |
