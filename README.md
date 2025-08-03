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

### 3. Converting `Total_Stops` to Ordinal Data

We converted the `Total_Stops` feature from categorical to ordinal values, as more stops generally indicate higher ticket prices.

```python
df["Total_Stops"] = df["Total_Stops"].map({
    "non-stop": 0,
    "1 stop": 1,
    "2 stops": 2,
    "3 stops": 3,
    "4 stops": 4
})


We converted the `Total_Stops` feature from categorical to ordinal values, as more stops generally indicate higher ticket prices.

```python
df["Total_Stops"] = df["Total_Stops"].map({
    "non-stop": 0,
    "1 stop": 1,
    "2 stops": 2,
    "3 stops": 3,
    "4 stops": 4
})
