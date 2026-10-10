# Your First Go Live from Mobile

This is a private setup test. Moblin or IRL Pro will send video to your BELABOX receiver container, and OBS will receive it. When the picture appears in OBS while you are on the `Live` scene, the core IRL streaming path is working.

## Before you start

- Make sure the BELABOX receiver container is running.
- Have the receiver's authentication key from `config.json` ready.
- For the first test, connect your phone and the OBS computer to the same home Wi-Fi network. Use the OBS computer's internal network IP address as the receiver address in the app.
- Confirm OBS has the BELABOX Media Source set up as described in [the receiver setup guide](<Setting Up a BELABOX SRTLA Receiver.md>).

The SRTLA destination URL has this format:

```text
srtla://RECEIVER_ADDRESS:5000?streamid=live/stream/belabox?srtauth=YOUR_SECRET_KEY
```

Replace `RECEIVER_ADDRESS` with the internal network IP address of the computer running Docker, and replace `YOUR_SECRET_KEY` with the key from `config.json`. Keep the stream ID and port as shown.

## Moblin (iPhone)

1. In Moblin, open **Settings → Streams → Create → Custom → SRT(LA)**.
2. Moblin opens a simple setup wizard. Paste the full SRTLA destination URL from above when prompted, then tap **Next** to continue through the wizard and save the stream.
3. Select the new stream in Moblin before starting the test.

## IRL Pro (Android)

1. Open **Settings → Connections** and add a connection.
2. Give it a name, such as `Home BELABOX`.
3. Set the protocol to **SRTLA**.
4. Paste the full SRTLA destination URL from above.
5. Save the connection and select it for streaming.

## Test the feed in OBS

1. Open OBS and select the `Live` scene.
2. Confirm the BELABOX Media Source is visible in the scene.
3. Start the stream from Moblin or IRL Pro. The app is sending to your BELABOX container; this does not start a public broadcast.
4. Check the OBS preview. When your phone's video appears, the phone-to-container-to-OBS path is working.
5. Stop the stream in the mobile app when you have finished testing.

If the feed does not appear, check that the receiver container is running, the phone is using the correct receiver IP and secret key, and UDP port `5000` is allowed through Windows Firewall. 

## References

- [BELABOX SRTLA receiver setup](<Setting Up a BELABOX SRTLA Receiver.md>)
- [Windows Firewall rules for BELABOX](<Setting Up Windows Firewall Rules for BELABOX.md>)
