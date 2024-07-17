
---
### Description

Wingman contains the control code running on the Jetson during Area Survey.

Code is provided both for real time running and for simulation.

Wingman is responsible for:

- Knowing when the drone has entered the Area Survey region
- Capturing images once it has entered Area Survey
- Running the ML models to search for & classify ODLCs
- Converting the ODLC inferences to GPS coordinates
- Telling PX4 where to go for payload drop
- Dropping the payloads

Wingman is essentially riding in the shotgun seat alongside PX4.

The goal is to have one crisp, centralized Python package running everything on the Jetson.

---

### Offboard Control

1. The drone will follow for the mission until the 2nd to last waypoint
2. From the 9th to last waypoint, the camera will start taking pictures and pinpointing ODLCs
3. After the 2nd to last waypoint, the drone switches into hold as it processes the images, and then moves to the cluster with the most number of inferences
4. The camera then identifies the shape of the object, and accordingly drops the correct payload