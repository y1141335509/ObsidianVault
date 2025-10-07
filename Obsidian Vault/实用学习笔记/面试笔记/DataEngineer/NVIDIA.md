
### 简历上的项目相关问题

#### Bubbles & Books
1. "You mentioned 25% stability improvement. How exactly did you measure that?"
2. "What was failing before, and how did Kubernetes specifically fix it?
3. ~~"Can you walk me through a specific incident that happened before K8s~~ 
   ~~and show how K8s would have prevented it?"~~
4. "25% seems conservative for moving to Kubernetes. What were the actual 
   improvements in uptime, MTTR, and incident count?"
	1. I measured stability using a composite metric tracking three key dimensions. **Stability = Availability x Responsiveness x Reliability**. I tracked for 3 months before and 3 months after Kubernetes migration. 
		Before Kubernetes: 
			**Downtime** was about 17.3 hr per month --> Availability = (3 mo x 30 days x 24 hr - 17.3 hr) / (3 mo x 30 days x 24 hr) = 99.2%
			**Mean Time To Recovery (MTTR)** about 48 minutes --> 50 incidents in total, 48 minutes per incident to recover. (notice issue 5-15 min; Diagnose 10-20 min; Manually fix 15-30 min; Verify fix 5 min)
			**Incident Count** about 17 incidents per month --> Memory leaks (2 times/week); Failed deployment (3-4 times / month); Config drift (1-2 timex / month)
		After Kubernetes: 
			**Downtime** about 2.2 hr per month --> Availability = (3 mo x 30 days x 24 hr - 2.2 hr) / (3 mo x 30 days x 24 hr) = 99.9%. Improved 15.1 hours. Reduced 87% in downtime.
			**MTTR** about 3 minutes --> Kubernetes auto-restart in 30 seconds; Auto-rollback failed deployments in 2 minutes; 
			**Incident Count** about 3 per month. 84% reduction.
5. "Did you experience any stability issues AFTER moving to Kubernetes? 
   How did you handle them?"
6. "How did you measure stability? What metrics did you track?"
	- PagerDuty: tracks Incident Count; and MTTR
	- Datadog: Uptime monitoring, Alert history
	- CloudWatch: Application logs; Error Rates
	- Custom Dashboard: Aggregated Stability Score
7. "What was your rollback plan if Kubernetes made things worse?"
	- I hope for the best and plan for the worst. I implemented rollback capabilities at 3 levels
		1. Application-level within K8s
		2. Infrastructure-level K8s itself - 
		3. Complete fallback (back to the old architecture)
8. "You mentioned 30% efficiency enhancement through API integration. 
   What exactly became 30% more efficient?"
9. "How did you measure this 30%? Was it processing time, cost, or developer productivity?"
	This is measured based on the average time it needs for each data entry clerk to finish each order. Initially it took 3 minutes. The reason why we wanted to enhance this efficiency was because of high volume of orders during peak days, such as Thanksgiving, Black Friday, Christmas holidays, etc. I addressed this via REST API. Here's how it works:
		Customer submitted order through web app
		Then, API validates order (check inventory availability, validate customer information, calculate pricing with discounts)
		Next, the API automatically creates orders in Netsuite with all fields populated which reduced the manual entry.
		The employees only need to review the order information before confirming.
10. "Walk me through the API design. What made it efficient?"
11. "What was the architecture before the API integration? Why was it inefficient?"
	Web App (React front) 
			|
			|  (Https) POST   /api/v1/orders   JSON payload
			v
	API Gateway      (JWT authentication; Rate limiting; Request validation)
			|
			v
	Python/Flask (in K8s, Data Transformation; Orchestration, Business logic) 
			|
			|---------------------------------------------------
			|                                    |                                       |
			v                                   v                                       v 
		 Netsuite                  MySQL (metadata)                     S3
12. ~~"Did you do any A/B testing to validate the 30% improvement?"~~
13. ~~"What trade-offs did you make to achieve this efficiency?"~~
14. ~~"If you were to redesign this API today, what would you do differently?"~~
15. "That's a huge improvement (12x). Walk me through the deployment process before and after."
16. ~~"What was taking 1 hour in the old process? Where was the bottleneck?"~~
17. ~~"How did Kubernetes specifically reduce it to 5 minutes?"~~
18. "What about rollback time? How long does that take?"
	The rollback is faster than deployment (about 2 minutes). It's triggered when the new version deployment fails
