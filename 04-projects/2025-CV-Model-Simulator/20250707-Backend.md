---
created: 2025-07-07
tags: 
org:
  - GC
goal: Implement Backend API of the app
---
Reference: 
- [[20250702-Design Doc|Design Doc]]

# Version 1
## **POST /api/run**

- Input - Request body: `{file: File, model_id: 'yolov8n'}`
- Get the file and model id
- Looks up `model.json` to find the file path which belongs to the model id
- Loads the .pt file for that model
- Gets the image/video from the file path
- Runs the inference
- Return results as JSON

Now, update the run-detect logic according to this. and add a model.json file to store the model id and model path pair. 

## **GET /api/models**

- Returns a list of models in json format
- .json file should include: id, model name, path

## **POST /api/models**

- (User uploads a new `.pt` file)
- Save the file to a path (backend/app/models/{model-name})
- Update models.json (id, model name, path)

## **DELETE /api/models/:id**

- Update models.json


Should have two blueprints:

- model
- inference

# Version 2

