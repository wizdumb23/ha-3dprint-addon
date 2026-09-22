# OrcaSlicer Sidecar HA Add-on

![Version][version]

![Supports amd64 Architecture][amd64-shield]

## About
This add-on packages [maziggy/orca-slicer-api](https://github.com/maziggy/orca-slicer-api), a small REST API
wrapping headless [OrcaSlicer](https://github.com/SoftFever/OrcaSlicer). It's intended to run alongside
**BambuBuddy** as an external slicer backend, so BambuBuddy can submit 3MF/STL files and get back sliced G-code.

## Notes
1. **Architecture**
   - amd64 only — the upstream image bundles an x86_64 OrcaSlicer AppImage and is not published for aarch64/armv7.

2. **Port**
   - The API listens on `3000` inside the container, mapped to host port `3003`.
   - Change the host-side port mapping in the add-on's **Network** tab if `3003` conflicts with something else.

3. **Data directory**
   - `DATA_PATH` is set to `/data`, which Home Assistant persists automatically across add-on restarts/updates.

4. **Connecting BambuBuddy**
   - In BambuBuddy's settings, set the slicer/sidecar URL to `http://localhost:3003` (BambuBuddy runs with
     `host_network: true`, so it can reach this add-on's published port via `localhost`).
   - If BambuBuddy instead expects the sidecar address via an environment variable (e.g. `SLICER_API_URL`),
     that variable would need to be added to `bambuddy-daily/config.yaml`'s `environment:` block first —
     it isn't passed through today.

## Known issues
- None so far — this add-on has not yet been validated end-to-end with BambuBuddy. If the first slice request
  fails to connect, double-check the URL/port above.

## Installation
1. [Add the repository][repository] to your Home Assistant add-ons.
2. Install the **OrcaSlicer Sidecar** add-on.
3. Start the add-on.
4. Check `http://<HOME_ASSISTANT_HOST>:3003/health` to confirm it's running.

## Support
- If you encounter any issues, please open an issue on the [wizdumb23/ha-3dprint-addon GitHub repository](https://github.com/wizdumb23/ha-3dprint-addon/issues).
- Include your add-on logs and a brief description of the problem.

<!--
Assets
-->

[amd64-shield]: https://img.shields.io/badge/amd64-yes-green.svg
[version]: https://img.shields.io/badge/version-v1.0.0-blue.svg
[repository]: https://my.home-assistant.io/redirect/supervisor_add_addon_repository/?repository_url=https://github.com/wizdumb23/ha-3dprint-addon
