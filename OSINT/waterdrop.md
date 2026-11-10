## 💧 (Water Drop)

* **Category:** OSINT
* **Points:** 75
* **URL:** [https://osint.pink.bayern/1](https://osint.pink.bayern/1)

### Challenge Description

A web-based GeoGuessr-style challenge presenting a 360° interactive panorama (`pano.jpg`) overlooking a shoreline. The goal is to submit the exact GPS coordinates where the panorama was shot.
<img width="8192" height="4096" alt="waterdrop_pano" src="https://github.com/user-attachments/assets/cc61626e-ebd7-4e18-a81b-e10873c3829e" />


### Investigation & Geolocation


1. **Visual Landmark Identification:**
   * Looking west across the water in `pano.jpg`, two distinctive 55-meter tall church towers with baroque onion domes are visible on the horizon.
   * Cross-referencing architectural styles around German and Swiss alpine lakes identified these as the **Schlosskirche Friedrichshafen** (Castle Church) located on Lake Constance (*Bodensee*), Baden-Württemberg, Germany.

2. **Foreground & Perspective Matching:**
   * The camera was placed directly on a pebble beach with a curved concrete promenade retaining wall and stairs immediately behind/to the right.
   * East of the viewpoint sits the harbor breakwater of the **Württembergischer Yacht-Club (WYC)** near the **Graf-Zeppelin-Haus (GZH)**.
   * Matching the line of sight to the church towers pinned the position directly onto the Uferpromenade pebble beach in Friedrichshafen.

3. **Submitting Coordinates:**
   * The frontend uses Leaflet/Pannellum and checks guesses via `POST /?check=1`. The server enforces a strict tolerance threshold (≈ 15–25 m).
   * We submitted the exact beach coordinates through the browser console:

```javascript
// Exact camera placement on the beach
selectedLat = 47.6509165;
selectedLon = 9.4757084;
submitButton.disabled = false;
submitButton.click();
```

The banner turned green, confirming the solve!

### Solved Location

* **Coordinates:** `47.6509165, 9.4757084`
  <img width="932" height="588" alt="Screenshot 2026-11-10 at 21 02 00" src="https://github.com/user-attachments/assets/b4c0fabc-d881-4a3e-9352-ff3d9974d53a" />

* **Location:** Uferpromenade Pebble Beach, Friedrichshafen (*Bodensee*), Germany

---
