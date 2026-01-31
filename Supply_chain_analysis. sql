create database sdb;
use sdb;
create table sales_data (Product char(30) ,
Brand char(30),	
Product_Code varchar(30),
Product_Specification varchar(350),	
Price int,	
Inward_Date	date,
Dispatch_Date date,	
Quantity_Sold int,
Customer_Name	char(40),
Customer_Location varchar(40),	
Region	varchar(14),
Cp_unit VARCHAR(13),
Processor_Specification	varchar(40),
RAM	varchar(10),
ROM	varchar(10),
SSD varchar(10)
);
select *from sales_data;
SET GLOBAL LOCAL_INFILE=ON;
load data local infile 'C:/Users/admin/OneDrive/Documents/Supply chain project/m&s data.csv'
INTO TABLE sales_data FIELDS TERMINATED BY','
ignore 1 lines;
-- kpi's 
-- total_revenue
CREATE VIEW REVENUE AS
select sum(price*Quantity_sold) as total_revenue,
	sum(Quantity_sold) as total_quantity from sales_data;
    SELECT *FROM REVENUE;
    
-- PRODUCT-WISE DEMAND 
CREATE VIEW PRODUCT_WISE_DEMAND AS
SELECT  PRODUCT,
sum(QUANTITY_SOLD) AS TOTAL_QUANTITY
 FROM SALES_DATA GROUP BY PRODUCT 
 ORDER BY TOTAL_QUANTITY DESC; 
 SELECT *FROM product_wise_demand;
 
 -- REGION WISE SALES
 CREATE VIEW REGION_WISE_SALES AS
 SELECT REGION,sum(PRICE*QUANTITY_SOLD) AS TOTAL_REVENUE
   FROM SALES_DATA GROUP BY REGION 
        ORDER BY TOTAL_REVENUE DESC;
        SELECT *FROM REGION_WISE_SALES;
        
-- AVERAGE LEAD TIME
CREATE VIEW AVERAGE_LEAD_TIME AS 
SELECT AVG(DATEDIFF(DISPATCH_DATE,INWARD_DATE)) AS AVG_LEAD_TIME FROM SALES_DATA ; 
SELECT*FROM average_lead_time;

-- lead time distribution 
CREATE VIEW LEAD_TIME_DISTRIBUTION AS
SELECT 
    CASE 
        WHEN DATEDIFF(Dispatch_Date, Inward_Date) <= 15 THEN '0–15 Days'
        WHEN DATEDIFF(Dispatch_Date, Inward_Date) BETWEEN 16 AND 30 THEN '16–30 Days'
        WHEN DATEDIFF(Dispatch_Date, Inward_Date) BETWEEN 31 AND 45 THEN '31–45 Days'
        ELSE '45+ Days'
    END AS lead_time_bucket,
    COUNT(*) AS orders
FROM sales_data
GROUP BY lead_time_bucket ORDER BY lead_time_bucket asc;
SELECT*FROM lead_time_distribution;

-- PRODUCT-WISE LEAD TIME
CREATE VIEW PRODUCT_WISE_LEAD_TIME AS
SELECT PRODUCT,BRAND,
      AVG(DATEDIFF(DISPATCH_DATE,INWARD_DATE)) AS AVG_LEAD_TIME
      FROM SALES_DATA GROUP BY PRODUCT,BRAND
       ORDER BY AVG_LEAD_TIME DESC;
 SELECT *FROM product_wise_lead_time;  
 
-- region wise lead time
CREATE VIEW REGION_WISE_LEAD_TIME AS
SELECT REGION , 
           avg(DATEDIFF(DISPATCH_DATE ,INWARD_DATE)) AS AVG_LEAD_TIME 
           FROM SALES_DATA GROUP BY REGION
           ORDER BY AVG_LEAD_TIME DESC;
SELECT *FROM  region_wise_lead_time;  
        
-- FAST VS SLOW MOVING PRODUCTS
CREATE VIEW FAST_VS_SLOW_MOVING_PRODUCTS AS
SELECT PRODUCT,BRAND,
       sum(QUANTITY_SOLD) AS TOTAL_QUANTITY,
       CASE 
           WHEN sum(QUANTITY_SOLD)>=6500 THEN 'FAST MOVING'
           ELSE 'SLOW MOVING'
           END AS MOVEMENT FROM SALES_DATA 
           GROUP BY PRODUCT,BRAND ;
SELECT*FROM FAST_VS_SLOW_MOVING_PRODUCTS;

--  top 10 products by revenue
CREATE VIEW PRODUCT_BY_REVENUE AS
select  Product, brand,
           sum(price*quantity_sold) as total_revenue 
           from sales_data group by  Product,brand  
           order by total_revenue desc limit 10 ;
 SELECT *FROM product_by_revenue;  
 
    create view  brands as
    select distinct brand from sales_data order by brand asc;
select *from brands;


