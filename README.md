#  Human Pose Estimation & Activity Classification

---

##  Project Structure

```
project/
│
├── pose_activity_classifier.ipynb                      
├── input.mp4                  
├── output.mp4                  ← Generated: annotated video               


---

##  Setup

### 1. Install Dependencies

```bash
pip install mediapipe opencv-python numpy pandas matplotlib scipy tqdm scikit-learn Pillow
```

### 2. Add Your Video

Place your video file

```
input.mp4
```

> **Tip — download from YouTube:**
> ```bash
> pip install yt-dlp
> yt-dlp -o input_video.mp4 "https://www.youtube.com/watch?v=YOUR_VIDEO_ID"
> ```

---

##  Running

### Option A — Jupyter Notebook (recommended)
```bash
jupyter notebook pose_activity_classifier.ipynb
```
Run all cells top-to-bottom. Each task is clearly labelled.

### Option B — Python Script
```bash
python pose_activity_classifier.py
```

---

##  Customise Ground Truth

In **Task 3**, edit the `GT_SEGMENTS` list to match your actual video layout:

```python
# Example: first 5 seconds standing, next 5 squatting, rest arms raised
GT_SEGMENTS = [
    (0,       5*fps,  'standing'),
    (5*fps,   10*fps, 'squatting'),
    (10*fps,  total_frames, 'arms_raised'),
]
```

The segments must match what the person is actually doing in your video.

---

##  Threshold Tuning

If accuracy is low, adjust thresholds in `THRESHOLDS` (in the Configuration cell):

| Activity     | Key angles          | Default thresholds         |
|-------------|---------------------|----------------------------|
| Standing    | Knee, Hip           | Knee > 155°, Hip > 155°    |
| Squatting   | Knee, Hip           | Knee < 120°, Hip < 130°    |
| Arms Raised | Elbow               | Elbow < 110°               |

---

##  Output

| File | Description |
|------|-------------|
| `output_pose.mp4` | Video with skeleton overlay + live activity label |
| `results/dashboard.png` | Summary visualization |
| `results/frame_results.csv` | Frame-by-frame predictions |
| `results/task3_confusion_matrix.png` | Confusion matrix |

---

##  Report

Open `report.html` in any browser. To convert to PDF:
- Chrome/Edge → Print → Save as PDF

---

##  Technical Details

| Component | Choice | Reason |
|-----------|--------|--------|
| Pose model | MediaPipe Pose (complexity=1) | Pre-trained, CPU-real-time, 33 keypoints |
| Smoothing | Savitzky-Golay (w=11, p=3) | Preserves peaks unlike moving average |
| Angles | Cosine rule at joint vertex | Robust to scale and camera distance |
| Classifier | Rule-based threshold system | Interpretable, zero training data needed |
| Accuracy | Frame-level vs manual GT | Standard evaluation protocol |
