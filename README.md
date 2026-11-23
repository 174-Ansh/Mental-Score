# Mental Score

Mental Score is a student-wellness demo that estimates a mental-health score from lifestyle and social-media habits. It pairs a responsive web form with a FastAPI service and a pre-trained regression model.

> **Important:** This project is for educational and informational use only. Its score is not a diagnosis or a substitute for professional care.

## Features

- Collects profile, academic, social-media, sleep, activity, and stress information.
- Validates form values in the browser and in the API.
- Displays a predicted score with a visual gauge and a plain-language band.
- Includes the model-training notebook and the dataset used by the project.

## Project files

| File | Purpose |
| --- | --- |
| `index.html`, `style.css`, `script.js` | Responsive frontend and form behavior |
| `main.py` | FastAPI application and `/predict` endpoint |
| `Mental_Health_Model.pkl` | Pre-trained scikit-learn regression pipeline |
| `Student Social Media And Mental Health Impact.csv` | Dataset used for model exploration and training |
| `ML_Project.ipynb` | Data analysis and model-training notebook |

## Run locally

You need Python and pip. From the project directory, create and activate a virtual environment, then install the backend packages:

```shell
python -m venv venv
```

Windows PowerShell:

```powershell
.\venv\Scripts\Activate.ps1
```

macOS or Linux:

```shell
source venv/bin/activate
```

Install dependencies:

```shell
python -m pip install fastapi "uvicorn[standard]" pandas scikit-learn joblib
```

Start the API in one terminal:

```shell
python -m uvicorn main:app --reload --host 127.0.0.1 --port 2200
```

Start the frontend from the project directory in a second terminal:

```shell
python -m http.server 5500 --bind 127.0.0.1
```

Open [http://127.0.0.1:5500](http://127.0.0.1:5500). When the page is served from `localhost` or `127.0.0.1`, it sends prediction requests to the local API at port `2200`. The API's interactive documentation is available at [http://127.0.0.1:2200/docs](http://127.0.0.1:2200/docs).

The model is loaded when the API starts. Because it is serialized with joblib, use a scikit-learn version compatible with the one used to create the model; the notebook contains the training workflow.

## Deploy to Render

Use the repository's `render.yaml` as the Render Blueprint. Alternatively, configure a Render Web Service with:

- Build command: `pip install -r requirements.txt`
- Start command: `uvicorn main:app --host 0.0.0.0 --port $PORT`

Do not include a space between `--` and `host` (`-- host`). The start command must use `--host`.

## Prediction API

`POST /predict` accepts JSON with the form fields below:

```json
{
  "age": 21,
  "gender": "Female",
  "country": "India",
  "academic_level": "Undergraduate",
  "most_used_platform": "Instagram",
  "purpose_of_use": "Education",
  "avg_daily_usage_hours": 4.5,
  "daily_unlocks": 100,
  "study_hours": 4,
  "physical_activity_hours": 1.5,
  "sleep_hours_per_night": 7,
  "stress_level": "Medium"
}
```

Successful responses contain the predicted value:

```json
{
  "predicted_mental_health_score": 6.8
}
```

The API validates categorical values and numeric ranges; invalid requests receive a validation error response.
