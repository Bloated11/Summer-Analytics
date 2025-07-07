# link to colab notebook https://colab.research.google.com/drive/1rlS6YSIBj8DHIeltpSJqKzFFyG5vzrE0?usp=sharing
# 🚗 Dynamic Parking Lot Pricing System

A data-driven simulation that models and visualizes daily dynamic pricing for urban parking lots. It processes real-world parking data (occupancy, queue, traffic, vehicle types) to compute optimal pricing using demand models.

This project includes:
- 🧠 Demand-based dynamic pricing algorithms
- 📈 Interactive daily pricing visualization using Bokeh
- 🧹 Data preprocessing and feature engineering
- 🧪 Designed to run offline with Pandas

---

## ⚙️ Tech Stack

| Tool         | Purpose                                 |
|--------------|------------------------------------------|
| **Pandas**   | Data processing and aggregation          |
| **Bokeh**    | Interactive visualization (plotting)     |
| **Jupyter**  | Development & notebook execution         |
| **Mermaid**  | Architecture diagram in markdown         |
| **Python 3** | Programming language                     |

---

## 🏗 Architecture Diagram

```
flowchart TD
  A[dataset.csv] --> B[Preprocessing (Pandas)]
  B --> C[Daily Aggregation by Lot + Date]
  C --> D[Demand-Based Pricing Model]
  D --> E[Dynamic Price Calculation]
  E --> F[Result DataFrame]
  F --> G[Bokeh Plot: Price Over Time]
```

---

## 🔄 Workflow & Architecture

### 1. **Dataset Ingestion**
- The dataset includes parking lot status with timestamps split across `LastUpdatedDate` and `LastUpdatedTime`.
- Features include `Occupancy`, `Capacity`, `QueueLength`, `TrafficConditionNearby`, `VehicleType`, and `IsSpecialDay`.

### 2. **Preprocessing**
- Combines date & time into a proper `timestamp`.
- Encodes:
  - `TrafficConditionNearby` → numerical scale (`low=1`, `medium=2`, `high=3`)
  - `VehicleType` → weight scores (`bike=0.5`, `car=1.0`, `truck=1.5`)
- Extracts daily grouping variable.

### 3. **Aggregation**
- Groups data by `SystemCodeNumber` and `date`.
- Computes daily averages for occupancy, queue, traffic, vehicle weight.

### 4. **Dynamic Pricing Formula**
```python
demand = (
    α * (occupancy / capacity)
    + β * queue_length
    - γ * traffic_level
    + δ * is_special_day
    + ε * vehicle_weight
)

normalized_demand = (demand - min_d) / (max_d - min_d)

price = base_price * (1 + λ * normalized_demand)
```

- Demand values are normalized across all lots.
- Final prices are clipped to a range between **$5 and $20**.

### 5. **Visualization**
- Each parking lot is plotted with a time series of daily prices.
- Hover tooltips display:
  - Lot ID
  - Date
  - Exact price

---

## 📂 Project Structure

```text
📦 dynamic-parking-pricing
├── dataset.csv                 # Raw input data
├── dynamic_pricing_park.ipynb  # Jupyter Notebook (uploaded)
├── README.md                   # You're here
```

---

## 🚀 Getting Started

### ✅ Requirements

Install required Python packages:

```bash
pip install pandas bokeh
```

### ▶ Run Locally

1. Clone the repo or download the notebook
2. Open `dynamic_pricing_park.ipynb`
3. Upload your `dataset.csv` or use the provided one
4. Run all cells to:
   - Preprocess data
   - Compute dynamic prices
   - Visualize with Bokeh

---

## 📌 Notes

- The notebook runs fully offline (no real-time streaming engine).
- Designed for performance, interpretability, and easy extensibility.
- You can adapt the pricing formula to support external data or predictive models.

