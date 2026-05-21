# Home Assistant 3D Apartment Dashboard

AI-generated 3D apartment dashboard for Home Assistant with Google Assistant SDK integration, YAML configuration, dimmers, sensors and interactive controls.

![Preview](./3D%20Apartment%20Dashboard.png)

---

# Features

- 🏠 3D Apartment Dashboard
- 🤖 AI-generated top-view rooms
- 💡 Light & Dimmer controls
- 🌡️ Temperature sensors
- ⚡ Power monitoring
- 🔥 Floor heating control
- 🧠 Google Assistant SDK integration
- 🚫 Control devices WITHOUT "OK Google"
- 🎨 YAML customization
- 📱 Fully interactive Home Assistant UI

---

# Files

| File | Description |
|------|-------------|
| `apartment.yaml` | 3D apartment picture-elements dashboard |
| `panel.yaml` | Right-side control panel dashboard |
| `3D Apartment Dashboard.png` | Main apartment image |
| `clipboard_image_*.png` | Additional preview/screenshot |

---

# Requirements

Before using this dashboard you should install/configure:

- Home Assistant
- HACS
- Mushroom Cards
- Google Assistant SDK
- Google OAuth credentials
- Picture Elements Card

---

# Installation

## 1. Upload PNG image

Upload:

```text
3D Apartment Dashboard.png

to:

/config/www/

Then image will be available as:

/local/3D Apartment Dashboard.png
2. Copy YAML

Copy content from:

apartment.yaml

into your dashboard.

3. Change entities

Replace entities with your own:

Example:

light.dimmer_kukhnia
sensor.kompiuter_power
climate.floor_heating
input_number.lamps_eva_brightness

Your entities WILL be different.

Google Assistant SDK

This dashboard also supports controlling Google Home devices directly from Home Assistant WITHOUT saying:

"OK Google"

Used service:

google_assistant_sdk.send_text_command

Example:

turn on Lamps Eva
AI Workflow

Rooms were generated using:

ChatGPT
AI top-view rendering
BTI apartment plan
Manual room photo references
Notes

This dashboard is fully customizable.

You can add:

lights
dimmers
sockets
sensors
cameras
automations
Tesla
energy monitoring
Telegram automations
AI controls
YouTube

Tutorial video:
(Add your YouTube link here)

Author

Anton Babenko

GitHub:
https://github.com/bobantonbob/home-assistant-stack
