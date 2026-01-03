
# 📈 Retail Sales Optimization Engine

### 📊 **High-Velocity Peak Detection using Two-Pointer Intelligence**

![Industry Banner](https://img.shields.io/badge/Industry-Retail-blue)
![Python Version](https://img.shields.io/badge/Python-3.8%2B-green)
![Library](https://img.shields.io/badge/Library-Pandas-orange)
![Source](https://img.shields.io/badge/Source-Kaggle-blue)

---

## 📌 Business Problem
In **high-volume e-commerce** environments, identifying **"Sales Bursts"** is critical for **inventory management** and detecting **high-velocity buying patterns** (e.g., viral trends or bot activity). 

The goal of this project is to scan **540,000+ transactions** to identify the exact **time window** where a specific product reached its **highest sales density** without exceeding a predefined **operational limit**. Failing to identify these windows leads to:

* **Inventory Stockouts:** Rapid demand spikes can leave shelves empty if not detected in real-time.
* **Missed Revenue:** Inability to pinpoint the "Golden Window" of peak demand prevents targeted marketing.
* **Operational Overload:** Processing millions of transactions with inefficient code stalls decision-making.

## 💡 The Solution: Two-Pointer Aggregation
Traditional **nested loops** would result in **$O(n^2)$ complexity**, making analysis slow and resource-heavy. This implementation utilizes a **Two-Pointer/Sliding Window** approach to find the **optimal contiguous sequence** in a **single pass**.



## ⚙️ Technical Optimization
* **Time Complexity:** **$O(n)$** — Scans the dataset in a **single pass**.
* **Space Complexity:** **$O(k)$** — Where $k$ is the size of the **active window**.
* **Efficiency:** Uses **vectorized operations** to ensure high performance on **large-scale CSV files**.

## 📂 Dataset
This analysis uses the **Store Sales Dataset** from Kaggle.
* **Download Method:** Automated via **`kagglehub`**.
* **Data Points:** **Sales revenue**, **Timestamp**, and **Item metadata**.

## 🛠️ Implementation

```python
import pandas as pd
import kagglehub
import os

# 1. Automated Data Acquisition
# Downloads the latest version of the Store Sales dataset
path = kagglehub.dataset_download("abhishekjaiswal4896/store-sales-dataset")
csv_path = os.path.join(path, "store_sales.csv")

# 2. Data Loading & Pre-processing
df = pd.read_csv(csv_path)
arr = df['sales'].tolist()
max_sum = 5000 

# 3. Two-Pointer / Sliding Window Logic
def find_best_window(arr, max_sum):
    left = 0
    current_sum = 0
    best = 0
    best_nums = []
    
    # Expand the right pointer to include new data
    for right in range(len(arr)):
        current_sum += arr[right]
        
        # Shrink the left pointer if the sum exceeds our limit
        while current_sum > max_sum:
            current_sum -= arr[left]
            left += 1
        
        # Update the best window found so far
        if current_sum > best:
            best = current_sum
            best_nums = arr[left:right+1]
    
    return best, best_nums

# Execute Optimization
best_sum, numbers = find_best_window(arr, max_sum)

# 4. Results Output
print(f"--- PEAK SALES WINDOW DISCOVERED ---")
print(f"Maximum Sum: {best_sum}")
print(f"Items in Window: {len(numbers)}")
print(f"Sequence: {'+'.join(str(x) for x in numbers)}")
