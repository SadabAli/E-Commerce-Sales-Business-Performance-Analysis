--copy com
COPY olist_customers_dataset FROM 'Z:/archive/olist_customers_dataset.csv' DELIMITER ',' CSV HEADER;

COPY olist_geolocation_dataset FROM 'Z:/archive/olist_geolocation_dataset.csv' DELIMITER ',' CSV HEADER;

COPY olist_products_dataset FROM 'Z:/archive/olist_products_dataset.csv' DELIMITER ',' CSV HEADER;

COPY olist_sellers_dataset FROM 'Z:/archive/olist_sellers_dataset.csv' DELIMITER ',' CSV HEADER;

COPY olist_orders_dataset FROM 'Z:/archive/olist_orders_dataset.csv' DELIMITER ',' CSV HEADER;

COPY olist_order_items_dataset FROM 'Z:/archive/olist_order_items_dataset.csv' DELIMITER ',' CSV HEADER;

COPY olist_order_payments_dataset FROM 'Z:/archive/olist_order_payments_dataset.csv' DELIMITER ',' CSV HEADER;

COPY olist_order_reviews_dataset FROM 'Z:/archive/olist_order_reviews_dataset.csv' DELIMITER ',' CSV HEADER;

COPY product_category_name_translation FROM 'Z:/archive/product_category_name_translation.csv' DELIMITER ',' CSV HEADER;



\i Z:/archive/olist_geolocation_dataset_sql/olist_geolocation_dataset_part001.sql

\d olist_orders_dataset