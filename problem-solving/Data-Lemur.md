# **[`DataLemur`](https://datalemur.com/)**

- **EASY**
    - [**[Easy Linkedin](https://datalemur.com/questions/duplicate-job-listings)**] ****Duplicate Job Listings****
        
        ```sql
        -- Solution 1
        SELECT COUNT(DISTINCT j1.company_id) AS co_w_duplicate_jobs 
        FROM job_listings j1
        JOIN job_listings j2 ON j1.company_id=j2.company_id
        WHERE (j1.job_id != j2.job_id) AND
        (j1.title = j2.title) AND 
        (j1.description=j2.description);
        ```
        
        ```sql
        -- Solution 2
        WITH dup_jobs AS (SELECT company_id, COUNT(job_id) 
        FROM job_listings GROUP BY company_id, title, description HAVING COUNT(job_id) > 1)
        SELECT COUNT(*) AS co_w_duplicate_jobs FROM dup_jobs;
        ```
        
    - [**[Easy Tesla](https://datalemur.com/questions/tesla-unfinished-parts)**] ****Unfinished Parts****
        
        ```sql
        SELECT DISTINCT(part)
        FROM parts_assembly
        WHERE (assembly_step IS NOT NULL)
        AND (finish_date IS NULL);
        ```
        
    - [[**Easy Paypal**](https://datalemur.com/questions/final-account-balance)] ****Final Account Balance****
        
        ```sql
        -- Solution 1
        WITH deposit_trans AS (
        SELECT account_id, SUM(amount) AS total_deposit
        FROM transactions WHERE transaction_type='Deposit'
        GROUP BY account_id
        ),
        withdrawal_trans AS (
        SELECT account_id, SUM(amount) AS total_withdrawal
        FROM transactions WHERE transaction_type='Withdrawal'
        GROUP BY account_id
        )
        SELECT d.account_id, (d.total_deposit - w.total_withdrawal) AS final_balance
        FROM deposit_trans d
        JOIN withdrawal_trans w ON d.account_id=w.account_id;
        ```
        
        ```sql
        -- Solution 2
        SELECT account_id,
        SUM(CASE WHEN transaction_type='Deposit' THEN amount
        WHEN transaction_type='Withdrawal' THEN -amount END) AS final_balance
        FROM transactions
        GROUP BY account_id;
        ```
        
    - [**[Easy Twitter]](https://datalemur.com/questions/sql-histogram-tweets) Histogram of Tweets**
        
        ```sql
        WITH tweet_count AS (
        SELECT user_id, COUNT(tweet_id) AS tweet_bucket
        FROM tweets
        WHERE tweet_Date BETWEEN '2022-01-01' AND '2022-12-31'
        GROUP BY user_id
        )
        SELECT tweet_bucket, COUNT(user_id)
        FROM tweet_count
        GROUP BY tweet_bucket;
        ```
        
    - [**[Easy Amazon](https://datalemur.com/questions/sql-avg-review-ratings)**] ****Average Review Ratings****
        
        ```sql
        SELECT
        EXTRACT (MONTH FROM submit_date) AS mth,
        product_id AS product,
        ROUND(AVG(stars), 2) AS avg_stars
        FROM reviews GROUP BY mth, product_id ORDER BY mth, product;
        ```
        
        Check the `EXTRACT` function.
        
    - [****[Easy LinkedIn](https://datalemur.com/questions/linkedin-power-creators)****] ****LinkedIn Power Creators (Part 1)****
        
        ```sql
        SELECT p.profile_id
        FROM personal_profiles p JOIN company_pages c ON p.employer_id=c.company_id
        WHERE p.followers > c.followers ORDER BY p.profile_id;
        ```
        
    - [[**Easy eBay**](https://datalemur.com/questions/sql-highest-products)] ****Highest Number of Products****
        
        ```sql
        SELECT user_id, COUNT(product_id) AS product_num
        FROM user_transactions
        GROUP BY user_id HAVING SUM(spend)>=1000
        ORDER BY COUNT(product_id) DESC, SUM(spend) DESC LIMIT 3;
        ```
        
    - [**********[Easy Microsoft](https://datalemur.com/questions/sql-spare-server-capacity)** ] ****Spare Server Capacity****
        
        ```sql
        WITH demand AS (
        SELECT datacenter_id, SUM(monthly_demand) AS monthly_total_demand
        FROM forecasted_demand GROUP BY datacenter_id
        ),
        demand_capacity AS (
        SELECT a.datacenter_id, a.monthly_total_demand, b.monthly_capacity
        FROM demand a
        JOIN datacenters b
        ON a.datacenter_id=b.datacenter_id
        )
        SELECT datacenter_id, (monthly_capacity - monthly_total_demand) AS spare_capacity
        FROM demand_capacity
        ORDER BY datacenter_id;
        ```
        
    - [**[Easy Microsoft](https://datalemur.com/questions/teams-power-users)**] ****Teams Power Users****
        
        ```sql
        SELECT sender_id, COUNT(message_id) AS message_count
        FROM messages
        WHERE (EXTRACT(YEAR FROM sent_date)=2022) AND
        (EXTRACT(MONTH FROM sent_date)=8)
        GROUP BY sender_id
        ORDER BY message_count DESC LIMIT 2;
        ```
        
    - [**[Easy Yelp](https://datalemur.com/questions/sql-top-businesses)**] ****Top Rated Businesses****
        
        ```sql
        WITH top_rated AS (
        SELECT COUNT(business_id) AS business_count FROM reviews
        WHERE review_stars=4 OR review_stars=5
        )
        SELECT business_count,
        ((business_count*100)/(SELECT COUNT(business_id) FROM reviews)) AS top_rated_pct
        FROM top_rated;
        ```
        
    - [**[Easy Google](https://datalemur.com/questions/ad-campaign-roas)**] ****Ad Campaign ROAS****
        
        ```sql
        SELECT advertiser_id, ROUND(((SUM(revenue)/SUM(spend))::DECIMAL), 2) as ROAS
        FROM ad_campaigns
        GROUP BY advertiser_id
        ORDER BY advertiser_id;
        ```
        
        PostgreSQL requires the input to the `ROUND` function to be a numeric data type, so we need to convert the resulting ROAS to a decimal type before rounding. We can accomplish this using the double-colon `::`conversion, as shown below. You can read more about different numeric types in the PostgreSQL documentation [**here**](https://www.postgresql.org/docs/current/datatype-numeric.html)
        .
        
    - [**[Easy Visa](https://datalemur.com/questions/apple-pay-volume)**] **Apple Pay Volume**
        
        ```sql
        SELECT merchant_id,
        SUM(CASE WHEN LOWER(payment_method) = 'apple pay' THEN transaction_amount
        ELSE 0 END
        ) AS total_transaction
        FROM transactions GROUP BY merchant_id
        ORDER BY total_transaction DESC;
        ```
        
    - [**[Easy Facebook](https://datalemur.com/questions/sql-app-ctr)**] App Click-through Rate (CTR)
        
        ```sql
        SELECT app_id,
        ROUND((100.0*(SUM(CASE WHEN event_type='click' THEN 1 ELSE 0 END))/
        (SUM(CASE WHEN event_type='impression' THEN 1 ELSE 0 END))), 2) AS ctr
        FROM events WHERE EXTRACT(YEAR FROM timestamp)=2022
        GROUP BY app_id;
        ```
        
    - [**[Easy TikTok](https://datalemur.com/questions/second-day-confirmation)**] Second Day Confirmation
        
        ```sql
        -- This solution is simpler than whats given on the site.
        SELECT e.user_id
        FROM emails e JOIN texts t ON e.email_id=t.email_id
        GROUP BY e.user_id HAVING COUNT(t.signup_action)=2;
        ```
        
    - [**[Easy Facebook](https://datalemur.com/questions/sql-average-post-hiatus-1)**] Average Post Hiatus (Part 1)
        
        ```sql
        SELECT user_id,
        DATE_PART('DAY', (MAX(post_date) - MIN(post_date))) AS days_between
        FROM posts
        WHERE EXTRACT(YEAR FROM post_date)=2021
        GROUP BY user_id HAVING DATE_PART('DAY', (MAX(post_date) - MIN(post_date)))>0;
        ```
        
- **MEDIUM**
    - [**[Medium Uber](https://datalemur.com/questions/sql-third-transaction)**] User's Third Transaction
        
        ```sql
        WITH rank_table AS (
        SELECT *, ROW_NUMBER() OVER(PARTITION BY user_id ORDER BY transaction_date) AS rank_row
        FROM transactions)
        SELECT user_id, spend, transaction_date FROM rank_table WHERE rank_row=3;
        ```
        
    - [**[Medium Accenture](https://datalemur.com/questions/compensation-outliers)**] Compensation Outliers
        
        ```sql
        WITH avg_salary AS (
        SELECT title, ROUND(AVG(SALARY), 2) AS av_salary FROM employee_pay 
        GROUP BY title
        ),
        final_table AS(
        SELECT e.employee_id, e.salary,
        CASE
        WHEN e.salary > 2*(a.av_salary) THEN 'Overpaid'
        WHEN e.salary < 0.5*(a.av_salary) THEN 'Underpaid'
        END AS status
        FROM employee_pay e JOIN avg_salary a ON e.title=a.title)
        SELECT * FROM final_table WHERE status IS NOT NULL;
        ```
        
    - [**[Medium Snapchat](https://datalemur.com/questions/time-spent-snaps)**] Sending vs. Opening Snaps
        
        ```sql
        -- Solution 1: Two CTEs
        WITH send AS(
        SELECT b.age_bucket, SUM(a.time_spent) AS total_send
        FROM activities a JOIN age_breakdown b ON a.user_id=b.user_id
        WHERE a.activity_type = 'send'
        GROUP BY age_bucket
        ),
        open AS(
        SELECT b.age_bucket, SUM(a.time_spent) AS total_open
        FROM activities a JOIN age_breakdown b ON a.user_id=b.user_id
        WHERE a.activity_type = 'open'
        GROUP BY age_bucket
        )
        SELECT s.age_bucket,
        ROUND((100.0*s.total_send/(o.total_open+s.total_send)), 2) AS send_perc,
        ROUND((100.0*o.total_open/(o.total_open+s.total_send)), 2) AS open_perc
        FROM send s JOIN open o ON o.age_bucket=s.age_bucket
        ORDER BY s.age_bucket;
        ```
        
        ```sql
        -- Solution 2: Using CASE
        WITH open_send AS(
        SELECT b.age_bucket,
        SUM(CASE WHEN activity_type='send' THEN a.time_spent ELSE 0 END) AS total_send,
        SUM(CASE WHEN activity_type='open' THEN a.time_spent ELSE 0 END) AS total_open
        FROM activities a JOIN age_breakdown b ON a.user_id=b.user_id
        GROUP BY age_bucket
        ORDER BY age_bucket
        )
        SELECT age_bucket,
        ROUND((100.0*total_send/(total_open+total_send)), 2) AS send_perc,
        ROUND((100.0*total_open/(total_open+total_send)), 2) AS open_perc
        FROM open_send;
        ```
        
    - [****************************[Medium Twitter](https://datalemur.com/questions/rolling-average-tweets)****************************] Tweets' Rolling Averages [Imp]
        
        ```sql
        WITH twitter AS (
        SELECT user_id, tweet_date,
        COUNT(DISTINCT tweet_id) AS tweet_count
        FROM tweets
        GROUP BY user_id, tweet_date
        ORDER BY user_id, tweet_date
        )
        SELECT user_id, tweet_date,
        ROUND((AVG(tweet_count)
        OVER(PARTITION BY user_id ROWS BETWEEN 2 PRECEDING AND CURRENT ROW)), 2) AS rolling_avg_3days
        FROM twitter;
        ```
        
        Check ROWS BETWEEN, PRECEDING & CURRENT ROW
        
    - [**[Medium Google](https://datalemur.com/questions/odd-even-measurements)**] Odd and Even Measurements [Imp]
        
        ```sql
        WITH measure AS (
        SELECT CAST(measurement_time AS date), measurement_value,
        RANK() OVER(PARTITION BY CAST(measurement_time AS date) ORDER BY measurement_time) AS time_rank
        FROM measurements
        )
        SELECT measurement_time,
        SUM(CASE WHEN time_rank%2!=0 THEN measurement_value ELSE 0 END) AS odd_sum,
        SUM(CASE WHEN time_rank%2=0 THEN measurement_value ELSE 0 END) AS even_sum
        FROM measure
        GROUP BY measurement_time;
        ```
        
    - [**[Medium Amazon](https://datalemur.com/questions/sql-highest-grossing)**] Highest-Grossing Items
        
        ```sql
        WITH rank_product AS (
        SELECT category, product,
        SUM(spend) AS total_spend,
        DENSE_RANK() OVER(PARTITION BY category ORDER BY SUM(spend) DESC) AS spend_rank
        FROM product_spend
        WHERE DATE_PART('YEAR', transaction_date) = 2022
        GROUP BY category, product
        )
        SELECT category, product, total_spend
        FROM rank_product
        WHERE spend_rank IN (1,2);
        ```
        
    - [**[Medium Etsy](https://datalemur.com/questions/sql-first-transaction)**] First Transaction
        
        ```sql
        WITH transaction_ranked AS (
        SELECT user_id, transaction_date, spend,
        DENSE_RANK() OVER(PARTITION BY user_id ORDER BY transaction_date, transaction_id) AS date_rank
        FROM user_transactions
        ORDER BY user_id, transaction_date, spend
        )
        SELECT COUNT(user_id) AS users 
        FROM transaction_ranked 
        WHERE date_rank=1 AND spend > 50.00;
        ```
        
    - [[**Medium LinkedIn**](https://datalemur.com/questions/linkedin-power-creators-part2)] LinkedIn Power Creators (Part 2)
        
        ```sql
        WITH profile_ranked AS(
        SELECT p.profile_id, p.followers AS p_followers, e.company_id, c.followers AS c_followers,
        DENSE_RANK() OVER(PARTITION BY profile_id ORDER BY c.followers DESC) AS ranking
        FROM personal_profiles p
        JOIN employee_company e ON p.profile_id=e.personal_profile_id
        JOIN company_pages c ON e.company_id=c.company_id
        )
        SELECT profile_id
        FROM profile_ranked WHERE ranking=1 AND p_followers>c_followers
        ORDER BY profile_id;
        ```
        
    - [**[Medium Spotify](https://datalemur.com/questions/top-fans-rank)**] ****Top 5 Artists****
        
        ```sql
        WITH top_artist AS(
        SELECT a.artist_name, COUNT(s.song_id) AS no_of_songs,
        DENSE_RANK() OVER(ORDER BY COUNT(s.song_id) DESC) AS artist_rank
        FROM artists a JOIN songs s ON a.artist_id=s.artist_id
        JOIN global_song_rank g ON s.song_id=g.song_id
        WHERE g.rank<=10
        GROUP BY a.artist_name
        )
        SELECT artist_name, artist_rank
        FROM top_artist
        WHERE artist_rank<=5;
        ```
        
    - [**[Medium TikTok](https://datalemur.com/questions/signup-confirmation-rate)**] ****Signup Confirmation Rate (To-Do)****
        
        ```sql
        -- Solution 1
        WITH confirmed_signup AS(
        SELECT e.*, t.text_id, t.signup_action FROM emails e
        JOIN texts t ON e.email_id=t.email_id
        WHERE t.signup_action='Confirmed'
        )
        SELECT ROUND((COUNT(*)::DECIMAL/(SELECT COUNT(DISTINCT email_id) FROM emails)::DECIMAL),2) AS confirm_rate
        FROM confirmed_signup;
        ```
        
        ```sql
        -- Solution 2
        WITH signup_status AS(
        SELECT e.email_id,
        CASE WHEN signup_action='Confirmed' THEN 1 ELSE 0 END AS status
        FROM emails e JOIN texts t ON e.email_id=t.email_id
        )
        SELECT ROUND(((SUM(status))::DECIMAL/(COUNT(email_id))::DECIMAL),2) AS confirm_rate
        FROM signup_status;
        ```
        
    - [[**Medium Google**](https://datalemur.com/questions/consulting-bench-time)] ****Consulting Bench Time [Imp]**
        
        ```sql
        WITH working_days AS(
        SELECT s.employee_id, (end_date - start_date) AS work_days
        FROM staffing s
        JOIN consulting_engagements c ON s.job_id=c.job_id
        WHERE is_consultant='true'
        )
        SELECT employee_id, (365-(SUM(work_days)+COUNT(*))) AS bench_days
        FROM working_days
        GROUP BY employee_id;
        ```
        
        Notice that I am adding  COUNT(*) to the SUM(work_days), this is to add the inclusive day, that is the last working day.
        
    - [**[Medium Accenture](https://datalemur.com/questions/fill-missing-product)**] ****Fill Missing Client Data [Imp - [FIRST_VALUE](https://www.postgresqltutorial.com/postgresql-window-function/postgresql-first_value-function/)()]**
        
        ```sql
        WITH products2 AS (
        SELECT product_id, category, name,
        COUNT(category) OVER(ORDER BY product_id) AS category_id
        FROM products
        )
        SELECT product_id,
        FIRST_VALUE(category) OVER (PARTITION BY category_id) AS category,
        name
        FROM products2;
        ```
        
    - [**[Medium Spotify](https://datalemur.com/questions/spotify-streaming-history)**] ****Spotify Streaming History****
        
        ```sql
        WITH streaming_history AS (
        SELECT user_id, song_id, song_plays
        FROM songs_history
        UNION ALL
        SELECT user_id, song_id, COUNT(listen_time) AS play_count
        FROM songs_weekly 
        WHERE listen_time < '08/05/2022 00:00:00' 
        GROUP BY user_id, song_id
        )
        SELECT user_id, song_id, SUM(song_plays) AS song_plays
        FROM streaming_history GROUP BY user_id, song_id
        ORDER BY song_plays DESC;
        ```
        
    - [**[Medium Etsy](https://datalemur.com/questions/same-week-purchases)**] ****Same Week Purchases****
        
        ```sql
        WITH early_purchasers AS (
        SELECT DISTINCT s.user_id
        FROM signups s JOIN user_purchases u ON s.user_id=u.user_id
        WHERE DATE_PART('Day', (u.purchase_date - s.signup_date))<=7
        )
        SELECT ROUND((COUNT(*)::DECIMAL ** 100.00 /
        						(SELECT COUNT(DISTINCT user_id) FROM signups)::DECIMAL),2) AS single_purchase_pct
        FROM early_purchasers;
        ```
        
    - [**[Medium Google](https://datalemur.com/questions/invalid-search-pct)**] ****Invalid Search Results****
        
        ```sql
        WITH search_results AS (
        SELECT **,
        ROUND((num_search*invalid_result_pct/100.00),2) AS invalid_search_count
        FROM search_category
        WHERE num_search IS NOT NULL AND invalid_result_pct IS NOT NULL
        )
        SELECT country, SUM(num_search) AS total_search,
        ROUND((SUM(invalid_search_count)/SUM(num_search)*100.00),2) AS invalid_searches_pct
        FROM search_results GROUP BY country
        ORDER BY country;
        ```
        
    - [**[Medium PayPal](https://datalemur.com/questions/money-transfer-relationships)**] ****Unique Money Transfer Relationships [Imp - [INTERSEC](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-intersect/)T]**
        
        ```sql
        WITH common_transactions AS (SELECT payer_id, recipient_id FROM payments
        INTERSECT
        SELECT recipient_id, payer_id FROM payments)
        SELECT COUNT(*)/2 AS unique_relationships FROM common_transactions;
        ```
        
    - [**[Medium Stitch Fix](https://datalemur.com/questions/sql-repeat-purchases)**] ****Repeat Purchases on Multiple Days****
        
        ```sql
        -- Solution 1
        WITH repeat_purchases AS(
        SELECT user_id, product_id,DATE(purchase_date) AS date, COUNT(quantity) AS purchase_count
        FROM purchases
        GROUP BY user_id, product_id, DATE(purchase_date)
        ),
        repeat_purchase_gt1 AS(
        SELECT user_id, product_id, COUNT(date)
        FROM repeat_purchases
        GROUP BY user_id, product_id HAVING COUNT(date)>1
        )
        SELECT COUNT(DISTINCT user_id) AS repeat_purchasers FROM repeat_purchase_gt1;
        ```
        
        ```sql
        -- Solution 2
        WITH repeat_purchases AS(
        SELECT user_id, product_id
        FROM purchases
        GROUP BY user_id, product_id HAVING COUNT(DISTINCT purchase_date::DATE) > 1
        )
        SELECT COUNT(DISTINCT user_id) AS repeat_purchasers FROM repeat_purchases;
        ```
        
    - [**[Medium Amazon](https://datalemur.com/questions/sql-purchasing-activity)**] ****Cumulative Purchases by Product Type****
        
        ```sql
        SELECT order_date, product_type,
        SUM(quantity) OVER (PARTITION BY product_type ORDER BY order_date) AS cum_purchased
        FROM total_trans
        ORDER BY order_date;
        ```
        
    - [**[Medium Adobe](https://datalemur.com/questions/photoshop-revenue-analysis)**] ****Photoshop Revenue Analysis****
        
        ```sql
        -- Solution 1
        SELECT customer_id, SUM(revenue) AS revenue
        FROM adobe_transactions
        WHERE (customer_id IN (SELECT DISTINCT customer_id FROM adobe_transactions 
        											 WHERE product='Photoshop'))
        AND (product!='Photoshop') 
        GROUP BY customer_id ORDER BY customer_id
        ```
        
        ```sql
        -- Solution 2: Better readability!?
        WITH photoshop_customer AS (
        SELECT * 
        FROM adobe_transactions
        WHERE (customer_id IN (SELECT DISTINCT customer_id FROM adobe_transactions 
        											 WHERE product='Photoshop'))
        AND (product!='Photoshop')
        )
        SELECT customer_id, SUM(revenue) AS revenue
        FROM photoshop_customer GROUP BY customer_id ORDER BY customer_id;
        ```
        
    - [**[Medium Amazon](https://datalemur.com/questions/amazon-shopping-spree)**] ****User Shopping Sprees****
        
        ```sql
        -- Solution 1
        SELECT DISTINCT(t1.user_id)
        FROM transactions t1 JOIN transactions t2 ON t1.user_id=t2.user_id
        WHERE t1.transaction_date!=t2.transaction_date
        AND(
        DATE_PART('Day',t2.transaction_date)
        IN (DATE_PART('Day',t1.transaction_date)-1, 
        		DATE_PART('Day',t1.transaction_date)+1, 
        		DATE_PART('Day',t1.transaction_date)+2)
        );
        ```
        
        ```sql
        -- Solution 2
        SELECT DISTINCT(t1.user_id)
        FROM transactions t1 JOIN transactions t2
        ON t1.user_id=t2.user_id AND DATE(t1.transaction_date)=DATE(t2.transaction_date)+1
        JOIN transactions t3 ON t2.user_id=t3.user_id AND DATE(t2.transaction_date)=DATE(t3.transaction_date)+1
        ORDER BY user_id;
        ```
        
    - [****************************[Medium Walmart](https://datalemur.com/questions/frequently-purchased-pairs)****************************] ****Frequently Purchased Pairs****
        
        ```sql
        WITH product_transactions AS(
        SELECT
        t.transaction_id, p.product_id, p.product_name
        FROM transactions t
        JOIN products p ON t.product_id=p.product_id
        )
        SELECT COUNT(*) AS combo_num
        FROM product_transactions t1
        JOIN product_transactions t2 ON t1.transaction_id = t2.transaction_id
        AND t1.product_id > t2.product_id
        ```
        
- **HARD**
    - [**[Hard Facebook](https://datalemur.com/questions/user-retention)**] ****Active User Retention****
        
        ```sql
        WITH june_users AS (
        SELECT user_id FROM user_actions 
        WHERE DATE_PART('Month', event_date)=6
        ),
        july_users AS (
        SELECT user_id, event_date FROM user_actions 
        WHERE DATE_PART('Month', event_date)=7
        )
        SELECT DATE_PART('Month', event_date) AS month, 
        COUNT(DISTINCT jul.user_id) AS monthly_active_users
        FROM june_users jun 
        JOIN july_users jul ON jun.user_id = jul.user_id 
        GROUP BY DATE_PART('Month', event_date);
        ```
        
    - [**[Hard Visa](https://datalemur.com/questions/sql-monthly-merchant-balance)**] ****Monthly Merchant Balance****
        
        ```sql
        WITH month_day_transactions AS(
        SELECT DATE_PART('month', transaction_date) AS transaction_month,
        DATE(transaction_date),
        SUM(CASE WHEN type='deposit' THEN amount
        WHEN type='withdrawal' THEN -amount END) AS day_balance
        FROM transactions
        GROUP BY transaction_month, DATE(transaction_date)
        )
        SELECT date AS transaction_date,
        SUM(day_balance) OVER(PARTITION BY transaction_month ORDER BY date) AS balance
        FROM month_day_transactions;
        ```
        
    - [**[Hard Wayfair](https://datalemur.com/questions/yoy-growth-rate)**] Y-on-Y Growth Rate (had Fun!)
        
        ```sql
        WITH curr_year_transactions AS(
        SELECT DATE_PART('Year', transaction_date) AS year, product_id,
        SUM(spend) AS curr_year_spend
        FROM user_transactions
        GROUP BY DATE_PART('Year', transaction_date), product_id
        ),
        overall_transactions AS(
        SELECT c1.year, c1.product_id, c1.curr_year_spend,
        CASE WHEN c2.curr_year_spend IS NOT NULL THEN c2.curr_year_spend END AS prev_year_spend
        FROM curr_year_transactions c1
        LEFT JOIN curr_year_transactions c2 ON c1.year = c2.year+1 
        																			 AND c1.product_id = c2.product_id
        )
        SELECT *,
        ROUND(((curr_year_spend-prev_year_spend)*100.00/(prev_year_spend)), 2) AS yoy_rate
        FROM overall_transactions
        ORDER BY product_id, year
        ```
        
    - [**[Hard DoorDash](https://datalemur.com/questions/sql-bad-experience)**] ****Bad Delivery Rate (Yours is easy, share this one)****
        
        ```sql
        WITH ordersin14days AS (
        SELECT o.order_id,
        CASE WHEN status!='completed successfully' THEN 1 ELSE 0 END AS status_code
        FROM orders o
        JOIN customers c ON o.customer_id=c.customer_id
        WHERE (DATE_PART('Month', signup_timestamp)=6) AND
              (DATE_PART('Year', signup_timestamp)=2022) AND
              (DATE_PART('Day', (order_timestamp-signup_timestamp))<15)
        )
        SELECT 
        ROUND(((SUM(status_code)::DECIMAL)*100.00/COUNT(*)::DECIMAL),2) AS bad_experience_pct
        FROM ordersin14days;
        ```
        
    - [****************[Hard Amazon](https://datalemur.com/questions/total-utilization-time)****************] ****Server Utilization Time****
        
        ```sql
        WITH grouped_server_utilization AS(
        SELECT server_id, status_time, session_status,
        RANK() OVER(PARTITION BY server_id ORDER BY status_time)
        FROM server_utilization
        ),
        server_uptimes AS(
        SELECT a.server_id, a.status_time, b.status_time AS b_status_time,
        (b.status_time::DATE - a.status_time::DATE) AS uptime_days
        FROM grouped_server_utilization a JOIN grouped_server_utilization b ON a.server_id=b.server_id
        WHERE a.rank+1=b.rank AND a.rank%2!=0
        )
        SELECT SUM(uptime_days) AS total_uptime_days FROM server_uptimes;
        ```
        
        ```sql
        - ## grouped_server_utilization
        -- used RANK() to give status code
        -- Afer applying the RANK(), ODD numbers is assigned to start session_status
        -- and EVEN numbers to stop session_status
        -- Pair of one ODD and EVEN number makes one complete session.
        -- For example: for server_id 1-> (1,2) => (start, stop) 2 days
        -- similarly (3,4) => (start, stop) 0 days, therefore server_id=1 has 2 start-stop sessions
        ```
        
        ```sql
        - ## server_uptimes
        -- Self joining the table
        -- getting only those records where rank+1 = rank of table b.
        -- This way we will avoid matching of similar ranks and also the ranks which fall into other session set.
        -- Also, we are only fetching the records where rank of table 1 is ODD number(a.rank%2!=0)
        -- this way we would only get unique start-stop session(s) for each servers.
        ```
        
    - [**********[Hard Amazon](https://datalemur.com/questions/prime-warehouse-storage)**********] ****Maximize Prime Item Inventory (easy but tedious one!)****
        
        ```sql
        WITH prime_items AS (
        SELECT item_type,
        FLOOR(500000/SUM(square_footage)) * COUNT(item_id) item_count,
        500000 - ROUND((FLOOR(500000/SUM(square_footage)) * SUM(square_footage)),2) AS remaining_space
        FROM inventory
        WHERE item_type='prime_eligible'
        GROUP BY item_type
        ),
        not_prime_items AS (
        SELECT item_type,
        FLOOR((SELECT remaining_space FROM prime_items)/SUM(square_footage)) * COUNT(item_id) item_count,
        (SELECT remaining_space FROM prime_items) - ROUND((FLOOR((SELECT remaining_space FROM prime_items)/SUM(square_footage)) * SUM(square_footage)),2) AS remaining_space
        FROM inventory
        WHERE item_type='not_prime'
        GROUP BY item_type
        )
        SELECT item_type, item_count FROM prime_items UNION ALL
        SELECT item_type, item_count FROM not_prime_items;
        ```
        
    - [**[Hard Google](https://datalemur.com/questions/median-search-freq)**] ****Median Google Search Frequency (To Do)****
        
        Need to study Diff functions required to solve this question
        
    - [**[Hard Facebook](https://datalemur.com/questions/updated-status)**] ****Advertiser Status****
        
        ```sql
        -- Solution 2
        WITH joined_table AS (
        SELECT a.user_id, a.status, d.paid
        FROM advertiser a LEFT JOIN daily_pay d ON a.user_id = d.user_id
        UNION
        SELECT d.user_id, a.status, d.paid
        FROM daily_pay d LEFT JOIN advertiser a ON a.user_id = d.user_id
        )
        SELECT user_id,
        CASE
        WHEN status IS NULL AND paid IS NOT NULL THEN 'NEW'
        WHEN paid IS NULL THEN 'CHURN'
        WHEN status='CHURN' AND paid IS NOT NULL THEN 'RESURRECT'
        WHEN paid IS NOT NULL THEN 'EXISTING'
        END AS new_status
        FROM joined_table
        ORDER BY user_id;
        ```
        
        ```sql
        -- Solution 1
        WITH joined_table AS (
        SELECT a.**, d.user_id AS d_user_id, d.paid
        FROM advertiser a LEFT JOIN daily_pay d ON a.user_id = d.user_id
        UNION
        SELECT a.**, d.user_id AS d_user_id, d.paid
        FROM advertiser a RIGHT JOIN daily_pay d ON a.user_id = d.user_id
        ),
        enriched_table AS(
        SELECT
        CASE
        	WHEN d_user_id IS NULL THEN user_id
        	WHEN user_id IS NULL AND d_user_id IS NOT NULL THEN d_user_id
        	ELSE user_id
        END AS user_id,
        status, paid
        FROM joined_table
        )
        SELECT user_id,
        CASE
        WHEN status IS NULL AND paid IS NOT NULL THEN 'NEW'
        WHEN paid IS NULL THEN 'CHURN'
        WHEN status='CHURN' AND paid IS NOT NULL THEN 'RESURRECT'
        WHEN paid IS NOT NULL THEN 'EXISTING'
        END AS new_status
        FROM enriched_table
        ORDER BY user_id;
        ```
        
    - [**********[Hard Stripe](https://datalemur.com/questions/repeated-payments)**********] ****Repeated Payments****
        
        ```sql
        WITH repeated_payments AS(
        SELECT
        	t1.merchant_id, t1.transaction_id, t1.amount, t2.transaction_id, t2.amount,
        	(t2.transaction_timestamp-t1.transaction_timestamp)::TIME AS time_diff,
        	t1.transaction_timestamp, t2.transaction_timestamp
        FROM transactions t1
        JOIN
        	transactions t2 ON t1.merchant_id=t2.merchant_id AND
        	t1.transaction_timestamp!=t2.transaction_timestamp
        WHERE 
        	(t2.transaction_timestamp-t1.transaction_timestamp)::TIME<='00:10:00' AND
        	(t1.credit_card_id = t2.credit_card_id) AND
        	(t1.amount=t2.amount)
        )
        SELECT COUNT(*) AS payment_count FROM repeated_payments;
        ```
        
        Just put `SELECT * FROM repeated_payments` after the CTE to check how the query works.
        
