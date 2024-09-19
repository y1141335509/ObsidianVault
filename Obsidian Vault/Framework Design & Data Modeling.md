# Framework Design

1. **how did i set up data ingestion**
2. **experience working on data ingestion**
3. **how would such data ingestion plan be helpful for stakeholders so they can just go there and get data themselves.**
4. **there will be a zoom whiteboarding session about framework design. (you will need to think out loud make sure you're on the right direction)**

### 1. **Framework Design Scenario Case Study Interview Questions**

#### **Case Study 1: Building a Scalable Data Pipeline for Marketing Campaigns**
**Question:**  
The marketing team wants to track the performance of various campaigns (e.g., email, social media, ad channels) and compare results across platforms. They request a scalable framework for collecting, storing, and analyzing this data. How would you design such a pipeline?

**Answer Thought Process:**
- **Data Sources:** Identify different campaign data sources (e.g., Google Ads, Facebook Ads, email providers). Highlight the need for API integrations or flat file ingestion.
- **Data Ingestion Layer:** Use ELT tools like dbt or an ingestion pipeline using AWS Lambda, AWS Glue, or Apache Airflow for scheduled extraction.
- **Data Storage:** Propose a scalable data warehouse such as Snowflake to store data with partitioning by date, platform, and campaign type.
- **Transformation Layer:** Outline how dbt can be used for transforming raw data into a clean, usable format (e.g., campaign performance metrics).
- **Automation & Scaling:** Discuss monitoring with tools like AWS CloudWatch, and how you would scale to accommodate higher data volume.
- **Reporting:** Provide output in a centralized BI tool like Tableau or Power BI for real-time marketing insights.

****
#### **Case Study 2: Framework for Financial Reporting and Data Reconciliation**
**Question:**  
The finance team needs a framework to automate monthly financial reporting while ensuring that data from multiple sources (e.g., payment gateways, sales platforms) reconciles correctly. How would you design this system?

**Answer Thought Process:**
- **Data Sources:** List financial data sources such as Stripe, in-app purchases, or other third-party payment processors.
- **Data Ingestion:** Use event-driven architecture (e.g., S3 and Lambda) to ingest transactional data or batch processing with AWS Glue.
- **Data Consistency:** Implement checksums, data versioning, and reconciliation processes during ingestion to catch mismatches early.
- **Storage:** Propose using a Snowflake data warehouse with an appropriate schema to track transactions by order ID, payment status, etc.
- **Auditing and Reconciliation:** Implement automated scripts in dbt to validate transaction totals between sources and flag discrepancies for review.
- **Reporting:** Use BI tools to create visual dashboards for financial data trends, ensuring monthly reporting accuracy.

#### **Case Study 3: Real-time Data Processing for User Engagement**
**Question:**  
The marketing team wants real-time updates on user engagement with various features of the app (e.g., time spent on lessons, quiz completion rates). How would you design a real-time processing framework?

**Answer Thought Process:**
- **Data Sources:** Identify the app’s event tracking system (e.g., Firebase, Mixpanel) to capture engagement metrics.
- **Real-time Ingestion:** Propose using a message broker like Apache Kafka or AWS Kinesis to ingest events in real-time.
- **Processing Framework:** Design a stream-processing pipeline using Spark Streaming or AWS Lambda to calculate metrics like time spent or lesson completion rates in near real-time.
- **Data Storage:** Store processed data in a low-latency database like Snowflake or Redshift for real-time querying.
- **Scaling and Optimization:** Discuss partitioning strategies to ensure the pipeline scales effectively with user growth.
- **Visualization:** Provide a reporting tool (e.g., Grafana or Tableau) for real-time insights into engagement.

### 2. **Data Modeling Scenario Case Study Interview Questions**

#### **Case Study 1: Marketing Attribution Model**
**Question:**  
The marketing team wants to understand how various marketing channels contribute to user acquisition. Design a data model to support multi-touch attribution across campaigns.

**Answer Thought Process:**
- **Fact Table:** Start with a fact table capturing user interactions with different marketing channels (e.g., impressions, clicks, conversions). Include user ID, timestamp, channel ID, and event type.
- **Dimension Tables:** Create dimension tables for `channels` (channel name, cost, etc.), `users` (demographics, location, etc.), and `campaigns` (start date, end date, campaign type).
- **Attribution Logic:** Use logic (e.g., first-touch, last-touch, or fractional attribution) for assigning conversion value to each marketing channel.
- **Schema Design:** Propose a star schema with the fact table at the center, connected to the dimension tables for easy querying.
- **Performance Consideration:** Discuss partitioning the fact table by date and indexing frequently queried columns for optimization.

#### **Case Study 2: Subscription-based Revenue Data Model**
**Question:**  
Design a data model to track user subscriptions and revenue over time, accounting for new users, renewals, and cancellations.

**Answer Thought Process:**
- **Fact Table:** Build a fact table `subscriptions` capturing events like `sign_up`, `renewal`, and `cancellation` with user ID, date, plan type, and revenue amount.
- **Dimension Tables:** Create dimension tables for `users` (user ID, signup date, demographic data), `plans` (plan ID, price, duration), and `events` (event type, description).
- **Historical Tracking:** Consider slowly changing dimensions (SCD) to track changes in user or subscription details over time.
- **Reporting Needs:** Discuss metrics like monthly recurring revenue (MRR), churn rate, and cohort analysis, and how the data model will support these calculations.
- **Query Optimization:** Explain how denormalization or materialized views could speed up recurring queries.

#### **Case Study 3: User Engagement and Retention Data Model**
**Question:**  
The product team wants to analyze how different app features impact user retention. How would you design a data model to track feature usage and correlate it with retention?

**Answer Thought Process:**
- **Fact Table:** Create a fact table for `feature_usage` with user ID, feature ID, session ID, usage duration, and event date.
- **Retention Tracking:** Define retention cohorts in the `users` table, based on the first use date, and track feature usage frequency over time.
- **Dimension Tables:** Build dimension tables for `features` (feature name, category), `users` (demographics, signup date), and `sessions` (session length, device type).
- **Schema Design:** Propose a snowflake schema to easily join user demographics with feature usage patterns.
- **Performance Considerations:** Use partitioning and indexing on date and user ID columns to ensure the model scales for large datasets.

In all cases, highlight your approach to ensuring data consistency, optimizing for query performance, and collaborating with stakeholders to define the requirements.
## Potential Questions































