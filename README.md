# Fastener Detector

A Python-based web application for detecting fasteners and shear pins from point clouds, CAD files, and images.

## Features

- ✅ Point cloud upload (.pcd, .ply, .xyz)
- ✅ RANSAC-based cylinder detection
- ✅ Real-time 3D visualization with Three.js
- ✅ JSON export of detection results
- ✅ Local web server (no external dependencies)
- ✅ Batch and real-time processing support
- 🔜 ML-based detection (Phase 2)
- 🔜 Image-based detection (Phase 2)
- 🔜 STEP/CAD file support (Phase 2)

## Installation

### Requirements

- Python 3.8+
- pip

### Setup

1. Clone the repository:

```bash
git clone https://github.com/codygx/fastener-detector.git
cd fastener-detector
```

2. Create virtual environment (optional but recommended):

```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

3. Install dependencies:

```bash
pip install -r backend/requirements.txt
```

## Usage

### Starting the Server

```bash
python backend/main.py
```

The application will start on `http://localhost:5000`

### Changing the Port

Edit `backend/config.py` and change the `PORT` variable:

```python
PORT: int = int(os.getenv("PORT", "5000"))  # Change 5000 to desired port
```

Or set environment variable:

```bash
export PORT=8000
python backend/main.py
```

### Using the Web Interface

1. Open browser to `http://localhost:5000`
2. Drag and drop your point cloud file or click to browse
3. Adjust detection parameters if needed
4. Click "Detect Fasteners"
5. View results in 3D viewer
6. Download JSON results

## Project Structure

```
fastener-detector/
├── backend/
│   ├── main.py                 # FastAPI server entry point
│   ├── config.py               # Configuration settings
│   ├── requirements.txt        # Python dependencies
│   ├── detectors/
│   │   ├── ransac_detector.py  # RANSAC cylinder detection
│   │   ├── ml_detector.py      # Placeholder for ML (Phase 2)
│   │   └── hybrid_detector.py  # Placeholder for hybrid (Phase 2)
│   └── processors/
│       ├── point_cloud_processor.py  # File I/O and preprocessing
│       └── utils.py                   # Helper functions
├── frontend/
│   ├── index.html              # Main UI
│   ├── style.css               # Styling
│   ├── app.js                  # Frontend logic
│   └── 3d-viewer.js            # Three.js visualization
├── data/
│   ├── uploads/                # Uploaded files
│   └── results/                # Detection results
└── README.md
```

## API Endpoints

### Upload Point Cloud

```
POST /api/upload
Content-Type: multipart/form-data

Body: point_cloud_file
Response: { "session_id": "uuid", "filename": "name", "status": "uploaded" }
```

### Run Detection

```
POST /api/detect?session_id=<uuid>&downsample=true
Response: {
  "session_id": "uuid",
  "status": "success",
  "total_points": 10000,
  "detected_cylinders": 5,
  "cylinders": [
    {
      "center": [x, y, z],
      "axis": [x, y, z],
      "radius": 0.005,
      "height": 0.02,
      "points_count": 150,
      "confidence": 0.85
    }
  ],
  "remaining_points": 9000
}
```

### Get Results

```
GET /api/results/<session_id>
Response: Detection results JSON
```

### Download Results

```
GET /api/download/<session_id>
Response: JSON file download
```

## Configuration

Edit `backend/config.py` to adjust:

- Server host and port
- RANSAC parameters (iterations, threshold, radius limits)
- Upload size limits
- Point cloud preprocessing options

## Detection Parameters

- **Min/Max Radius**: Minimum and maximum cylinder radius to detect (in meters)
- **Downsample**: Reduce point cloud density for faster processing
- **RANSAC Iterations**: Number of RANSAC iterations (higher = slower but better)
- **Distance Threshold**: Maximum distance from axis (in meters)

## Future Phases

### Phase 2
- [ ] ML-based detection models
- [ ] Image-based detection
- [ ] STEP file support
- [ ] Batch processing API
- [ ] Improved cylinder fitting

### Phase 3
- [ ] Real-time detection streaming
- [ ] Multi-user support
- [ ] Detection history/database
- [ ] Advanced filtering and analytics
- [ ] Team collaboration features

## Troubleshooting

### Port already in use

```bash
export PORT=8000
python backend/main.py
```

### File upload fails

- Check file format (.pcd, .ply, .xyz)
- Ensure file size < 100MB
- Check `data/uploads/` directory permissions

### Detection produces no results

- Adjust Min/Max Radius parameters
- Ensure point cloud is not too sparse
- Try disabling downsampling

## License

MIT

## Contributing

Contributions welcome! Please feel free to submit PRs or open issues.

## Support

For issues or questions, please open a GitHub issue.
