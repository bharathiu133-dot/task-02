# Task-02: Data Cleaning and EDA
# Dataset: Global Sales Dataset

import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Step 1: Load dataset
df = pd.read_csv("sales.csv")

# Step 2: Clean column names
df.columns = df.columns.str.strip()

# Step 3: Basic checks
print(df.head())
print(df.info())
print(df.isnull().sum())

# Step 4: Remove duplicates
df.drop_duplicates(inplace=True)

# Step 5: Convert date columns
df["Order Date"] = pd.to_datetime(df["Order Date"])
df["Ship Date"] = pd.to_datetime(df["Ship Date"])

# =========================
# Exploratory Data Analysis
# =========================

# 1️⃣ Total Profit by Region
region_profit = df.groupby("Region")["Total Profit"].sum().reset_index()

sns.barplot(x="Total Profit", y="Region", data=region_profit)
plt.title("Total Profit by Region")
plt.show()

# 2️⃣ Sales Channel Distribution
sns.countplot(x="Sales Channel", data=df)
plt.title("Sales Channel Distribution")
plt.show()

# 3️⃣ Item Type vs Total Revenue
item_revenue = df.groupby("Item Type")["Total Revenue"].sum().reset_index()

sns.barplot(x="Item Type", y="Total Revenue", data=item_revenue)
plt.xticks(rotation=45)
plt.title("Total Revenue by Item Type")
plt.show()

# 4️⃣ Order Priority Distribution
sns.countplot(x="Order Priority", data=df)
plt.title("Order Priority Distribution")
plt.show()

# 5️⃣ Units Sold by Region
units_region = df.groupby("Region")["Units Sold"].sum().reset_index()

sns.barplot(x="Units Sold", y="Region", data=units_region)
plt.title("Units Sold by Region")
plt.show()

# 6️⃣ Profit Trend Over Time
profit_trend = df.groupby("Order Date")["Total Profit"].sum()

plt.plot(profit_trend.index, profit_trend.values)
plt.title("Profit Trend Over Time")
plt.xlabel("Order Date")
plt.ylabel("Total Profit")
plt.show()

# 7️⃣ Correlation Heatmap
numeric_cols = ["Units Sold", "Unit Price", "Unit Cost",
                "Total Revenue", "Total Cost", "Total Profit"]

sns.heatmap(df[numeric_cols].corr(), annot=True, cmap="coolwarm")
plt.title("Correlation Heatmap")
plt.show()


