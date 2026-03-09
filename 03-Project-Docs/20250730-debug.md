---
created: 2025-07-30
tags: 
org:
  - GC
goal: debug
---
**When did I see this error?**
- During containerizing the backend
- After I added a Dockerfile and started it

**Testing process:**
```bash
docker-compose down
docker-compose up --build
docker ps -a
```

Before
```bash
curl -X POST http://localhost:5000/run-detect \
  -F "file=@backend/tests/test_img1.jpeg" \
  -F "model_id=player_detector_v8n"
```

After
```bash
curl -X POST http://127.0.0.1:5000/run-detect \
  -F "file=@backend/tests/test_img1.jpeg" \
  -F "model_id=player_detector_v8n"
```

**Expected behavior (if it works correctly):**
- It should display model outputs like x-y coordinates and %accuracy in `json`  format. 

**Error message:**

```bash
➜  cv-model-simulator git:(run-model-localstack) curl -X POST http://localhost:5000/run-detect \
  -F "file=@backend/tests/test_img1.jpeg" \
  -F "model_id=player_detector_v8n"
{"error":"An internal error occurred while processing the image"}
```

**container information:**

```bash
➜  cv-model-simulator git:(run-model-localstack) docker ps -a
CONTAINER ID   IMAGE                          COMMAND                  CREATED          STATUS                   PORTS                                                                NAMES
0ce9c5d0a213   localstack/localstack:latest   "docker-entrypoint.sh"   8 minutes ago    Up 8 minutes (healthy)   0.0.0.0:4510-4559->4510-4559/tcp, 0.0.0.0:4566->4566/tcp, 5678/tcp   localstack
983fdd299a28   cv-model-simulator-flask-api   "flask run"              18 minutes ago   Up 18 minutes            0.0.0.0:5000->5000/tcp                                               cv-model-simulator-flask-api-1
```

**Logs:**

full output too long, see details in: `docker logs cv-model-simulator-flask-api-1`

main errors:
1. 
```
Failed to establish a new connection: [Errno 111] Connection refused
```

2. 
```
Exception: Unable to download server-side/models/sport/baseball/clipping/event_clipping/2024-05-16-yolov8n-640.pt from bucket gc-computer-vision-models-qa: Could not connect to the endpoint URL: "http://localhost:4566/gc-computer-vision-models-qa/server-side/models/sport/baseball/clipping/event_clipping/2024-05-16-yolov8n-640.pt"
```

So, errors are:
- Cannot connect to localstack
- Cannot load models

I checked to see that the models are indeed in docker

```bash
➜  cv-model-simulator git:(run-model-localstack) docker exec -it localstack bash
root@0ce9c5d0a213:/opt/code/localstack# awslocal s3 ls s3://gc-computer-vision-models-qa/server-side/models/sport/baseball/clipping/event_clipping/ --recursive
2025-07-31 17:20:42       6148 server-side/models/sport/baseball/clipping/event_clipping/.DS_Store
2025-07-31 17:21:04    6236825 server-side/models/sport/baseball/clipping/event_clipping/2024-05-16-yolov8n-640.pt
2025-07-31 17:21:04   51580923 server-side/models/sport/baseball/clipping/event_clipping/2024-05-16-yolov9c-640.pt
2025-07-31 17:21:05   15558734 server-side/models/sport/baseball/clipping/event_clipping/2025-04-23-pitch-detection_diamond_roi_1000ms_16_frames_model.pth
2025-07-31 17:20:42        534 server-side/models/sport/baseball/clipping/event_clipping/model.json
2025-07-31 17:21:06    6549796 server-side/models/sport/baseball/clipping/event_clipping/yolov8n.pt
root@0ce9c5d0a213:/opt/code/localstack# 
```

The problem is that they are in different containers, the backend container cannot get models from local stack container, therefore it cannot get correct results from the backend api. 

correct me if i am wrong. give me suggestions on how to get the models (pt files) if they are inside another container. only describe the suggestions in text, do not change any code. 

Cursor responds with:

> 1. Docker Network Solution (Recommended):
- Create a custom Docker network
- Connect both containers to this network
- Instead of using localhost:4566 in the Flask container, use localstack:4566 (container name as hostname)
- This is the cleanest solution as it follows Docker networking best practices

> 2. Docker Compose Solution:
- Move the Flask API configuration into the same docker-compose.yml file
- Docker Compose automatically creates a network for services defined in the same file
- This provides better maintainability and ensures consistent networking

**Solution:**
- Added the flask-api container to docker-compose.yml
- Changed the `localhost` into the IP address `127.0.0.1`

```bash
curl -X POST http://127.0.0.1:5000/run-detect \
  -F "file=@backend/tests/test_img1.jpeg" \
  -F "model_id=player_detector_v8n"
```
