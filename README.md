# RipscoutBBS

**A bulletin board system drawn entirely in RIPscrip 1.54 graphics.** Every screen is vector
graphics with clickable buttons, from the welcome screen and the signup wizard to the full Sysop
Admin system. It's built to show off RIPscrip as far as it goes, and made for
[DialComm](https://github.com/Dialsoft/dialcomm-releases).

*By Dialsoft LLC, Wharton NJ.*

![Welcome screen](screenshots/welcome.png)

## Download

Get **RipscoutBBS.exe** from the [latest release](https://github.com/Dialsoft/ripscoutbbs-releases/releases/latest).
It's one Windows EXE (about 8 MB) with nothing to install.

## Features

- **Telnet BBS** for up to 250 callers at once, with automatic RIPscrip detection
- **Modern signup wizard**: a step tracker, real input boxes, a live "name available" check and a live password-strength meter
- **Message boards** with clickable lists, quoted replies and a word-wrapping editor
- **Teleconference**: real-time multi-node chat
- **RIP Showcase**: seven demo pages covering fonts, patterns, curves, every button style, pick lists, palette-cycling and sprite animation
- **Sysop Admin, all in RIP**: users, board settings, message areas, nodes and broadcasts, statistics, news and rules
- **Every setting takes effect live, with no restart**, including the telnet port
- Six color themes each caller can choose
- **BMX theme**: a spinning 5-spoke mag wheel on the welcome screen
- **BMX Games**: *Dirt Jumper* (side-scroller), *Cone Blaster* (shooter), *Spoke Breaker* (brick breaker), *Block Drop* (falling blocks) and *Turbo Lap* (3-D arcade road racer), with high score tables
- **File Libraries**: browse, search, download and upload files with built-in **Zmodem** (DialComm starts receiving by itself)
- **Echomail**: share message areas with **fsxNet, FidoNet** and other FTN networks through a built-in binkp mailer, set up in RIP or the Control Panel
- **Content Editor in RIP**: edit every menu (items, icons, modules, submenus), draw pixel icons, swap backgrounds, all live
- **Linked Teleconference**: link boards into one chat room, with line numbers and private messages across boards
- **Tray icon and Control Panel**: no console window. The tray icon opens a Control Panel with live stats, callers, users, settings and the log
- **BMX Newsmap**: BMX news as a RIP treemap, with a **QR code** on every story to open it on your phone
- **Auto-update**: checks this page every 6 hours, verifies each download, and installs only when nobody is online

| | |
|---|---|
| ![Signup](screenshots/signup.png) | ![Main menu](screenshots/main-menu.png) |
| ![Teleconference](screenshots/teleconference.png) | ![Palette magic](screenshots/palette-magic.png) |

| ![BMX Games](screenshots/bmx-games.png) | ![Dirt Jumper](screenshots/dirt-jumper.png) |
|---|---|
| ![Cone Blaster](screenshots/cone-blaster.png) | ![Sysop Admin](screenshots/sysop-admin.png) |
| ![BMX Newsmap](screenshots/newsmap.png) | ![Story with QR code](screenshots/news-story-qr.png) |
| ![Spoke Breaker](screenshots/spoke-breaker.png) | ![Block Drop](screenshots/block-drop.png) |
| ![Turbo Lap](screenshots/turbo-lap.png) | ![Turbo Lap in a curve](screenshots/turbo-lap-curve.png) |

| ![Content Editor](screenshots/content-editor.png) | ![Pixel icon editor](screenshots/pixel-editor.png) |
| ![Linked Teleconference](screenshots/linked-chat.png) | ![Private message from a linked board](screenshots/linked-private.png) |
| ![File Libraries](screenshots/file-libraries.png) | ![File details with Zmodem download](screenshots/file-details.png) |
| ![Echomail setup](screenshots/echomail.png) | ![A message from fsxNet](screenshots/echomail-message.png) |

![Control Panel](screenshots/control-panel.png)

## Quick start

1. Put `RipscoutBBS.exe` in its own folder (for example `C:\RipscoutBBS`) and double-click it. The Control Panel opens and the wheel icon appears in the system tray.
2. Allow the port in Windows Firewall (Command Prompt as Administrator):
   `netsh advfirewall firewall add rule name="RipscoutBBS" dir=in action=allow protocol=TCP localport=23`
3. In DialComm, add a Telnet entry for the server with **Emulation: RIPscrip**.
4. Call in and choose **New User Signup**. **The first account becomes the Sysop.**

The full manual is the **[Sysop Guide](SYSOP-GUIDE.md)**.

> Hosting note: RipscoutBBS has to run on a Windows PC or server. Web hosts like Netlify can't keep
> telnet connections open.
