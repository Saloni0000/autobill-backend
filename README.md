
# Autobill Backend

🚀 A FastAPI-based real-time object detection backend using YOLOv8 (Ultralytics), OpenCV, and WebSockets. This backend processes incoming base64-encoded image frames, runs object detection, and returns predictions over WebSocket. It also supports CORS and is designed to be integrated with frontend apps such as web-based camera feeds.

👉 Live demo available on [HuggingFace Spaces](https://huggingface.co/spaces/salonik07/autobill-backend)

---

## 📦 Features

- 🧠 Object Detection using `YOLOv8` and a custom `best.pt` model
- 📸 Image input via base64-encoded frames (e.g., from a webcam)
- 🔁 Real-time detection via `WebSocket` connection
- 🌐 CORS-enabled for frontend integration
- 📦 Dockerized for easy deployment
- 🛑 REST API endpoints to start/stop detection

---

## 🛠 Tech Stack

- **FastAPI** — High-performance Python web framework
- **Ultralytics YOLOv8** — Object detection
- **OpenCV** — Image decoding and preprocessing
- **WebSocket** — Real-time bi-directional communication
- **Docker** — Containerization
- **Python 3.9+**

---

## 🚀 Getting Started

### 🔧 Installation

Clone the repository and install dependencies:

```bash
git clone https://github.com/Saloni0000/autobill-backend.git
cd autobill-backend
pip install -r requirements.txt
```

> Make sure you have `best.pt` (YOLO model weights) in the root directory.

### ▶️ Run the Server

```bash
uvicorn app:app --host 0.0.0.0 --port 8000
```

The backend will be available at: `http://localhost:8000`

---

## 🐳 Docker Support

Build and run with Docker:

```bash
docker build -t autobill-backend .
docker run -p 8000:8000 autobill-backend
```

---

## 🔌 API Endpoints

### REST API

| Method | Endpoint     | Description                  |
|--------|--------------|------------------------------|
| `GET`  | `/`          | Health check endpoint        |
| `GET`  | `/start`     | Start detection (toggle flag)|
| `POST` | `/stop`      | Stop detection (toggle flag) |

### WebSocket API

#### `ws://localhost:8000/ws`

Send base64-encoded image (as a Data URI, e.g., `"data:image/jpeg;base64,..."`) and receive real-time YOLO predictions.

**Response Format:**

```json
[
  {
    "class_name": "cat",
    "confidence": 0.91
  },
  ...
]
```

---

## 📂 Project Structure

```
.
├── app.py               # FastAPI app with WebSocket + REST endpoints
├── best.pt              # Custom YOLOv8 weights
├── requirements.txt     # Python dependencies
├── Dockerfile           # Docker setup
└── .gitattributes       # Git attributes
```

---

## 🧪 Sample WebSocket Test (Python)

```python
import asyncio
import websockets
import base64

async def send_image():
    uri = "ws://localhost:8000/ws"
    async with websockets.connect(uri) as websocket:
        with open("sample.jpg", "rb") as f:
            encoded = base64.b64encode(f.read()).decode("utf-8")
            data_uri = f"data:image/jpeg;base64,{encoded}"
            await websocket.send(data_uri)
            response = await websocket.recv()
            print(response)

asyncio.run(send_image())
```

---

## 🧠 Model Info

- The `best.pt` model is a custom-trained YOLOv8 model.
- You can retrain or fine-tune your model using [Ultralytics](https://docs.ultralytics.com/).

---

## 🛡️ Security Note

- Ensure CORS settings are configured correctly before deploying to production.
- Validate and sanitize incoming data in production settings.

---

## 📃 License

This project is licensed under the MIT License.

---

## 👩‍💻 Author

**Saloni0000**  
🔗 [GitHub Profile](https://github.com/Saloni0000)

---

> Feel free to fork, contribute, or open issues to improve this project!
