# Antipodal Point Marker — Design Spec

## Context

The ISS tracker already displays the ISS position as a red glowing marker on a 3D globe. The user wants to also show the **antipodal point** — the point on Earth's surface diametrically opposite the ISS ground position. This helps visualize the furthest point on Earth from the station.

## What It Does

- Computes the antipodal point from the ISS lat/lon: `antiLat = -lat`, `antiLon = lon > 0 ? lon - 180 : lon + 180`
- Displays a **cyan glowing marker** on the globe surface at the antipodal position
- Labeled "ANTIPODAL" next to the marker
- Smoothly interpolates position (lerp), same as the ISS marker
- Displays antipodal lat/lon in the info panel
- Updates in sync with each ISS position update

## Visual Design

- **Marker:** Cyan (#00ddff) glowing sphere + radial glow sprite, same size as the ISS marker
- **Label:** "ANTIPODAL" text sprite in cyan, positioned next to the dot
- **Info panel:** Two new rows below the existing LAT/LON showing "A-LAT" and "A-LON" for the antipodal coordinates

## Implementation Details

**Files to modify:** `index.html` only

**Reuse existing patterns:**
- `latLonToVec3()` for coordinate conversion (already exists)
- ISS marker pattern: `THREE.Group` with sphere + glow sprite + label sprite (replicate in cyan)
- Lerp pattern from ISS position state (replicate for antipodal)

**Changes needed:**
1. Add antipodal marker group (sphere + glow + label) after the ISS marker section
2. Add antipodal position state variables (`antiCurrentPos`, `antiTargetPos`)
3. In `updateISSPosition()`: compute antipodal lat/lon, call `latLonToVec3` for the surface point (radius = `EARTH_RADIUS`, no altitude offset), update info panel
4. In `animate()`: lerp antipodal position, update group position and orientation
5. Add two rows to the info panel HTML: A-LAT and A-LON

## Non-Goals

- No connecting line between ISS and antipodal point
- No additional API calls
- No new UI panels or overlays
