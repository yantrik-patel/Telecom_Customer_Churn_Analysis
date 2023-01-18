# Telecom_Customer_Churn_Analysis
In this project, I am analyzing one of the Telecom Company' s customers and their risk of churn. 
The dataset have 39 columns(attributes) and 7043 rows(records). It is open dataset availble on MavenAnalytics website.
First we will start with customer analysis and move on to churn analysis.

so......  

![](https://media.giphy.com/media/XHX9s5YLavonUU4Cbr/giphy.gif)

***  
## A. Customer Analysis  
### (1) Total Revenue in Millions  
```sql
SELECT
	ROUND(SUM(Total_Revenue)/1000000,2) as total_revenue_in_Millions
FROM telecom_customer
```  
![Screenshot 2023-01-18 104829](https://user-images.githubusercontent.com/116425101/213090447-34c04562-ec37-4c07-a3fe-2eb017807b90.png)

### (2) Average Revenue per customer  
```sql
SELECT
	ROUND(AVG(Total_Revenue),2) as average_revenue_per_customer
FROM telecom_customer
```
![Screenshot 2023-01-18 105204](https://user-images.githubusercontent.com/116425101/213090892-a09692b1-8b2d-43b0-9347-93c34f8e8fba.png)
  
### (3) Status wise total customers and their respective revenues   
We have three status categories of customers
- Stayed
- Churned
- Joined

```sql
SELECT
      Customer_Status,
      COUNT(Customer_ID) as total_customers,
      ROUND(COUNT(Customer_ID)*100 / (select count(*) from telecom_customer),2) as percent_of_total_customer,
      ROUND(SUM(Total_Revenue),2) as total_revenue,
      ROUND(SUM(Total_Revenue) *100/ (select SUM(Total_Revenue) from telecom_customer),2) as percent_of_total_revenue
FROM telecom_customer
GROUP BY Customer_Status
```
![Screenshot 2023-01-18 110122](https://user-images.githubusercontent.com/116425101/213092065-08bbe9d7-fbd3-474e-ba8a-821323a00e0c.png)

The company's 82% revenue came from 67% of customer who have stayed so the company have a good quality customer base. But 17% of revenue came from 26% of customer who have churned this is a quite big number which can't be overlooked. we will further investigate Churned Category.  

### (4) Histogram of Age group and respective total number of customers and total revenues  
I have divided age into following categories  
- 30 years and younger
- 31 to 40 Years
- 41 to 50 Years
- 51 to 60 Years
- 61 to 70 Years
- 71 Years and older  
```sql
WITH age_grouping AS 
	(SELECT
          Customer_ID,
          Age,
          Total_Revenue,
          CASE WHEN Age <= 30 THEN '30 years and Younger'
             WHEN Age BETWEEN 31 AND 40 THEN '31 to 40 Years'
             WHEN Age BETWEEN 41 AND 50 THEN '41 to 50 Years'
             WHEN Age BETWEEN 51 AND 60 THEN '51 to 60 Years'
             WHEN Age BETWEEN 61 AND 70 THEN '61 to 70 Years'
             ELSE '71 years and Older' END as age_group
	FROM telecom_customer)
SELECT
	age_group,
    COUNT(Customer_id) as total_customers,
    ROUND(SUM(Total_Revenue),2) as total_revenue
FROM age_grouping
GROUP BY age_group
ORDER BY SUM(Total_Revenue) desc
```
![Screenshot 2023-01-18 112410](https://user-images.githubusercontent.com/116425101/213095291-eb0dbd9c-888c-4a47-904c-3f7e206210e9.png)

### (5) Top 10 Cities with Highest number of customers  
```sql
SELECT
      City,
      COUNT(Customer_ID) as total_customers
FROM telecom_customer
GROUP BY City
ORDER BY COUNT(Customer_ID) desc
LIMIT 10
```  
![Screenshot 2023-01-18 112855](https://user-images.githubusercontent.com/116425101/213096043-631c07f5-b846-4b5d-b5e7-629ec8c568c0.png)

### (6) Contract wise total customers and their respective revenues  
  
We have three categories in Contract 
- Month to Month
- One Year
- Two Year

```sql
SELECT
      Contract,
      COUNT(Customer_ID) as total_customers,
      ROUND(SUM(Total_Revenue),2) as total_revenue,
      ROUND(SUM(Total_Revenue) / COUNT(Customer_ID),2) as average_revenue
FROM telecom_customer
GROUP BY Contract
ORDER BY total_revenue desc
```
![Screenshot 2023-01-18 113620](https://user-images.githubusercontent.com/116425101/213097025-6eb11165-9697-4158-b964-57aa5e4564f8.png)

The result on above table tells a very interesting story. If company can convert "Month to Month" customers into "One Year" or "Two Year" customer, they can be a good assets for driving the major part of revenue. The number of customers in each category shows that "One Year" and "Two Year" customers are lesser but contributes well in total revenue.  
***
## B. Churn Analysis 












