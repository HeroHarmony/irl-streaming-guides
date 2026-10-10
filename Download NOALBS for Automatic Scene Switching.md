# 5. Download NOALBS for Automatic Scene Switching

NOALBS can automatically switch OBS scenes based on the incoming stream's bitrate and connection status. It supports Windows and connects to OBS through OBS WebSocket.

Before configuring NOALBS, complete [4. Enable OBS WebSocket for Remote Control](<Setting Up OBS WebSocket.md>) so NOALBS can connect to OBS.

## Download NOALBS

1. Open the [official NOALBS releases page](https://github.com/NOALBS/nginx-obs-automatic-low-bitrate-switching/releases).
2. Open the latest release and download the Windows build from its **Assets**.
3. Extract the downloaded files to a permanent folder on the OBS computer, such as `C:\Tools\NOALBS`.
4. Keep the downloaded files together in that folder. Do not run NOALBS from inside the compressed archive.

## Use the example configuration

The repository includes a sanitized [NOALBS example configuration](<noalbs-config.example.json>) for this BELABOX setup. Copy it into the NOALBS folder as `config.json`, then replace both `REPLACE_WITH_SECRET_KEY` values with the key from the receiver's `config.json` and replace `REPLACE_WITH_OBS_WEBSOCKET_PASSWORD` with the password configured in OBS.

The example uses the scene names from this guide: `Staging`, `Live`, `Low`, `Finding Signal`, and `BRB / Privacy`. Its bitrate and RTT thresholds are generic starting values; adjust them to suit your connection. Account names, chat permissions, custom scene collections, and credentials from any personal configuration are not included.

Downloading NOALBS does not turn on automatic scene switching by itself. NOALBS must also be configured to connect to OBS and read the receiver's stream statistics. The [official NOALBS README](https://github.com/NOALBS/nginx-obs-automatic-low-bitrate-switching/blob/v2/README.md) has its current setup and configuration instructions.

## References

- [NOALBS releases](https://github.com/NOALBS/nginx-obs-automatic-low-bitrate-switching/releases)
- [NOALBS README](https://github.com/NOALBS/nginx-obs-automatic-low-bitrate-switching/blob/v2/README.md)
