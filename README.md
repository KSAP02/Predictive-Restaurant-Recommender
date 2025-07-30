# Customer_Vendor Prediction System

## 🧠 Problem Statement
This project aims to predict which vendor a customer is likely to place an order with, using available customer and location metadata. The prediction is made for each possible (customer, location, vendor) combination.

---

## 📁 Dataset Description

### 🔹 Train Data Components:
- `train_customers.csv`: customer_id, gender, age, location_number
- `train_locations.csv`: location_number, location_type, coordinates
- `orders.csv`: historical order data (customer_id, vendor_id)
- `vendors.csv` : vendor data (vendor_id, location data)

### 🔹 Test Data Components:
- `test_customers.csv`: similar to train_customers.csv
- `test_locations.csv`: similar to train_locations.csv
- No vendor or target is provided.

---

## ⚙️ Project Workflow

### 1. **Data Construction**
- Merged all the given train data to understand customer metadata.

### 2. **Feature Engineering**
- Selected features: `gender`, `location_type`, `latitude`, `longitude`
- Dropped `age` due to excessive missing values.
- Applied **Label Encoding** on categorical features (`gender`, `location_type`)

### 3. **Negative Sampling**
- Identified all the unique vendors first
- Performed a **cross-join** of all (customer, location) pairs with all vendors to create a complete training set (`cust_loc_vendor`).
- Labeled each row with the `target` with 0(didn't order) or 1(ordered) by checking if that customer ordered from that vendor.

### 4. **Model Training**
- Trained an **XGBoost Classifier** on the full dataset with encoded features and target.
- Evaluated using standard metrics (accuracy, F1-score on validation set).

### 5. **Test Data Preparation**
- Merged test customers with test locations.
- Cross-joined with **all vendor IDs** seen during training.
- Encoded using the **same LabelEncoders** as train set.
- Fed the resulting dataset into the trained model to get predictions.

### 6. **Prediction Formatting**
- Final output is formatted as:

---

## 📦 Output Format

| (CID X LOC_NUM X VENDOR) | target |
|--------------------------|--------|
| Z59FTQD X 0 X 243        | 0      |
| ...                      | ...    |

---

## 🛠️ Tech Stack
- Python, Pandas, NumPy
- Scikit-learn for encoding
- XGBoost for modeling

---

## ✅ Key Highlights
- Every row in final training and test dataset represents a unique (customer, customer location, vendor) combination.
- Smart target construction from raw orders data.
- Robust pipeline for encoding, cross-joining, and prediction.
