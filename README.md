# The-Dream-Team

Project about leveraging AI for team building.

---

## Setup Instructions

## Local Development

Follow these steps to set up the project locally:

1. **Clone the repository**

   ```bash
   git clone git@github.com:TDT4D/The-Dream-Team.git
   cd The-Dream-Team
   ```

2. **Install DVC (Data Version Control)**

   Requires python.

   ```bash
   pip install 'dvc[gdrive]'
   ```

3. **Pull required data and models**

   You need to be granted access to the dvc storage.

   ```bash
   dvc pull
   ```

4. **Install Docker (if not already installed)**

   - [Install Docker](https://docs.docker.com/get-docker/)

5. **Start the services**

   This could take several minutes on first go.

   ```bash
   docker-compose -f docker-compose.yml up --build
   ```

6. **Access the services locally**
   - Frontend: http://localhost:3000
   - Backend: http://localhost/api
   - ML Service: http://localhost/ml
   - API Docs (Java backend): http://localhost/docs/index.html
   - ML Docs (FastAPI): http://localhost/ml/docs
     - Direct access incase above doesn't work: http://localhost:9696/docs

---

## Deployment

To access the deployed version:

1. Ensure the instance is running and the correct DNS entries point to it.
2. Ensure the project is up and running
   ```bash
   docker-compose -f docker-compose.yml -f docker-compose.deploy.yml up --build
   ```
3. Navigate to the appropriate subdomains:
   - Frontend: https://the-dream-team.demola.fi
   - Backend: https://backend.the-dream-team.demola.fi
   - API Docs: https://apidoc.the-dream-team.demola.fi
   - ML Service: https://ml.the-dream-team.demola.fi
   - ML Docs: https://mldoc.the-dream-team.demola.fi

---

## Initializing the ML System

To initialize the system from raw data with scoring:

### Option A: Use the backend endpoint

Send a POST request to:

```
POST http://localhost/api/initWithMotivation
```

This will:

1. Clean main dataset
2. Clean motivation data
3. Train default model
4. Train motivation model
5. Generate predictions for both

### Option B: Manual step-by-step via ML service

Use the following ML service endpoints in this order:

1. **Clean data**

   ```bash
   POST http://localhost/ml/data/clean?cleaner=data_cleaning_version4&saveFile=clean_default
   ```

2. **Clean motivation data**

   ```bash
   POST http://localhost/ml/data/clean?cleaner=motivation_data_cleaning_version2&saveFile=motivation_default
   ```

3. **Train default model**

   ```bash
   POST http://localhost/ml/training/train?modelType=stacking_model&modelName=stacking_model_default&data=clean_default
   ```

4. **Train motivation model**

   ```bash
   POST http://localhost/ml/training/train?modelType=stacking_model&modelName=stacking_model_motivation&data=motivation_default
   ```

5. **Generate predictions**

   ```bash
   POST http://localhost/ml/score/predict?modelType=stacking_model&modelName=stacking_model_default&data=clean_default&saveFile=score_default
   ```

6. **Generate motivation predictions**
   ```bash
   POST http://localhost/ml/score/predict?modelType=stacking_model&modelName=stacking_model_motivation&data=motivation_default&saveFile=motivation_score
   ```

Once complete, results will be available in the storage path configured within the ML service.
