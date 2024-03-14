# SQL_Ecommerce_Project

## 1. Introduction:

This e-commerce dataset is stored in a public Google BigQuery dataset. This dataset contains information about user sessions on a website collected from Google Analytics in 2017.

Based on this dataset, the author performs queries to analyze website activity in 2017, such as calculating bounce rate per traffic source, define average amount of money spent per sesssion, analyzing user behavior on pages and various other types of analysis. This project aims to have an overview on the business situation, measure the effectiveness of customer purchasing activities through the company website.

To query and solve these business questions, the author uses the Google BigQuery tool to write and execute SQL queries.

## 2. Objectives of the project:

* Overview of customers' activities on the company website
* Analysis of bounce rates
* Analysis of revenue
* Analysis of transactions

## 3. Import dataset:

The Ecommerce dataset is stored in a public Google BigQuery dataset. To access the dataset, follow these steps:

* Log in to your Google Cloud Platform account and create a new project, rename it what you want.
* Navigate to the BigQuery console and select your newly created project.
* Select the "Type to search" in the navigation panel on the left and then type "bigquery-public-data.google_analytics_sample.ga_sessions" and click "Enter".
* Click on the "ga_sessions_" table to open it.

## 4. Explain the dataset:

| Field Name                       | Data Type | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|----------------------------------|-----------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| fullVisitorId                    | STRING    | The unique visitor ID.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| date                             | STRING    | The date of the session in YYYYMMDD format.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| totals                           | RECORD    | This section contains aggregate values across the session.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| totals.bounces                   | INTEGER   | Total bounces (for convenience). For a bounced session, the value is 1, otherwise it is null.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| totals.hits                      | INTEGER   | Total number of hits within the session.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| totals.pageviews                 | INTEGER   | Total number of pageviews within the session.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| totals.visits                    | INTEGER   | The number of sessions (for convenience). This value is 1 for sessions with interaction events. The value is null if there are no interaction events in the session.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| totals.transactions              | INTEGER   | Total number of ecommerce transactions within the session.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| trafficSource.source             | STRING    | The source of the traffic source. Could be the name of the search engine, the referring hostname, or a value of the utm_source URL parameter.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| hits                             | RECORD    | This row and nested fields are populated for any and all types of hits.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| hits.eCommerceAction             | RECORD    | This section contains all of the ecommerce hits that occurred during the session. This is a repeated field and has an entry for each hit that was collected.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| hits.eCommerceAction.action_type | STRING    | The action type. Click through of product lists = 1, Product detail views = 2, Add product(s) to cart = 3, Remove product(s) from cart = 4, Check out = 5, Completed purchase = 6, Refund of purchase = 7, Checkout options = 8, Unknown = 0. Usually this action type applies to all the products in a hit, with the following exception: when hits.product.isImpression = TRUE, the corresponding product is a product impression that is seen while the product action is taking place (i.e., a "product in list view"). Example query to calculate number of products in list views: SELECT COUNT(hits.product.v2ProductName) FROM [foo-160803:123456789.ga_sessions_20170101] WHERE hits.product.isImpression == TRUE Example query to calculate number of products in detailed view: SELECT COUNT(hits.product.v2ProductName), FROM [foo-160803:123456789.ga_sessions_20170101] WHERE hits.ecommerceaction.action_type = '2' AND ( BOOLEAN(hits.product.isImpression) IS NULL OR BOOLEAN(hits.product.isImpression) == FALSE ) |
| hits.product                     | RECORD    | This row and nested fields will be populated for each hit that contains Enhanced Ecommerce PRODUCT data.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| hits.product.productQuantity     | INTEGER   | The quantity of the product purchased.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| hits.product.productRevenue      | INTEGER   | The revenue of the product, expressed as the value passed to Analytics multiplied by 10^6 (e.g., 2.40 would be given as 2400000).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| hits.product.productSKU          | STRING    | Product SKU.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| hits.product.v2ProductName       | STRING    | Product Name.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| fullVisitorId                    | STRING    | The unique visitor ID.                                                                                

## 5. Business questions and code to solve:

### 5.1 Calculate total visit, pageview, transaction for Jan, Feb and March 2017 (order by month)

