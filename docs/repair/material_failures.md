# 🔧 Material Testing & Workshop Diagnosis Guide (Rev 4)

*Note: The SVA-Fencing-Tester is strictly a specialized workshop diagnostic tool for material inspection, maintenance, and repair. It is **not** a piste scoring box (Melder) and is not intended for tournament signaling.*

When testing equipment, the tester's high-speed sampling loop and real-time data stream will help you isolate typical mechanical and electrical material failures before the gear enters the strip.

---

## 1. Dirty or Oxidized Weapon Tips (Foil / Epee)

### 📊 Detection via Tester
The real-time status output or parameter log shows erratic resistance spikes or inconsistent state changes when the point is depressed slowly.

### 🔍 Root Cause
Sweat, dirt, and insulating oxide layers accumulate inside the barrel. This prevents clean electrical contact, confusing standard scoring boxes later on.

### 🛠️ Workshop Action
1. Disassemble the weapon tip (remove the tiny tip screws safely).
2. Clean the internal contact spring and the cavity using a cotton swab dipped in Isopropyl Alcohol (IPA).
3. Check for structural deformations of the barrel. Reassemble and run a dynamic compression test using the tester's live view.

---

## 2. Mechanical Contact Bouncing (Prellen)

### 📊 Detection via Tester
A single mechanical compression registers as multiple, rapid micro-breaks or multiple rapid hits within a few milliseconds in the data stream.

### 🔍 Root Cause
"Contact Bouncing" (Prellen) happens when metal strikes metal. While the SVA firmware handles software-level debouncing, severe mechanical bouncing indicates a degraded or structurally fatigued spring that vibrates excessively upon impact.

### 🛠️ Workshop Action
1. Test the weight spring tension using the official FIE specification gauges (500g for Foil / 750g for Epee).
2. If the tester registers prolonged bouncing beyond acceptable microsecond margins, stretch the return spring slightly or replace it with a factory-new spring.

---

## 3. Sliding Contact Noise ("Bürstenfeuer") in Cord Reels

### 📊 Detection via Tester
The tester logs rapid CRC errors, flickering connection IDs, or massive electrical noise spikes specifically *while* the cable is being pulled out or reeled back into the drum.

### 🔍 Root Cause
The sliding carbon brushes or copper tracks inside the floor reel (Rolle) are worn, dusty, or oxidized. Movement triggers micro-arcs and transient breaks ("Bürstenfeuer"), causing heavy signal degradation.

### 🛠️ Workshop Action
1. Mount the reel to the workshop bench and connect it to the tester without a weapon attached.
2. Slowly pull the cable to its full length while observing the binary data stream.
3. If noise occurs, open the reel housing, remove dust/grease build-up from the copper tracks, and clean the brushes. Apply a microscopic layer of specialized conductive contact grease if necessary.

---

## 4. Body Cord & Mask Cord Micro-Breaks

### 📊 Detection via Tester
The tester reports a stable connection, but sudden breaks occur when the cable is flexed, twisted, or shaken near the plugs.

### 🔍 Root Cause
Internal copper strand fractures inside the insulation, usually right at the strain relief of the three-pin or two-pin plug.

### 🛠️ Workshop Action
1. Connect the cable to the tester and activate the hardware log / touch-debug routine if available.
2. Systematically flex the cable centimeter by centimeter, especially near the connectors.
3. Once the tester signals a break, cut the cable 5 cm below that point, strip the wires, and re-terminate the plug terminals.
