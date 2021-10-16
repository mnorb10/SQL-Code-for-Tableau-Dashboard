# SQL-Code-for-Tableau-Dashboard
Google Big Query was used for these SQL Queries

# Merge Data 
SELECT 
    * EXCEPT(ProductID, SalesTeamID, CustomerID, StoreId)
FROM   
    `us-sales-data.sales.sales-orders` AS sales
LEFT JOIN 
    `us-sales-data.sales.product` AS product ON sales.ProductID = product.ProductID
LEFT JOIN 
    `us-sales-data.sales.sales-team` AS team ON sales.SalesTeamID = team.SalesTeamID
LEFT JOIN 
    `us-sales-data.sales.customers` AS customer ON sales.CustomerID = customer.CustomerID
LEFT JOIN 
    `us-sales-data.sales.location` AS location ON sales.StoreID = location.StoreID

# Check for duplicate warehouse codes
SELECT WarehouseCode, COUNT(WarehouseCode) AS WarehouseCodeCount
FROM `us-sales-data.sales.sales-merged` 
GROUP BY WarehouseCode

# Orders per Warehouse

SELECT WarehouseCode, COUNT(WarehouseCode) AS WarehouseCodeCount
FROM `us-sales-data.sales.sales-merged` 
GROUP BY WarehouseCode

# Check different currencies used

SELECT CurrencyCode, COUNT(CurrencyCode) AS CurrencyCodeCount
FROM `us-sales-data.sales.sales-merged` 
GROUP BY CurrencyCode

# Order Quantity Ascending

SELECT Order_Quantity, 
FROM `us-sales-data.sales.sales-merged` 
ORDER BY Order_Quantity ASC

# Unit Price Ascending
SELECT Unit_Price, 
FROM `us-sales-data.sales.sales-merged` 
ORDER BY Unit_Price ASC

# View different sales channels 
SELECT Product_Name, COUNT(Product_Name) AS Product_Name_Count 
FROM `us-sales-data.sales.sales-merged` 
GROUP BY Product_Name
ORDER BY Product_Name

# View different products
SELECT Product_Name, COUNT(Product_Name) AS Product_Name_Count 
FROM `us-sales-data.sales.sales-merged` 
GROUP BY Product_Name
ORDER BY Product_Name

# View different states

SELECT State, COUNT(State) AS State_Count 
FROM `us-sales-data.sales.sales-merged` 
GROUP BY State
ORDER BY State

# View Different Costs
SELECT Unit_Cost 
FROM `us-sales-data.sales.sales-merged` 
ORDER BY Unit_Cost

# Create Total_Price column, Gross_Profit column, Gross_Margin column and ShippingTime column

SELECT * ,
    ROUND(Unit_Price * Order_Quantity * (1-Discount_Applied),2) AS Total_Price,
    ROUND(Unit_Cost * Order_Quantity,2) AS Total_Cost,   
    ROUND((Unit_Price * Order_Quantity* (1-Discount_Applied)) - (Unit_Cost * Order_Quantity),2) AS Gross_Profit,
    ROUND(((Unit_Price * Order_Quantity* (1-Discount_Applied)) - (Unit_Cost * Order_Quantity))/(Unit_Price * Order_Quantity * (1-Discount_Applied))*100,2) AS Gross_Margin,
    DATE_DIFF(DeliveryDate,OrderDate,DAY) AS ShippingTime
FROM 
    `us-sales-data.sales.sales-merged` 
