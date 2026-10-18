# Nikon D750 ColorChecker presets for LightCraft

Two LightCraft presets that correct the colour rendering of Nikon D750 raw files (NEF), tuned against a
ColorChecker-based DNG camera profile in Lightroom. LightCraft has no measured D750 profile yet and
cannot load `.dcp` files, so these presets approximate the profile with LightCraft's own sliders.

Both presets are in one file, `presets/D750.lcpreset`:

| Preset | Use for |
|---|---|
| D750 ColorChecker Outdoor 2026-10-18 | Daylight and flash |
| D750 ColorChecker Indoor 2026-10-18 | Warm indoor light (LED / tungsten) |

For mixed light, pick the preset that matches the dominant light source.

## Before / after

Both LightCraft columns use the same white balance (picked on the grey row) and exposure; the only
difference is the preset.

**Daylight / flash**

| Lightroom + ColorChecker DCP (reference) | LightCraft, no preset | LightCraft + Outdoor preset |
|---|---|---|
| ![](images/daylight_flash-lightroom.jpg) | ![](images/daylight_flash-lightcraft-before.jpg) | ![](images/daylight_flash-lightcraft-after.jpg) |

**Indoor**

| Lightroom + ColorChecker DCP (reference) | LightCraft, no preset | LightCraft + Indoor preset |
|---|---|---|
| ![](images/indoor-lightroom.jpg) | ![](images/indoor-lightcraft-before.jpg) | ![](images/indoor-lightcraft-after.jpg) |

## How to use

1. In LightCraft, import `D750.lcpreset` via the Presets panel (Import). This adds both presets.
2. Select your D750 photos (search `camera:d750`) and apply the matching preset.
3. Then set exposure and white balance per photo as usual. The presets deliberately contain no
   exposure or white balance (neither temperature nor tint). Set white balance on something neutral;
   the presets were tuned on correctly white-balanced chart shots and look wrong on a colour cast.

Applying a preset also sets the profile to LightCraft's **Color** profile (`lc.color`) at 100 %.

Use the presets on **Nikon D750 files only**. They compensate LightCraft's current colour estimate
for this camera; on other cameras they will distort colour.

## What the presets contain

Profile, Saturation/Vibrance, Calibration and Color Mixer. Point Color and B&W mix are stored at zero.

| Setting | Outdoor | Indoor |
|---|---|---|
| Profile | Color, 100 % | Color, 100 % |
| Saturation / Vibrance | +32 / −8 | +32 / −4 |
| Shadows tint | +10 | +6 |
| Red primary hue / saturation | −14 / −18 | −6 / −10 |
| Green primary hue / saturation | +20 / +3 | +20 / −5 |
| Blue primary hue / saturation | +4 / +11 | −20 / 0 |

Color Mixer (hue / saturation / luminance):

| Band | Outdoor | Indoor |
|---|---|---|
| Red | −6 / +1 / +25 | +1 / −3 / +9 |
| Orange | +4 / +2 / +24 | +4 / 0 / +24 |
| Yellow | −5 / +10 / +41 | −5 / +2 / +45 |
| Green | +8 / +8 / +12 | +3 / −6 / +8 |
| Aqua | −8 / +3 / +3 | −12 / +11 / +13 |
| Blue | −20 / 0 / −55 | −19 / −3 / −22 |
| Purple | +8 / −8 / −22 | 0 / −8 / −26 |
| Magenta | +8 / +13 / −11 | 0 / −2 / −15 |

Accuracy against the Lightroom reference, mean ΔE2000 over the 24 patches (lower is better; about 1–2
is barely visible):

| | No preset | Previous version | Current version |
|---|---|---|---|
| Daylight / flash | 5.2 | 2.8 (2026-10-17) | **1.5** |
| Indoor | 4.0 | 3.1 (2026-10-17) | **1.3** |