```sql
SELECT format_date("%Y%m", parse_date("%Y%m%d", date)) as year_month 
      ,sum(totals.visits) as visits
      ,sum(totals.pageviews) as pageviews
      ,sum(totals.transactions) as transactions
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`  
where _table_suffix between '0101' and '0331' 
group by year_month
order by year_month;
```
| Row |  year_month   |  visits   |  pageviews   |  transactions   |
|-----|---------------|-----------|--------------|-----------------|
|   1 | 201701        |     64694 |       257708 |             713 |
|   2 | 201702        |     62192 |       233373 |             733 |
|   3 | 201703        |     69931 |       259522 |             993 |

### 5.2 Bounce rate per traffic source in July 2017 (Bounce_rate = num_bounce/total_visit) (order by total_visit DESC)

```sql
SELECT trafficSource.source,
       sum(totals.visits) as total_visit, 
       sum(totals.bounces) as total_no_of_bounces,
      (sum(totals.bounces)/sum(totals.visits))*100.0 as bounce_rate
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_201707*` 
Group by trafficSource.source 
--having sum(totals.bounces) is not null 
order by total_visit DESC;
```
| Row |  source              |  total_visit  |  total_no_of_bounces  |  bounce_rate       |
|-----|----------------------|---------------|-----------------------|--------------------|
|   2 | (direct)             |         19891 |                  8606 | 43.265798602382986 |
|   3 | youtube.com          |          6351 |                  4238 | 66.729648874193032 |
|   4 | analytics.google.com |          1972 |                  1064 |   53.9553752535497 |
|   5 | Partners             |          1788 |                   936 | 52.348993288590606 |
|   6 | m.facebook.com       |           669 |                   430 |  64.27503736920778 |
|   7 | google.com           |           368 |                   183 | 49.728260869565219 |
|   8 | dfa                  |           302 |                   124 | 41.059602649006621 |
|   9 | sites.google.com     |           230 |                    97 | 42.173913043478265 |
|  10 | facebook.com         |           191 |                   102 |     53.40314136125 |

### 5.3 Revenue by traffic source by week, by month in June 2017

```sql
Select * 
FROM 
(select 'week' as type_time, 
      format_date('%Y%W',PARSE_DATE('%Y%m%d',date)) as time, 
      trafficSource.source, 
     sum(product.productRevenue)/1000000 as Prd_Revenue 
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_201706*`, 
UNNEST (hits) hits, 
UNNEST (hits.product) product 
WHERE product.productRevenue is not null 
Group by format_date('%Y%W',PARSE_DATE('%Y%m%d',date)), 
      trafficSource.source 
Union all 
select 'month' as type_time, 
      format_date('%Y%m',PARSE_DATE('%Y%m%d',date)) as time, 
      trafficSource.source, 
     sum(product.productRevenue)/1000000 as Prd_Revenue 
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_201706*`, 
UNNEST (hits) hits, 
UNNEST (hits.product) product 
WHERE product.productRevenue is not null 
Group by format_date('%Y%m',PARSE_DATE('%Y%m%d',date)), 
      trafficSource.source) 
Order by Prd_Revenue DESC;
```
| Row |  type_time  |  time  |  source  |  Prd_Revenue  |
|-----|-------------|--------|----------|---------------|
|   1 | month       | 201706 | (direct) |  97333.619695 |
|   2 | week        | 201724 | (direct) |  30908.909927 |
|   3 | week        | 201725 | (direct) |  27295.319924 |
|   4 | month       | 201706 | google   |   18757.17992 |
|   5 | week        | 201723 | (direct) |  17325.679919 |
|   6 | week        | 201726 | (direct) |   14914.80995 |
|   7 | week        | 201724 | google   |   9217.169976 |
|   8 | month       | 201706 | dfa      |   8862.229996 |
|   9 | week        | 201722 | (direct) |   6888.899975 |
|  10 | week        | 201726 | google   |   5330.569964 |

### 5.4 Average number of pageviews by purchaser type (purchasers vs non-purchasers) in June, July 2017

```sql
with 
purchaser_data as(
  select
      format_date("%Y%m",parse_date("%Y%m%d",date)) as month,
      (sum(totals.pageviews)/count(distinct fullvisitorid)) as avg_pageviews_purchase,
  from `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`
    ,unnest(hits) hits
    ,unnest(product) product
  where _table_suffix between '0601' and '0731'
  and totals.transactions>=1
  and product.productRevenue is not null
  group by month
),

non_purchaser_data as(
  select
      format_date("%Y%m",parse_date("%Y%m%d",date)) as month,
      sum(totals.pageviews)/count(distinct fullvisitorid) as avg_pageviews_non_purchase,
  from `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`
      ,unnest(hits) hits
    ,unnest(product) product
  where _table_suffix between '0601' and '0731'
  and totals.transactions is null
  and product.productRevenue is null
  group by month
)

select
    pd.*,
    avg_pageviews_non_purchase
from purchaser_data pd
full join non_purchaser_data using(month)
order by pd.month;
```
| Row |  time  |  avg_pageviews_purchase  |  avg_pageviews_non_purchase  |
|-----|--------|--------------------------|------------------------------|
|   1 | 201706 |        94.02050113895217 |           316.86558846341671 |
|   2 | 201707 |       124.23755186721992 |             334.056559795680 |

