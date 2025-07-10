
# Real-Time Urban Parking Pricing Engine

A smart, real-time pricing system for 14 urban parking spaces built using **Pathway**, **Pandas**, and **Bokeh**. It dynamically adjusts parking fees based on demand, traffic, and real-world conditions — improving utilization and reducing congestion.

---

## Overview

In cities, fixed parking prices cause inefficiencies — either **overcrowding** or **underutilization**. This project introduces two models for **dynamic pricing**:

* **Model 1**: Linear pricing based on current occupancy.
* **Model 2**: Demand-based pricing using multiple real-time signals.

The system ingests **live data streams**, processes pricing decisions **on the fly**, and visualizes pricing changes **in real time**.

---

## ⚙️ Tech Stack

| Category       | Stack / Tools                                                   |
| -------------- | --------------------------------------------------------------- |
| Stream Engine  | [Pathway](https://pathway.com)                                  |
| Plotting       | [Bokeh](https://bokeh.org) + [Panel](https://panel.holoviz.org) |
| Data Handling  | Numpy, Pandas                                                   |
| Time Windows   | Tumbling Window (30-min)                                        |
| UI Environment | Google Colab                                         |

---

## Architecture Diagram

```mermaid
graph TD
    A[CSV Stream via Pathway] --> B[Timestamp Parsing]
    B --> C[Occupancy, Queue, Traffic Extraction]
    C --> D[30-min Tumbling Window]
    D --> E1[Model 1: Linear Occupancy-Based Pricing]
    D --> E2[Model 2: Demand-Based Pricing (Multi-feature)]
    E1 --> F1[Pricing Table (Model 1)]
    E2 --> F2[Pricing Table (Model 2)]
    F1 --> G[Real-Time Bokeh Plot]
    F2 --> G
```

---

## Model Details

### Model 1: Baseline Linear Model

**Logic:**

```python
price = base_price + α × (occupancy / capacity)
```

* Increases linearly as occupancy rises.
* Resets every 30-min tumbling window.
* Simple and interpretable.

**Visualized** using Bokeh's streaming plot.

---

### Model 2: Demand-Based Pricing

**Logic:**

```python
norm_demand = (
    occupancy / capacity
    + 0.3 × queue_length
    - 0.2 × traffic
    + 0.4 × is_special_day
    + 0.3 × vehicle_type_weight
)
price = base_price × clip(norm_demand, 0, 2)
```

* Multi-signal demand estimator.
* Pricing reflects real-world features:

  * Holiday traffic
  * Truck vs. car vs. bike
  * Real-time congestion
* Smoother and more fair than static pricing.

**Streamed Plot** updates every second with new prices per lot.

---



## References


* [Pathway Real-Time Windowing](https://pathway.com/developers/user-guide/windowing/)



