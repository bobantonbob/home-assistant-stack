# Home Assistant 3D Apartment & Solar Roof Dashboard

AI-generated 3D dashboards for Home Assistant: a top-view apartment control dashboard and a solar roof energy dashboard with animated SVG power-flow overlays, YAML configuration, dimmers, sensors, cameras, smart switches and interactive controls.

![Apartment Preview](./Apartment_Dashboard.png)

![Solar Roof Preview](./roof_Dashboard.png)

---

# Features

- 🏠 3D Apartment Dashboard
- ☀️ 3D Solar Roof Dashboard
- ⚡ Animated energy-flow visualization with SVG overlays
- 🔌 Separate flows for PV strings and microinverters
- 🔁 Conditional microinverter flow display based on switch state
- 🤖 AI-generated top-view rooms and roof layout
- 💡 Light & dimmer controls
- 🌡️ Temperature and floor heating controls
- 🔋 Battery, load and inverter monitoring
- 📈 Solcast PV forecast indicators
- 📷 Camera preview support
- 🧠 Google Assistant SDK integration
- 🚫 Control devices WITHOUT saying “OK Google”
- 🎨 YAML customization
- 📱 Interactive Home Assistant UI

---

# Files

| File | Description |
|------|-------------|
| `apartment.yaml` | Main 3D apartment `picture-elements` dashboard |
| `panel.yaml` | Right-side control panel dashboard |
| `3D_roof.yaml` | Solar roof dashboard with PV, inverter, battery and forecast indicators |
| `roof.png` | Main solar roof image for Home Assistant `/config/www/` |
| `animated_roof_strings_overlay.svg` | Animated SVG overlay for central PV strings |
| `animated_roof_microinverters_overlay_v2.svg` | Animated SVG overlay for microinverter panels |
| `Apartment_Dashboard.png` | Main apartment preview image |
| `HA_home11.png` | Apartment image for `/config/www/` |
| `clipboard_image_*.png` | Additional preview/screenshot |
| `README.md` | Project documentation |

---

# Requirements

Before using these dashboards, install/configure:

- Home Assistant
- HACS
- Mushroom Cards
- Button Card
- Card Mod
- Picture Elements Card
- Google Assistant SDK
- Google OAuth credentials
- Solcast PV Forecast integration, optional for roof forecast cards
- Camera/NVR integration, optional for camera preview cards

---

# Installation

## 1. Upload apartment image

Upload the apartment image to:

```text
/config/www/
```

Example image file:

```text
HA_home11.png
```

It will be available in Home Assistant as:

```text
/local/HA_home11.png
```

If your YAML uses another image name, update the path inside the dashboard YAML.

---

## 2. Add apartment dashboard

Copy YAML from:

```text
apartment.yaml
```

This is the main 3D apartment `picture-elements` card with lights, sensors, power indicators and interactive controls.

---

## 3. Add control panel

Copy YAML from:

```text
panel.yaml
```

This is the right-side control panel with:

- dimmers
- sliders
- switches
- temperatures
- power monitoring
- controls

---

## 4. Upload solar roof files

Upload these files to:

```text
/config/www/
```

Required roof files:

```text
roof.png
animated_roof_strings_overlay.svg
animated_roof_microinverters_overlay_v2.svg
```

They will be available in Home Assistant as:

```text
/local/roof.png
/local/animated_roof_strings_overlay.svg
/local/animated_roof_microinverters_overlay_v2.svg
```

---

## 5. Add solar roof dashboard

Copy YAML from:

```text
3D_roof.yaml
```

This dashboard shows:

- PV string production
- microinverter production
- Deye inverter load
- battery state
- Solcast forecast
- camera preview
- animated energy-flow overlays

---

# Animated Energy Flow Overlays

The roof dashboard uses SVG overlays on top of the static roof image.

## PV strings overlay

```text
animated_roof_strings_overlay.svg
```

This overlay represents the central PV strings, mainly the two central 5-panel rows.

## Microinverter overlay

```text
animated_roof_microinverters_overlay_v2.svg
```

This overlay represents the other PV panels connected through microinverters.

The microinverter overlay can be shown only when the microinverter switch is ON.

Example entity:

```yaml
switch.sonoff4_ha_sonoff_1000066f8f_1
```

Example conditional overlay:

```yaml
- type: conditional
  conditions:
    - entity: switch.sonoff4_ha_sonoff_1000066f8f_1
      state: "on"
  elements:
    - type: image
      image: /local/animated_roof_microinverters_overlay_v2.svg
      style:
        left: 50%
        top: 50%
        width: 100%
        transform: translate(-50%, -50%)
        pointer-events: none
```

The strings overlay can stay visible permanently or can be controlled by PV production sensors.

---

# Example Roof Entities

Replace the entities with your own if needed.

```yaml
sensor.deye_pv1_power
sensor.deye_pv2_power
sensor.inverter_pv1_power
sensor.inverter_pv2_power
sensor.inverter_2_pv1_power
sensor.inverter_2_pv2_power
sensor.beny_pv_power
sensor.deye_load_power
sensor.deye_battery
sensor.deye_today_production
sensor.deye_microinverter_energy_today
sensor.today_sun
sensor.solcast_pv_forecast_forecast_today
switch.sonoff4_ha_sonoff_1000066f8f_1
camera.network_video_recorder_channel_1
camera.network_video_recorder_channel_2
```

---

# Example Apartment Entities

Replace the entities with your own.

```yaml
light.dimmer_kukhnia
sensor.kompiuter_power
climate.floor_heating
input_number.lamps_eva_brightness
sensor.kholodilnik_power
sensor.smart_plug_power
sensor.boiler_power
switch.tesla_switch
```

---

# Notes

- Put all images and SVG overlays inside `/config/www/`.
- In Home Assistant, `/config/www/file.png` becomes `/local/file.png`.
- Keep SVG overlays the same aspect ratio as the base roof image.
- Use `pointer-events: none` on SVG overlays so they do not block clicks on dashboard buttons.
- For best results, keep the roof dashboard as a square `picture-elements` card.

---

# Author

Anton Babenko

GitHub:  
https://github.com/bobantonbob/home-assistant-stack
