# ⚽ Football Players Analytics Performance

<div align="center">


**AI-powered football match analysis using YOLOv8 object detection + KMeans jersey classification**

### 🚀 [Try it Live on Hugging Face →](https://huggingface.co/spaces/Nawaf200/Football_players_Analytics_performance)

</div>

---

## 📌 Overview

Football Players Analytics Performance is a full AI pipeline that takes a raw football match video and automatically produces professional-grade analytics — player tracking, possession stats, pass networks, heatmaps, speed metrics, and an interactive web dashboard — all without any manual annotation.

---

## ✨ Features

| Feature | Description |
|---|---|
| 🎯 **Player Detection** | YOLOv8 real-time person & ball detection on every frame |
| 👕 **Team Classification** | KMeans clustering on HSV jersey colors — no manual labeling |
| 🏃 **Speed & Distance** | Per-player total distance, top speed, average speed, sprint count |
| ⚽ **Ball Possession** | Smoothed possession tracking per team across the full match |
| 🔗 **Pass Detection** | Automatic pass event detection with from/to player IDs and distances |
| 🗺️ **Heatmaps** | Positional heatmaps for both teams across 9 pitch zones |
| 🕸️ **Pass Networks** | Visual pass network graphs per team |
| 📊 **Web Dashboard** | Interactive HTML dashboard with live charts, tables, and CSV downloads |
| ☁️ **Hugging Face Deploy** | One-click deployment of the dashboard to a public Hugging Face Space |

---

## 🖥️ Demo

🔗 **Live Dashboard:** [https://huggingface.co/spaces/Nawaf200/Football_players_Analytics_performance](https://huggingface.co/spaces/Nawaf200/Football_players_Analytics_performance)

---

## 🗂️ Notebook Structure

The pipeline is organized into 7 cells in a Google Colab notebook:

```
Cell 1 — Install Dependencies        Install ultralytics, scikit-learn, opencv
Cell 2 — Configure Paths & Teams     Set video path, team names, output folder
Cell 3 — Run Full AI Pipeline        Detection → Tracking → Classification → Stats
Cell 4 — Preview Annotated Video     Watch the output video + green download button
Cell 5 — Generate Web Dashboard      Build an interactive HTML report
Cell 6 — Download All Outputs        Save all 9 output files to your computer
Cell 7 — Deploy to Hugging Face      Publish dashboard as a live public website
```

---

## ⚙️ How It Works

### Phase 1 — Jersey Sampling (60 frames)
Scans the first 60 frames and collects 400+ jersey color samples. Fits a KMeans model on HSV hue values to lock in the two team colors automatically.

### Phase 2 — Frame-by-Frame Processing
For every frame in the video:
- YOLOv8 detects all players and the ball
- Each player is matched to their track using proximity
- Jersey color is re-sampled and team is re-predicted
- Speed and distance are calculated per player per frame
- Ball possession is assigned with a smoothing window
- Pass events are detected when ball control changes between same-team players

### Phase 3 — Report Generation
After processing, the pipeline generates:
- Heatmaps for both teams
- Pass network visualizations
- A full interactive HTML dashboard with embedded CSVs

---

## 📦 Output Files

| File | Description |
|---|---|
| `overlay.mp4` | Annotated video with player IDs, speeds, team colors, and ball possession |
| `match_report.html` | Interactive HTML dashboard |
| `player_stats.csv` | Per-player stats (distance, speed, sprints, passes, zone) |
| `pass_events.csv` | Every detected pass (frame, team, from → to, distance) |
| `heatmap_team1.png` | Positional heatmap for Team 1 |
| `heatmap_team2.png` | Positional heatmap for Team 2 |
| `passnet_team1.png` | Pass network graph for Team 1 |
| `passnet_team2.png` | Pass network graph for Team 2 |
| `soccer_analytics_website.html` | Standalone deployable web dashboard |

---

## 🚀 Quick Start

### 1. Open in Google Colab

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1emeEWVxEgVmGeaENANonPzQbBc32YGFQ)

### 2. Install Dependencies (Cell 1)
```python
!pip install ultralytics scikit-learn opencv-python-headless -q
```

### 3. Configure Your Video (Cell 2)
```python
VIDEO_IN   = '/content/your_match_video.mp4'   # path to your video
TEAM_NAMES = ['Team A', 'Team B']               # your team names
MAX_FRAMES = 0                                  # 0 = full video | 150 = quick test
```

### 4. Run the Pipeline (Cell 3)
Click **Run** on Cell 3 and wait. Progress prints every 60 frames.

> ⏱️ Full match (~5400 frames) takes **15–40 minutes** on GPU.

### 5. Deploy to Hugging Face (Cell 7, optional)
```python
HF_TOKEN   = "hf_your_token_here"
HF_REPO_ID = "your-username/your-space-name"
```

---

## 🛠️ Requirements

| Package | Purpose |
|---|---|
| `ultralytics` | YOLOv8 detection and tracking |
| `scikit-learn` | KMeans jersey clustering |
| `opencv-python-headless` | Video processing and drawing |
| `numpy` | Numerical operations |
| `huggingface_hub` | Deploying dashboard to HF Spaces |

---

## 📐 Key Parameters

| Parameter | Default | Description |
|---|---|---|
| `CONF_THRESH` | `0.35` | YOLOv8 detection confidence threshold |
| `POSS_RADIUS` | `110 px` | Pixel radius to assign ball possession |
| `SPRINT_KMH` | `25.0` | Speed threshold to count a sprint |
| `HI_KMH` | `15.0` | High-intensity running threshold |
| `MAX_PASS_M` | `50.0 m` | Maximum distance to count as a pass |
| `MIN_PASS_M` | `1.2 m` | Minimum distance to count as a pass |
| `PASS_COOLDOWN` | `10 frames` | Frames to wait before logging next pass |

---

## 🧠 Technical Details

- **Detection model:** YOLOv8 (person class 0, ball class 32)
- **Tracker:** Custom proximity-based tracker with 12-frame grace period
- **Team classifier:** KMeans (k=2) on HSV hue + saturation of jersey crop
- **Pitch detection:** Green HSV mask + morphological operations to find pitch ROI
- **Possession smoothing:** Sliding window of 8 frames
- **Speed calculation:** Euclidean pixel displacement × meters-per-pixel × FPS × 3.6

---

## 📊 Dashboard Preview

The generated interactive dashboard includes:

- 📈 Live possession bar with real-time percentages
- 🏃 Per-player stats table (distance, speed, sprints, passes, zone)
- 📋 Pass event feed with from/to player and distance
- 📥 One-click CSV export for players and passes
- 🗺️ Embedded heatmap and pass network images

---

## 📄 License

This project is licensed under the **MIT License** — free to use, modify, and distribute.

---

## 🙌 Author

**Nawaf** — [Hugging Face Profile](https://huggingface.co/Nawaf200)

---

<div align="center">

⭐ **Star this repo if you found it useful!** ⭐

[🚀 Try Live Demo](https://huggingface.co/spaces/Nawaf200/Football_players_Analytics_performance) 
</div>
