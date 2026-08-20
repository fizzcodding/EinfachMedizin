# EinfachMedizin

Autonomous medicine dispensing module. This repo covers the mechanical build only: CAD files, design decisions, and assembly. No firmware, no software.

The machine holds up to 21 doses on a rotating carousel disc, indexes to the correct slot using a stepper motor and absolute encoder, and releases the dose through a servo-actuated gate into a collection drawer at the bottom. Dispense only happens after 3-way verification clears — that system isn't documented here.

The problem it's built to solve is medication adherence. Specifically, patients who need the right pill at the right time and can't always rely on a caregiver being physically present to hand it to them.
---
## BOM
This is the Bill of Materials (BOM) for the project.

- stepper motor - NEMA17HS3401 - 1 piece = 9.75 USD 
- Servo motor - TowerPro MG996R 180° - 2 piece = 3.65 USD*2 = 7.30 USD
- stepper driver - TMC2209 - 1 piece = 6.7 USD
- magnetic encoder - AS5600 - 1 piece = 2.5 USD
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
