# Project_DS_fro-international_supermarket
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

### Load Dataset

data = pd.read_csv("C:/Users/Len/Documents/courses of FCDS/DS Mesedology/supermarket_dataset.csv")

data = pd.DataFrame(data)
### Start Exploring
Showing the first 5 data rows
data.head()
### Showing the data
data
### Shape And Informations about the Dataset and Column's Data Types
data.shape
data.info()
#### We found that the price and quantity column are object data types
### Start Cleaning
data = data.dropna()
data = data.drop_duplicates()
data 
#### We have to edit the data types of price, quantity, and country  
### Identify non-numeric values in the 'Price' column

non_numeric_prices = data[pd.to_numeric(data['Price'], errors='coerce').isnull()]['Price']
print(non_numeric_prices)
data['Price'].value_counts()
#### We noticed that the problem is we found "USD" in the Price column
data["Price"] = data["Price"].replace("15 USD","15")
#### Changing data type from Object to Float64
data["Price"] = data["Price"].astype("float64")
#### Checking that the data type has changed
print(data['Price'].dtypes)
### Identify non-numeric values in the 'Quantity' column
non_numeric_Quantities = data[pd.to_numeric(data['Quantity'], errors='coerce').isnull()]['Quantity']
print(non_numeric_Quantities)
data['Quantity'].value_counts()
#### Changing "Five" into 5
data["Quantity"] = data["Quantity"].replace("Five",5)
#### Changing data type from Object to INT64
data["Quantity"] = data["Quantity"].astype("int64")
#### Checking that the data type has changed
print(data['Quantity'].dtypes)
data.head()
### Check if there are any duplicate rows in the DataFrame
data.duplicated().any()
### Start Analysis
#### Getting some Statistical information
data.describe()
data["Product Category"].value_counts()
data["Country"].value_counts()
#### We noticed that there are some countries are duplicated with different Fonts
data['Country'] = data['Country'].replace({'USA': 'United States', 'U.S.A.': 'United States'})
data['Country'] = data['Country'].replace({ 'INDIA': 'India'})
data["Country"].value_counts()
#### Unique Elements from Payment Methods
data['Payment Method'].nunique()
### Conclusions From Visualization of Columns
data['Payment Method'].value_counts()[:6].plot(kind='bar')
data.groupby('Product Category').size().plot(kind='barh', color=sns.palettes.mpl_palette('Dark2'))
top_10_products = data['Product Name'].value_counts().head(6)
top_10_products.plot(kind='bar', color='green')
plt.title('Top 6 Most Sold Products')
plt.xlabel('Product Name')
plt.ylabel('Count')
plt.xticks(rotation=45)
plt.show()
sns.boxplot(x=data['Quantity'], color='red')
plt.title('Box Plot of Quantity Ordered')
plt.show()
### Statiscal Anaylsis
np.mean(data['Quantity'])
np.median(data['Quantity'])
np.max(data['Quantity'])
np.min(data['Quantity'])
 q1 = data['Quantity'].quantile(0.25)
q1
q3 = data['Quantity'].quantile(0.75)
q3
IQR = q3-q1
IQR
plt.figure(figsize=(8, 8))


sales_by_country = data.groupby('Country')['Total Amount'].sum()


plt.figure(figsize=(8, 8))
plt.pie(sales_by_country, labels=sales_by_country.index, autopct='%1.1f%%', colors=plt.cm.Paired.colors)
plt.title("Sales Distribution by Country")
plt.show()
sns.histplot(data['Total Amount'], kde=True, color='purple')
plt.title('Distribution of Total Amount')
plt.xlabel('Total Amount ($)')
plt.show()
### Visualizations of Relations Between Columns
#### Correlation between Quantity , price , and total amount
numerical_cols = ['Quantity', 'Price', 'Total Amount']

correlation_matrix = data[numerical_cols].corr()
sns.heatmap(correlation_matrix, annot=True, cmap='coolwarm')
plt.title("Correlation Heatmap")
plt.show()
sales_by_category = data.groupby('Product Category')['Total Amount'].sum().sort_values()

sales_by_category.plot(kind='bar', figsize=(10, 6), color='skyblue')
plt.title("Total Sales by Product Category")
plt.ylabel("Total Amount")
plt.xlabel("Product Category")
plt.xticks(rotation=45)
plt.show()
plt.figure(figsize=(10, 6))
sns.histplot(data=data, x='Total Amount', hue='Product Category', kde=True, palette='coolwarm')
plt.title("Histogram of Total Amount by Product Category")
plt.xlabel("Total Amount")
plt.ylabel("Frequency")
plt.show()
#### We found that the histogram follows left skewed Distribution
plt.figure(figsize=(10, 6))
sns.countplot(data=data, x='Product Category', hue='Payment Method', palette='Set2')
plt.title("Payment Method Distribution by Product Category")
plt.xlabel("Product Category")
plt.ylabel("Count of Orders")
plt.xticks(rotation=45)
plt.show()
plt.figure(figsize=(12, 6))
category_country = data.groupby(['Country', 'Product Category']).size().unstack(fill_value=0)
category_country.plot(kind='bar', stacked=True, colormap='coolwarm', figsize=(12, 6))
plt.title('Distribution of Product Categories by Country')
plt.xlabel('Country')
plt.ylabel('Count')
plt.xticks(rotation=45)
plt.legend(title='Product Category')
plt.tight_layout()
plt.show()

plt.figure(figsize=(10, 6))
payment_country = data.groupby(['Country', 'Payment Method']).size().unstack(fill_value=0)
sns.heatmap(payment_country, annot=True, fmt='d', cmap='coolwarm')
plt.title('Heatmap of Payment Methods by Country')
plt.xlabel('Payment Method')
plt.ylabel('Country')
plt.tight_layout()
plt.show()

## We got some conclusions from our visualization that will help in our future plan