19. ~~"Have you ever had a deployment that took longer than 5 minutes? What happened?"~~
20. "What's your blue-green or canary deployment strategy?"

#### Kubernetes
1. "Why did you choose Kubernetes over alternatives like ECS, Docker Swarm, or serverless?"
	The main reasons are： K8s is industry standard that has massive ecosystem. It has better community and can be tested at scale. Also, it's easy to move it to any cloud platform.
2. ~~"Walk me through your namespace design. How did you organize resources?"~~
3. "What's your multi-environment strategy? (dev/staging/prod)"
4. "How do you handle secrets management in Kubernetes?"
5. "What's your networking setup? (Service mesh? Ingress controller?)"
6. "How do you handle persistent storage in Kubernetes?"
	I minimize persistent storage in K8s. Most data goes to RDS/S3
7. "What's your disaster recovery strategy?"
	The targets are: RTO (Recovery Time Objective): 30 minutes; RPO (Recovery Point Objective): 5 minutes. This means, if total disaster, we can restore service in 30 minutes, losing at most 5 minutes of data.
















NVIDIA has been transforming computer graphics, PC gaming, and accelerated computing for more than 25 years. It’s a unique legacy of innovation that’s fueled by great technology—and amazing people. Today, we’re tapping into the unlimited potential of AI to define the next era of computing. An era in which our GPU acts as the brains of computers, robots, and self-driving cars that can understand the world. Doing what’s never been done before takes vision, innovation, and the world’s best talent. As an NVIDIAN, you’ll be immersed in a diverse, supportive environment where everyone is inspired to do their best work. Come join the team and see how you can make a lasting impact on the world.

Join as a Data Engineer Cloud Gaming to develop and maintain Kubernetes-based GPU-accelerated data processing services, optimizing query response times and driving platform engagement with innovative solutions.

**What you’ll be doing:**
- Optimize distributed computing infrastructure by analyzing cost and right-sizing for latency and performance.
- Identify benchmarks and establish metrics for tracking on dashboards and alerting.
- Improve and sustain a strong, expandable, dependable, live data processing service.
- Develop reusable framework deployments for data ingestion, processing, and analysis.
- Build data systems and pipelines, ensuring that data sources, ingestion components, transformation functions, and destinations are well understood for implementation.
- Prepare data for prescriptive and predictive modeling by ensuring the data is complete, cleansed, and has necessary rules in place.
- Train data engineers, data scientists, and production engineers on adopting data processing workflows.
    
**What we need to see:**
- Master's or Bachelor's degree in Computer Science, Electrical Engineering, or CE, or equivalent experience.
- Minimum of 5 years of work experience in similar domain.
- Built required infrastructure for optimal extraction, transformation, and loading of data from various sources using AWS, Azure, SQL, or other technologies, and knowledgeable in Databricks, Splunk, or Snowflake solutions.
- Strong coding skills, including the ability to write readable, testable, maintainable, and extensible code (primarily Python).
- Strong Kubernetes experience on-premise and/or CSP, developing containerized microservices.
- Hands-on experience in performance tuning/troubleshooting spark applications.
- Familiarity with metrics collection, health monitoring, and observability tools.
- Strong experience in data cleaning, aggregation, transformation, and extraction.
- Experience in active ML production pipelines is a plus (MLflow, Kubeflow).
    
**Ways to stand out from the crowd:**
- Strong ability to drive continuous improvement of systems and processes.
- Prior data processing at scale on NVIDIA GPUs.
- Proven ability to work in a fast-paced environment where strong organizational skills are essential.
- Expertise in optimizing Spark applications and micro services for enhanced performance and scalability.
    
Are you dedicated, upbeat, and dynamic with excellent analytical ability? Are you an engineer passionate and highly motivated about solving complex problems? If so, you may be a perfect fit for NVIDIA!

Your base salary will be determined based on your location, experience, and the pay of employees in similar positions. The base salary range is 148,000 USD - 235,750 USD for Level 3, and 184,000 USD - 287,500 USD for Level 4.












