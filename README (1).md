<div align="center">

# ⚽ Football Players Analytics Performance

**AI-powered match analysis using YOLOv8 object detection + KMeans jersey classification**

[![Python](https://img.shields.io/badge/Python-3.9%2B-blue?logo=python&logoColor=white)](https://python.org)
[![YOLOv8](https://img.shields.io/badge/YOLOv8-Ultralytics-00BFFF?logo=data:image/svg+xml;base64,)](https://ultralytics.com)
[![scikit-learn](https://img.shields.io/badge/scikit--learn-KMeans-F7931E?logo=scikit-learn&logoColor=white)](https://scikit-learn.org)
[![OpenCV](https://img.shields.io/badge/OpenCV-4.x-5C3EE8?logo=opencv)](https://opencv.org)
[![Colab](https://img.shields.io/badge/Google%20Colab-Ready-F9AB00?logo=googlecolab&logoColor=white)](https://colab.research.google.com)
[![HuggingFace](https://img.shields.io/badge/Hugging%20Face-Spaces-FFD21E?logo=huggingface&logoColor=black)](https://huggingface.co/spaces)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)

<br/>

Upload a football match video → get a fully annotated video, interactive HTML report, heatmaps, pass networks, and a live public dashboard — all in one notebook run.

</div>

---

## 🗺 Processing Pipeline

![Pipeline](assets/pipeline.png)

The pipeline runs end-to-end with a single cell execution, processing from raw video all the way through to a deployable web dashboard.

---

## 🏗 System Architecture

![Architecture](assets/architecture.png)

---

## ✨ Features

![Features](assets/features_banner.png)

| Feature | Description |
|---|---|
| 🔍 **YOLOv8 Detection** | Detects players and the ball every frame with configurable confidence threshold |
| 🎨 **KMeans Jersey Classification** | Auto-learns team colors from 60 seed frames — no manual labeling needed |
| 🗺 **Heatmap Generation** | Gaussian-spread positional heatmaps per team overlaid on a pitch diagram |
| 🔗 **Pass Network** | Detects ball-custody changes and draws average-position pass graphs |
| ⚡ **Speed & Distance Metrics** | Converts pixel displacement to real-world km/h and metres per player |
| 📊 **CSV Export** | Full per-player stats and per-pass event logs downloadable as CSV |
| 🌐 **HTML Match Report** | Self-contained dark-mode report with embedded heatmaps and pass log |
| 🌐 **Interactive Dashboard** | Full SPA web app with team tabs, charts, and pass feed |
| 🚀 **Hugging Face Deploy** | One-click publish to a live public URL on Hugging Face Spaces |

---

## 📊 Sample Outputs

### Heatmaps

![Heatmap Preview](assets/heatmap_preview.png)

*Per-team positional density overlaid on a correctly proportioned pitch. Hotter colours indicate higher presence.*

### Pass Networks

![Pass Network Preview](assets/passnet_preview.png)

*Each node is a tracked player positioned at their average pitch location. Line thickness represents pass frequency.*

---

## 🚀 Quick Start (Google Colab)

```
1. Open in Colab  →  File › Upload notebook › select the .py file
2. Runtime › Change runtime type › GPU (T4 recommended)
3. Edit Cell 2 with your video path and team names
4. Run All  (Runtime › Run all)
```

> **Full match (~5 400 frames) takes 15–40 min on GPU.**  
> Set `MAX_FRAMES = 150` for a quick 5-second smoke test.

---

## 📋 Notebook Cells

| Cell | Purpose | Run time |
|------|---------|----------|
| **Cell 1** | Install dependencies (`ultralytics`, `scikit-learn`, `opencv`) | ~2 min |
| **Cell 2** | Configure paths, team names, `MAX_FRAMES` | instant |
| **Cell 3** | Full AI analysis pipeline (detect → track → stats) | 15–40 min |
| **Cell 4** | Preview annotated video + green download button | instant |
| **Cell 5** | Generate interactive web dashboard (HTML SPA) | ~10 s |
| **Cell 6** | Download all 9 output files to your machine | instant |
| **Cell 7** | Deploy dashboard to Hugging Face Spaces | ~1 min |

---

## ⚙️ Configuration

Edit **Cell 2** before running:

```python
DATASET_ZIP  = '/content/football-players-detection.v1i.yolov8 (1).zip'
VIDEO_IN     = '/content/your_match_video.mp4'   # ← your video
TEAM_NAMES   = ['Al-Hilal', 'Al-Nassr']          # ← team names
MAX_FRAMES   = 0     # 0 = full video | 150 = quick test
```

### Tuning Constants (Cell 3)

| Constant | Default | Description |
|----------|---------|-------------|
| `CONF_THRESH` | `0.35` | YOLO detection confidence threshold |
| `POSS_RADIUS` | `110` px | Ball-to-player possession radius |
| `MIN_HOLD_FRAMES` | `4` | Minimum frames before a pass is counted |
| `SPRINT_KMH` | `25.0` | Speed threshold to count as a sprint |
| `HI_KMH` | `15.0` | High-intensity run threshold |
| `MAX_PASS_M` | `50.0` | Maximum valid pass distance (metres) |

---

## 📦 Output Files

After **Cell 3** completes, you'll find these files in `/content/soccer_output/`:

```
soccer_output/
├── overlay.mp4                  # Annotated video with HUD
├── match_report.html            # Self-contained dark-mode HTML report
├── soccer_analytics_website.html# Full interactive dashboard (SPA)
├── player_stats.csv             # Per-player metrics
├── pass_events.csv              # Per-pass event log
├── heatmap_team1.png            # Team 1 positional heatmap
├── heatmap_team2.png            # Team 2 positional heatmap
├── passnet_team1.png            # Team 1 pass network chart
└── passnet_team2.png            # Team 2 pass network chart
```

### `player_stats.csv` Schema

| Column | Type | Description |
|--------|------|-------------|
| `player_id` | int | Tracker-assigned player ID |
| `role` | str | Team name |
| `total_dist_m` | float | Total distance covered (metres) |
| `top_speed_kmh` | float | Peak recorded speed (km/h) |
| `avg_speed_kmh` | float | Mean speed while on pitch |
| `sprint_count` | int | Number of sprints (> 25 km/h) |
| `sprint_dist_m` | float | Total sprint distance |
| `passes_made` | int | Passes completed |
| `passes_received` | int | Passes received |
| `main_zone` | str | Most-occupied pitch zone |

### `pass_events.csv` Schema

| Column | Type | Description |
|--------|------|-------------|
| `frame` | int | Video frame number |
| `team` | str | Team name |
| `from_pid` | int | Passer player ID |
| `to_pid` | int | Receiver player ID |
| `dist_m` | float | Pass distance (metres) |

---

## 🚀 Deploy to Hugging Face Spaces

1. Create a free account at [huggingface.co](https://huggingface.co)
2. Go to **Settings → Access Tokens** → create a token with **write** role
3. Fill in Cell 7:

```python
HF_TOKEN   = "hf_your_token_here"
HF_REPO_ID = "your-username/football-analytics"
```

4. Run Cell 7 → your live URL will be:  
   `https://your-username-football-analytics.hf.space`

---

## 🧠 How It Works

### Phase 1 — Jersey Color Learning
The classifier scans 60 evenly-spaced frames from the first 1 500 frames, collecting 400+ jersey HSV samples from detected players inside the pitch ROI. It then fits a **2-cluster KMeans model** on the hue and saturation channels to separate the two teams — no manual colour picking required.

### Phase 2 — Frame-by-Frame Processing
Every frame runs through:
1. **YOLOv8** detects bounding boxes for `person` (class 0) and `ball` (class 32)
2. Pitch ROI detector filters out spectators using green-channel HSV masking
3. Players are matched to existing tracks by closest-centroid (< 85 px)
4. Each player's jersey crop is classified against the fitted KMeans model
5. Speed is computed from pixel displacement × metres-per-pixel × FPS
6. Ball proximity determines possession; custody changes that meet hold-frame and distance thresholds are logged as passes

### Phase 3 — Post-Match Analytics
After the main loop, the system generates:
- **Gaussian heatmaps** by spreading player positions over a virtual pitch canvas
- **Pass network graphs** using each player's average position as the node location
- **HTML report** with embedded base64 images, team scorecards, and CSV download links
- **CSV exports** for downstream analysis in Excel, Power BI, or Python

---

## 🛠 Requirements

```
ultralytics >= 8.0
scikit-learn >= 1.0
opencv-python-headless >= 4.5
numpy
```

Install in one command:

```bash
pip install ultralytics scikit-learn opencv-python-headless
```

Or let **Cell 1** do it automatically in Colab.

---

## 📁 Dataset

The notebook expects a **Roboflow YOLOv8-format** football player detection dataset.

**Recommended:** [Football Players Detection v1 on Roboflow](https://universe.roboflow.com/roboflow-jvuqo/football-players-detection-3zvbc)

Download as **YOLOv8 format ZIP** and set `DATASET_ZIP` in Cell 2.

---

## 🗂 Project Structure

```
football-analytics/
├── football_players_analytics_performance.py   # Main Colab notebook (as .py)
├── README.md
└── assets/
    ├── pipeline.png
    ├── architecture.png
    ├── heatmap_preview.png
    ├── passnet_preview.png
    └── features_banner.png
```

---

## 📄 License

This project is licensed under the **MIT License** — see [LICENSE](LICENSE) for details.

---

<div align="center">

Built with ❤️ using [Ultralytics YOLOv8](https://ultralytics.com) · [scikit-learn](https://scikit-learn.org) · [OpenCV](https://opencv.org)

⭐ Star this repo if it helped you!

</div>
