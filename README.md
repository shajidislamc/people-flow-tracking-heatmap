# 🧠 People Flow Detection using Object Tracking & Heatmap Visualization

## 📋 Project Overview
This project implements a people counting system that tracks individuals as they move through a defined area, counting entries (IN) and exits (OUT) based on line crossings, while generating a heatmap of movement patterns.

## 🎥 Video Source
- **URL**: https://media.roboflow.com/supervision/video-examples/people-walking.mp4

## 🔧 Detection Method
- **Model**: YOLOv8 (ultralytics)
- **Target Class**: Person (class ID: 0)
- **Confidence Threshold**: 0.3

## 📍 Line Coordinates
Two horizontal lines are drawn across the video frame for counting:

| Line | Y-Position | Color | Purpose |
|------|------------|-------|---------|
| Upper Line (IN) | 40% of frame height | 🟢 Green | Counts people entering (moving downward) |
| Lower Line (OUT) | 60% of frame height | 🔴 Red | Counts people exiting (moving upward) |

**Note**: Line coordinates are calculated as percentages of frame dimensions for adaptability to different video resolutions.

## 🎯 IN/OUT Counting Logic

### Entry Detection (IN Count)
1. Track each person's center point (bounding box center)
2. Store previous Y-position for each tracked ID
3. **IN condition**: 
   - Previous position is ABOVE the upper line (smaller Y value)
   - Current position is BELOW the upper line (larger Y value)
   - Movement direction: Top → Bottom (downward)

### Exit Detection (OUT Count)
1. Track each person's center point (bounding box center)
2. Store previous Y-position for each tracked ID
3. **OUT condition**:
   - Previous position is BELOW the lower line (larger Y value)
   - Current position is ABOVE the lower line (smaller Y value)
   - Movement direction: Bottom → Top (upward)

```
Frame Layout:
┌─────────────────────────────────┐
│           TOP                   │
│                                 │
│ ═══════════════════════════════ │ ← Upper Line (Green) - IN detection
│                                 │
│          COUNTING ZONE          │
│                                 │
│ ═══════════════════════════════ │ ← Lower Line (Red) - OUT detection
│                                 │
│          BOTTOM                 │
└─────────────────────────────────┘

IN:  Person moves ↓ across Upper Line
OUT: Person moves ↑ across Lower Line
```

## 🔥 Heatmap Generation
- **Method**: Accumulate bounding box center positions across all frames
- **Visualization**: Gaussian blur applied to create smooth density visualization
- **Color Map**: JET colormap (blue → green → yellow → red)
- **Output**: Overlay on last video frame + separate heatmap image

## 📦 Dependencies
```
ultralytics>=8.0.0
supervision>=0.19.0
opencv-python>=4.8.0
numpy>=1.24.0
matplotlib>=3.7.0
```

## 🚀 How to Run
1. Open the Jupyter notebook `people_flow_detection.ipynb`
2. Run all cells sequentially
3. Wait for video processing to complete
4. Find outputs in the project directory

## 📤 Output Files
| File | Description |
|------|-------------|
| `output_tracked.mp4` | Processed video with bounding boxes, IDs, and live counters |
| `heatmap.png` | Final heatmap visualization of movement patterns |
| `heatmap_overlay.png` | Heatmap overlaid on the last video frame |

## 📊 Features
- ✅ Real-time people detection using YOLOv8
- ✅ ByteTrack for robust multi-object tracking
- ✅ Unique ID assignment for each person
- ✅ Bidirectional counting (IN/OUT)
- ✅ Visual line indicators with color coding
- ✅ Live counter display on video
- ✅ Motion heatmap generation
- ✅ Supports any video resolution

## 🛠️ Technical Architecture
```
Video Input → YOLO Detection → ByteTrack Tracking → Line Crossing Logic → Counter Update
                    ↓                    ↓
              Bounding Boxes      Position History
                    ↓                    ↓
              Annotated Frame    Heatmap Accumulation
                    ↓                    ↓
              Output Video       Final Heatmap
```

## 📝 Notes
- The system uses the `supervision` library for streamlined detection and tracking
- ByteTrack provides robust tracking even with occlusions
- Heatmap intensity correlates with time spent in each area
- Line positions can be adjusted in the notebook configuration section

## 👤 Author
Shajid Islam Chowdhury

