# REblox Future Updates & Enhancements

This document outlines potential features and long-term improvements planned for the REblox Termux Auto-Reconnector project.

## 1. Remote Control via Discord Bot (Two-Way Communication)
Currently, the script sends logs *to* Discord via Webhooks. A future update could build a listener in Termux that receives commands *from* the Discord Bot.
* **Feature:** Typing `/reconnect all` or `/stop device-1` in the Discord server commands the Termux app running on the phone to instantly obey and restart/stop the game remotely.

## 2. Auto-Unlock Screen & Wake Device
If a phone screen turns off or locks, Android forcefully pauses all background activity (including Roblox and the script).
* **Feature:** Add logic that detects if the screen is off and uses `su -c "input keyevent 26"` to automatically wake the device back up, unlock the screen, and resume the game.

## 3. VPN Auto-Reconnect Integration
Roblox frequently IP-bans or shadow-bans accounts that are running automated farms on the same IP.
* **Feature:** Add an option in the "Create Config" menu to link a VPN app (like ProtonVPN or NordVPN). If the script detects that Roblox disconnected, it briefly uses an intent to trigger the VPN to "switch servers" to a new IP address *before* launching Roblox again, preventing IP bans.

## 4. VIP Server Private Link Support
Currently, the script uses `roblox://placeId=...` which only joins public servers.
* **Feature:** Parse massive Roblox Private Server Links that contain long Job IDs, and inject those into the `am start` intent so users can automatically farm in their own isolated, private servers instead of public lobbies.

## 5. Multi-Account Rotation (Account Switching)
If a user is running multiple instances of Roblox (`com.roblox.client1`, `client2`, etc.), they are likely rotating accounts.
* **Feature:** Build an automated script that clears the app data (`rm -rf /data/data/com.roblox.clientX/shared_prefs`) and logs into a *new* account from a pre-saved list every 24 hours to prevent a single account from being flagged for playing too long.

## 6. Battery Level Failsafes
For farmers running this on real phones instead of emulators.
* **Feature:** Monitor the battery level using `dumpsys battery`. If the battery drops below 15% and the phone isn't charging, the script smartly pauses Roblox to save power, explicitly sending a Discord webhook: *"WARNING: Device is dying, paused REblox."*

## 7. Advanced Crash Detection System
The previous crash detection system was removed to simplify the release. It used `dumpsys` and window focus checks to see if Roblox was in a "Not Responding" or "Crashed" state, rather than just checking `pidof`.
* **Feature:** Re-introduce a lightweight, robust crash detection layer to accurately identify App Not Responding (ANR) and Crash Dialogs so the reconnector can restart the app even if the process technically still exists in memory.
