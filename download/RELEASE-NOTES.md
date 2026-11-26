**RipscoutBBS 2.0.0: You decide when updates install**

- **New update setting** (Sysop Admin > Updates, or the Control Panel's Updates tab). When a new version comes out:
  - **Ask me first** (the new default): the board downloads it and checks its SHA-256 checksum, tells you with a notification, and **waits**. Nothing is installed until you press Install Now.
  - **Automatic:** installs by itself when nobody is online, as before.
  - **Off:** the board never contacts GitHub on its own (Check Now still works).
- **What's New:** read what an update changes before you install it.
  - In RIP: a What's New button in Sysop Admin > Updates, also offered before Install Now.
  - In the Control Panel: a What's new box on the Updates tab.
  - Both show the notes of every version newer than yours, so skipping versions loses nothing.
- **Staff notification:** sysops and co-sysops who are online get a notification when an update is ready ("Update 2.1.0 is ready"). Click it to open the Updates screen. Staff logging in later get the same notification.
- Update availability and installs are recorded in the audit trail.

**About this update:** boards set to install updates automatically install this one by themselves, as before. From now on, every board starts on **Ask me first**, so later versions wait for the sysop. Choose Automatic in Sysop Admin > Updates if you prefer it.
