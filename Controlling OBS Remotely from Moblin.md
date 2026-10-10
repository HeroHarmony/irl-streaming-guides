# 6. Control OBS Remotely from Moblin

OBS remote control lets you control your streaming computer from your phone while you are out. You can switch OBS scenes and start or stop OBS streaming to the platform configured in OBS, such as Twitch or YouTube. The SRTLA video feed Moblin sends to the BELABOX receiver is a separate connection; OBS uses that feed in its scenes and broadcasts the selected scene when you start streaming in OBS.

Complete [4. Enable OBS WebSocket for Remote Control](<Setting Up OBS WebSocket.md>) first. OBS WebSocket must be enabled, and you will need the OBS WebSocket password.

## Add the OBS connection in Moblin

1. In Moblin, open **Settings → Streams → [your stream] → OBS remote control**.
2. Enable **OBS remote control**, then open that setting.
3. Enter the WebSocket address for the computer running OBS, using this format:

   ```text
   ws://OBS_COMPUTER_ADDRESS:4455
   ```

   `ws://` identifies the WebSocket connection, `OBS_COMPUTER_ADDRESS` is the reachable IP address or host name of the OBS computer, and `4455` is the OBS WebSocket port from Guide 4. For example, on a trusted home network the address might look like `ws://192.168.1.50:4455`.

4. If Moblin asks for a password, enter the OBS WebSocket password you set in Guide 4. Save the settings.

Use the OBS computer's internal network IP when your phone is on the same Wi-Fi. When you are away from home, a direct `ws://` connection needs a secure path back to your network, such as a VPN. Do not forward port `4455` to the public internet. Moblin's [OBS Remote Control Relay](https://moblin.app/) is an alternative for remote access that does not require router port forwarding or a VPN; follow Moblin's current setup instructions if you choose that method.

Do not use `localhost` or `127.0.0.1` in Moblin on your phone: those addresses refer to the phone itself, not your OBS computer.

## Test remote scene switching

With OBS open and the WebSocket server enabled, open Moblin and confirm it connects to OBS. Try switching between scenes before relying on the controls during a stream. Moblin can also start or stop streaming in OBS. **Starting OBS streaming sends the selected scene to the configured platform and can make it publicly viewable; it is different from the private mobile-to-container test in Guide 3.** Confirm OBS is configured for the intended Twitch or YouTube account and that the selected scene is ready before using the stream control. If Moblin cannot connect, check the address, port, password, Windows Firewall access, and whether the phone can reach the OBS computer over the network or VPN.

## Alternative: OBS Blade

[OBS Blade](https://github.com/Kounex/obs_blade) is another app for controlling OBS over WebSocket. Configure it with the OBS computer's reachable address, port `4455`, and the same WebSocket password. Like Moblin, the phone must have a network path to the OBS computer; do not expose the WebSocket port directly to the public internet.

## References

- [Guide 4: Enable OBS WebSocket for Remote Control](<Setting Up OBS WebSocket.md>)
- [Moblin documentation](https://github.com/eerimoq/moblin)
- [OBS Blade](https://github.com/Kounex/obs_blade)
