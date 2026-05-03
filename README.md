# RFM analysis customers' engagement a retailing company - SuperStore | Python

Please check the code below or access via the link:  
🔗 https://colab.research.google.com/drive/1JpsC7WbEjqM-twWg9XuNCCI8iE5cpbAa?usp=sharing 🔗    

Author: Nguyễn Hải Long  
Date: 2025-04  
Tools Used: Python  

---

## 📑 Table of Contents  
1. [📌 Background & Overview](#-background--overview)  
2. [📂 Dataset Description & Data Structure](#-dataset-description--data-structure)
3. [⚒️ Main Process](#%EF%B8%8F-main-process)
4. [📌 Key Takeaways](#-key-takeaways)

---

## 📌 Background & Overview  

### 📖 This project is about using Python to analyze given dataset.

✔️ SuperStore is a global retail company. **To celebrate Christmas and New Year**, Marketing team wants to deploy **marketing campaigns** in order to show appreciation to loyalty customers. Beside that, they want to engage with potential customers who could become loyal clients.  
✔️ Marketing director suggests using **RFM model** in Python to classify customers, then launch marketing campaigns to **appreciate loyalty customers**, as well as **engaging potential customers**.  
✔️ RFM analysis (Recency - Frequency - Monetary) is a marketing technique used the **quintile rank** and group customers based on the recency, frequency and monetary total of their **recent transactions** to identify the best customers and perform targeted marketing campaigns.  
✔️ Choosing from 1 to 5 because it is the most common ranking and easy to express (1 = the worst, 5 = the best).  
✔️ Based on the "Segmentation" table, customers are classified according to their individual RFM scores. For example, customers in the **Champions** segment typically have scores such as 555, 554, etc., indicating high recency, frequency, and monetary values. On the other hand, **Hibernating customers** segment may have scores like 332, 322, etc., reflecting lower engagement across one or more dimensions.  

### 👤 Who is this project for?  

✔️ Data leaders.  
✔️ Marketing team leaders.  
✔️ Sales team leaders.  

---

## 📂 Dataset Description & Data Structure  

### 📌 Data Source  
- Source: Company database.  
- Size: The dataset is 01 excel file with 2 sheets: 'ecommerce retail' & 'Segmentation'.  
- Format: .xlsx

### 📊 Data Structure & Relationships  
#### 1️⃣ Table used: 
Using the whole dataset.  

#### 2️⃣ Table Schema & Data Snapshot:  
<details>
 <summary>Table using in this project:</summary>

Sheet 'ecommercer  retail'  

| Field Name | Data Type | Description |
|------------|-----------|-------------|
| InvoiceNo | object | Invoice number. Nominal, a 6-digit integral number uniquely assigned to each transaction. If this code starts with letter 'C', it indicates a cancellation. |
| StockCode | object | Product (item) code. Nominal, a 5-digit integral number uniquely assigned to each distinct product. |
| Description | object | Product (item) name. Nominal. |
| Quantity | int64 | The quantities of each product (item) per transaction. Numeric. |
| InvoiceDate | datetime64 | Invoice Date and time. Numeric, the day and time when each transaction was generated. |
| UnitPrice | float64 | Unit price. Numeric, Product price per unit in sterling. |
| CustomerID | float64 | Customer number. Nominal, a 5-digit integral number uniquely assigned to each customer. |
| Country | object | Country name. Nominal, the name of the country where each customer resides. |

Sheet 'Segmentation'

| Segment | RFM Score |
|---------|-----------|
| Champions | 555, 554, 544, 545, 454, 455, 445 |
| Loyal	| 543, 444, 435, 355, 354, 345, 344, 335 |
| Potential Loyalist	| 553, 551, 552, 541, 542, 533, 532, 531, 452, 451, 442, 441, 431, 453, 433, 432, 423, 353, 352, 351, 342, 341, 333, 323 |
| New Customers	| 512, 511, 422, 421, 412, 411, 311 |
| Promising	| 525, 524, 523, 522, 521, 515, 514, 513, 425, 424, 413, 414,415, 315, 314, 313 |
| Need Attention	| 535, 534, 443, 434, 343, 334, 325, 324 |
| About To Sleep	| 331, 321, 312, 221, 213, 231, 241, 251 |
| At Risk	| 255, 254, 245, 244, 253, 252, 243, 242, 235, 234, 225, 224, 153, 152, 145, 143, 142, 135, 134, 133, 125, 124 |
| Cannot Lose Them	| 155, 154, 144, 214, 215, 115, 114, 113 |
| Hibernating customers	| 332, 322, 233, 232, 223, 222, 132, 123, 122, 212, 211 |
| Lost customers	| 111, 112, 121, 131, 141, 151 |

</details>

- Sheet 'ecommerce retail' will provide data for EDA, calculating.  
- Using sheet 'Segmentation' for segment customers based on the score.

---

## ⚒️ Main Process  

*Note: Click the white triangle to see codes*  

### 💽 EDA
<details>
 <summary><em>💾 Import libraries and dataset, copy dataset, and explore tables:</em></summary>
  
  ```python
  # import thư viện
  import pandas as pd
  import numpy as np
  from google.colab import drive
  import matplotlib.pyplot as plt
  import seaborn as sns
  
  # import excel files with sheet name 'ecommerce retail'
  drive.mount('/content/drive')
  
  path = '/content/drive/MyDrive/DAC K34/Python/Project_3/ecommerce retail.xlsx'
  ecommerce_retail = pd.read_excel (path, sheet_name ='ecommerce retail')
  segmentation = pd.read_excel (path, sheet_name ='Segmentation')
  
  #copy data frame ecommerce_retail
  df_ecommerce_detail = ecommerce_retail.copy()
  
  # copy data frame Segmentation
  df_seg = segmentation
  ```
</details>  

#### 1️⃣ Ecommerce retail table    

<details>
 <summary><em>💾 Basic exploration:</em></summary>

 ```python
 df_ecommerce_detail.head()

 # show rows and columns count
 print(f'Rows count: {df_ecommerce_detail.shape[0]}\nColums count: {df_ecommerce_detail.shape[1]}')
 print('')
 
 # show data type
 df_ecommerce_detail.info()
 
 # further checking on columns
 df_ecommerce_detail.shape
 df_ecommerce_detail.describe()
 
 # check unique values
 num_unique = df_ecommerce_detail.nunique().sort_values()
 print('')
 print('---Percentage of unique values (%)---')
 print(100/num_unique)
 
 # check null values
 print('')
 print('---Number of null values---')
 print(df_ecommerce_detail.isnull().sum())
 
 # check missing data
 missing_value = df_ecommerce_detail.isnull().sum().sort_values(ascending = False)
 missing_percent = df_ecommerce_detail.isnull().mean().sort_values(ascending = False)
 print('')
 print('---Number of missing values in each column---')
 print(missing_value)
 print('')
 print('---Percentage of missing values (%)---')
 if missing_percent.sum():
   print(missing_percent[missing_percent > 0] * 100)
 else:
   print('None')
 
 # check for duplicates
 ## show number of duplicated rows
 print('')
 print(f'Number of entirely duplicated rows: {df_ecommerce_detail.duplicated().sum()}')
 ## show all duplicated rows
 df_ecommerce_detail[df_ecommerce_detail.duplicated()]
 ```
</details>

![](https://github.com/longnguyen0102/RFM-analysis-customers-engagement-a-retailing-company-SuperStore--Python/blob/main/handle_ecommerce_detail_table_1.png)  
![](https://github.com/longnguyen0102/RFM-analysis-customers-engagement-a-retailing-company-SuperStore--Python/blob/main/handle_ecommerce_detail_table_2.png)

1. **Structure:** The data frame has 8 columns (`InvoiceNo`, `StockCode`, `Description`, `Quantity`, `InvoiceDate`, `UnitPrice`, `CustomerID`, `Country`) and 541,909 rows.  

2. **Missing values:**  
* `CustomerID` has the most missing values (**24.9%**), it will be looked further to identify the problems.  
* `Description` has **1,454** missing rows.  

3. **Data type:**  
* `InvoiceDate` is in datetime data type. It can be seperated to Day, Month for further analysis.  
* `CustomerID` could be changed to string data type.  

4. **Observations:**  
* Percentage of unique values in all columns are acceptable.  
* All others columns do not have any missing values.  
* There are **5,268 rows** with duplicated values. However, some rows have same data like `InvoiceNo`, `Quantity`, `InvoiceDate`, `CustomerID`, `Country` which means they are the same order with different items -> no need to investigate more.  

<details>
 <summary><em>💾 Negative values:</em></summary>

 ```python
 # change data type of InvoiceNo to string data type
 df_ecommerce_detail['InvoiceNo'] = df_ecommerce_detail['InvoiceNo'].astype(str)
 ```

```python
# print out some rows where Quantity < 0
print('---Some rows have Quantity < 0---')
print(df_ecommerce_detail[df_ecommerce_detail['Quantity']<0].head())
print('')

# print out some rows where UnitPrice < 0
print('---Some rows have UnitPrice < 0---')
print(df_ecommerce_detail[df_ecommerce_detail['UnitPrice']<0].head())

# further checking
## make a new column: True if InvoiceNo has 'C', False if InvoiceNo has no 'C'
df_ecommerce_detail['Cancellation'] = df_ecommerce_detail['InvoiceNo'].str.contains('C')
## check InvoiceNo has 'C' and Quantity < 0
print('')
print('---Data frame which has Cancellation and Quantity < 0---')
print(df_ecommerce_detail[(df_ecommerce_detail['Cancellation'] == True) & (df_ecommerce_detail['Quantity'] < 0)].head())

## check InvoiceNo has no 'C' and Quantity < 0
print('')
print('---Data frame which has no Cancellation and Quantity < 0---')
df_ecommerce_detail[(df_ecommerce_detail['Cancellation'] == False) & (df_ecommerce_detail['Quantity'] < 0)].head()
```

</details>

![](https://github.com/longnguyen0102/RFM-analysis-customers-engagement-a-retailing-company-SuperStore--Python/blob/main/handle_ecommerce_detail_table_negative_value_1.png)

Based on data type, columns `Quantity` and `UnitPrice` might have negative values (they both are in numbers).  

1. Column `Quantity` have 2 cases: Invoices which are **cancelled** and **non-cancelled**:  
* **Cancelled**: These invoices are cancelled order.  
* **Non-cancelled**: Might be free gifts for promotional campaigns (buy 1 get 1 free, special offer, etc.). These will affect the stock of the store.  

2. Column `UnitPrice`: As can be seen in `Description`, they are ***Adjust bad dept*** in 2 invoices **A563186** and **A563187**.  

In this case, we can drop all rows with **negative values** and `InvoiceID` contains 'C' for cancellation.  

<details>
 <summary><em>Missing values:</em></summary>
  
 ```python
 # show up some rows with missing values
 df_ecommerce_detail_null = df_ecommerce_detail.isnull()
 rows_with_null = df_ecommerce_detail_null.any(axis=1)
 df_ecommerce_detail_with_null = df_ecommerce_detail[rows_with_null]
 
 print('---Some rows with missing values---')
 df_ecommerce_detail_with_null.head(10)
 ```
</details>

 ![](https://github.com/longnguyen0102/RFM-analysis-customers-engagement-a-retailing-company-SuperStore--Python/blob/main/handle_ecommerce_detail_table_missing_value_1.png)

 As can be seen above, only `CustomerID` column has missing values. In this case, Marketing team wants to deploy marketing campaigns in order to show appreciation to **loyalty customers**. However, with these missing data, these rows can be dropped for identifying loyalty customers by using RFM model.

<details>
 <summary><em>Duplicated values:</em></summary>
  
 ```python
 # locate the values are not duplicated in the selected columns
 df_no_dup = df_no_nan.loc[~df_ecommerce_detail.duplicated(subset = ['InvoiceNo','StockCode','InvoiceDate','UnitPrice','CustomerID','Country'])].reset_index(drop=True).copy()
 ```

```python
# check an example of duplicate in InvoiceNo
df_no_dup.query('InvoiceNo == "536365"')
df_no_dup.query('InvoiceNo == "581587"')
```

```python
# drop duplicates, keep the first row of subset
df_main = df_ecommerce_detail.drop_duplicates(subset=["InvoiceNo", "StockCode","InvoiceDate","CustomerID"], keep = 'first')

print('---Data frame after dropping duplicates---')
df_main.head(10)
```

</details>

![](https://github.com/longnguyen0102/RFM-analysis-customers-engagement-a-retailing-company-SuperStore--Python/blob/main/handle_ecommerce_detail_table_duplicated_value_1.png)

1. Exploring duplicates among columns: `InvoiceNo`, `StockCode`, `InvoiceDate`, `UnitPrice`, `CustomerID`, `Country`:
* In this step, we will find rows which have **duplicated information among these columns** then we will keep rows which have no duplicated values.
* This step is a **wide exploration of data frame**, there might be some invoices which have same `UnitPrice` and `Country` values. The reason for this is **system error** or **typing error**, it creates double invoices.
* We will drop rows which have duplicated information of 6 columns.

2. Exploring duplicates in columns: `InvoiceNo`, `StockCode`, `InvoiceDate`, `CustomerID`:
* This step only choose 4 columns for identifying **the unique of a transaction**.
* `InvoiceNo` + `StockCode` are the **important key** because if there are any duplicates, it might be **error**.
* `InvoiceDate` + `CustomerID` identify the **time of transaction** for which customer.
* If there are any duplication, we only use the **first row**.
* The reason for using only **4 columns** because they are "strong key" and other columns might be **distraction** for the calculation.

3. After dropping all duplicated, this table will be the main data frame to be used in RFM analysis.

<details>
 <summary><em>Creating Sales column and final version of data frame</em></summary>

```python
# create Sales column (Quantity * UnitPrice)
df_main['Sales'] = df_main.Quantity * df_main.UnitPrice

# take max('Day') for recently interaction of customer
df_last_day = df_main.Day.max()

print('---Final version of data frame for calculating RFM---')
df_main.head(10)
```
 
</details>

![](https://github.com/longnguyen0102/RFM-analysis-customers-engagement-a-retailing-company-SuperStore--Python/blob/main/handle_ecommerce_detail_table_creating_sales_column.png)

Using ***df_main*** to create `Sales` column for calculating **Monetary** and creating a new data frame ***df_last_day*** for calculating **Recency**.  

#### 2️⃣ Handle Segmentation table

<details>
 <summary><strong>Transform df_seg:</strong></summary>
  
 ```python
 #transform df_seg
 df_seg['RFM Score'] = df_seg['RFM Score'].str.split(',')
 df_seg = df_seg.explode('RFM Score').reset_index(drop=True)
 
 print('---Segmentation table after split---')
 df_seg.head()
 ```

</details>

![](https://github.com/longnguyen0102/RFM-analysis-customers-engagement-a-retailing-company-SuperStore--Python/blob/main/handle_segmentation_detail_split.png)

* ***Segmentation*** table contains 2 columns: `Segment` and `RFM Score`.
* The transformation of the Segmentation table will split segments based on predefined RFM scores. These scores are currently separated by commas, so this process will parse them into the required segments accordingly. 

### 🧮 CALCULATING RFM

<details>
 <summary><em>Code:</em></summary>

 ```python
 df_RFM = df_main.groupby('CustomerID').agg(
     Recency = ('Day', lambda x: df_last_day - x.max()),
     Frequency = ('CustomerID','count'),
     Monetary = ('Sales','sum'),
     Start_Date = ('Day','min')
 ).reset_index()
 
 df_RFM['Recency'] = df_RFM['Recency'].dt.days.astype('int16')
 # take opposite of Recency
 df_RFM['Reverse_Recency'] = -df_RFM['Recency']
 df_RFM['Start_Date'] = pd.to_datetime(df_RFM.Start_Date)
 ```

 ```python
 # label R, F, M
 df_RFM['R'] = pd.qcut(df_RFM['Reverse_Recency'], 5, labels = range(1,6)).astype(str)
 df_RFM['F'] = pd.qcut(df_RFM['Frequency'], 5, labels = range(1,6)).astype(str)
 df_RFM['M'] = pd.qcut(df_RFM['Monetary'], 5, labels = range(1,6)).astype(str)
 df_RFM['RFM'] = df_RFM.R + df_RFM.F + df_RFM.M
 
 print('---After calculating Recency, Frequency, and Monetary and labeling---')
 df_RFM.head(10)
 ```
</details>

![](https://github.com/longnguyen0102/RFM-analysis-customers-engagement-a-retailing-company-SuperStore--Python/blob/main/rfm_calculating_rfm.png)

**In this stage, RFM is calculated:**  
  1. **Recency** is computed as the last purchase date minus the dataset’s maximum date, the low value the better. However, the convenience in label, we use negative value of Recency. That means **the bigger the better**, and ranking is from 1 = worst to 5 = best.  
  2. **Frequency** measures how often a customer makes a purchase and is computed as counting the number of appearance of each customer, **the bigger the better**.  
  3. **Monetary** represents the total of money spending from each customer, **the bigger the better**.
Afterward, the results of the three metrics are assigned scores on a scale from 1 to 5.

<details>
 <summary><em>Merging with Segmentation table:</em></summary>

 ```python
 # merge with Segementation for comparison
 df_RFM_final = df_RFM.merge(df_seg, how='left', left_on='RFM', right_on='RFM Score')
 
 # clear space
 df_seg['RFM Score'] = df_seg['RFM Score'].str.strip()
 
 print('---Merge data frame with Segmentation table for segment customers---')
 df_RFM_final.head(10)
 ```

 ![](https://github.com/longnguyen0102/RFM-analysis-customers-engagement-a-retailing-company-SuperStore--Python/blob/main/rfm_merge_with%20segmentation_table.png)

In this final step, the combined RFM scores are matched against the ***Segmentation*** table to assign each customer to a corresponding segment.  

</details>

### 💟 DETERMINE LOYAL AND NON-LOYAL AND SHOWING CHARACTERISTIC OF POTENTIAL LOYALIST  

<details>
 <summary><em>Determine Loyal and Non Loyal:</em></summary>

 ```python
 # loyal status
 df_RFM_final['Loyal_Status'] = df_RFM_final['Segment'].apply(lambda x: 'Loyal' if x in ('Loyal','Champions') else 'Non Loyal')
 
 print('---Some rows of data frame determine Loyal and Non Loyal---')
 df_RFM_final.head(10)
 ```
</details>

![](https://github.com/longnguyen0102/RFM-analysis-customers-engagement-a-retailing-company-SuperStore--Python/blob/main/determine_loyal_determine.png)

<details>
 <summary><em>Loyal and Non Loyal summary:</em></summary>

 ```python
 # calculating the percentage of Loyal_Status
 loyal_counts = df_RFM_final['Loyal_Status'].value_counts()
 loyal_percents = df_RFM_final['Loyal_Status'].value_counts(normalize=True) * 100
 
 # creating data frame loyal_summary for visulization
 loyal_summary = pd.DataFrame({
     'Count': loyal_counts,
     'Percentage (%)': loyal_percents
 })
 
 print('---Loyal summary---')
 display(loyal_summary)
 ```
</details>

![](https://github.com/longnguyen0102/RFM-analysis-customers-engagement-a-retailing-company-SuperStore--Python/blob/main/determine_loyal_loyal_summary.png)

<details>
 <summary><em>Segmentation summary:</em></summary>

 ```python
 # calculating the percentage of Segmentation
 segment_counts = df_RFM_final['Segment'].value_counts()
 segment_percents = df_RFM_final ['Segment'].value_counts(normalize=True) * 100
 
 # creating data frame segment_summary for visulization
 segment_summary = pd.DataFrame({
     'Count': segment_counts,
     'Percentage (%)': segment_percents
 })
 
 print('---Segment summary---')
 display(segment_summary)
 ```
</details>

![](https://github.com/longnguyen0102/RFM-analysis-customers-engagement-a-retailing-company-SuperStore--Python/blob/main/determine_loyal_segment_summary.png)

Company identifies **Loyal** and **Champion** are in "Loyal status". Based on that there is only **7.7%** loyal customers. The number of non-loyal customers is quite high (**more than 90%**).  
As can be seen on ***Segment Summary*** table, **Loyal**, **Promising**, **Potential Loyalist** have the same amount (about **3%**). **Lost customers** is almost high as **Champions**. Marketing team and sales team should be aware and focus on these groups: decreasing **Lost customers**, increasing **Loyal**.  

<details>
 <summary><em>Calculating average of Quantity and Sales according to CustomerID:</em></summary>

 ```python
 # average of Quantity and Sales according to CustomerID
 df_potential_average = df_main.groupby('CustomerID').agg(
     Quantity_Average = ('Quantity','mean'),
     Sales_Average = ('Sales','mean')
 ).reset_index()
 
 print('---Calculating average of quantity and sales---')
 df_potential_average.head(10)
 ```
</details>

![](https://github.com/longnguyen0102/RFM-analysis-customers-engagement-a-retailing-company-SuperStore--Python/blob/main/determine_loyal_calculate_average.png)  

Calculating the average of quantity and sales hepls further understanding of **purchasing behvior** of each customer:
1. `Sales_Average` differentiates the scale of orders. Customers with **high monetary** might purchase lots of order or they might have few orders with large price.

2. Identify the characteristic of segment. Some groups might have high **Frequency** but low average number of sales. This one proves that their purchasing is constant with low price products. Marketing team might launch a compaign to encourage them to chosse high price products.

3. `Quantity_Average` will help teams finding customer groups that purchase high volume, teams might offer them wholesale policy or discount price for certain products.

<details>
 <summary><em>Finding first Sales and Quantity according to CustomerID:</em></summary>

 ```python
 # first Sales and Quantity according to CustomerID
 ## based on InvoiceDate to get first order -> Quantity, Sales
 df_main['Ranking'] = df_main.groupby('CustomerID')['InvoiceDate'].rank(method = 'first')
 df_potential_first = df_main[df_main.Ranking == 1][['CustomerID','Quantity','Sales']]
 df_potential_first = df_potential_first.rename(columns={'Quantity':'First_Quantity','Sales':'First_Sales'})
 
 print('---First order of each customer---')
 df_potential_first.head(10)
 ```
</details>

![](https://github.com/longnguyen0102/photo/blob/main/RFM_analysis-retail-python/determine_loyal_calculate_first_order.png) 

This step aims to find out the quantity and sales of first order of each customer:  
 1. Identify **"starting point"**: `Start_Date` helps identify the date of first order. Comparing with promotional events in the past, we can see the effective of events: the number of orders, the number of new customers, etc.
 2. `First_Quantity` and `First_Sales` provides the behavior of customers: they might spend a lot of money on the first order or they might need time for experience new method.
 3. This step can deliver how much time does a customer bring profit to the company.

<details>
 <summary><em>Merge all to form a final data frame for visualization:</em></summary>

 ```python
 # merge all
 df_RFM_final = df_RFM_final.merge(df_potential_average, how = 'left', on = 'CustomerID')
 df_RFM_final = df_RFM_final.merge(df_potential_first, how = 'left', on = 'CustomerID')
 
 print('---Final data frame---')
 df_RFM_final.head(10)
 ```
</details>

![](https://github.com/longnguyen0102/RFM-analysis-customers-engagement-a-retailing-company-SuperStore--Python/blob/main/determine_loyal_final_data_frame.png) 

Merging 3 tables (***Main data frame***, ***Average number of quantity & sales***, ***First order of each customer***) for a clear review and visualization.  

### 📊 VISUALIZATION

<details>
 <summary><em>Creating histogram for R, F, M scores:</em></summary>

 ```python
 # Histograms for R, F, and M scores with smoothed KDE
 fig, axes = plt.subplots(1, 3, figsize=(18, 6))
 
 # Convert R, F, and M columns to integer type for correct ordering
 df_RFM_final['R_int'] = df_RFM_final['R'].astype(int)
 df_RFM_final['F_int'] = df_RFM_final['F'].astype(int)
 df_RFM_final['M_int'] = df_RFM_final['M'].astype(int)
 
 # Using bw_adjust to smooth the KDE curve into a single trend line
 sns.histplot(data=df_RFM_final, x='R_int', ax=axes[0], kde=True, discrete=True, kde_kws={'bw_adjust': 2})
 axes[0].set_title('Distribution of Recency (R) Scores')
 axes[0].set_xlabel('Recency Score')
 axes[0].set_ylabel('Number of Customers')
 axes[0].set_xticks(range(1, 6))
 
 sns.histplot(data=df_RFM_final, x='F_int', ax=axes[1], kde=True, discrete=True, kde_kws={'bw_adjust': 2})
 axes[1].set_title('Distribution of Frequency (F) Scores')
 axes[1].set_xlabel('Frequency Score')
 axes[1].set_ylabel('Number of Customers')
 axes[1].set_xticks(range(1, 6))
 
 sns.histplot(data=df_RFM_final, x='M_int', ax=axes[2], kde=True, discrete=True, kde_kws={'bw_adjust': 2})
 axes[2].set_title('Distribution of Monetary (M) Scores')
 axes[2].set_xlabel('Monetary Score')
 axes[2].set_ylabel('Number of Customers')
 axes[2].set_xticks(range(1, 6))
 
 plt.tight_layout()
 plt.show()
 
 def get_kde_values(data, points):
     #KDE with bw_adjust=2 same in plot
     kde = gaussian_kde(data, bw_method='scott')
     kde.set_bandwidth(bw_method=kde.factor * 2)
     return kde.evaluate(points)
 
 # points from 1 to 5
 x_points = np.array([1, 2, 3, 4, 5])
 
 # calculate density for r, f, m
 r_density = get_kde_values(df_RFM_final['R_int'], x_points)
 f_density = get_kde_values(df_RFM_final['F_int'], x_points)
 m_density = get_kde_values(df_RFM_final['M_int'], x_points)
 
 # create summary table
 kde_summary = pd.DataFrame({
     'RFM_Score': x_points,
     'Recency_Density': r_density,
     'Frequency_Density': f_density,
     'Monetary_Density': m_density
 })
 
 print('')
 print('--- KDE Density at each score ---')
 display(kde_summary.style.format("{:.4f}", subset=['Recency_Density', 'Frequency_Density', 'Monetary_Density']))
 ```
</details>

![](https://github.com/longnguyen0102/RFM-analysis-customers-engagement-a-retailing-company-SuperStore--Python/blob/main/visualization_R_F_M.png)  

As can be seen from the histogram:  
- **Recency (R):** As can be seen from the **KDE** - Kernel Density Estimation, it leans to **the right** which has **high score** (4 and 5). This means most customers have **high Recency**. The majority of customers have made recent purchases. However, there is still a significant portion of customers with **low Recency scores** (1, 2, or 3), suggesting they haven't purchased in a while.
- **Frequency (F):** The frequency distribution is left-skewed, with most customers having **low Frequency** scores (1 and 2). This indicates that the majority of customers do not purchase frequently. A small segment of customers with high Frequency scores (4 and 5) represents those who buy very regularly.
- **Monetary (M):** The Monetary distribution is also left-skewed, similar to Frequency. This suggests that most customers have **low spending** values. Only a small number of customers have high Monetary scores (4 and 5), representing high-value spenders. 

<details>
 <summary><em>Visualization of spending amount and number of user according to Segment:</em></summary>

 ```python
 # Visualize spending amount and number of user according to Segment.
 user_by_segment = df_RFM_final[['Segment','CustomerID']].groupby(['Segment']).count().reset_index()
 user_by_segment = user_by_segment.rename(columns = {'CustomerID':'user_volume'})
 user_by_segment['contribution_percent'] = round(user_by_segment['user_volume'] / user_by_segment['user_volume'].sum() * 100)
 user_by_segment['type'] = 'user contribution'
 
 spending_by_segment = df_RFM_final[['Segment','Monetary']].groupby(['Segment']).sum().reset_index()
 spending_by_segment = spending_by_segment.rename(columns = {'Monetary':'spending'})
 spending_by_segment['contribution_percent'] = spending_by_segment['spending'] / spending_by_segment['spending'].sum() * 100
 spending_by_segment['type'] = 'spending contribution'
 
 segment_agg = pd.concat([user_by_segment, spending_by_segment])
 
 plt.figure(figsize=(20, 10))
 sns.barplot(segment_agg, x='Segment', y='contribution_percent', hue='type')
 plt.title='Spending amount and number of user according to Segment'
 
 plt.show()
 ```
</details>

![](https://github.com/longnguyen0102/RFM-analysis-customers-engagement-a-retailing-company-SuperStore--Python/blob/main/visualization_segment.png)  

<details>
 <summary><em>Visualization of Sales trend over time:</em></summary>

 ```python
 # Calculate total sales per month
 monthly_sales = df_main.groupby('Month')['Sales'].sum().reset_index()
 
 # Convert Month to datetime for proper sorting
 monthly_sales['Month'] = pd.to_datetime(monthly_sales['Month'])
 
 # Sort by month
 monthly_sales = monthly_sales.sort_values('Month')
 
 # Visualize the sales trend over time
 plt.figure(figsize=(12, 6))
 sns.lineplot(data=monthly_sales, x='Month', y='Sales')
 plt.xlabel('Month')
 plt.ylabel('Total Sales')
 plt.xticks(rotation=45)
 plt.tight_layout()
 plt.show()
 ```
</details>

![](https://github.com/longnguyen0102/RFM-analysis-customers-engagement-a-retailing-company-SuperStore--Python/blob/main/visualization_sales.png)  

### 🔍 Insights and Actions (drawing from both graphs of Spending amount and Sales trending)  

✔️ The **"Champions"** segment is the core revenue driver: The chart shows that the **"Champions"** group contributes the largest share of revenue—over 60%—despite representing only around 18% of the total customer base. This highlights the critical importance of this segment to SuperStore. These are the most frequent, recent, and high-spending customers.  
➡️ **Action:** It is essential to focus on maintaining and enhancing the experience for **"Champions"** to ensure stable and sustainable revenue.

✔️ The **"Loyal"** segment also makes a significant contribution: The **"Loyal"** customers account for approximately 10% of the total customer base and contribute a notable portion of revenue—over 10%.  
➡️ **Action:** This is a high-potential segment that can be nurtured to become future **"Champions"** Targeted initiatives such as personalized offers, loyalty programs, or incentives could encourage them to increase purchase frequency and order value.  

✔️ The **"Potential Loyalist"** segment shows promise but needs activation: The **"Potential Loyalist"** group represents a relatively high share of the customer base (11%) but contributes only around 3.2% of total revenue. This aligns with the typical characteristics of this segment—good Recency and Frequency, but low Monetary value.  
➡️ **Action:** Targeted campaigns should aim to increase spending per transaction for this group in order to convert them into **"Loyal"** or even **"Champions"** over time. Strategies could include personalized upselling, product bundling, or limited-time promotions to encourage higher basket sizes.

✔️ Based on the *sales trending* graph:  
➡️ Quarter fourth is a good time for **upselling**. This is the time that customers will spend more money for preparing for Holiday Season. Upselling programs are focus on increasing average order value instead of discount.  
➡️ Months in early and middle of the year are the time for launching **customer incentive and relation programs**. During these time, the need for buying is low. That is the reason for these programs to step in, they will attract more customers (even new ones) and increase customers' Frequency, like: price discount, buy 1 get 1, voucher for the next buying,...  
➡️ Months before sales increasing (such as September) is the time for **"heat up the market"**. Launching early promotion programs, new products, new collections are not the bad idea.  

---

## 📌 Key Takeaways
✔️ Understand how **RFM analysis** can be used to evaluate customer behavior based on purchase frequency and spending value.  
✔️ **Classify customers** into specific segments using RFM scores, helping identify which segments require enhanced experiences and which should be retained and nurtured to move toward higher-value tiers.  
✔️ Determine the **optimal timing** for launching promotional campaigns and upselling strategies, enabling the business to both retain existing customers and attract new ones.
