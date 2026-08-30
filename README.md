OLIST E-COMMERCE SALES & BUSINESS PERFORMANCE ANALYSIS 
==============================================================

## PURPOSE OF THIS FILE

This is a quick-reference file for understanding the project without
opening the Power BI presentation.

Use it for revision before an interview or when reviewing the project
months later.

For every measure, remember four things:

1.  What does it mean?
2.  Why did I create it?
3.  What business question does it answer?
4.  Where is it used?

## PROJECT STACK

Dataset: Olist Brazilian E-Commerce Public Dataset

Database: PostgreSQL

Reporting: Power BI

Connection: DirectQuery

Main reporting view: bi_fact_sales

Supporting views: - bi_dim_product - bi_fact_order -
bi_fact_review_latest - bi_payments_order

Date table: DimDate

## 1.PROJECT IN ONE MINUTE


This project analyzes an e-commerce business using PostgreSQL and Power
BI.

The source data is split into business tables such as: - Customers -
Orders - Order Items - Products - Sellers - Payments - Reviews -
Geolocation - Product Category Translation

PostgreSQL is used to store and prepare the data and create BI views.
Power BI connects through DirectQuery and is used for DAX measures,
interactive analysis and reporting.

The project answers business questions about:

SALES - Revenue - Orders - Customers - AOV - Growth

PRODUCTS - Category performance - Product/category revenue - Orders -
AOV - Ratings

CUSTOMERS - Geographic revenue - State/city performance

LOGISTICS - Delivery time - Late orders - On-time percentage

REVIEWS - Average rating - Positive reviews - Negative reviews - Ratings
by category - Late vs on-time ratings

PAYMENTS - Payment value - Payment type share - Installments

SELLERS - Top sellers - Revenue per seller - Seller geography -
Seller/category performance

## 2. THE MOST IMPORTANT MEASURES


## REVENUE

DAX: Revenue := SUM ( 'public bi_fact_sales'\[price\] )

What it means: Total product price/sales value in the reporting view.

Business question: "How much sales value did the business generate?"

Used for: - Executive Overview - Revenue trends - Category analysis -
Customer geography - Seller analysis - Many other visuals