All values are measured on the 2026-10-18 chart shots against the 2026-10-18 profile, with the same
white balance and exposure in all three columns of a row. On the 18 colour patches alone (without
the grey row) the current presets reach ΔE 1.4 (Outdoor) and 1.0 (Indoor); much of the rest is tone:
LightCraft renders the light greys slightly darker than Lightroom.

## Method

### 1. Chart shots

- Target: X-Rite ColorChecker Passport, classic 24-patch side.
- Camera: Nikon D750, firmware 1.15, ISO 100, raw (NEF), chart lit evenly from the front (light at about 90° to the chart surface).
- Exposure: white patch bright but not clipped.
- Two lights: the built-in flash (stands in for daylight) and warm indoor LED/tungsten light. There
  is no separate validation shot.
- Shot as two 9-frame brackets (−4 to +4 EV) on 2026-10-18. The presets are fitted on the 0 EV
  frames, which are in `raw/`. (A first pair of shots from 2026-10-16 was replaced by these.)

Capture settings, read from the NEFs in `raw/`:

| | Daylight / flash | Indoor |
|---|---|---|
| Lens | Tamron SP 24-70mm f/2.8 Di VC USD (A007N) at 70 mm | same |
| Exposure | M, 1/40 s, f/5, ISO 100 | M, 1/10 s, f/3.2, ISO 100 |
| Flash | Built-in, TTL | off |
| White balance (camera) | Auto1 (R/B multipliers 1.90 / 1.45) | Auto1 (R/B multipliers 1.36 / 2.18) |
| Raw format | 14-bit lossless compressed NEF, 6032 × 4032, black level 600 | same |
| Picture Control / Active D-Lighting | Standard / Auto | Standard / Auto |

Usable bracket frames:

- **Indoor −4 to +2 EV.** In the +3 and +4 EV frames the built-in flash fired, so they were not used.
  At 1/160 s (−4 EV) and 1/80 s (−3 EV) the room light flickers (mains frequency): the camera's own
  white balance shifts and the −4 EV frame came out about 1.5 EV darker than intended. From 1/40 s on
  the light is stable.
- **Flash: only 0 EV.** The flash was in TTL mode, so its output compensated each bracket step; the
  frames differ in the mix of flash and room light, not in exposure.

Picture Control, Active D-Lighting and the camera's white balance only affect the embedded JPEG
preview and Nikon's own software, not the raw data. They do not influence the presets.

### 2. Reference profile (Lightroom)

1. Exported a flash and an indoor NEF as DNG copies (Lightroom: File ▸ Export, format DNG).
2. Calibrite PROFILER ▸ camera calibration ▸ *Dual Illuminant DNG*: loaded both DNGs, checked the
   patch grid, created the profile.
3. Lightroom Classic 15.6: applied the profile, white balance from the second-lightest grey patch,
   all other settings at zero.

The current profile is `20261018_NikonD750_DualIllumination_ColorCheckerPassport.dcp`, built from
the two 0 EV frames. PROFILER labels each of its two calibration points with the nearest standard
illuminant from the DNG specification; nothing was set by hand. The indoor light measured about
3200 K (labelled *ISO Studio Tungsten*), the flash frame about 4900 K (labelled *D55*; warm room
light mixed into the flash). The label only tells Lightroom which colour temperature each matrix
belongs to.

The first profile, `20261016_…`, was built from the 2026-10-16 shots under cooler indoor light
(about 4000 K, labelled *Cool White Fluorescent*). It is kept for reference only.

### 3. Matching in LightCraft

LightCraft 0.9.1. The first version was matched by eye. The current version was fitted numerically
with `lightcraft-cli render`:

1. Render the chart NEF with candidate settings, sample the centre of all 24 patches, and compare them
   in CIELAB with the same patches in the Lightroom reference render (ΔE2000).
2. Adjust one slider at a time (Saturation, Vibrance, the seven Calibration sliders and the 24 Color
   Mixer sliders), keeping any change that lowers the mean ΔE, with step sizes shrinking from 16 to 2.
