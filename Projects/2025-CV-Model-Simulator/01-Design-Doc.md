---
created: 2025-07-02
tags:
  - "#software-architecture"
  - "#frontend"
  - "#backend"
  - database
org:
  - GC
goal: Overall design doc of the project
---
# Frontend
- More Detailed Plans: [[20250706-Frontend]]
### **User Flow - Upload new model**

1. Clicks ‘Upload Model’, select a .pt file
2. If success, default select ‘Run Inference’
3. A pop-up window shows up for users to upload image/video
4. Displays results of the image

### **User Flow - Run existing model**

1. User sees the models list.
2. User clicks on the model name to select this model
3. Clicks ‘Run Inference’
4. A pop-up window shows up for users to upload image/video
5. Displays results of the image

### **Component Tree**

![[Screenshot 2025-07-02 at 10.54.04 AM.png]]
### **HTTP Request**

1. Upload model
    1. form-data
        1. .pt file
        2. name
2. Run inference
    1. form data
        1. event_id
        2. model id
        3. temp path to file (image/video)

# Backend

## API Endpoints

1. Clicks ‘Upload Model’, select a .pt file
    1. **POST /api/models**
2. User sees their custom model in the models list.
    1. **GET /api/models**
3. User clicks on the model name to select this model
    1. Frontend just stores the selected model id
4. Clicks ‘Run Inference’
    1. **POST /api/run**
5. Displays results of the image

**GET /api/models**

- Returns a list of models in json format
- .json file should include: id, model name, path

**POST /api/models**

- (User uploads a new `.pt` file)
- Save the file to a path (backend/app/models/{model-name})
- Update models.json (id, model name, path)

**DELETE /api/models/:id**

- Update models.json

**POST /api/run**

- Request body:
    - event_id
    - model id
    - temp path to file (image/video)
- Loads the .pt file for that model
- Gets the file
- Runs the inference
- ~~(Stores the results in DB)~~
- Return results as JSON

Should have two blueprints:

- model
- inference

# Database

Fields:

- event_id (PK)
- model_id
- results
- input_file
- timestamp

Deployment

# Reference

previous proposal: [CV Playground Backend](https://www.notion.so/CV-Playground-Backend-21d2db43956f80f4ad3aecb8172ce106?pvs=21)