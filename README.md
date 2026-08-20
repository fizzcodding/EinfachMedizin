# EinfachMedizin

Autonomous medicine dispensing module. This repo covers the mechanical build only: CAD files, design decisions, and assembly. No firmware, no software.

The machine holds up to 21 doses on a rotating carousel disc, indexes to the correct slot using a stepper motor and absolute encoder, and releases the dose through a servo-actuated gate into a collection drawer at the bottom. Dispense only happens after 3-way verification clears — that system isn't documented here.

The problem it's built to solve is medication adherence. Specifically, patients who need the right pill at the right time and can't always rely on a caregiver being physically present to hand it to them.
---
## BOM
This is the Bill of Materials (BOM) for the project.
#### ngl the prices in amazon is high, these are so cheap in my countries D:
- stepper motor - NEMA17HS3401 - 1 piece = 42 USD - https://www.amazon.com/17HS3401-4-Lead-Stepper-Motor-42BYGH/dp/B0DKBV65SR?crid=3VSPGB6ZKSSDH&dib=eyJ2IjoiMSJ9.l8wL1VlsRmTlAxQUhtfiswNTrtBRZ8-cHb3huTAs-ZEiY_6EuQJTNKz2wd4-EVM-MwnJRA3vM_Ius-B-voanOzhY1G5SIk90WqfBnLOX7s1l6jQAjS5VjykAolrcXek4L4GiATX_3otySFJNcPy1BlmM37EL6b_UNAkfZHLSdWR5T2ezRc5xV_0yzcBKsWJ01dOR3rvntZt0XxTxvpNgI4_3zzlR-SnI8WVFC1Hyzbc.uevI8DroZoR_DQIuMIa8mJh7il_nZXWG35Rn_oZNmRI&dib_tag=se&keywords=nema%2B17hs3401&qid=1787200841&sprefix=NEMA17HS3401%2Caps%2C396&sr=8-4&th=1 
- Servo motor - TowerPro MG996R 180° - 2 piece = 9.99 USD - https://www.amazon.com/AEDIKO-MG996R-Control-Digital-Helicopter/dp/B09BZ5955Z?crid=JJZ2OMQ88L9K&dib=eyJ2IjoiMSJ9.e8wsdUgAfahI3u10d93eBZhzZ0jNALZQKN1pfPkJZM7wCza7kheOgAwxo3ocWs5OHg6W_vI-BKdmFrD3novHo8MzPmpfqUz6ryhqMjMxkPQ.7WouFpAa-v1QPw3ukw95AM4rIHctVbUiZY2NrSY8hGE&dib_tag=se&keywords=TowerPro+MG996R&qid=1787200788&sprefix=towerpro+mg996r+%2Caps%2C405&sr=8-2
- stepper driver - TMC2209 - 1 piece = 11 USD - https://www.amazon.com/TMC2209-Stepper-Continuous-Microstepping-Heatsink/dp/B0H6XVZR7Q?crid=2MPG4NXEHX15I&dib=eyJ2IjoiMSJ9.ELbHVQn-o8i1_o640QUgxa6ShPMXq5g0gXmC39FpOkovTXJDG-zmJT-g95VwiMiQcA16jH85fnq321t3x5cosJW8yvXxwvSfAs1sv9_plbodJsBT9q7n8DYIe-_kJTcSHq9Z7_sL4Vo5hEyUEI_2B9qB_BECXCDBkqYaDw9_PtjZg8eJ-ITvpiNixGjSUx00ftp32mqP5H8Z2e-QLNvPwol0NZLGOzBR1IT1G-FPrZc.oZYHkPXDltj3wOQ1td6gsKQGHSf_Vd-VPiA4fkhBt8s&dib_tag=se&keywords=tmc2209+stepper+driver&qid=1787200748&sprefix=TMC2209+%2Caps%2C401&sr=8-19
- magnetic encoder - AS5600 - 1 piece = 6 USD - www.amazon.com/Magnetic-Encoder-Induction-Measurement-Precision/dp/B0GWWND79S?crid=10GXJ9I8UYX2V&dib=eyJ2IjoiMSJ9.sRkyQzCSLKrouEOa6e23GlldUhq7O8OW-pzImgFcGQcYi-3cP-rZLooNAZZhX93R-zH7nipYZ57iWoBm4-vS5dq8ARd9JFuVg5kdCizsGIStXFZo94Jmp27rA94WvTwAFOi4UVA2d6OzW-4mXF_CawcUCN73aMngRcCiI_M0cQYFNmXOzkwnRgHqHmfH2KCXZHmdaGWhuDXCnjbIurk7eHiSY9ximhOgFq8R-EimiBI.oWKwHE8aNsdnJryt8nzT2Q_5y3R_CQ3v39gIENoHxD0&dib_tag=se&keywords=as5600+magnetic+encoder&qid=1787200709&sprefix=as5600+magnetic+enco%2Caps%2C358&sr=8-14
- 3d printing - 30 USD??? Not sure how much this is, but it's definitely not free.