3. Exposure and white balance were fitted too, so they don't distort the comparison, but they are not
   stored in the presets.
4. A small penalty on large slider values keeps the corrections moderate, so they stay usable on real
   photos rather than only on the chart.

Exposure and white balance used for the chart renders:

| | Daylight / flash | Indoor |
|---|---|---|
| White balance | 7940 K, tint +3 | 8080 K, tint −13 |
| Exposure | −0.34 | −0.46 |

These are LightCraft's slider values, not the colour temperature of the light. Set white balance
on the grey row in your own photos rather than copying these numbers.

Seven Calibration sliders and an eight-band mixer cannot reproduce the full hue/saturation table of a
DCP, so small differences remain.

### 4. Bracket check (Indoor)

How well the Indoor preset holds on under- and overexposed shots. Each bracket frame was rendered in
LightCraft with the preset, with exposure and white balance set per frame, and compared with
Lightroom's render of the same frame (exposure compensated in Lightroom, same profile). Mean ΔE2000:

| Frame | Shutter | LightCraft + Indoor preset | LightCraft, no preset |
|---|---|---|---|
| −4 EV | 1/160 s | 10.1 | 11.4 |
| −3 EV | 1/80 s | 4.9 | 6.4 |
| −2 EV | 1/40 s | 2.7 | 3.3 |
| −1 EV | 1/20 s | 2.1 | 3.8 |
| 0 EV | 1/10 s | **1.3** | 3.9 |
| +1 EV | 1/5 s | 2.3 | 4.5 |
| +2 EV | 0.4 s | 4.6 | 6.6 |

The preset improves every frame. Between −2 and +1 EV it stays within about ΔE 3. Further out, the
difference is mostly tone rather than colour, with or without the preset: LightCraft 0.9.1 renders
the black patch at L\* 17.5 (−4 EV) and 13.9 (+2 EV) where Lightroom stays at about 7, and the light
greys come out darker. The −4 and −3 EV frames are also affected by the flicker described above.

## Files

| Path | Content |
|---|---|
| `presets/D750.lcpreset` | Both LightCraft presets |
| `profiles/20261018_NikonD750_DualIllumination_ColorCheckerPassport.dcp` | Current dual-illuminant DNG camera profile (colour matrices for about 3200 K and 5500 K), for Lightroom / Camera Raw / RawTherapee, and for LightCraft once `.dcp` import lands |
| `profiles/20261016_NikonD750_DualIllumination_ColorCheckerPassport.dcp` | First profile from the 2026-10-16 shots, superseded |
| `raw/` | The two 0 EV chart NEFs (flash, indoor) |
| `images/` | Chart renders shown above |
| [Release `bracket-2026-10-18`](https://github.com/dieli/d750-lightcraft-colorchecker/releases/tag/bracket-2026-10-18) | Usable bracket NEFs (indoor −4 to +2 EV, flash 0 EV), their Lightroom renders and the DCP, as one zip (210 MB) |

## Limitations

- Tuned for one camera body; other D750 bodies should be close but are untested.
- Tuned with LightCraft 0.9.1. LightCraft's rendering is still changing, so a later version may
  need slightly different values.
- Fitted to one chart shot per light source, against Lightroom's rendering of the DCP rather than
  measured patch values.
- Tuned on a flash-lit chart, not real daylight. Flash is close to daylight, but untested in sun or
  shade. A daylight bracket is planned (probably in a week or so); the Outdoor preset will then be
  re-fitted.
- Strong luminance shifts: Yellow +41 / +45 (Outdoor / Indoor), Orange +24 in both, and Blue −55 in
  Outdoor. Skin falls in Orange/Red, so check faces; blue skies may come out darker than expected.
  Reducing Blue luminance to −25 costs about 0.4 ΔE on the chart.

## Licence

Everything in this repository (presets, profile, raw files and images) is released under
[CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/). Use it however you like, including in
LightCraft itself.