Important: In this project, Revenue is based on the "price" field.

  ---------------------------------
  FREIGHT
  ---------------------------------
  DAX: Freight := SUM ( 'public
  bi_fact_sales'\[freight_value\] )

  What it means: Total freight
  value.

  Business question: "How much
  freight value is associated with
  the sales data?"
  ---------------------------------

## GMV

DAX: GMV := [Revenue](#revenue) + \[Freight\]

What it means: The project's definition of gross merchandise value is
product revenue plus freight.

Business question: "What is the total value represented by product price
plus freight?"

Important: This is the project's definition. GMV can be defined
differently in different companies.

  ----------------------------
  ORDERS
  ----------------------------
  DAX: Orders := DISTINCTCOUNT
  ( 'public
  bi_fact_sales'\[order_id\] )

  What it means: Number of
  unique orders.

  Why DISTINCTCOUNT? Because
  one order can contain
  multiple order-item rows.

  Business question: "How many
  orders did customers place?"

  This is a very important
  measure.

  Do NOT use COUNTROWS on the
  sales view to represent
  orders if there can be
  multiple items per order.
  ----------------------------

## CUSTOMERS

DAX: Customers := DISTINCTCOUNT ( 'public
bi_fact_sales'\[customer_unique_id\] )

What it means: Number of unique customers.

Business question: "How many distinct customers are represented in the
analysis?"

Important: The project uses customer_unique_id rather than customer_id
for customer counting.

  -----------------
  ITEMS SOLD
  -----------------
  DAX: Items Sold
  := COUNTROWS (
  'public
  bi_fact_sales' )

  What it means:
  Number of rows in
  the sales view.

  In this project
  the sales view is
  item-level, so
  this represents
  sales-item
  records.

  Business
  question: "How
  many item records
  are represented?"

  Do not
  automatically
  call this "units
  sold" unless the
  source structure
  supports that
  interpretation.
  -----------------

## AOV

AOV = Average Order Value

DAX: AOV := DIVIDE ( [Revenue](#revenue), \[Orders\] )

What does AOV stand for? Average Order Value.

What does it tell us? How much revenue the average order generates.

Formula: AOV = Revenue / Orders

Example: Revenue = 100,000 Orders = 1,000 AOV = 100

Business use: AOV helps explain whether revenue is changing because: -
the number of orders changed OR - the value per order changed

Very important interview example:

"Suppose revenue falls but orders stay almost the same. I would check
AOV. If AOV also falls, the problem may be lower basket/order value."

  ----------------------------------------------------------------
  ITEMS PER ORDER
  ----------------------------------------------------------------
  DAX: Items per Order := DIVIDE ( \[Items Sold\], \[Orders\] )

  What it means: Average number of item records per order.

  Business question: "How many items are associated with an
  average order?"

  Business use: Helps understand basket size.

 
 ##  3. CUSTOMER / SERVICE METRICS


  AVG RATING
  ----------------------------------------------------------------

DAX: Avg Rating := AVERAGE ( 'public bi_fact_sales'\[review_score\] )

What it means: Average available review score.

Business question: "How are customers rating the products/orders?"

Important: This is a review-score average, not a general measure of all
customer satisfaction.

  --------------------------------
  DELIVERED ORDERS
  --------------------------------
  DAX: Delivered Orders :=
  CALCULATE ( \[Orders\], 'public
  bi_fact_sales'\[order_status\] =
  "delivered" )

  What it means: Number of unique
  orders with delivered status.

  Business question: "How many
  orders were actually delivered?"
  --------------------------------

## LATE ORDERS

DAX: Late Orders := CALCULATE ( \[Orders\], 'public
bi_fact_sales'\[order_status\] = "delivered", 'public
bi_fact_sales'\[is_late\] = 1 )

What it means: Delivered orders whose actual delivery was after the
estimated delivery date.

Business question: "How many delivered orders were late?"

  ------------------------
  ON-TIME %
  ------------------------
  DAX: On-time % := DIVIDE
  ( \[Delivered Orders\] -
  [Late
  Orders](#late-orders),
  \[Delivered Orders\] )

  What it means:
  Percentage of delivered
  orders that were not
  late.

  Formula: On-time % =
  (Delivered Orders - Late
  Orders) / Delivered
  Orders

  Business question: "How
  reliably are orders
  being delivered within
  the estimated date?"

  Business use: This is a
  service-level KPI.

  If it goes down: -
  investigate late
  orders - investigate
  state/category
  patterns - investigate
  logistics issues
  ------------------------

## LATE %

DAX: Late % := DIVIDE( [Late Orders](#late-orders), \[Delivered Orders\]
)

What it means: Percentage of delivered orders that were late.

Relationship: On-time % + Late % = approximately 100% when both use the
same delivered-order population.

  ----------------------------------------------------------------
  AVG DELIVERY DAYS
  ----------------------------------------------------------------
  DAX: Avg Delivery Days := AVERAGE ( 'public
  bi_fact_sales'\[delivery_days\] )

  What it means: Average number of delivery days represented by
  the reporting data.

  Business question: "How long does delivery take on average?"

  Use with: - Category - State - Time period

  
  ##  4. TIME & GROWTH METRICS
  

  REVENUE YTD
  ----------------------------------------------------------------

DAX: Revenue YTD := TOTALYTD ( [Revenue](#revenue), DimDate\[Date\] )

YTD stands for: Year To Date.

What it means: Revenue accumulated from the beginning of the selected
year to the current date in the visual/filter context.

Business question: "How much revenue have we generated so far this
year?"

  ----------------------
  REVENUE LY
  ----------------------
  DAX: Revenue LY :=
  CALCULATE (
  [Revenue](#revenue),
  SAMEPERIODLASTYEAR (
  DimDate\[Date\] ) )

  LY stands for: Last
  Year.

  What it means: Revenue
  for the comparable
  previous-year period.

  Business question:
  "How did we perform
  during the equivalent
  period last year?"
  ----------------------

## YOY %

DAX: YoY % := DIVIDE ( [Revenue](#revenue) - \[Revenue LY\], \[Revenue
LY\] )

YoY stands for: Year over Year.

What it means: Percentage change in revenue compared with the previous
year.

Formula: (Current Revenue - Previous Year Revenue) / Previous Year
Revenue

Business question: "Is revenue growing or declining compared with last
year?"

  ----------------------------------------------------------------
  ROLLING 30D REVENUE
  ----------------------------------------------------------------
  DAX: Rolling 30D Revenue := CALCULATE( [Revenue](#revenue),
  DATESINPERIOD ( DimDate\[Date\], MAX ( DimDate\[Date\] ), -30,
  DAY ) )

  What it means: Revenue during the latest 30-day period relative
  to the current date context.

  Business question: "What has recent 30-day revenue performance
  looked like?"

  Why useful: It smooths the focus onto recent performance instead
  of only using one month.

  
 ##  5. REVIEW METRICS


  POSITIVE REVIEWS %
  ----------------------------------------------------------------

DAX concept: Count rows with review_score \>= 4 divided by count of
nonblank review scores.

What it means: Share of available review records that are positive.

Business question: "What percentage of reviewed orders received a strong
rating?"

Positive: 4 or 5

  ------------------
  NEGATIVE REVIEWS %
  ------------------
  DAX concept: Count
  rows with
  review_score \<= 2
  divided by count
  of nonblank review
  scores.

  What it means:
  Share of available
  review records
  that are negative.

  Negative: 1 or 2

  Business question:
  "How large is the
  negative-review
  group?"
  ------------------

## REVIEW BUCKET

Categories: - No Review - Positive (4-5) - Neutral (3) - Negative (1-2)

Why: Turns numeric review scores into business-friendly groups.

Used in: Rating Distribution donut chart.

##  6. PAYMENT METRICS

## PAYMENT VALUE

DAX: Payment Value := SUM ( 'public bi_fact_sales'\[payment_value\] )

What it means: Total payment value represented in the sales reporting
view.

Business question: "How much payment value is represented by the
selected payment context?"

Important: Payment Value is not the same field as Revenue.

Revenue uses: price

Payment Value uses: payment_value

  ----------------------------------------------------------------
  AVG INSTALLMENTS
  ----------------------------------------------------------------
  DAX: Avg Installments := AVERAGE ( 'public
  bi_fact_sales'\[payment_installments\] )

  What it means: Average number of installments in the payment
  records.

  Business question: "How are payment installments distributed
  across payment types?"

  Use in: Payments page.

  
 ##  7. SELLER METRICS


  SELLERS
  ----------------------------------------------------------------

DAX: Sellers := DISTINCTCOUNT ( 'public bi_fact_sales'\[seller_id\] )

What it means: Number of distinct sellers represented.

Business question: "How many sellers are contributing to the selected
data?"

  ----------------------------------------------------------------
  REVENUE PER SELLER
  ----------------------------------------------------------------
  DAX: Revenue per Seller := DIVIDE ( [Revenue](#revenue),
  [Sellers](#sellers) )

  What it means: Average revenue per seller in the current filter
  context.

  Formula: Revenue / Sellers

  Business question: "How much revenue is generated per seller on
  average?"

  Important: This is an average, not the revenue of every
  individual seller.

  
  ## 8. DRILLTHROUGH MEASURE / TITLE
  
  DRILL TITLE
  ----------------------------------------------------------------

Concept: "Drillthrough:" & COALESCE( SELECTEDVALUE(category), "All" )

What it does: Changes the title based on the selected category.

Example: If category = watches_gifts

Title: Drillthrough: watches_gifts

Why: Helps the user understand which selection is currently being
analyzed.
