# Setting Up Windows Firewall Rules for BELABOX

This guide covers the Windows Defender Firewall inbound rules for a `datagutt/belabox-receiver` Docker setup. It focuses on the receiver ports needed for SRTLA streaming and OBS playback.

## Table of Contents

- **Essentials**
  - [Ports to allow](#ports-to-allow)
  - [Check the active network profile](#check-the-active-network-profile)
  - [Allow SRTLA input](#allow-srtla-input)
- **Advanced**
  - [Allow OBS playback from another computer](#allow-obs-playback-from-another-computer)
  - [Optional receiver ports](#optional-receiver-ports)
  - [Verify or remove a rule](#verify-or-remove-a-rule)
  - [References](#references)

## Ports to allow

For the usual setup, create one inbound rule on the Docker host:

| Port | Protocol | When it is needed |
| --- | --- | --- |
| `5000` | UDP | Required for SRTLA input from Moblin, IRL Pro, or BELABOX. |
| `4000` | UDP | Only needed in Windows Firewall if OBS or another SRT player is on a different computer on your local network. |

You do not need a Windows Firewall inbound rule for OBS playback when OBS runs on the same computer as Docker and connects to `127.0.0.1`.

## Check the active network profile

Open PowerShell **as Administrator** on the Windows computer running Docker and check the network category:

```powershell
Get-NetConnectionProfile
```

The commands below apply to the **Private** profile, which is appropriate for a trusted home network. If the network is categorized as **Public**, the rules will not apply until the active profile matches. Only change a network to Private when you trust the devices on that network.

## Allow SRTLA input

Create an inbound rule for UDP port `5000`:

```powershell
New-NetFirewallRule `
  -DisplayName "BELABOX SRTLA Input (UDP 5000)" `
  -Direction Inbound `
  -Action Allow `
  -Protocol UDP `
  -LocalPort 5000 `
  -Profile Private
```

This allows SRTLA packets to reach the receiver on the Windows computer. The home router must also forward **UDP port 5000** to that computer for connections arriving from outside the home network. The Windows Firewall rule and router port forwarding are separate settings.

No separate outbound rule is needed for a typical Windows Firewall configuration.

## Allow OBS playback from another computer

If OBS is on a different computer on the same local network, add an inbound rule for UDP port `4000`, limited to the local subnet:

```powershell
New-NetFirewallRule `
  -DisplayName "BELABOX OBS Playback (UDP 4000)" `
  -Direction Inbound `
  -Action Allow `
  -Protocol UDP `
  -LocalPort 4000 `
  -RemoteAddress LocalSubnet `
  -Profile Private
```

In OBS, use the Docker host computer's internal network IP address in the SRT URL. Do not forward port `4000` through the router for this local playback setup.

## Optional receiver ports

The receiver documents these additional ports. Add rules only if you use the corresponding feature:

| Port | Protocol | Purpose |
| --- | --- | --- |
| `4001` | UDP | Direct SRT publishing without SRTLA. Not needed for Moblin or IRL Pro SRTLA input. |
| `8181` | TCP | Statistics HTTP API. Keep restricted to trusted local access if you use it. |
| `3000` | TCP | Legacy statistics and event hooks. Usually not needed. |
| `8282` | UDP | Legacy combined publish/play port for older setups. Not needed for the separate-port setup in the main guide. |

Port `4002/udp` is used internally by the receiver over localhost and does not need to be opened in Windows Firewall. Avoid creating broad inbound rules for the statistics ports or forwarding them through the router.

## Verify or remove a rule

List the BELABOX firewall rules:

```powershell
Get-NetFirewallRule -DisplayName "BELABOX*" |
  Format-Table DisplayName, Enabled, Direction, Action, Profile
```

Inspect the port and protocol filters:

```powershell
Get-NetFirewallRule -DisplayName "BELABOX SRTLA Input (UDP 5000)" |
  Get-NetFirewallPortFilter
```

Remove the rules created by this guide if needed:

```powershell
Remove-NetFirewallRule -DisplayName "BELABOX SRTLA Input (UDP 5000)"
Remove-NetFirewallRule -DisplayName "BELABOX OBS Playback (UDP 4000)"
```

## References

- [datagutt BELABOX Receiver manual](https://github.com/datagutt/bbox-receiver#manual)
- [Microsoft: New-NetFirewallRule](https://learn.microsoft.com/en-us/powershell/module/netsecurity/new-netfirewallrule)
- [Microsoft: Configure Windows Firewall with command-line tools](https://learn.microsoft.com/en-us/windows/security/operating-system-security/network-security/windows-firewall/configure-with-command-line)
- [BELABOX SRTLA receiver setup guide](<Setting Up a BELABOX SRTLA Receiver.md>)
