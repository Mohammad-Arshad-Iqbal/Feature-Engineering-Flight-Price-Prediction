# Flight Price Prediction - Feature Engineering and Data Preprocessing

## 📄 Dataset Overview

The dataset `flight_price.xlsx` contains the following features:

| Feature           | Description |
|------------------|-------------|
| **Airline**         | The name of the airline company |
| **Date_of_Journey** | The date of the journey (DD/MM/YYYY format) |
| **Source**          | The city from which the flight departs |
| **Destination**     | The city to which the flight arrives |
| **Route**           | The route taken by the flight |
| **Dep_Time**        | The time at which the flight departs |
| **Arrival_Time**    | The time at which the flight arrives at the destination |
| **Duration**        | Total time taken by the flight to reach the destination |
| **Total_Stops**     | Number of stops (e.g., "non-stop", "1 stop", etc.) |
| **Additional_Info** | Additional information (e.g., "No info", "In-flight meal not included", etc.) |
| **Price**           | Target variable (cost of the ticket) |

---
## 📊 Dataset Snapshot (Before Feature Engineering)

The dataset before applying feature engineering looks like this:

![Dataset Screenshot](images/before_feature_engineering.png)

> *Note: This screenshot was taken before any preprocessing or feature transformations.*

## 🔧 Feature Engineering

### ✅ Date of Journey
- Extracted day, month, and year from `Date_of_Journey`:
  ```python
  df["Date of Journey"] = df["Date_of_Journey"].str.split("/").str[0]
  df["Month of Journey"] = df["Date_of_Journey"].str.split("/").str[1]
  df["Year of Journey"] = df["Date_of_Journey"].str.split("/").str[2]
  
### 2. Extracting Arrival Hour and Minute

To extract more meaningful time-based features from the `Arrival_Time` column, we split it into hour and minute components:

```python
df["Arrival_hour"] = df["Arrival_Time"].str.split(":").str[0]
df["Arrival_minute"] = df["Arrival_Time"].str.split(":").str[1]
```
### 3. Converting Total_Stops to Ordinal Data
We mapped stop categories to numerical values. More stops often lead to higher prices, so this ordinal mapping helps:

```python
df["Total_Stops"] = df["Total_Stops"].map({
    "non-stop": 0,
    "1 stop": 1,
    "2 stops": 2,
    "3 stops": 3,
    "4 stops": 4
})
```
### 4. Extracting Departure Hour and Minute
We split the Dep_Time column into separate hour and minute features:

```python
df["Departure_hour"] = df["Dep_Time"].str.split(":").str[0]
df["Departure_minute"] = df["Dep_Time"].str.split(":").str[1]
```
### 5. Processing Duration Column
We extracted hours and minutes from the Duration column and created a new feature for total duration in minutes:

```python
df["Duration_hour"] = df["Duration"].str.extract(r'(\d+)h')
df["Duration_minute"] = df["Duration"].str.extract(r'(\d+)m")

# Fill missing values with 0
df["Duration_hour"] = df["Duration_hour"].fillna(0).astype(int)
df["Duration_minute"] = df["Duration_minute"].fillna(0).astype(int)

# Combine to single value in minutes
df["Duration in minute"] = df["Duration_hour"] * 60 + df["Duration_minute"]
```
### 6. One-Hot Encoding for Categorical Columns
We applied OneHotEncoding on the Airline, Source, and Destination columns:

```python
from sklearn.preprocessing import OneHotEncoder
encoder = OneHotEncoder()

# Fit and transform
encoded = encoder.fit_transform(df[["Airline", "Source", "Destination"]]).toarray()

# Create dataframe with column names
df1 = pd.DataFrame(encoded, columns=encoder.get_feature_names_out())
```

