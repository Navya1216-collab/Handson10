# 🚖 Hands-On L10: Spark Structured Streaming with MLlib

## 🧠 Overview
This assignment showcases how **Apache Spark Structured Streaming** and **Spark MLlib** can be used together to perform **real-time predictive analytics** on ride-sharing data.  
It focuses on two core tasks:

- **Task 4 – Real-Time Fare Prediction:** Building and applying a regression model that predicts trip fares from ride distance in a live stream.  
- **Task 5 – Time-Based Fare Trend Forecasting:** Creating a time-windowed regression model that tracks and forecasts the average fare across consecutive 5-minute periods.

The live input data is continuously streamed through a Python socket generator that simulates real-time ride events.

---

## 🗂 Repository Structure
```
Handson-L10/
├── data_generator.py
├── task4.py
├── task5.py
├── training-dataset.csv
├── models/
│   ├── fare_model/
│   └── fare_trend_model_v2/
├── outputs_task4.txt
├── outputs_task5.txt
└── README.md
```

---

## ⚙️ Environment Setup

### Step 1: Install Dependencies
```bash
pip install pyspark faker
```

### Step 2: Start the Data Stream Generator  
Run the generator in **Terminal 1**:
```bash
python3 data_generator.py
```

Expected terminal output:
```text
Streaming data to localhost:9999...
Sent: {"trip_id":"...","driver_id":15,"distance_km":18.6,"fare_amount":51.75,"timestamp":"2025-10-21 17:30:00"}
```
This indicates that the generator is sending continuous JSON ride events to the socket on port 9999.

---

## 🚗 Task 4 – Real-Time Fare Prediction

### 🎯 Objective
Train a **Linear Regression** model that learns the relationship between trip distance and fare amount, then apply it to streaming ride data for instant predictions.

### 🔧 Model Training
1. Load `training-dataset.csv`.  
2. Convert numerical columns to the correct types.  
3. Build feature vectors using `VectorAssembler(inputCols=["distance_km"])`.  
4. Fit a `LinearRegression` model using MLlib.  
5. Save the trained pipeline under `models/fare_model/`.

### ⚡ Streaming Inference
- Parse incoming JSON records from the socket.  
- Extract numeric features for prediction.  
- Use the trained model to estimate fares in real time.  
- Compute the absolute deviation between actual and predicted values.  
- Continuously display the results on the console.

### ▶️ How to Run
Keep the generator active, open **Terminal 2**, and execute:
```bash
python3 task4.py | tee outputs_task4.txt
```

#### Example Output
```
trip_id | driver_id | distance_km | fare_amount | predicted_fare | deviation
---------------------------------------------------------------------------
6ac4aefb-6a35-4f5a-a8a7-3c4da90912c9 | 11 | 12.45 | 35.8 | 37.9 | 2.1
b02fa55f-af38-4c48-b7dc-b32f2fa20eb0 | 34 | 21.73 | 59.7 | 63.5 | 3.8
```

---

## ⏱ Task 5 – Time-Based Fare Trend Prediction

### 🎯 Objective
Analyze and forecast average fare behavior over time by performing **5-minute window aggregations** and training a regression model with time-related features.

### 🔧 Model Training
1. Group historical fare data into fixed 5-minute windows.  
2. Compute the mean fare (`avg_fare`) for each window.  
3. Derive new features such as `hour_of_day` and `minute_of_hour`.  
4. Train a regression model to predict the next window’s average fare.  
5. Save it as `models/fare_trend_model_v2/`.

### ⚡ Streaming Inference
- Read live data from the same socket feed.  
- Perform sliding 5-minute window aggregations that update every minute.  
- Apply the regression model to predict average fare trends in near real time.

### ▶️ How to Run
Open **Terminal 3** and execute:
```bash
python3 task5.py | tee outputs_task5.txt
```

#### Example Output
```
window_start          | window_end            | avg_fare | predicted_next_avg_fare
-------------------------------------------------------------------------------
2025-10-21 21:29:00   | 2025-10-21 21:34:00   | 75.62    | 88.15
2025-10-21 21:30:00   | 2025-10-21 21:35:00   | 78.04    | 90.02
```

---

## 🧩 Core Concepts

| Concept | Description |
|----------|-------------|
| **Structured Streaming** | Provides continuous data ingestion and processing from the socket source. |
| **VectorAssembler** | Combines numeric input columns into MLlib feature vectors. |
| **LinearRegression (MLlib)** | Builds regression models for both fare prediction and trend estimation. |
| **Window Aggregations** | Enables time-based grouping of streaming data for analyzing temporal trends. |
| **Pipeline Consistency** | Ensures identical preprocessing during training and streaming inference. |

---

## 📸 Screenshots
- **Screenshot 1:** Task 4 – Real-Time Fare Prediction  
- **Screenshot 2:** Task 5 – Time-Based Fare Trend Forecast  

---

## ✅ Submission Checklist
- [x] `data_generator.py` – streams JSON ride data  
- [x] `task4.py` – trains & performs real-time fare predictions  
- [x] `task5.py` – predicts 5-minute fare trends  
- [x] Models created (`fare_model`, `fare_trend_model_v2`)  
- [x] Output logs: `outputs_task4.txt`, `outputs_task5.txt`  
- [x] Screenshots added  
- [x] README documented  

---

## 🏁 Results & Insights
- **Task 4:** Successfully streamed real-time predictions with calculated deviations between actual and estimated fares.  
- **Task 5:** Accurately identified short-term average-fare patterns across overlapping 5-minute intervals.  

✅ This assignment demonstrates a **complete end-to-end streaming machine-learning workflow** in Apache Spark—from data ingestion and model training to live inference and trend forecasting.
