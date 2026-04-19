# dumb_video_analyzer

Streamlit + MediaPipe webcam demo that detects:

- A raised middle finger (draws a label over the video)
- Alternating hand motion (waving hands out of phase)

Uses [streamlit-webrtc](https://github.com/whitphx/streamlit-webrtc) to stream
the browser webcam into a Python video processor, [MediaPipe Hands](https://google.github.io/mediapipe/solutions/hands)
for landmark detection, and OpenCV for drawing.

## Deploy

The app is designed for [Streamlit Community Cloud](https://share.streamlit.io/)
(the only free host that runs Streamlit + WebRTC out of the box). To deploy:

1. Push this repo to GitHub (already done).
2. Sign in at https://share.streamlit.io with the GitHub account that owns the repo.
3. Click **New app**, pick `iamsorenl/dumb_video_analyzer`, branch `main`, main file `app.py`.
4. Leave Python version on "From repo" — `.python-version` pins it to 3.12 for MediaPipe compatibility.
5. Click **Deploy**. First build takes ~2–3 minutes (MediaPipe wheels are large).

Once it's up, Streamlit Cloud gives you a public URL you can paste back into
the repo's About box so the badge below resolves.

[![Open in Streamlit](https://static.streamlit.io/badges/streamlit_badge_black_white.svg)](https://share.streamlit.io/)

### Known limitation: WebRTC and symmetric NAT

The app only configures Google's public STUN server, so users on networks that
do symmetric NAT (some corporate, hotel, and mobile networks) won't get a peer
connection and the video box will stay blank. Fix: add a TURN server — the
[Twilio Network Traversal Service](https://www.twilio.com/docs/stun-turn) has a
free tier and is the usual choice for streamlit-webrtc demos.

## Run locally

```bash
python3.12 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
streamlit run app.py
```

Then open http://localhost:8501 — your browser will prompt for webcam access.

The `opencv_test.py` script is a standalone OpenCV-only version (no Streamlit)
for quick iteration on the detection logic.
