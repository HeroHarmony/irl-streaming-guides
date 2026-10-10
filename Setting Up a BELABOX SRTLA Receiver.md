# Setting Up a BELABOX SRTLA Receiver with Docker on Windows

This guide explains how to set up a self-hosted SRTLA receiver on a fresh Windows computer using **datagutt/belabox-receiver**.

It allows mobile streaming apps such as Moblin and IRL Pro to transmit video over the internet to OBS Studio on a Windows computer.

## Table of Contents

- [Install Docker Desktop](#1-install-docker-desktop)
- [Create a Configuration File](#2-create-a-configuration-file)
- [Download and Start the Receiver](#3-download-and-start-the-receiver)
- [Configure Windows Firewall and Router](#4-configure-windows-firewall-and-router)
- [Configure Moblin (iOS)](#5-configure-moblin-ios)
- [Configure IRL Pro (Android)](#6-configure-irl-pro-android)
- [Configure OBS Studio](#7-configure-obs-studio)
- [Useful Docker Commands](#8-useful-docker-commands)
- [References](#references)

## 1. Install Docker Desktop

1. Download and install [Docker Desktop for Windows](https://www.docker.com/products/docker-desktop/).
2. Enable WSL 2 if prompted. Restart Windows if required.
3. Launch Docker Desktop and ensure the Docker engine is running.
4. Open PowerShell and verify the installation:

```powershell
docker --version
docker info
```

## 2. Create a Configuration File

Create a folder:

`C:\Docker\belabox`

Inside it, create a file named `config.json` containing:

```json
{
  "auth": [
    {
      "user": "belabox",
      "key": "REPLACE_WITH_SECRET_KEY"
    }
  ]
}
```

Replace `REPLACE_WITH_SECRET_KEY` with a private authentication key. The same key will be used by your streaming apps and OBS.

## 3. Download and Start the Receiver

Open PowerShell and navigate to the folder:

```powershell
cd C:\Docker\belabox
```

Run the following command:

```powershell
docker run -d `
  --name belabox `
  --restart=always `
  -p 5000:5000/udp `
  -p 4000:4000/udp `
  -p 4001:4001/udp `
  -p 8181:8181/tcp `
  -p 3000:3000/tcp `
  -v "${PWD}\config.json:/app/config.json:ro" `
  datagutt/belabox-receiver:latest
```

Docker will download the image and start the receiver.

**Automatic startup**

The `--restart=always` parameter instructs Docker to restart the container automatically whenever Docker starts, including following a computer reboot or unexpected container termination.

To ensure Docker starts automatically with Windows:

1. Open Docker Desktop.
2. Navigate to **Settings → General**.
3. Enable **Start Docker Desktop when you sign in to your computer**.
4. Apply the setting.

Note that Docker Desktop's standard automatic startup occurs when the Windows user signs in, rather than necessarily before sign-in.

Verify the container is running:

```powershell
docker ps
```

## 4. Configure Windows Firewall and Router

Allow inbound UDP traffic on port `5000` through Windows Firewall.

For detailed rule setup instructions, see [Setting Up Windows Firewall Rules for BELABOX](<Setting Up Windows Firewall Rules for BELABOX.md>).

On your router, forward UDP port `5000` to the Windows computer's local IP address.

Ideally, configure a DHCP reservation for the computer so that its local IP address does not change.

Port `4000` is used for OBS playback and does not need to be forwarded when OBS is running on the same computer.

Do not expose the administration and statistics ports unnecessarily.

## 5. Configure Moblin (iOS)

On your iPhone, open Moblin and configure an SRTLA stream destination.

**Protocol:** SRTLA

**Server URL:**

```text
srtla://YOUR_PUBLIC_IP:5000?streamid=live/stream/belabox?srtauth=REPLACE_WITH_SECRET_KEY
```

Replace `YOUR_PUBLIC_IP` with your home internet public IP address and use the secret key from `config.json`.

Save the configuration and select it as the streaming destination.

## 6. Configure IRL Pro (Android)

On your Android device, open IRL Pro and navigate to **Settings → Connections**.

Add a new connection using the following information:

**Name:** Home BELABOX

**Protocol:** SRTLA

**Connection URL:**

```text
srtla://YOUR_PUBLIC_IP:5000?streamid=live/stream/belabox?srtauth=REPLACE_WITH_SECRET_KEY
```

Replace the public IP address and secret key with the same values used for Moblin.

Save the connection and select it for streaming.

Moblin and IRL Pro can use the same receiver configuration, although only one should publish to this particular stream identity at a time. Separate identities can be configured to support multiple simultaneous streams.

## 7. Configure OBS Studio

Open OBS Studio on the Windows computer.

1. Add a new **Media Source**.
2. Uncheck **Local File**.
3. Enter the following URL in the Input field:

```text
srt://127.0.0.1:4000?streamid=play/stream/belabox?srtauth=REPLACE_WITH_SECRET_KEY
```

4. Click OK.

If OBS and the Docker container are running on the same computer, use `127.0.0.1` in the URL. If they are on separate computers, use the Docker host computer's internal network IP address instead.

Start streaming from Moblin or IRL Pro. The video feed should appear in OBS.

## 8. Useful Docker Commands

**Check running containers:**

```powershell
docker ps
```

**View receiver logs:**

```powershell
docker logs belabox
```

**Restart the receiver:**

```powershell
docker restart belabox
```

**Stop the receiver:**

```powershell
docker stop belabox
```

**Start the receiver:**

```powershell
docker start belabox
```

**Check the restart policy:**

```powershell
docker inspect -f '{{.HostConfig.RestartPolicy.Name}}' belabox
```

The output should be `always`.

**Enable automatic restart on an existing container:**

```powershell
docker update --restart=always belabox
```

This avoids having to recreate the container just to change its restart policy.

## References

- [datagutt BELABOX Receiver GitHub](https://github.com/datagutt/bbox-receiver)
- [Docker Hub](https://hub.docker.com/r/datagutt/belabox-receiver)
- [Docker Restart Policies](https://docs.docker.com/engine/containers/start-containers-automatically/)
- [IRL Pro SRTLA Connection Guide](https://irlserver.com/guides/connect-irl-pro)
