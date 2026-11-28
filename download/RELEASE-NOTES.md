**RipScout BBS 2.8.0: Live Camera**

- **Live Camera:** show a network camera to callers as live RIPscrip video. Set it up on the Control Panel's new **Camera** tab:
  - Camera types: **MOBOTIX** (Q24, M24, Q25, M16 and others), **Axis**, or any camera with an **MJPEG** or **snapshot** address. Basic and Digest logins both work.
  - Name it, choose who can watch (every member, validated, trusted, co-sysops or only you), **Standard** or **Sharp** picture, and the most frames a second (default 8).
  - **Test** connects for 30 seconds and shows exactly what callers will see.
  - While it's turned on, **Live Camera** appears on the Main Menu (key V). Callers press **S** to switch Standard/Sharp and **Q** to go back.
  - The board talks to the camera only while someone is watching, and everyone shares one connection. It picks the best 16 colors for the scene and sends only what changed, so a still scene costs almost nothing: about 50-150 KB a second for Standard.
  - Every viewer is logged in the Audit tab. All settings are live.
