# Framework Design

1. **how did i set up data ingestion**
2. **experience working on data ingestion**
3. **how would such data ingestion plan be helpful for stakeholders so they can just go there and get data themselves.**
4. **there will be a zoom white-boarding session about framework design. (you will need to think out loud make sure you're on the right direction)**
5. **data ingestion pipeline**
### 1. **Framework Design Scenario Case Study Interview Questions**
在开始作答之前需要问：
1. 关于Input Data
	1. What are we getting the input data from? And, how often will be get the data?
	2. What is the schema?
	3. What is the input data size?
2. 关于Transformation
	1. What are the business rules?
3. 关于Output Data
	1. Understand the metric in detail
	2. How the output data will be used?
	3. What is the refresh rate?
	4. What is the query pattern?
	5. What is the data retention?
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

1. data modeling exercise and your test #2
2. why did you do that
3. how to make model incremental
4. they will also present some model and ask you how to improve

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


#### Design a data pipeline to get count of login events (Real-Time)

**Questions**
1. Understand the metric in detail?
2. How the output data will be used? - Dashboard
3. What's the refresh rate? - Real-time
4. What is the data retention? - 24 hours
5. How are we getting the input data? - Logging service that sends events
6. Do we get all kind of events? - Only logging events
7. What is the data structure? - user_id, timestamp, country
8. How many events do we get per min? - 500k events per min
9. Any business logic that needs to be taken care of? - N/A
10. Refresh rate for your dashboard? - 10 seconds automatically

---
### Python
1. live python coding session (coderbyte)
2. you can also think out loud


### System Design
1. (5 min) Define the problem space - asking questions to help you narrow down and convert vague requirements into more specific ones.
2. (10 min) Understand functional and non-functional requirements - make assumptions if needed
	1. Who are out clients?
	2. Do we need to talk to pieces of existing systems?
	3. What are the existing pieces?
	4. business objectives - for example, availability, consistency, speed, security, reliability, cost. 如果同时有多个要求，那就focus在更重要的需求上
	5. Estimate the amount of data you need to deal with - you can calculate the bandwidth or data size which will help you on what scaling you might need later.
3. (10 min) Design the system at a high level
	1. start by APIs - 首先询问有什么系统需要进行交互，然后来确定最合适的API是什么。（通常包括REST, SOAP, GraphQL, 和RPC）你需要考虑在使用API的时候所需要的parameters和response type是什么。
	2. 然后从high level层面粗略架构一下用户、API server、各种services、database之间该如何进行交互 
4. (10 min) Deep Dive the design
	1. 例如一个online system 需要refresh data，那就要考虑如何speed up data ingestion process和query process。如果需要refresh的数据比较大，就需要对数据进行partition来balance storage。这时候就可以引入一些balancer来将read/write traffic 分散
5. (10 min) Identify bottlenecks and scaling opportunities - 需要考虑你刚刚设计出的架构是如何来应对未来需求的变动的
	1. Question to consider：
		1. Is there a single point of failure? If so, what to do  to improve the robustness and enhance system's availability?
		2. Is the data valuable to require replication? If we want to replicate the data, how important is it to keep all the versions consistent?
		3. Do we support a global service? If so, do we need to deploy multi-geo data centers to improve data locality?
		4. Are there any edge cases such as peak time usage or hot users that create a particular usage pattern that could deteriorate performance?
		5. How to scale the system when there are 10 x more users?
		6. As scale up the system, do we gradually upgrade each component or migrate to another architecture? (这时候就需要horizontal sharding、CDN、caching、rate limiting、SQL、NoSQL databases相关的知识了)
6. (5 min) Review and wrap up
	1. 



#### Kinesis Data Streams vs. Data Firehose

| Compare              | Kinesis Data Streams (KDS)                                          | Kinesis Data Firehose                                        |
| -------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------ |
| 上游连接对象               | IoT, SDK, Kinesis Agent, CloudWatch, KPL                            | Kinesis Agent, IoT, KPL, CloudWatch, KDS                     |
| 下游连接对象               | 1. Spark on EMR<br>2. EC2<br>3. Lambda<br>4. Kinesis Data Analytics | 1. S3<br>2. Redshift<br>3. Amazon Elasticsearch<br>4. Splunk |
| 目的、作用                | 1. Low latency的Streaming Service<br>2. 用来Ingest数据at scale           | 1. 用来做Data Transfer。<br>例如将数据转移到S3, Redshift等                |
| Provisioning<br>（配置） | 需要用户手动配置Shard                                                       | 可以自动Shard                                                    |
| 数据存储                 | 可以存放数据1-7天                                                          | 不能存放数据                                                       |
| 数据处理                 | 处理real-time数据非常高效。<br>处理普通实时数据的延迟在200ms<br>当数据是fan-out结构，延迟在70ms    | 处理near real-time数据<br>性能高度取决于buffer size                     |
| Replay               | 支持                                                                  | 不支持                                                          |
| Scaling              | 需要手动配置Sharding                                                      | 自动配置Sharding                                                 |

#### Sharding vs. Partitioning
**Sharding是指**，当你有很大一块数据的时候，你把它切分成若干小块，然后分发到若干个计算机上。（也叫Horizontal Partiitoning很迷惑）。你也可以理解成Sharding切分的是 schema。你可以通过 时间、地域进行Sharding。
**Partition是指**，你将一个很大的数据表格切分成若干个小表格(sub-tables)，这种切分通常发生在同一台计算机上。目的是为了提高数据库的性能。尤其是当你需要对一个大表查询的时候，你需要对整张表read，这时候的cost就非常高。如果你这样先定义partition，然后再在特定的partition中查找，那就能降低cost
```mysql
PARTITION BY RANGE (id) (
  PARTITION p_0 VALUES LESS THAN (150000),
  PARTITION p_1 VALUES LESS THAN (250000),
  PARTITION p_2 VALUES LESS THAN (MAXVALUE)
);

-- Example query to retrieve users with IDs less than 150,000
SELECT * FROM users PARTITION (p_0);

-- Example query to retrieve users with IDs between 150,000 and 250,000
SELECT * FROM users PARTITION (p_1);

-- Example query to retrieve users with IDs greater than 250,000
SELECT * FROM users PARTITION (p_2);
```
#### Availability
可以通过**Sharding, CDN (Content Delivery Network), Caching, Rate Limiting, SQL/NoSQL**来提高availability。

**Scaling System**
1. Sharding - 通过添加更多的servers (nodes or computers)来分散压力。常见的数据库例如MongoDB, Cassandra (NoSQL)，此外还有些SQL数据库也支持
2. CDN - 意味着geographically, static content (e.g., images, scripts) are stored in data centers to reduce latency
3. Caching - 如果使用caching的话要么就是在application level里加一个cache；要么就是在数据库层加一个cache。作用就是减少数据库的查询压力，同时提高查询速度。Cache都是in-memory（Redis, Memcached等）存放的
4. Rate Limiting - 防止系统过载。比如系统短时间内接受到多次访问请求。也是可以防止DDoS Attack

**Load Balancer** - 上游就是Load很多的地方、下游就是要解决Load的地方。这种地方通常是你Server层的上游（因为需要保护Server）

**如何处理是当前10x 用户量的Traffic？**
1. 由于数据变为10倍，所以要做Sharding
2. 由于做了Sharing，所以需要Load Balancer将10x的traffic分散给更多的 nodes
3. 由于需要更多的Nodes，所以可以通过Auto-Scaling来根据traffic的量 自动增减nodes
4. 





















