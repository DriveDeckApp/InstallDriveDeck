<div align="center">

# Install DriveDeck

**Turn your iPhone or iPad into an Apple CarPlay head unit.**

[![Add Source to AltStore](https://img.shields.io/badge/AltStore-Add_Source-1A66DB?style=for-the-badge)](https://drivedeck-carplay.vercel.app/altstore/)
[![Flash ESP32](https://img.shields.io/badge/ESP32-Flash_in_Browser-C8F75A?style=for-the-badge&logoColor=black)](https://drivedeck-carplay.vercel.app)
[![Patreon](https://img.shields.io/badge/Patreon-$5_Lifetime_Access-FF424D?style=for-the-badge)](https://www.patreon.com/DriveDeck)

</div>

---

> ### ⚠️ AltStore distribution is still in progress
>
> The AltStore route described below is **not finalised yet**. It works, but it is still being set up
> and may change or break without warning. If you want a supported install today, get **lifetime
> TestFlight access** instead at
> **[drivedeck-carplay.vercel.app](https://drivedeck-carplay.vercel.app/#access)** — one $10 payment,
> installs through Apple's own TestFlight app, and updates arrive automatically.

---

DriveDeck is a real CarPlay **receiver**, not a simulator. Mount an old iPhone or iPad in your car and
it becomes the head unit — Maps, Music, Messages, Siri — streamed from the phone in your pocket, or
from the very same device.

This guide takes about **15 minutes** end to end. Read it once before starting; step 1 is the fiddly
part and everything after is quick.

---

## Table of contents

1. [What you'll need](#1-what-youll-need)
2. [Install AltStore](#2-install-altstore)
3. [Connect your Patreon account](#3-connect-your-patreon-account)
4. [Add the DriveDeck source](#4-add-the-drivedeck-source)
5. [Install DriveDeck](#5-install-drivedeck)
6. [Flash your ESP32](#6-flash-your-esp32)
7. [First run](#7-first-run)
8. [Troubleshooting](#troubleshooting)
9. [Keeping the app alive](#keeping-the-app-alive)

---

## 1 · What you'll need

| | Why |
|---|---|
| **iPhone or iPad on iOS 17 or later** | Runs DriveDeck as the head unit |
| **A Mac or Windows PC** | One-time only, to install AltStore |
| **An ESP32 board** (~$5–$15) | Provides the Wi-Fi network CarPlay requires |
| **A USB cable** | To flash the ESP32, and to power it in the car |
| **[DriveDeck Patreon](https://www.patreon.com/DriveDeck) membership — one-time $5** | The app is distributed to patrons. $5 once, lifetime access — not a subscription |

### Which ESP32 to buy

Either of these works. The first is the cheapest thing that does the job; the second adds a screen
that shows the network details and whether your phone has connected — genuinely useful in a car.

- **[ESP32 classic dev board](https://a.co/d/0bIfPpNL)** — cheapest option
- **[LilyGO T-Display-S3 Touch](https://a.co/d/07MaoqIS)** — adds an on-board status screen

Any ESP32, ESP32-S2, ESP32-S3 or ESP32-C3 board will work. You do **not** need a SIM card, an antenna,
or any soldering.

> **Why an ESP32 at all?** Wireless CarPlay refuses to start unless the device is on a Wi-Fi network —
> iOS requires a real Wi-Fi address before it will connect. You do not need *internet*, just a network.
> A $5 ESP32 provides one. [Full explanation here.](https://github.com/DriveDeckApp/drivedeck-esp32-ap#why-this-exists)

---

## 2 · Install AltStore

DriveDeck cannot be on the App Store — it uses private system frameworks that Apple does not permit —
so it is distributed through [AltStore](https://altstore.io), a well-established sideloading tool.

1. On your **computer**, download **AltServer** from [altstore.io](https://altstore.io) and install it.
2. Connect your iPhone or iPad to the computer with a cable, and **unlock** the device.
3. In AltServer, choose **Install AltStore** and pick your device.
4. Enter your **Apple ID** and password when prompted.
   - Start with your normal Apple ID password; AltServer will prompt for the 2FA code separately.
   - If it rejects that, create an **app-specific password** at
     [appleid.apple.com](https://appleid.apple.com) and use that instead — AltServer requires one on
     some two-factor setups.
   - A free Apple ID is fine. See [Keeping the app alive](#keeping-the-app-alive) for what that means.
5. On the device, go to **Settings → General → VPN & Device Management**, tap your Apple ID, and
   choose **Trust**.
6. Open AltStore on the device. You should see the Browse tab.

Detailed official walkthrough: **[faq.altstore.io](https://faq.altstore.io)**

> If AltServer fails with an **HTTP 503**, that is Apple's authentication server, not AltStore and not
> DriveDeck. Wait 30–60 minutes and try again — see [Troubleshooting](#troubleshooting).

---

## 3 · Connect your Patreon account

DriveDeck is available to patrons, so AltStore needs to know who you are.

1. Join at **[patreon.com/DriveDeck](https://www.patreon.com/DriveDeck)** — a **one-time $5 payment
   for lifetime access**. It is not a recurring subscription.
2. In AltStore, open the **Settings** tab.
3. Tap **Patreon** and sign in with the same account.

You only have to do this once.

---

## 4 · Add the DriveDeck source

**The easy way** — open this page on the iPhone or iPad itself and tap the button:

### → [drivedeck-carplay.vercel.app/altstore](https://drivedeck-carplay.vercel.app/altstore/)

**The manual way** — in AltStore, go to **Browse → Sources → ➕** and paste:

```
https://drivedeck-carplay.vercel.app/source.json
```

DriveDeck will then appear in the Browse tab.

---

## 5 · Install DriveDeck

1. In AltStore, open **Browse** and tap **DriveDeck**.
2. Tap **Install** and wait — it is about 12 MB.
3. The DriveDeck icon appears on your home screen.

If install is refused, check that your Patreon membership is active and connected (step 3).

---

## 6 · Flash your ESP32

This is the Wi-Fi network DriveDeck needs. **No IDE and no command line required** — it flashes
directly from your browser.

### → [Open the web flasher](https://drivedeck-carplay.vercel.app)

1. Plug the ESP32 into your **computer** with a USB cable.
2. Open the flasher in **Chrome, Edge or Opera** on desktop.
   *(Safari and Firefox do not support Web Serial, and no browser on iOS does.)*
3. Pick your board from the dropdown.
4. Press **Connect & Flash**, choose the serial port when prompted, and confirm.
5. Under a minute later the board is broadcasting:

| | |
|---|---|
| **Network** | `DriveDeckAP` |
| **Password** | `drivedeck123` |
| **Channel** | `6` |

Prefer to build it yourself, or want to see what you are flashing? Full source, wiring notes and
manual instructions for the Arduino IDE and `arduino-cli`:

**[github.com/DriveDeckApp/drivedeck-esp32-ap](https://github.com/DriveDeckApp/drivedeck-esp32-ap)**

---

## 7 · First run

1. Power the ESP32 from any USB port — the car's own USB socket is ideal.
2. On the iPhone, go to **Settings → Wi-Fi** and join **DriveDeckAP** (password `drivedeck123`).
   - iOS will warn there is **no internet connection**. That is expected and harmless; your data keeps
     working over cellular.
   - **You only have to do this once.** iOS remembers the network and re-joins automatically whenever
     the board is powered and nearby.
3. Turn **off** the iPhone's own **Personal Hotspot** (Settings → Personal Hotspot).
   Hosting a hotspot uses the same radio that wireless CarPlay needs and will stop it starting.
4. Open **DriveDeck**. The defaults are already correct for the ESP32 — network name, password and
   channel are pre-filled.
5. Tap **Start Receiver**.
6. Approve the CarPlay prompt if iOS shows one.

CarPlay should appear within a few seconds. The in-app **Setup Guide** repeats these steps on the
device, and the **Protocol Inspector** shows exactly what is happening if something stalls.

---

## Troubleshooting

<details>
<summary><strong>AltServer: "Apple's authentication servers returned an error (HTTP 503)"</strong></summary>

This is Apple's end, not AltStore or DriveDeck. Usually rate limiting from repeated sign-in attempts.

- Wait **30–60 minutes** before retrying. Rapid retries make it worse.
- Check [Apple's System Status](https://www.apple.com/support/systemstatus/).
- Try an **app-specific password** from [appleid.apple.com](https://appleid.apple.com) if the normal
  one is rejected.
</details>

<details>
<summary><strong>Install is refused in AltStore</strong></summary>

- Confirm your [Patreon](https://www.patreon.com/DriveDeck) membership is **active** (one-time $5, lifetime access).
- Confirm Patreon is connected: AltStore → **Settings → Patreon**.
- Pull to refresh the source: AltStore → Browse → Sources.
</details>

<details>
<summary><strong>"Untrusted Developer" when opening the app</strong></summary>

**Settings → General → VPN & Device Management** → tap your Apple ID → **Trust**.
</details>

<details>
<summary><strong>The DriveDeckAP network does not appear</strong></summary>

- Confirm the ESP32 has power. On a T-Display-S3 the screen shows the network details and turns green
  when a device connects.
- Re-flash it, and check the serial monitor at **115200 baud** — the firmware prints its SSID and
  channel on boot.
</details>

<details>
<summary><strong>Nothing happens after Start Receiver</strong></summary>

In order of likelihood:

1. The iPhone's **Personal Hotspot is on** — turn it off. This is the most common cause.
2. The phone is not actually joined to `DriveDeckAP` — check Settings → Wi-Fi.
3. The network name, password or **channel** in DriveDeck does not match the board.
4. An older build of the app is still installed and holding the network port — delete it.

The **Protocol Inspector** inside the app gives the exact reason, on-device, with no computer needed.
</details>

<details>
<summary><strong>No sound in the car</strong></summary>

Audio stays on the iPhone by design, so it reaches your car however the phone normally does — usually
Bluetooth. In DriveDeck, set **Audio** to **Bluetooth**, connect your car, and tap
**Remember this device**. If the car is not connected, sound comes out of the phone's speaker instead.
</details>

<details>
<summary><strong>The picture is sharp but the layout is wrong, or there are black bars</strong></summary>

Set **Resolution** in Settings to match your screen, then restart the receiver. The resolution is
negotiated once per session. On a folding device, **unfold it before starting**.
</details>

---

## Keeping the app alive

Sideloaded apps are signed with *your* Apple ID, and that signature expires:

| Apple ID | App lasts | Refresh |
|---|---|---|
| **Free** | 7 days | Open AltStore on the same Wi-Fi as AltServer and tap **Refresh All** |
| **Paid Developer ($99/yr)** | 1 year | Rarely |

AltStore can refresh in the background when your device is on the same network as a running AltServer.
Leaving AltServer running on a computer at home means it usually happens without you noticing.

This is a limitation of sideloading on iOS, not of DriveDeck.

---

## Links

| | |
|---|---|
| **Add AltStore source** | [drivedeck-carplay.vercel.app/altstore](https://drivedeck-carplay.vercel.app/altstore/) |
| **Source URL (manual)** | `https://drivedeck-carplay.vercel.app/source.json` |
| **ESP32 web flasher** | [drivedeck-carplay.vercel.app](https://drivedeck-carplay.vercel.app) |
| **ESP32 firmware source** | [github.com/DriveDeckApp/drivedeck-esp32-ap](https://github.com/DriveDeckApp/drivedeck-esp32-ap) |
| **Patreon** — one-time $5, lifetime | [patreon.com/DriveDeck](https://www.patreon.com/DriveDeck) |
| **AltStore** | [altstore.io](https://altstore.io) · [FAQ](https://faq.altstore.io) |
| **Buy an ESP32** | [classic board](https://a.co/d/0bIfPpNL) · [T-Display-S3 Touch](https://a.co/d/07MaoqIS) |

---

<div align="center">
<sub>DriveDeck is a research project. It uses private system frameworks and cannot be distributed on the App Store.</sub>
</div>