---

## Stack

Four layers, top to bottom:

```
[ Carousel      ]  ← holds the pills
[ Gate          ]  ← controls release
[ Funnel        ]  ← guides pill downward
[ Drawer        ]  ← collection point
```

All four layers are friction-fit. No fasteners, no adhesive between them. Everything is PLA.

CAD is in Onshape.

---

## Carousel

6.500" OD disc with 21 compartment slots arranged around the circumference. Slots are evenly spaced — 360° / 21 ≈ 17.14° per step. No dedicated index/reference slot; absolute position comes from the encoder, not a mechanical marker.

The slot walls use a full-round fillet all the way around the opening. The reason is simple: any sharp corner or chamfer is a snag point for capsules. A full round lets the pill slide out clean when the gate opens. It costs nothing in print time and eliminates a whole failure mode.

The carousel sits on a central shaft driven directly by a NEMA17 stepper. No belt, no gearing. Direct drive keeps the mechanism tight, removes backlash, and means the encoder reading maps cleanly to actual disc position without having to compensate for drivetrain slop.

Two drive-system features shown in the reference render are not yet implemented: outer rim gear teeth around the disc circumference, and a horizontal drive shaft arrangement to replace the current center-shaft drive. Both are planned for an upcoming revision.

---

## Gate Mechanism

Pinion-rack, servo-actuated. When a dispense is triggered, the servo drives a rack that slides the gate open *horizontally* — a linear push, not a flap or trap-door. The slot opening is exposed, the dose drops through, gate closes.

The linear slide was chosen over a rotary flap because it's more predictable: defined open/closed positions, no gravity fighting the mechanism at odd angles, and the travel distance is easy to constrain mechanically.

---

## Funnel

Tapered channel that sits below the gate and guides the released pill down to the drawer.

---

## Drawer

Collection tray at the bottom. Friction-fits onto the funnel. Patient pulls it out to get the dose.

---

## Position Tracking

Position is tracked with an AS5600 magnetic absolute encoder. The diametrically magnetized magnet mounts on a shaft extending from the underside of the carousel disc.

The reason for using an absolute encoder over incremental (hall-effect pulse counting, basic optical encoder, etc.): incremental encoders lose position on power loss. For a dispenser, getting the wrong slot after a power cycle isn't just inconvenient — it's a real failure mode. The AS5600 gives absolute position on every power-up with no homing routine needed, and the cost delta over an incremental solution is negligible.

---

## Assembly

Stack in order: carousel on top, then gate layer, funnel, drawer at the bottom. Push each layer together — friction-fit holds them.

The NEMA17 mounts below the carousel layer and drives the central shaft from underneath. The AS5600 encoder sits on a shaft extending from the underside of the carousel disc.

# Medicine Dispenser Wiring

## ESP32 → TMC2209

| ESP32 | TMC2209 |
|---|---|
| STEP GPIO | STEP |
| DIR GPIO | DIR |
| ENABLE GPIO | EN/ENN |
| UART TX/RX | PDN_UART |
| 3.3V | VIO |
| GND | GND |

## TMC2209 → NEMA17 17HS3401

| TMC2209 | NEMA17 |
|---|---|
| OA1 | Coil A |
| OA2 | Coil A |
| OB1 | Coil B |
| OB2 | Coil B |

## ESP32 → AS5600

| ESP32 | AS5600 |
|---|---|
| 3.3V | VCC |
| GND | GND |
| SDA GPIO | SDA |
| SCL GPIO | SCL |

## ESP32 → MG995 #1 — Shutter

| Connection | MG995 #1 |
|---|---|
| Servo GPIO | SIGNAL |
| Servo Power | V+ |
| Common GND | GND |

## ESP32 → MG995 #2 — Slider Crank

| Connection | MG995 #2 |
|---|---|
| Servo GPIO | SIGNAL |
| Servo Power | V+ |
| Common GND | GND |

## Power

| Source | Destination |
|---|---|
| USB-C Power Bank | Main Power Input |
| Main Power | ESP32 |
| Main Power | TMC2209 Motor Power |
| Main Power | Servo Power Rail |
| Servo Power Rail | MG995 #1 V+ |
| Servo Power Rail | MG995 #2 V+ |
| Common GND | ESP32 GND |
| Common GND | TMC2209 GND |
| Common GND | AS5600 GND |
| Common GND | MG995 #1 GND |
| Common GND | MG995 #2 GND |

## AS5600 Physical Placement

| Component | Position |
|---|---|
| Carousel Magnet | Directly above AS5600 |
| AS5600 | Centered underneath magnet |
| AS5600 PCB | Fixed beneath carousel |