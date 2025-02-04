---
id: live
title: Live View
---

Frigate has different live view options, some of which require [restream](restream.md) to be enabled.

## Live View Options

Live view options can be selected while viewing the live stream. The options are:

| Source | Latency | Frame Rate                             | Resolution     | Audio                        | Requires Restream | Other Limitations                            |
| ------ | ------- | -------------------------------------- | -------------- | ---------------------------- | ----------------- | -------------------------------------------- |
| jsmpeg | low     | same as `detect -> fps`, capped at 10  | same as detect | no                           | no                | none                                         |
| mse    | low     | native                                 | native         | yes (depends on audio codec) | yes               | not supported on iOS, Firefox is h.264 only  |
| webrtc | lowest  | native                                 | native         | yes (depends on audio codec) | yes               | requires extra config, doesn't support h.265 |

### WebRTC extra configuration:

webRTC works by creating a websocket connection on extra ports. One of the following is required for webRTC to work:
* Frigate is run with `network_mode: host` to support automatic UDP port pass through locally and remotely. See https://github.com/AlexxIT/go2rtc#module-webrtc for more details
* Frigate is run with `network_mode: bridge` and has:
    * Router setup to forward port `8555` to port `8555` on the Frigate device.
    * For local webRTC, you will need to create your own go2rtc config:

```yaml
log:
  format: text

webrtc:
  listen: ":8555"
  candidates:
    - <Frigate host ip address>:8555 # <--- enter Frigate host IP here
    - stun:8555
```

and pass that config to Frigate via docker or `frigate-go2rtc.yaml` for addon users:

See https://github.com/AlexxIT/go2rtc#module-webrtc for more details

```yaml
volumes:
  - /path/to/your/go2rtc.yaml:/config/frigate-go2rtc.yaml:ro
```
