# sales-based-on-vechile-type
EV vechile sales
# Step 1: Import necessary libraries
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from google.colab import files

# Step 2: Upload your dataset with error handling
try:
    uploaded = files.upload()  # This will prompt you to upload the CSV file
    file_name = list(uploaded.keys())[0]  # Get the name of the uploaded file
except Exception as e:
    print("Error uploading file:", e)

# Step 3: Load the dataset into a pandas DataFrame with error checking
try:
    df = pd.read_csv(file_name)  # Load the uploaded file
except Exception as e:
    print("Error loading the dataset:", e)

# Step 4: Perform basic data analysis
print("Dataset Overview:")
print(df.head())           # Display first few rows of the dataset
print("\nData Summary:")
print(df.describe())       # Summary statistics

# Step 5: Visualize the data - Total Sales by Vehicle Type

# Calculate total sales per vehicle type across all states
vehicle_totals = df[['Two Wheeler', 'Three Wheeler', 'Four Wheeler', 'Goods Vehicles',
                     'Public Service Vehicle', 'Special Category Vehicles',
                     'Ambulance/Hearses', 'Construction Equipment Vehicle', 'Other']].sum()

# Plot total sales by vehicle type
plt.figure(figsize=(10, 6))
vehicle_totals.plot(kind='bar', color='skyblue', edgecolor='black')
plt.title("Total Sales by Vehicle Type Across All States")
plt.ylabel("Total Sales")
plt.xlabel("Vehicle Type")
plt.xticks(rotation=45)
plt.show()

# Step 6: Data Segmentation - Top Vehicle Type by State

# Identify the top-selling vehicle type in each state
df['Top Vehicle Type'] = df[['Two Wheeler', 'Three Wheeler', 'Four Wheeler',
                             'Goods Vehicles', 'Public Service Vehicle',
                             'Special Category Vehicles', 'Ambulance/Hearses',
                             'Construction Equipment Vehicle', 'Other']].idxmax(axis=1)
df['Top Vehicle Sales'] = df[['Two Wheeler', 'Three Wheeler', 'Four Wheeler',
                              'Goods Vehicles', 'Public Service Vehicle',
                              'Special Category Vehicles', 'Ambulance/Hearses',
                              'Construction Equipment Vehicle', 'Other']].max(axis=1)

print("\nTop Selling Vehicle Type by State:")
print(df[['State Name', 'Top Vehicle Type', 'Top Vehicle Sales']])

# Visualize frequency of top vehicle types across states
top_vehicle_counts = df['Top Vehicle Type'].value_counts()
plt.figure(figsize=(10, 6))
sns.barplot(x=top_vehicle_counts.index, y=top_vehicle_counts.values, palette="viridis")
plt.title("Frequency of Top-Selling Vehicle Type by State")
plt.xlabel("Vehicle Type")
plt.ylabel("Number of States where Vehicle Type is Top-Selling")
plt.xticks(rotation=45)
plt.show()

# Step 7: Data Segmentation - Statewise Vehicle Sales

# Group data by state and calculate total sales for each vehicle type
statewise_totals = df.groupby('State Name')[vehicle_totals.index].sum()

print("\nSales by State and Vehicle Type:")
print(statewise_totals.head())

# Optional: Heatmap of sales by state and vehicle type
plt.figure(figsize=(12, 8))
sns.heatmap(statewise_totals, annot=True, fmt=".0f", cmap="YlGnBu", cbar=True)
plt.title("Vehicle Sales by State and Type")
plt.xlabel("Vehicle Type")
plt.ylabel("State Name")
plt.xticks(rotation=45)
plt.show()

# Step 8: Analyze Electric Vehicle (EV) Sales

# Calculate total EV sales per state
# Replace 'Electric Vehicle' with the actual column names in your dataset if needed
df['Total EV Sales'] = df[['Two Wheeler', 'Three Wheeler', 'Four Wheeler', 'Goods Vehicles',
                            'Public Service Vehicle', 'Special Category Vehicles',
                            'Ambulance/Hearses', 'Construction Equipment Vehicle',
                            'Other']].sum(axis=1)  # Update with correct EV column names

# Identify the total EV sales for each state
ev_sales_by_state = df.groupby('State Name')['Total EV Sales'].sum()

# Find the state with the maximum and minimum EV sales
max_ev_state = ev_sales_by_state.idxmax()
max_ev_sales = ev_sales_by_state.max()
min_ev_state = ev_sales_by_state.idxmin()
min_ev_sales = ev_sales_by_state.min()

print("\nState with Most EV Sales:")
print(f"{max_ev_state} with {max_ev_sales} EV sales")

print("\nState with Least EV Sales:")
print(f"{min_ev_state} with {min_ev_sales} EV sales")

# Visualize EV sales by state
plt.figure(figsize=(12, 6))
ev_sales_by_state.plot(kind='bar', color='lightgreen', edgecolor='black')
plt.title("Total EV Sales by State")
plt.ylabel("Total EV Sales")
plt.xlabel("State Name")
plt.xticks(rotation=45)
plt.show()
