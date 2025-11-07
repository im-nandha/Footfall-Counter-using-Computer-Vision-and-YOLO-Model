# Real-Time Footfall Counter using YOLOv8

## Project Overview
This project implements a **real-time footfall counter** using **YOLOv8** for person detection and a **Centroid Tracking** algorithm for tracking individuals across video frames.  
It counts how many people **enter (IN)** or **exit (OUT)** a defined area by monitoring when detected individuals cross a **Region of Interest (ROI)** line.  

This solution is lightweight, modular, and can be used on both **recorded CCTV footage** or **live webcam feeds**, making it ideal for use in **shopping malls, offices, or event venues**.

---

## Approach

### Model Used
- **YOLOv8n (Ultralytics)** — a pre-trained object detection model trained on the COCO dataset.
- Detects multiple classes, but we only use the **"person"** class for this project.

### Pipeline Steps
1. **Video Input:** Each frame from the webcam or a video file is captured.
2. **Detection:** YOLOv8 detects all persons (`class_id = 0`) in each frame.
3. **Tracking:** A **Centroid Tracker** assigns a unique ID to every detected person and tracks them across frames.
4. **Counting Logic:** A **horizontal ROI line** is defined in the frame (e.g., middle of the screen).  
   - When a person’s centroid moves from **above → below**, it’s counted as **IN**.  
   - When it moves from **below → above**, it’s counted as **OUT**.
5. **Visualization:** Bounding boxes, IDs, and counters are overlaid on the video in real time.
6. **Output:** A processed `.mp4` video is saved (`final_footfall_output.mp4`) showing live detections and a **final summary screen** with total counts.

---

## Video Source

- **Source Type:** Real-time webcam or recorded CCTV footage.  
- **Example Used:**  
  - Custom surveillance-style video (you can replace it with any dataset or YouTube video).  
  - Example dataset suggestion: [MALL Dataset – CUHK](https://www.ee.cuhk.edu.hk/~xgwang/Mall.html)  
  - Example YouTube video (for educational use):  
    “Shopping Mall Entrance Footage” — https://www.youtube.com/watch?v=aqz-KE-bpKQ

To test with your own video, replace:
```python
video_path = "your_video.mp4"
```
with your file path.

---

## Counting Logic Explanation

The counting system uses **centroid-based tracking**.  
Each detected person’s bounding box center point (centroid) is tracked frame-by-frame.

```text
if previous_y < ROI_y and current_y >= ROI_y → IN (Entered)
if previous_y > ROI_y and current_y <= ROI_y → OUT (Exited)
```

The logic ensures that:
- Each person is only counted once per crossing.
- Both upward and downward movements are correctly categorized.

The ROI line can be adjusted depending on the camera angle:
```python
roi_y = int(frame_height * 0.5)   # Middle of the frame
```

---

## Dependencies and Setup Instructions

### Requirements
- Python ≥ 3.8  
- OpenCV  
- Ultralytics (for YOLOv8)  
- NumPy  
- SciPy  

### Installation

Run the following commands to install dependencies:
```bash
pip install ultralytics opencv-python numpy scipy
```

### To Run the Project
1. Clone or download this repository.  
2. Place your input video (e.g., `mall_footage.mp4`) in the same folder.  
3. Run the main script:
   ```bash
   python yolo_footfall_final_video.py
   ```
4. Wait for processing — the final output video will be saved as:
   ```
   final_footfall_output.mp4
   ```
   It includes:
   - Bounding boxes, person IDs, and live IN/OUT counts.
   - A final “summary screen” showing the total counts.

---

## Output

- **Output File:** `final_footfall_output.mp4`
- **Displayed Elements:**
  - ROI Line (Red)
  - Tracked IDs (Blue)
  - Live Counters (Green/Red)
  - Final Summary (last 3 seconds)

Example output screen:
```
FINAL FOOTFALL REPORT
Total Entries (IN): 12
Total Exits (OUT): 9
```

---

## Future Improvements
- Replace Centroid Tracker with **DeepSORT** for robust tracking.
- Integrate with a **Flask / Streamlit Dashboard** for live analytics.
- Store time-based entry logs in **CSV or database** for hourly reports.

---

## Author
**G Nandha Kumar**  
Real-Time Footfall Counting System using YOLOv8  
