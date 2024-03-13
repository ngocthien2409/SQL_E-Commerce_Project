# SQL_Ecommerce_Project

## Table of contents:

## 1.Introduction:

This e-commerce dataset is stored in a public Google BigQuery dataset. This dataset contains information about user sessions on a website collected from Google Analytics in 2017.

Based on this dataset, the author performs queries to analyze website activity in 2017, such as calculating bounce rate per traffic source, define average amount of money spent per sesssion, analyzing user behavior on pages and various other types of analysis. This project aims to have an overview on the business situation, measure the effectiveness of customer purchasing activities through the company website.

To query and solve these business questions, the author uses the Google BigQuery tool to write and execute SQL queries.

## 2.Objectives of the project:

* Overview of customers' activities on the company website
* Analysis of bounce rates
* Analysis of revenue
* Analysis of transactions

## 3.Import dataset:

The Ecommerce dataset is stored in a public Google BigQuery dataset. To access the dataset, follow these steps:

* Log in to your Google Cloud Platform account and create a new project, rename it what you want.
* Navigate to the BigQuery console and select your newly created project.
* Select the "Type to search" in the navigation panel on the left and then type "bigquery-public-data.google_analytics_sample.ga_sessions" and click "Enter".
* Click on the "ga_sessions_" table to open it.

## 4.Explain the dataset:

Field Name	^rData Type	^cDescription
fullVisitorId	STRING	
The unique visitor ID.
date	STRING	
The date of the session in YYYYMMDD format.
totals	RECORD	
This section contains aggregate values across the session.
totals.bounces	INTEGER	
Total bounces (for convenience). For a bounced session, the value is 1, otherwise it is null.
totals.hits	INTEGER	
Total number of hits within the session.
totals.pageviews	INTEGER	
Total number of pageviews within the session.
totals.visits	INTEGER	
The number of sessions (for convenience). This value is 1 for sessions with interaction events. The value is null if there are no interaction events in the session.
totals.transactions	INTEGER	
Total number of ecommerce transactions within the session.
trafficSource.source	STRING	
The source of the traffic source. Could be the name of the search engine, the referring hostname, or a value of the utm_source URL parameter.
hits	RECORD	
This row and nested fields are populated for any and all types of hits.
hits.eCommerceAction	RECORD	
This section contains all of the ecommerce hits that occurred during the session. This is a repeated field and has an entry for each hit that was collected.
hits.eCommerceAction.action_type	STRING	
The action type. Click through of product lists = 1, Product detail views = 2, Add product(s) to cart = 3, Remove product(s) from cart = 4, Check out = 5, Completed purchase = 6, Refund of purchase = 7, Checkout options = 8, Unknown = 0.
Usually this action type applies to all the products in a hit, with the following exception: when hits.product.isImpression = TRUE, the corresponding product is a product impression that is seen while the product action is taking place (i.e., a "product in list view").
Example query to calculate number of products in list views:
SELECT
COUNT(hits.product.v2ProductName)
FROM [foo-160803:123456789.ga_sessions_20170101]
WHERE hits.product.isImpression == TRUE
Example query to calculate number of products in detailed view:
SELECT
COUNT(hits.product.v2ProductName),
FROM
[foo-160803:123456789.ga_sessions_20170101]
WHERE
hits.ecommerceaction.action_type = '2'
AND ( BOOLEAN(hits.product.isImpression) IS NULL OR BOOLEAN(hits.product.isImpression) == FALSE )
hits.product	RECORD	
This row and nested fields will be populated for each hit that contains Enhanced Ecommerce PRODUCT data.
hits.product.productQuantity	INTEGER	
The quantity of the product purchased.
hits.product.productRevenue	INTEGER	
The revenue of the product, expressed as the value passed to Analytics multiplied by 10^6 (e.g., 2.40 would be given as 2400000).
hits.product.productSKU	STRING	Product SKU.
hits.product.v2ProductName	STRING	Product Name.
fullVisitorId	STRING	
The unique visitor ID.
