# 🚁 UAV-Based Traffic Conflict Analysis  

This repository contains UAV-powered computer vision pipelines to detect **safety-critical driver behaviors** at intersections. The projects combine **YOLO-based detection**, **ByteTrack multi-object tracking**, and **spatiokinematic conflict detection frameworks** to extract high-resolution behavioral data from drone videos.  

Two flagship projects are included:  

- **Hard Braking Detection**: Deriving vehicle speed profiles from drone videos and identifying severe deceleration events.  
- **Near-Miss Detection**: Capturing vehicle interactions in a dynamic spatial grid to flag near-miss events where two vehicles traverse the same cell within a critical time window.  

These pipelines demonstrate how UAVs can complement large-scale connected vehicle (CV) data by validating risks at the site level.  

---

## 📌 Project 1: Hard Braking Detection from Drone Videos  

### 🔹 Overview
This module extracts **vehicle trajectories and speeds** from drone footage and flags **hard braking events** using a multi-phase stability check.  

### 🔹 Steps
1. **Vehicle Detection & Tracking**  
   - Vehicles are detected with **YOLO** and tracked with **ByteTrack**, ensuring stable IDs even under occlusion.  

2. **Homography & View Transformation**  
   - A perspective transformation maps oblique drone footage into a **scaled top-down reference frame**, enabling speed estimation in mph.  

3. **Speed Estimation**  
   - Pixel displacement is converted to meters using scaling ratios.  
   - Speeds are computed as `distance/time`, smoothed with sliding windows.  

4. **Hard Braking Detection**  
   - Sliding 20-frame windows scan each trajectory.  
   - A hard braking event is logged if:  
     - Speed drop > **60–70%** within 20 frames.  
     - Initial speed > 15 mph.  
     - Speed remains low for the next 100 frames (no rebound).  
     - Pre-window speeds are stable.  

5. **Outputs**  
   - Annotated video with vehicle speeds.  
   - Excel file with detailed per-vehicle trajectory data.  
   - Hard braking summary file logging frame ranges, speed drop %, and movement tags.  

### 🔹 Demo
📹 [Watch Example Video](your-link-here)  
📊 [Sample Hard Braking Output](your-link-here)  

---

## 📌 Project 2: Near-Miss Detection from Drone Videos  

### 🔹 Overview
This module detects **spatiotemporal near-miss events** by overlaying a **grid structure** on the intersection and recording when vehicles activate the same cells within a short time threshold.  

### 🔹 Steps
1. **Vehicle Detection & Tracking**  
   - Vehicles detected with **YOLO** and tracked with **ByteTrack**.  

2. **Grid Overlay**  
   - The intersection is partitioned into **uniform cells**.  
   - As vehicles move, their **centers activate grid cells**.  

3. **Cell Activation Logging**  
   - Each activation is stored as `(vehicle_id, cell_id, timestamp)`.  
   - Activation info is exported to Excel for analysis.  

4. **Near-Miss Detection**  
   - If **two vehicles activate the same cell within Δt < 2s**, the interaction is flagged as a **near-miss conflict**.  
   - Each event is logged with vehicle IDs, shared cell, and timestamps.  

5. **Outputs**  
   - Annotated video with activated cells highlighted.  
   - Excel datasets with:  
     - Vehicle trajectories.  
     - Cell activation logs.  
     - Conflict tuples (`vehicleA, vehicleB, cell, t1, t2`).  

### 🔹 Demo
📹 [Grid Activation Demo](your-link-here)  
📊 [Near-Miss Sample Output](your-link-here)  

---

## ✨ Key Contributions
- Developed **UAV-based pipelines** for surrogate safety analysis at intersections.  
- Implemented **hard braking and near-miss detection** using **spatiokinematic conflict detection**.  
- Demonstrated alignment with **connected vehicle data risk indicators**.  
- Exported structured datasets enabling further safety modeling (e.g., PET, TTC, surrogate safety metrics).  

---

## 🛠️ Tech Stack
- **YOLOv8 / YOLO11** – Vehicle detection.  
- **ByteTrack** – Multi-object tracking.  
- **OpenCV** – Perspective transformation & visualization.  
- **Supervision** – Video frame processing, annotations.  
- **Pandas** – Data export (trajectories, conflict logs).  
- **Scipy** – Assignment & optimization tasks.  

---

## 👥 Authors
- **Gideon Asare Owusu** – PhD Researcher, Iowa State University.  
- Collaborators & Advisors: Prof. Anuj Sharma, Neal Hawkins, Skylar Knickerbocker.  

---

## 📄 License
This project is licensed under the [MIT License](LICENSE).  
