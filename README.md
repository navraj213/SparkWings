# ✈️ SparkWings: Predicting Airline Meal Demand & Cargo Efficiency with Machine Learning

> A project by Benoy Thomas, Navraj Singh, Luis Jaco  
> Developed using Apache Spark on Hadoop for Virgin Atlantic JFK operations

---

## 📌 Overview

**SparkWings** is a machine learning pipeline built to predict final passenger counts for each class (Upper, Premium, Economy) on Virgin Atlantic flights. Accurate predictions help:

- Optimize the number of meals loaded onboard
- Reduce food waste and operational costs
- Improve cargo space utilization
- Lower the aircraft's carbon footprint

The project is powered by real-world operational logs and weather data, processed at scale with Apache Spark.

---

## 🚨 The Problem

Airlines struggle to accurately estimate the number of meals to load:

- **Overestimating** → food waste, weight penalties, and higher emissions
- **Underestimating** → dissatisfied passengers and operational risk

Additionally, overestimating bag and passenger load reduces cargo allocation, directly impacting freight revenue and emissions efficiency.

---

## 🎯 Goal

Build a machine learning model that predicts **final passenger count** across all three cabins to:

- Right-size meal orders (buffered)
- Free up space for extra cargo
- Decrease CO₂ emissions per passenger
- Help Ops Agents and Turnaround Coordinators make informed decisions

---

## 🔍 Data Pipeline

### ✈️ Flight Dataset

- Daily operational logs for JFK Terminal 4 Virgin Atlantic flights (2012–Present)
- Focused on **Flight VS4 (JFK → LHR, 18:00 departure)** for data consistency

#### Captured Data:
- Flight Info: Date, Flight No., Aircraft Reg, Gate, Times
- Pax Info: Final counts by class, gender, infants, PADs
- Ops Info: Wheelchair requests, baggage delivery, cargo loads
- Meal counts by class and booking info
- Aircraft capacity (Seats J/W/Y)

### 🌦️ Weather Dataset

- Integrated via Visual Crossing API
- Includes: Temperature, precipitation, wind speed, snow, cloud cover, and visibility

---

## 🧼 Data Cleaning & Preprocessing

- Dropped rows with critical missing fields (e.g., arrival time, registration)
- Standardized time and label formats
- Validated aircraft registrations and chronology of event logs
- Merged and validated weather data

---

## ⚙️ Model Architecture

Built using `Apache Spark 3.4.2` on `Hadoop 3`. Core components include:

- **StringIndexer** for aircraft registration
- **Imputer** for missing values
- **VectorAssembler** to compile features
- **StandardScaler** for normalization
- **GBTRegressor** for final predictions (one per class)

### 🎯 Feature Columns
```python
[
  "month", "day", "year", "day_of_week",
  "seats_j", "seats_w", "seats_y", "temp",
  "humidity", "percip", "snow", "wind_speed",
  "cloud", "visibility", "wheelchair",
  "booked_j", "booked_w", "booked_y"
]
```

## 🍽️ Practical Impact

### Meal Waste Reduction (with buffers)

- **Economy:** 80% improvement (8.86 → 1.84 meals wasted)
- **Premium:** 18% improvement
- **Upper:** ~113% worse (requires model tuning or fallback to manual estimate)

### Cargo Efficiency

- The A330-900neo has approximately **3× unused cargo space**
- By adjusting estimates and loading 3× cargo:
  - **Average cargo increased by 13%**
  - **Carbon emissions per passenger decreased by:**
    - **Economy:** 343.8 → 299.1 kg CO₂e
    - **Premium:** 945.7 → 822.8 kg CO₂e
    - **Upper:** 1719 → 1495.5 kg CO₂e

