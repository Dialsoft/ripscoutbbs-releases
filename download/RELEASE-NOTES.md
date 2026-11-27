**RipscoutBBS 2.1.1: Updates that switch over cleanly**

- **No more two copies running after an update.** The new version now waits for the old one to quit before it opens the telnet port. If the old copy is still running after 15 seconds, the new one closes it. This also works when updating *from* an older version.
- **The Control Panel stays open through an update.** It keeps the same address, so during the restart it shows "Restarting? Reconnecting...", then carries on by itself and says "Update finished: now running version X". No need to reopen it.
- **Install Now always installs the newest version.** Before, if a newer release came out after one was already downloaded, the board could install the older download. Now the older download is thrown away and the newest is fetched.
- The log and the audit trail say exactly what happened during each update.
- Nothing else changed. Your settings, users and messages are untouched.
