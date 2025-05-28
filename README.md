<!---
-->

# Retail Store Analysis

  
<p align="justify"><b>Problem Statement :</b> The company needs a clear and comprehensive understanding of its sales performance across products, regions, categories, and customer segments to identify growth opportunities, optimize resource allocation, and address underperforming areas.</p>

<p align="justify"><b>Goal :</b> To provide a high-level, data-driven view of sales, profit, and order distribution across various dimensions, products, regions, categories, and customer segments, to support strategic decision-making and performance improvement.</p>

<p align="justify"><b>Tools used :</b> Mircrosoft Excel, Tableau</p>

<p align="justify"><b>Data :</b> More than 10,000 rows of sales data</p>

## Key Business Questions

- What are the overall sales and profit trends throughout the year?
- Which months show peak or low performance in sales?
- Which regions are performing well, and which are underperforming?
- How do different customer segments contribute to profitability?
- Is there an overdependence on any particular product category?
- What product categories or regions offer opportunities for growth or diversification?


## Data Cleaning
<p align="justify">This retail dataset, similar to the Superstore dataset, is quite messy and requires extensive cleaning. It contains issues such as typos in product names, 
mismatches between regions and states, and missing values in several columns. First, we will clean the data to address inconsistencies and errors, 
and then proceed to visualize key insights.</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/0aecf47c-48be-44d9-8075-9ce1fc9a8505" alt="Raw dataset" width="900" height="180" />
</p>

### Fixing typos in product column

<p align="center">
  <img src="https://github.com/user-attachments/assets/3393ae5f-47ca-4743-b638-ab7712a42663" alt="Typos" width="180" height="350" />
</p>

<p align="justilfy"> It has been observed that the Product column contains several typographical errors, such as "Lapotp," "Binderr," "Sttapler," and others. 
We will correct these using the IFS function, which checks for specific typos and replaces them with the appropriate corrected values.</p>

````sql
=IFS(
  J2="Binderr", "Binder",
  J2="Sttapler", "Stapler",
  J2="Lapotp", "Laptop", 
  J2="Deskk", "Desk",
  J2="Filing Cabnet", "Filing Cabinet",
  J2="Phne", "Phone",
  J2="Offce Chair", "Office Chair",
  J2="Papr Shredder", "Paper Shredder",
  J2="Moues", "Mouse",
  J2="Desk Lmp", "Desk Lamp",
  J2="Printter", "Printer",
  J2="Copir", "Copier",
  J2="Bookkcase", "Bookcase",
  TRUE, J2
)
````


### Fixing mismatch state and region columns

<p align="justify">There are also inconsistencies between states and their corresponding regions. To address this, we will create a reference mapping table with the correct state-region pairs elsewhere in the sheet. 
  Using VLOOKUP, we will retrieve the correct region for each state based on this mapping. </p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/97b65d6d-6095-4271-a4b0-5abc851348b7" alt="Mismatches" width="100" height="150" /></br>
</p>

````sql
=VLOOKUP(O2, $X$2:$Y$9, 2, FALSE)
````


### Fixing customer column

<p align="justify"> Next, we notice that the Customer column contains missing values. We will handle these appropriately either by 
filling them using relevant data where possible or removing the rows if the information is insignificant. </p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/d8d41693-7dfb-4e1f-88b9-75cb4599be2f" alt="Null values" width="200" height="350" /></br>
</p>

<p align="justify"> After filtering, we will use the SUBTOTAL() function to count the number of rows with missing values. 
  This allows us to dynamically calculate the number of affected rows based on the current filter view. </p>

````sql
=SUBTOTAL(103, A2:A10101)
````

<p align="center">
  <img src="https://github.com/user-attachments/assets/2ab1aafe-63c8-4c70-85fb-26faef4864c4" alt="Null values count" width="70" height="50" /></br>
</p>

<p align="justify"> There are only 255 missing entries, which is less than 3% of the total 10,000 rows. Given the small proportion,
we will proceed to delete these rows to maintain data quality without significantly impacting the dataset. </p> </br>

<p align="center">
  <img src="https://github.com/user-attachments/assets/1f27c39d-e534-4467-8371-72962c53f8d1" alt="Typos" width="200" height="350" /></br>
</p>

<p align="justify">We also identified a duplicate entry in the Customer column "Alicce Brown" instead of "Alice Brown." 
  To correct this, we will simply replace the incorrect name with the correct one to ensure consistency.</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/da6a65e4-ebfd-4bd6-b191-01e448602576" alt="Typos" width="350" height="200" /></br>
</p>

### Fixing discount column

<p align="justify"> There are also missing values in the Discount column. We will consider these no discount and handle them by filling them with 0 using the 
  IF() function to ensure calculations are not affected by blanks. </p>

````sql
=IF(A2="", 0, A2)
````

## Data Visulization

<p align="justify"> To clearly present key insights, an interactive dashboard was created using Tableau. The dashboard includes two filters, Product Category and Region, allowing users to explore the data dynamically and gain deeper insights based on their selection. </p>
  
<p align="center">
  <img src="https://github.com/user-attachments/assets/72125eb1-3d62-4575-a9e1-e0e328464fc1" alt="Mismatches" width="600" height="400" /></br>
</p>

## Key Insights

According to the dashboard
- Office Supplies dominate the product sales chart with 60% of total sales.
- Sales fluctuate throughout the year, with noticeable peaks in January and May, while June and October experience dips.
- The South region leads significantly in overall performance, whereas the East region is underperforming in comparison.
- The Home Office segment emerges as the most profitable, while Consumer and Corporate segments show nearly equal profit levels.

## Actionable Recommendations

- Sales Peaks Analysis: The spikes in January and May could be driven by seasonal promotions, end-of-year budgeting, or procurement cycles. Further analysis of order patterns and customer behavior during these months may reveal more insights.
- Underperformance in East: The East region may benefit from targeted marketing campaigns or operational improvements, such as better supply chain management or enhanced customer engagement strategies.
- Product Category Dependence: The business shows a heavy reliance on Office Supplies, which may pose a risk. There is a clear opportunity to diversify by expanding offerings in the Furniture and Technology categories to increase resilience and drive growth.
