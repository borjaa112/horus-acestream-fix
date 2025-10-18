

# AceStream on Docker for Horus on ARMv7 Devices

This tutorial explains how to run **AceStream Engine** in a Docker container for use with the **Horus addon** on ARMv7 devices running **LibreELEC**. It walks through installation, setup, and troubleshooting steps.

> 💬 **Comment:** This method fixes the common Horus issue where streams fail to play because the addon requests activating proxies or paying for Premium. Running AceStream externally in Docker bypasses that limitation.

---

## 1. Compatibility

> 💬 **Comment:** This setup is designed for devices with **ARMv7 architecture**, such as Raspberry Pi 2/3 or other ARMv7-based LibreELEC devices.  
> It works with the Horus addon as the AceStream client.

---

## 2. Install AceStream in Docker

> 💬 **Comment:** We will use the Docker image `futebas/acestream-engine-arm:3.2.7.6`, which is compatible with ARMv7 devices.

### Step-by-step instructions:

1. Open SSH to your LibreELEC device:

```bash
ssh root@<IP-of-your-LibreELEC>
````

2. Make sure Docker is installed. If not, install it from Kodi:

* `Add-ons → Download → Services → Docker`
* Reboot LibreELEC after installation.

3. Run the AceStream container:

```bash
docker run -d --name acestream \
  --restart unless-stopped \
  -p 6878:6878 \
  -v /storage/acestream:/opt/acestream \
  futebas/acestream-engine-arm:3.2.7.6
```

**Parameters explained:**

* `-d` → run in the background
* `--name acestream` → container name
* `--restart unless-stopped` → automatically restart on LibreELEC boot
* `-p 6878:6878` → external port 6878 mapped to the engine’s internal port
* `-v /storage/acestream:/opt/acestream` → persistent data (settings, cache)
* Image: `futebas/acestream-engine-arm:3.2.7.6`

> 💬 **Comment:** Docker will download the image if not present and start the container.

---

## 3. Verify and Test with Horus

> 💬 **Comment:** After installing the container, reboot LibreELEC.

1. Open Kodi and go to the Horus addon.
2. Configure the AceStream engine URL:

```
http://<IP-of-your-LibreELEC>:6878
```

3. Try playing a stream from Horus.
4. If the stream plays successfully, the setup is complete.

> 💬 **Comment:** Running AceStream externally in Docker fixes the proxy/Premium limitation that prevents Horus from playing streams.

---

## 4. Troubleshooting

> 💬 **Comment:** If Horus fails to connect to AceStream after installing the container:

* Stop Kodi.
* Delete the folder:

```
/storage/.kodi/userdata/addon_data/script.module.horus/acestream.engine/
```

* Restart Kodi and try again.

> 💬 **Comment:** This will remove any cached or corrupted AceStream engine data from Horus.

---

## 5. Additional Notes

* The container logs can be viewed using:

```bash
docker logs -f acestream
```

* Persistent data is stored in `/storage/acestream`, so it survives reboots.
* Make sure port **6878** is accessible within your network if connecting from other devices.

---

## Credits

* Docker image: [futebas/acestream-engine-arm](https://hub.docker.com/r/futebas/acestream-engine-arm)
* LibreELEC: [https://libreelec.tv](https://libreelec.tv)
* Horus addon: community-maintained Kodi addon

---

Enjoy AceStream on your ARMv7 LibreELEC device with Horus — now without proxy or Premium restrictions!

