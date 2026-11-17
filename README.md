# Steam in Container

This repository provides a container image that extends the [Selkies NVIDIA GLX Desktop](https://github.com/selkies-project) environment with full Steam client integration, while maintaining compatibility with modern NVIDIA drivers and accelerated OpenGL rendering pipelines.

The containerised desktop environment supports hardware-accelerated graphics, a fully interactive graphical user interface delivered through the browser via Selkies WebRTC technology, and persistence of all Steam runtime data and game libraries.

---

## Build Instructions

Run the following command from the repository directory:

```bash
podman build -t steam-in-container .
```

---

## Directory and Volume Mapping

To persist all Steam runtime files, game libraries, and user configuration, bind mount or map a volume to `/home/ubuntu/.steam/` directory.

> Note: The default container user is `ubuntu`.

---

## Deployment Instructions

### Host Preparation

1. Ensure that the **NVIDIA GPU drivers** are installed and compatible with the NVIDIA Container Toolkit. 
2. Confirm that either **Docker** or **Podman** is configured with GPU passthrough support.
3. Create persistent storage directories or named volumes for Steam data.

### Deployment 

```bash
podman run -d \
   --name steam-in-container \
   --restart=unless-stopped \
   --shm-size=2g \
   --gpus 0 \
   -e CDEPTH=24 \
   -e DPI=96 \
   -e NVIDIA_DRIVER_CAPABILITIES=all \
   -e PASSWD=passwd \
   -e REFRESH=60 \
   -e SIZEH=1080 \
   -e SIZEW=1920 \
   -e TZ=Asia/Kolkata \
   -e VIDEO_PORT=DFP \
   -e WEBRTC_ENCODER=nvh264enc \
   -p 8080:8080 \
   -v cad-in-container:/home/ubuntu/ \
   steam-in-container
```

### Environment Variables

| Variable                      | Purpose |
| ------------------------------ | ------- |
| `CDEPTH=24`                    | Colour depth (in bits per pixel) of the virtual desktop. `24` corresponds to **true colour** (8 bits per channel). |
| `DPI=96`                       | Sets the **dots-per-inch** for the virtual desktop display, affecting scaling of GUI elements. |
| `NVIDIA_DRIVER_CAPABILITIES=all` | Grants the container access to **all NVIDIA driver features**, including compute, graphics, video encoding/decoding, and display. Required for Steam and GLX desktop acceleration. |
| `PASSWD=`                      | Sets the password used for desktop access. |
| `REFRESH=60`                   | Target **refresh rate** of the virtual desktop in Hertz. |
| `SIZEH=1080`                   | Sets the **height** of the virtual desktop in pixels. |
| `SIZEW=1920`                   | Sets the **width** of the virtual desktop in pixels. |
| `TZ=Asia/Kolkata`              | Sets the container timezone to **Asia/Kolkata**. Ensures logs, timestamps, and any time-dependent operations within the container use the correct timezone. |
| `VIDEO_PORT=DFP`               | Specifies the default display output for the virtual desktop session. Typically `DFP` corresponds to the primary digital flat panel output. |
| `WEBRTC_ENCODER=nvh264enc`     | Selects the **hardware-accelerated H.264 video encoder** provided by NVIDIA for WebRTC streaming. Ensures efficient real-time video encoding. |

---

## Licensing

- The project, including all modifications and additions, is licensed under the
  **GNU Affero General Public License (AGPL-3.0)**.  
- The Selkies project components are licensed under the **Mozilla Public License 2.0 (MPL-2.0)**.  
- The base Linux distribution adheres to its upstream licensing terms.  
- The Steam client is distributed under proprietary licensing by Valve Corporation.  

---
