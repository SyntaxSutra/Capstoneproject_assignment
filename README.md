# Capstoneproject_assignment
# Dynamic Pricing for Urban Parking Lots

**Capstone Project – Summer Analytics 2025**  
Hosted by: Consulting & Analytics Club × Pathway

---

## 🚦 Problem Statement

Urban parking lots often suffer from inefficient pricing—either being underutilized or overcrowded. This project introduces a **dynamic pricing engine** for 14 parking spaces, adjusting prices in real-time based on:
- Occupancy
- Queue length
- Traffic congestion
- Vehicle type
- Special events
- Competitor pricing

---

## 🎯 Objective

To build a real-time simulation system using:
- 📈 Data-driven pricing models (built from scratch)
- 🔄 Real-time streaming with [Pathway](https://pathway.com)
- 📊 Visualization using Bokeh

---
# Dynamic Pricing Models for Urban Parking Lots

This directory contains all three pricing models implemented as part of the **Summer Analytics 2025 Capstone Project**. Each model introduces increasing levels of sophistication for pricing urban parking spaces dynamically based on real-time demand, congestion, and competition.

---

## 📌 Overview of Models

| Model # | Name                       | Core Logic                                  | Inputs Used                              | Realism |
|--------:|----------------------------|----------------------------------------------|-------------------------------------------|---------|
| 1       | Linear Occupancy Model     | Price ∝ Occupancy                           | Occupancy, Capacity                       | 🔰 Simple |
| 2       | Demand-Based Model         | Price ∝ Composite Demand Score              | Occupancy, Traffic, Queue, Special Day    | ⚙️ Moderate |
| 3       | Competitive Pricing Model  | Price ∝ Demand + Nearby Competitor Prices   | Geolocation, Nearby Prices, All from M2   | 🚀 Advanced |

---

## ⚙️ Model 1: Baseline Linear Model

### 📘 Description
This is a simple model where the price increases linearly with occupancy rate.

### 🧾 Formula
Price = Base_Price + α × (Occupancy / Capacity)


### 🧠 Assumptions
- Linear demand response to occupancy.
- Ignores other variables (queue, traffic, etc.).
- Used as a reference or baseline model.

### 🔧 Parameters
- `Base_Price = $10`
- `α = 5` (adjustable sensitivity)

---

## 📊 Model 2: Demand-Based Pricing Model

### 📘 Description
This model incorporates multiple features affecting demand and calculates a **normalized demand score** to adjust the price.

### 🧾 Demand Function
Demand = α1 × (Occupancy / Capacity)
+ α2 × QueueLength
- α3 × TrafficLevel
+ α4 × IsSpecialDay
+ α5 × VehicleTypeWeight


### 🧾 Price Formula
Price = Base_Price × (1 + λ × Normalized_Demand)


### 🧠 Assumptions
- Composite features better reflect true demand.
- Demand is normalized to avoid erratic pricing.
- Smooth price transitions between time intervals.

### 🔧 Parameters
- `Base_Price = $10`
- `λ = 0.5` (controls sensitivity to demand)
- Price capped between `$5` and `$20` (0.5x to 2x)

---

## 🌐 Model 3: Competitive Pricing Model

### 📘 Description
This model adds location intelligence and competition-aware logic using geospatial proximity of parking lots.

### 📏 Competitive Logic
- If **lot is full** and nearby **cheaper alternatives exist**: lower price or suggest rerouting.
- If **nearby lots are expensive**: allow price increase.
- Competitor prices considered only within a 500-meter radius.

### 🧾 Algorithm
- Compute distances using latitude/longitude (via `geopy`)
- Retrieve competitor prices from Model 2
- Adjust price accordingly:
  - Increase if demand is high and you're cheaper than others.
  - Decrease if you're expensive and others have space.

### 🧠 Assumptions
- Drivers are price-sensitive and may reroute to cheaper options.
- Distance within 500m is actionable for rerouting.
- Leverages Model 2's smart demand estimation.

