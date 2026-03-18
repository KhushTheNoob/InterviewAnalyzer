# Interview Analyzer

Interview Analyzer is a Flask web application that records a 60-second interview response, analyzes non-verbal behavior, and returns dimension-wise scores with actionable feedback.

## Features

- Random interview question selection from an editable question bank.
- Browser-based webcam recording using MediaRecorder (60 seconds).
- Server-side video analysis pipeline with modular analyzers:
	- posture/body language
	- hand gestures
	- face/eye-contact signals
- Rule-based scoring engine (ML-ready interface for future model swap).
- Structured results page with score breakdown and feedback sections.

## Project Structure

interview-analyzer/
├── app.py
├── questions.txt
├── requirements.txt
├── analyzer/
│   ├── __init__.py
│   ├── video_analyzer.py
│   ├── pose_analyzer.py
│   ├── gesture_analyzer.py
│   ├── face_analyzer.py
│   ├── scorer.py
│   └── feedback.py
├── ml/
│   ├── model.py
│   ├── dataset/
│   └── sample_dataset.py
├── static/
│   ├── css/style.css
│   └── js/recorder.js
└── templates/
		├── index.html
		├── record.html
		└── results.html

## Tech Stack

- Python
- Flask
- OpenCV
- MediaPipe
- NumPy
- Vanilla HTML/CSS/JS

## Setup

1) Clone repository

git clone https://github.com/KhushTheNoob/InterviewAnalyzer.git
cd InterviewAnalyzer

2) Create virtual environment (recommended)

Windows (PowerShell):

python -m venv .venv
.venv\Scripts\Activate.ps1

3) Install dependencies

pip install -r requirements.txt

## Run the App

python app.py

Open in browser:

http://127.0.0.1:5000

## Web Flow

1. Landing page loads one question from questions.txt.
2. Candidate opens recording page.
3. Browser records webcam + mic for 60 seconds.
4. Video uploads to /upload.
5. Backend analyzes frames and computes scores.
6. Results page shows:
	 - confidence_score
	 - posture_score
	 - gesture_score
	 - body_language_score
	 - overall_score
	 - strengths/improvements/mistakes

## Routes

- GET /  → Question page
- GET /record  → Recording interface
- POST /upload  → Video upload + analysis trigger
- GET /results/<id>  → Session result view

## Questions File

Edit questions in questions.txt.

Format:

[EASY] Tell me about yourself.
[MODERATE] Describe a time you handled conflict at work.

Lines starting with # are ignored.

## Scoring Design (Current Phase)

The current scorer is rule-based and uses stable feature-vector keys so you can later replace logic with a trained model without changing the API contract.

Primary dimensions:

- confidence_score
- posture_score
- gesture_score
- body_language_score
- overall_score (weighted aggregate)

Future ML integration points:

- analyzer/scorer.py
- ml/model.py

## Notes on MediaPipe Compatibility

Some environments install a MediaPipe build that exposes only tasks modules and not mediapipe.solutions.

Current behavior in this project:

- If mediapipe.solutions.holistic is available, the app uses full holistic analysis.
- If not available, it automatically falls back to an OpenCV-based analyzer path so the app still runs.

This keeps the project runnable while preserving a path to full landmark-level analysis in compatible environments.

## Output Data

- Uploaded recordings are stored in uploads/.
- Session result JSON files are stored in results/.

These runtime folders are ignored by git via .gitignore.

## Troubleshooting

1) Camera not opening in browser

- Ensure browser camera/mic permissions are allowed.
- Use https or localhost (127.0.0.1) for media device access.

2) Import errors for Flask/OpenCV/MediaPipe

- Confirm active venv.
- Reinstall requirements:
	pip install -r requirements.txt

3) Analysis seems weak or unstable

- Improve lighting.
- Keep full upper body and both hands in frame.
- Keep camera at eye level.

## Development Notes

- Keep analyzer functions small and focused for easy edits.
- Thresholds are defined as named constants in each analyzer module.
- Add your own dataset rows in ml/dataset/ for future training workflows.

## License

No license specified yet. Add a LICENSE file if you want to make usage terms explicit.