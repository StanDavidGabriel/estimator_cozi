# Security Queue Estimator — Python

A local Python app that estimates waiting times at airport security lanes. The Streamlit interface opens in your browser. Requires Python 3.11 or 3.12.

## Getting started

Open a terminal in the `estimator_cozi` folder.

```sh
python -m venv .venv
```

Activate the environment:
- Windows: `.venv\Scripts\activate`
- macOS / Linux: `source .venv/bin/activate`

```sh
python -m pip install -r requirements.txt
python -m streamlit run app.py
```

On macOS/Linux, use `python3` instead of `python` if needed.
Open the address shown in the terminal (usually http://localhost:8501).

## Usage

- Press **Start**, then choose the **Passengers** or **Detection** view.
- The **Limbă / Language** selector switches the interface between **Română** and **English** without stopping detection or changing the video source.
- The webcam is the camera of the computer running Python, not a remote camera.
- **Stop** releases the camera. Close the process with Ctrl+C when you are done.
- Do not expose this app to the internet; it is configured for localhost only.

## Files

- `app.py`: passenger display and technical view, start/stop controls and video sources.
- `i18n.py`: interface texts in Romanian and English.
- `detector.py`: YOLOv8n ONNX on CPU, OpenCV capture, simulated IR map.
- `queue_logic.py`: luggage association, the estimation formula and time smoothing.
- `test_queue_logic.py`: tests for the formula and the extra-time rules.
- `assets/`: the YOLOv8n ONNX model and the included WebM demo clip.

## Rules and limits

- The three lanes are equal vertical zones of the image, not physically calibrated lanes.
- People = 0.7 × YOLO + 0.3 × simulated IR; the base is 30 s per person per lane.
- Worst-case extra time is added: backpack +120 s, suitcase +90 s, laptop +60 s, handbag +30 s, nothing +0 s.
- The time is smoothed with alpha 0.15.
- IR is a simulation based on detections and noise, not a real thermal measurement.
- The included clip is generated for demonstration purposes.
- No history is kept and the camera is not recorded.
- Object detection does not reveal luggage contents; "nothing detected" does not guarantee there are no objects.
- Estimates are not validated for real airport operations.
- Target frame rate is 5 FPS, limited by the CPU.

## Tests

```sh
python -m unittest -v
```

## License note

The YOLOv8n model belongs to the Ultralytics ecosystem (AGPL-3.0 or commercial license).
See https://www.ultralytics.com/license before any commercial use; this repository does not grant a commercial license for the model.
