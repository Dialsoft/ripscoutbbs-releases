# RipscoutBBS Sysop Guide

This guide covers everything you need to run RipscoutBBS: installing it, letting callers reach it, and
managing it from inside the BBS.

## 1. Installing

1. Make a folder such as `C:\RipscoutBBS` and put `RipscoutBBS.exe` in it.
2. Double-click `RipscoutBBS.exe`. A console window titled **RipscoutBBS 1.0.0** opens. It shows:
   - the board name and data folder,
   - the telnet port (23 unless you change it),
   - the addresses callers can use (`telnet 192.168.x.x 23` and so on),
   - one line per call ("Node 1: call from ...", "Node 1: Marc hung up").
3. Leave the window open. **Closing the window shuts the board down.**

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

## 7. The data folder

| File | Contents |
|---|---|
| `config.json` | Board settings |
| `users.json` | Accounts. Passwords are stored only as salted PBKDF2-SHA256 hashes (210,000 rounds), so nobody (including you) can read them |
| `boards.json` | Message areas and messages |
| `stats.json` | Calls per day |
| `RipscoutBBS.log` | The same log lines shown in the console |

Files are saved safely: each one is written to a temporary file and then swapped in, so a power cut can't
leave a half-written file behind. To restore a backup, close the board, copy the folder back and start it again.

## 8. Troubleshooting

| Problem | Fix |
|---|---|
| Console says "Cannot listen on port 23" | Another program (often a Windows telnet server) is using the port. Stop it, or start with `-port 2323` |
| Callers see plain text instead of graphics | Their terminal isn't in RIPscrip mode. In DialComm, set the entry's Emulation to RIPscrip |
| Outside callers can't connect but local ones can | Check the firewall rule and router forwarding; try a port other than 23 |
| Forgot the Sysop password | Close the board. In `users.json`, set another staff account's `"level"` to 100, log in as that account and reset the Sysop password. If there is no other staff account, rename `users.json` to start a fresh user list (the boards and settings are kept) |
| "All lines are busy" | Raise Max nodes in Board Settings |
