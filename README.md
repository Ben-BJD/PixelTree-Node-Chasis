# PixelTree Node v1.0 — Enclosure Chassis & Drill Templates

Mechanical CAD files, 3D-printable sled STLs, PCB STEP reference, and 1:1 printable drill stencils for armoring the **[PixelTree Node v1.0](https://github.com/Ben-BJD/Pixel-Tree-Controller)** (an 800-LED, 8-channel ESP32-S3 WLED controller) inside a waterproof **BUD Industries `PTR-28483`** NEMA enclosure.

[![Project Available on PCBWay](pcb-project-image.png)](https://www.pcbway.com/project/shareproject/High_Current_ESP32_WLED_Controller_7ebfa690.html)

---

## Repository Structure

```text
├── PixelTree-Node-Chasis.FCStd                  # Native FreeCAD parametric chassis source
├── drill-guides/
│   ├── drill-guides.pdf                         # 1:1 scrap-plastic test sheet (6, 8, 14, 16, 18mm)
│   ├── drill-guides.svg                         # Inkscape source for scrap test sheet
│   ├── light-cable-glands-drill-guide-print.pdf # 1:1 back-wall 2+6 staggered PG9 lighting stencil
│   ├── light-cable-glands-drill-guide-print.svg # Inkscape source for back-wall stencil
│   ├── power-wifi-glands-drill-guide.pdf        # 1:1 side-wall power & RP-SMA antenna stencil
│   └── power-wifi-glands-drill-guide.svg        # Inkscape source for side-wall stencil
├── step/
│   └── Pixel-Tree-Controller.step               # 3D STEP model of PixelTree Node v1.0 PCB
└── stl/
    └── PixelTree-Node-Chasis.stl                # Exported chassis mesh ready for slicing
```

---

## Hardware Overview & Key Dimensions

- **Target Enclosure:** BUD Industries `PTR-28483` (`8.60" × 6.30"` / `218.4mm × 160.0mm`, 2.0mm ABS wall, IP66/IP67).
- **Chassis Footprint:** `169mm × 119mm × 2.40mm` baseplate with four `30mm` pillars.
  - Why `30mm`? Provides an "under-deck" routing bay so stiff 18 AWG xConnect pigtails can swoop underneath the PCB into the central `KF2EDGR` blocks while aligning the ESP32-S3's IPEX port horizontally with the side-mounted Wi-Fi antenna bulkhead.
- **Fasteners:**
  - **Chassis → Enclosure Corner Bosses:** 4× **M2.6 × 10mm** plastic self-tapping screws (included with the BUD `PTR-28483`).
  - **PCB (`H1`–`H4`) → Chassis Pillars (`2.98mm` holes):** 4× **M3 × 10mm** machine screws.

---

## 3D Printing Recommendations

- **Recommended Filament:** **PETG** (resists softening inside a sealed sun-exposed box and takes M2.6/M3 thread bite without splitting). *Avoid PLA for outdoor use.*

---

## Drilling & Cable Gland Guide

> **Print Scaling Warning:** Print all `.pdf` files onto adhesive **sticker paper** at **100% scale ("Actual Size")**. Disable any "Fit to Page" or "Shrink to Printable Area" options in your PDF viewer, and double-check one circle diameter with a ruler or calipers before drilling.

### 1. Recommended Drilling Workflow
1. Stick `drill-guides/drill-guides.pdf` onto **scrap plastic** and verify exact thread clearance with your physical glands before touching the ABS enclosure.
2. Apply `light-cable-glands-drill-guide-print.pdf` to the **long hinged back wall** (keeps the front lid latches clear and matches the upper/lower PCB terminal banks via a `2+6` staggered grid).
3. Apply `power-wifi-glands-drill-guide.pdf` to the **side wall**.
4. Center-tap every crosshair, drill small pilot holes, then step up using a **2mm-increment titanium step-down drill bit** with electrical tape wrapped as a physical depth stop:
   - **8× PG9 Lighting Glands (`Altech 5308 509/SET`):** Stop at **16mm**
   - **1× External Antenna Bulkhead (`GCT CABLE-161-RF-100-A-1`):** Stop at **6mm**
   - **1× 12V DC Power Gland (`HLG-320H-12` ~8–9mm tail):** Stop at **20mm**
5. Deburr inner and outer edges with a round needle file and peel off the sticker stencils.

### 2. Power Gland Sizing Note
If sourcing from DigiKey or local suppliers, **do not** use the reduced-range PG11 (`Altech 5309 211/SET`, 2–7mm — too small) or PG13.5 (`Altech 5308 513/SET`, 5–12mm — overly loose and lacks a flat panel washer) listed in early BOM revisions:
- **Australia:** Use **Jaycar `HP0727`** (5–10mm IP68 gland, includes flat rubber washer) + neutral-cure silicone + self-fusing silicone tape on the jacket under the compression clamp.
- **DigiKey Single-Cart Equivalent:** Use standard-range PG11 (**Altech `5308 511/SET`**, 3.5–10mm clamping range) paired with a flat washer or neutral-cure silicone.

---

## Critical Assembly Notes & Gotchas

- **ESP32-S3 `-1U` Variant Required:** Make sure you buy the **`ESP32-S3-DevKitC-1U-N8R8`** (the `-1U` has the U.FL/IPEX socket for an external antenna). Earlier BOM drafts listed the `-1-N8R8`, which only has an onboard PCB trace antenna.
- **Avoid the SMA vs. RP-SMA Trap:** Pair the **RP-SMA Female (Jack)** bulkhead (`GCT CABLE-161-RF-100-A-1`, external threads + **center pin**) with an **RP-SMA Male (Plug)** antenna (`Molex 2144150011`, internal threads + **center hole**). Standard SMA pigtails screw on normally but leave two center holes facing each other with zero electrical contact. Secure the IPEX connector with a small bead of hot glue.
- **Prevent Solder Creep on 12V Input:** Snip the factory pre-tinned tips off the Mean Well `HLG-320H-12` DC output tail back to bare stranded copper before clamping into **WAGO 221** lever nuts.
- **Two-Stage PCB Mounting:** Route the **central 4** female 18 AWG xConnect pigtails under/around the `30mm` pillars and plug them into the PCB *before* bolting the PCB down with M3 screws, then install the outer 4 pigtails afterward.
- **Pigtail Pinout & Fuses:** Standardize all 8 outputs on **Female** xConnect pigtails (`Brown = +12V`, `Yellow/Green = Data`, `Blue = GND`) crimped with `0.75mm²` white-collar bootlace ferrules, protected by **5A mini blade fuses** in `FH1`–`FH8`.
- **WLED Settings:** Set Ray Wu 12V WS2811 pixel color order to **RGB (`order: 1`)** across GPIOs 4–11 (`100` pixels per pin, `16×50` serpentine 2D matrix) with per-output power limiting set to `2750 mA` (`22A` total max).