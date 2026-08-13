# iot-heart-rate-monitor

from flask import Flask, render_template, Response, jsonify
import cv2
import random

app = Flask(__name__)

cap = cv2.VideoCapture("patient.mp4")

prev_frame = None

# ✅ Global shared data
current_data = {
    "heart_rate": 80,
    "spo2": 98,
    "status": "Normal",
    "alert": False
}

# -----------------------------
# Motion Detection
# -----------------------------
def detect_motion(frame):
    global prev_frame
    gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)

    if prev_frame is None:
        prev_frame = gray
        return "Normal"

    diff = cv2.absdiff(prev_frame, gray)
    prev_frame = gray

    if diff.mean() < 5:
        return "No Movement"
    return "Normal"

# -----------------------------
# Analysis
# -----------------------------
def analyze(frame):
    global current_data

    # Smooth variation
    current_data["heart_rate"] += random.randint(-2, 2)
    current_data["spo2"] += random.randint(-1, 1)

    current_data["heart_rate"] = max(60, min(120, current_data["heart_rate"]))
    current_data["spo2"] = max(90, min(100, current_data["spo2"]))

    motion = detect_motion(frame)

    # Condition check
    if current_data["heart_rate"] > 110:
        status = "Critical - High HR"
    elif current_data["spo2"] < 92:
        status = "Critical - Low SpO2"
    elif motion == "No Movement":
        status = "Critical - No Movement"
    else:
        status = "Normal"

    current_data["status"] = status
    current_data["alert"] = (status != "Normal")

# -----------------------------
# Video Stream
# -----------------------------
def gen_frames():
    while True:
        success, frame = cap.read()

        if not success:
            cap.set(cv2.CAP_PROP_POS_FRAMES, 0)
            continue

        analyze(frame)

        # Overlay on video
        cv2.putText(frame, f"Status: {current_data['status']}", (20,40),
                    cv2.FONT_HERSHEY_SIMPLEX, 1, (0,0,255), 2)

        cv2.putText(frame, f"HR: {current_data['heart_rate']}", (20,80),
                    cv2.FONT_HERSHEY_SIMPLEX, 0.8, (0,255,0), 2)

        cv2.putText(frame, f"SpO2: {current_data['spo2']}", (20,120),
                    cv2.FONT_HERSHEY_SIMPLEX, 0.8, (255,0,0), 2)

        _, buffer = cv2.imencode('.jpg', frame)
        frame = buffer.tobytes()

        yield (b'--frame\r\n'
               b'Content-Type: image/jpeg\r\n\r\n' + frame + b'\r\n')

# -----------------------------
# Routes
# -----------------------------
@app.route('/')
def index():
    return render_template('index.html')

@app.route('/video')
def video():
    return Response(gen_frames(),
                    mimetype='multipart/x-mixed-replace; boundary=frame')

@app.route('/data')
def data():
    return jsonify(current_data)

# -----------------------------
# Run
# -----------------------------
if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000, debug=True)
