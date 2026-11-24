# BU8000 customisation

This branch adapts CleanFrame's browser-first home interface for a Samsung UA43BU8000KLXL. It is an independent fork experiment, not a verified BU8000 build.

## Home screen

- The initial Home app set is YouTube and Prime Video.
- On first launch without saved preferences, Home controls default to icons-only.
- The front-row controls are HDMI inputs, AirPlay settings, Bluetooth audio, Picture mode, USB media and Settings.
- Existing List/Dock layouts, directional navigation and local preferences are retained.
- YouTube and Prime Video are also recognised by their installed display names during app discovery. The launcher then uses the app IDs returned by that TV, rather than assuming the reference Frame's IDs are universal.

## What the shortcuts actually do

| Shortcut | Implementation | Device verification still needed |
| --- | --- | --- |
| HDMI inputs | Reuses CleanFrame's Tizen VIDEO SOURCE discovery and `tvwindow.setSource` path. | Confirm reported ports and visible handoff on the BU8000. |
| AirPlay settings | Opens Samsung's TV Settings app. Navigate to Apple AirPlay Settings there. | Confirm the destination and menu path. |
| Bluetooth audio | Opens Samsung's TV Settings app. Navigate to Sound Output / Bluetooth Speaker List there. | Confirm the destination and menu path. |
| Picture mode | Opens Samsung's TV Settings app. Navigate to Picture / Picture Mode there. | Confirm the destination and menu path. |
| USB media | Uses the Tizen filesystem storage list to display mounted external volumes. Selecting one opens Samsung Home, where native Source / USB browsing remains responsible for playback. | Confirm external-volume reporting and the Samsung Home destination. |

The AirPlay, Bluetooth and Picture shortcuts do **not** deep-link into individual settings pages. CleanFrame currently uses the known Samsung Settings application entry point; the final selection inside Settings is made with the TV remote. No undocumented settings URI or app-control payload is assumed.

USB discovery is read-only. It filters for mounted `EXTERNAL` storage. Samsung's Tizen API documents `removable2` as the USB-host label where supported, but storage labels can vary by platform, so other external volumes are named generically. It does not enumerate files, write to a drive, or implement a media player. See the [Samsung Filesystem API](https://developer.samsung.com/smarttv/develop/api-references/tizen-web-device-api-references/filesystem-api.html).

## Safe development boundary

The branch changes only application source and its read-only filesystem privilege. It does not change native Home-button routing, startup behaviour, firmware, certificates, package identity, or the TV itself. Browser preview device actions remain unavailable and report that a TV is required.

Before installing on the BU8000, follow the contributor setup guide and use your own Samsung certificate profile and SDK-generated package/application identity in ignored `.local/signing.json`. Keep the reference source identity guards. Do not reuse another owner's certificates or private setup.

## First on-device verification

1. Confirm the exact model and software version from Settings → Support → About This TV.
2. Enable Developer Mode and connect only when the owner explicitly chooses to.
3. Sign and install an independent package using the owner's own credentials.
4. Confirm the actual YouTube and Prime Video app discovery results.
5. Test each HDMI entry, then check AirPlay, Bluetooth, Picture and USB handoffs individually.
6. Record visible results. A Tizen success callback alone is not proof that the intended Samsung screen appeared.

Do not attempt native Home/startup replacement as part of this branch.
