# OBS Scenes Setup for IRL

This guide lays out a practical OBS scene structure for IRL streaming with a BELABOX SRTLA receiver. It covers the minimum scenes to create, how to add the BELABOX feed, and optional scene organization for a more reusable setup.

## Table of Contents

- [Minimum scenes](#minimum-scenes)
- [Add the BELABOX feed](#add-the-belabox-feed)
- [Advanced scene organization](#advanced-scene-organization)
- [References](#references)

## Minimum scenes

Create these four scenes in OBS:

| Scene | Suggested use |
| --- | --- |
| `Staging` | A holding layout for preparing before going live. |
| `Live` | Your main on-air scene and default layout for the IRL feed. |
| `Low` | An alternate, simpler layout for situations where you want fewer visual elements. |
| `Finding Signal` | A clear holding screen to use when the incoming video feed is unavailable. |

Scene names are flexible, but using a consistent set makes it easier to switch deliberately while streaming.

## Add the BELABOX feed

Add the feed as an OBS **Media Source**:

1. Select the scene where you want the feed, or create the advanced `Capture: datagutt-belabox` scene described below.
2. In the **Sources** panel, select **+ → Media Source**.
3. Choose **Create new** and give the source a clear name, such as `BELABOX SRT`.
4. Uncheck **Local File**.
5. In **Input**, enter the receiver's SRT playback URL:

   ```text
   srt://127.0.0.1:4000?streamid=play/stream/belabox?srtauth=REPLACE_WITH_SECRET_KEY
   ```

   Use the Docker host computer's internal network IP address instead of `127.0.0.1` if OBS is on a different computer. Replace `REPLACE_WITH_SECRET_KEY` with the key from the receiver's `config.json`.

6. Select **OK**, then position and size the source in the scene.

If you use a separate capture scene, add that scene to `Live` and `Low` as a **Scene** source. This lets both layouts use the same configured BELABOX source.

For a normal feed that should stay connected while its scene is hidden, leave **Close file when inactive** unchecked in the Media Source properties. OBS documents that enabling it unloads the source while it is not active.

## Advanced scene organization

These scenes are optional, but can make the layout easier to reuse and manage.

### `Capture: datagutt-belabox`

Create a scene named `Capture: datagutt-belabox` and put the BELABOX Media Source there. Treat it as the central setup for the live feed, then add it to both `Live` and `Low` using **Sources → + → Scene**. You can adjust the capture source once and have both layouts use it.

### `Overlays`

Create a separate `Overlays` scene for graphics or other elements that should appear consistently across multiple layouts. Add it to `Live` and `Low` as a **Scene** source. This keeps shared elements in one place.

### `BRB / Privacy`

Create a scene with a full-screen BRB or privacy graphic and no live camera feed. Switch to it when you want to hide the feed but keep the broadcast connected. This is useful when reconnecting after stopping the stream may be difficult. Use `Finding Signal` to indicate a missing feed; use `BRB / Privacy` when you are intentionally hiding it.

Keep **Close file when inactive** unchecked on the BELABOX Media Source if you want OBS to keep that source loaded while you are on the privacy scene.

### `Sleep`

Create an empty scene with no sources for periods when you are not using the other layouts. With no scene sources active to render, it can reduce source-related workload while OBS remains open.

## References

- [Setting Up a BELABOX SRTLA Receiver](<Setting Up a BELABOX SRTLA Receiver.md>)
- [OBS Media Sources](https://obsproject.com/kb/media-sources)
- [OBS Sources Guide](https://obsproject.com/kb/sources-guide)
