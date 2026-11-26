**RipscoutBBS 1.7.0: Two-factor sign-in with Google Authenticator**

- **Two-factor sign-in** (Your Settings > Two-Factor): the board draws a QR code right on the RIP screen. Scan it with Google Authenticator (or Microsoft Authenticator, Authy...), type the 6-digit code, and from then on every login asks for the password and then the code.
  - **8 backup codes**, shown once, for a lost phone. Each works one time.
  - Callers can move to a **new phone**, make **new backup codes**, or **turn it off** themselves (each confirmed with a current code).
- **Reset tool for the sysop:** Reset 2FA on a member in Sysop Admin > Users, or on the Control Panel's Users tab, for someone who lost both their phone and their backup codes. Every reset is logged.
- **Authenticator name:** Board Settings (and the Control Panel's Settings tab) set the name callers see for this board in their authenticator app. Blank = the BBS name.

If you're running 1.6.0, this update installs itself the next time nobody is online.