### 5.5 Average number of transactions per user that made a purchase in July 2017

```sql
SELECT    
      Format_date('%Y%m',PARSE_DATE('%Y%m%d',date)) as time, 
      sum(totals.transactions)/count(distinct fullVisitorId) as Avg_total_transactions_per_user 
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_201707*`, 
UNNEST (hits) hits, 
UNNEST (hits.product) product 
where totals.transactions >=1 and product.productRevenue is not null 
Group by 1;
```
| Row |  time  |  Avg_total_transactions_per_user  |
|-----|--------|-----------------------------------|
|   1 | 201707 |                  4.16390041493776 |

### 5.6 Average amount of money spent per session. Only include purchaser data in July 2017

```sql
SELECT    
      Format_date('%Y%m',PARSE_DATE('%Y%m%d',date)) as time, 
      round((sum(product.productRevenue)/count(totals.visits))/1000000,2) as avg_revenue_by_user_per_visit 
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_201707*`, 
UNNEST (hits) hits, 
UNNEST (hits.product) product 
where totals.transactions is not null 
and product.productRevenue is not null 
Group by  1;
```
| Row |  time   |  avg_revenue_by_user_per_visit  |
|-----|---------|---------------------------------|
|   1 | 201707  |                           43.86 |

### 5.7 Other products purchased by customers who purchased product "YouTube Men's Vintage Henley" in July 2017. Output should show product name and the quantity was ordered

```sql
select product.v2ProductName, 
      sum(product.productQuantity) as Quantity 
from `bigquery-public-data.google_analytics_sample.ga_sessions_201707*`, 
UNNEST (hits) hits,   
UNNEST (hits.product) product  
Where fullVisitorId in( 
                        SELECT fullVisitorId 
                        FROM `bigquery-public-data.google_analytics_sample.ga_sessions_201707*`,   
                        UNNEST (hits) hits,   
                        UNNEST (hits.product) product 
                        where product.v2ProductName="YouTube Men's Vintage Henley" 
                        and product.productRevenue is not null 
                        and totals.transactions >=1) 
and product.v2ProductName !="YouTube Men's Vintage Henley" 
and product.productRevenue is not null 
and totals.transactions >=1 
Group by product.v2ProductName 
Order by Quantity DESC;
```
| Row |  v2ProductName                                   |  Quantity  |
|-----|--------------------------------------------------|------------|
|   1 | Google Sunglasses                                |         20 |
|   2 | Google Women's Vintage Hero Tee Black            |          7 |
|   3 | SPF-15 Slim & Slender Lip Balm                   |          6 |
|   4 | Google Women's Short Sleeve Hero Tee Red Heather |          4 |
|   5 | Google Men's Short Sleeve Badge Tee Charcoal     |          3 |
|   6 | YouTube Men's Fleece Hoodie Black                |          3 |
|   7 | Android Wool Heather Cap Heather/Black           |          2 |
|   8 | Red Shine 15 oz Mug                              |          2 |
|   9 | Google Men's Short Sleeve Hero Tee Charcoal      |          2 |
|  10 | YouTube Twill Cap                                |          2 |

### 5.8 Calculate cohort map from product view to addtocart to purchase in Jan, Feb and March 2017. For example, 100% product view then 40% add_to_cart and 10% purchase

```sql
with product_data as(
select
    format_date('%Y%m', parse_date('%Y%m%d',date)) as month,
    count(CASE WHEN eCommerceAction.action_type = '2' THEN product.v2ProductName END) as num_product_view,
    count(CASE WHEN eCommerceAction.action_type = '3' THEN product.v2ProductName END) as num_add_to_cart,
    count(CASE WHEN eCommerceAction.action_type = '6' and product.productRevenue is not null THEN product.v2ProductName END) as num_purchase
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_*`
,UNNEST(hits) as hits
,UNNEST (hits.product) as product
where _table_suffix between '20170101' and '20170331'
and eCommerceAction.action_type in ('2','3','6')
group by month
order by month
)

select
    *,
    round(num_add_to_cart/num_product_view * 100, 2) as add_to_cart_rate,
    round(num_purchase/num_product_view * 100, 2) as purchase_rate
from product_data;
```
| Row |  month   |  num_product_view  |  num_addtocart   |  num_purchase  |  add_to_cart_rate  |  purchase_rate  |
|-----|----------|--------------------|------------------|----------------|--------------------|-----------------|
|   1 | 201701   |              25787 |             7342 |           2143 |              28.47 |            8.31 |
|   2 | 201702   |              21489 |             7360 |           2060 |              34.25 |            9.59 |
|   3 | 201703   |              23549 |             8782 |           2977 |              37.29 |           12.64 |
