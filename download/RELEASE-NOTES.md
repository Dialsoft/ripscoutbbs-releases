**RipScout BBS 2.3.0: Callers are told before an update**

- **Caller notice:** when you press Install Now, everyone online gets a notification on whatever screen they're on. By default it reads *"The BBS is installing new version 2.3.0. You'll be disconnected in 30 seconds - please call back in a minute!"*. The card stays up for the whole warning time.
- **A last word before the restart:** anyone still online when the time is up sees *"Installing version ... now. Please call back in a minute!"* before being disconnected.
- **Your words, your timing:** change the message and the warning time (5 to 600 seconds, default 30) in Sysop Admin > Updates > **Caller Notice**, or in the new **Caller notice** box on the Control Panel's Updates tab. `{version}`, `{seconds}`, `{board}` and `{current}` are filled in for you, and both screens show exactly what callers will see. Changes are live.
- Nothing else changed. Your settings, users and messages are untouched.
