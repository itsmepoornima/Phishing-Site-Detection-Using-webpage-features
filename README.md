# 🛡️ Phishing Website Detection System (Webpage Feature-Based)
A real-time phishing detection system that uses only webpage features for prediction. A browser extension extracts key attributes and sends them to a backend ensemble model (RF, XGBoost, MLP and LightGBM) using soft voting to classify sites as phishing or legitimate.


---

## 🔍 Overview

- **Frontend:** A Chrome extension extracts webpage features in real time.
- **Backend:** A FastAPI server receives feature data and returns phishing/legitimate predictions.
- **Models Used:** Random Forest, XGBoost, MLP, LightGBM.
- **Features Used:** Includes visual features (iframe, anchor tags), network info (domain age, registration), and behavior indicators (redirection, login form, etc.).

---

## 🗂️ Project Structure
Phish Detection Extended/
├── backend/
│ ├── features/
│ │ ├── extractor.py # Feature extraction logic
│ │ ├── test.py # Sample tests
│ ├── models/
│ │ ├── *.pkl, *.json # Trained models and scaler
│ ├── prediction.py # Ensemble soft-voting logic
│ └── server.py # FastAPI server
├── extension/
│ ├── popup.html # Extension UI
│ ├── popup.js # Sends features to backend
│ ├── manifest.json # Chrome extension config
│ └── icon.png # Extension icon



## 🚀 How to Run

### 1. Clone the Repository


git clone https://github.com/yourusername/phishing-detection-system.git
cd phishing-detection-system
2. Setup Backend
Create and activate a virtual environment:

python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
Install requirements:

pip install -r requirements.txt
Run FastAPI server:

cd backend
uvicorn server:app --reload
The server will be available at http://127.0.0.1:8000.

3. Load Extension in Chrome
Open chrome://extensions/

Enable Developer Mode

Click Load Unpacked

Select the extension/ folder

Now the extension will appear in your browser toolbar.

📦 API Endpoint
POST /predict

bash
Copy
Edit
POST http://127.0.0.1:8000/predict
Content-Type: application/json
Example payload:
json
Copy
Edit
{
  "login_form": 0,
  "links_in_tags": 8.9,
  "iframe": 1,
  "right_click": 0,
  "empty_title": 0,
  "safe_anchor": 0.0,
  "ratio_intHyperlinks": 0.625,
  "ratio_extHyperlinks": 0.375,
  "nb_extCSS": 2,
  "nb_redirection": 0,
  "domain_registration_length": -1,
  "domain_age": -1,
  "web_traffic": 40459,
  "google_index": 0,
  "page_rank": 4,
  "brand_in_path": 0,
  "suspecious_tld": 0
}
Example response:
json
Copy
Edit
{
  "prediction": "Legitimate",
  "confidence": 0.9426,
  "features": {...},
  "model_probs": {
    "xgboost": [[0.03, 0.97]],
    "random_forest": [[0.09, 0.91]],
    "mlp": [[0.06, 0.94]],
    "lightgbm": [[0.04, 0.96]]
  }
}
