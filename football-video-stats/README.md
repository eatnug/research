# football-video-stats

Extract player stats from match footage using computer vision.

## Why

FM/FIFA rate players with thousands of human reviewers watching hundreds of hours of footage. The CV pipeline to automate most of this exists in pieces — detection, tracking, event spotting are largely solved. The unsolved part is stitching it together into a reliable end-to-end system that outputs meaningful per-player statistics.

This project: survey the landscape, build a working pipeline, and see how close we can get to human-level player rating.

## What's Possible Now

### Solved (from broadcast footage)
- **Player detection/tracking**: 97.7% detection, 94% tracking accuracy
- **Physical metrics**: sprint speed, distance, acceleration — from tracking data
- **Event detection**: pass, shot, tackle, cross — 12+ event classes
- **Spatial analysis**: heatmaps, formation, positioning

### Feasible as Proxies
- Pass quality (completion rate + difficulty context)
- Work rate (distance + sprint frequency)
- Positioning (spatial analysis relative to teammates/opponents)
- Aggression (tackle frequency/intensity)

### Not Possible from Video
- Mental attributes (vision, composure, decisions, anticipation, leadership) — ~14 attributes
- Technique quality (needs close-up + pose estimation)
- Rare events (free kicks, penalties — too few samples per match)

## Pipeline

```
Broadcast Video
→ Player Detection (YOLO) + Tracking (ByteTrack)
→ Camera Calibration → Pitch Coordinates
→ Team Classification (jersey color)
→ Ball Tracking
→ Event Detection (pass, shot, tackle, etc.)
→ Event Attribution (which player)
→ Physical Metrics (speed, distance, acceleration)
→ Statistical Aggregation (per-player)
→ Output: raw stats per player
```

Steps 1-6 largely solved. 7-9 improving fast.

## Key Resources

### Open Source
| Project | What |
|---------|------|
| [SoccerNet](https://github.com/SoccerNet) | Largest football CV benchmark — tracking, events, calibration |
| [Roboflow Sports](https://github.com/roboflow/sports) | YOLO + ByteTrack pipeline for sports |
| [SkillCorner Open Data](https://github.com/SkillCorner/opendata) | Broadcast tracking data (10 matches) |
| [StatsBomb Open Data](https://github.com/statsbomb/open-data) | Event data + 360 freeze frames |
| [Metrica Sports Sample Data](https://github.com/metrica-sports/sample-data) | Tracking + event sample data |

### Companies
- **SkillCorner** — XY tracking from broadcast, ~10cm accuracy
- **Metrica Sports** — tracking from broadcast + drones
- **Second Spectrum** — EPL official tracking (multi-camera)
- **StatsBomb 360** — freeze-frame positions at events

### Papers
- "A Review of Computer Vision Technology for Football Videos" (MDPI 2025)
- "FootyVision: Multi-Object Tracking, Localisation, and Augmentation" (ACM 2024)
- SoccerNet 2025 Challenge Results (arXiv 2025)
- T-DEED — transformer encoder-decoder for event spotting
- COMEDIAN — self-supervised action spotting

## Experiments

(tbd)
