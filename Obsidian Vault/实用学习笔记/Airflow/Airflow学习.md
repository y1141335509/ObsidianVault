## 1. 😈最不喜欢的配环境
参考教程 - [Airflow官方](https://airflow.apache.org/docs/apache-airflow/stable/howto/docker-compose/index.html)


（如果你之前已经在Docker中创建了项目，那你可以跳过前面的步骤）
🐳如果是创建新项目，那么首先打开Docker


💻打开命令行并`cd`到你要创建项目的文件夹路径下


💻在命令行运行：
```bash
docker run --rm "debian:bullseye-slim" bash -c 'numfmt --to iec $(echo $(($(getconf _PHYS_PAGES) * $(getconf PAGE_SIZE))))'
```
该命令默认会分配4GB内存空间给Docker（理想情况下是分配8GB）


💻获取`docker-compose.yaml`文件：
```bash
curl -LfO 'https://airflow.apache.org/docs/apache-airflow/2.8.1/docker-compose.yaml'
```
运行上面命令，从而获取`docker-compose.yaml`文件
（你可以根据需要更改Airflow的版本号。这里的版本号是2.8.1）


💻初始化环境：
```bash
mkdir -p ./dags ./logs ./plugins ./config
echo -e "AIRFLOW_UID=$(id -u)" > .env
```
这会在当前文件夹下创建4个新的文件夹 
* `dags` - 该文件夹用于存放你的DAG文件。通常每个.py文件就是一个DAG文件，这是Airflow默认的编程规范
* `logs` - 该文件夹中有task execution和scheduler
* `plugins` -  你可以添加custom log parser或者添加`airflow_local_settings.py`文件来配置cluster policy
* `config` - 用来存放你的[custom plugins](https://airflow.apache.org/docs/apache-airflow/stable/authoring-and-scheduling/plugins.html)


如果你发现`AIRFLOW_UID`尚未被设置，虽然你可以忽略它，但你也可以指定ID的值
```bash
AIRFLOW_UID=50000
```


接着，你需要安装Airflow所需要的数据库。通常默认数据库是sqlite3（需要等15-30分钟）
```bash
docker compose up airflow-init
```


运行Airflow
```bash
docker compuse up
```
启动完不要关闭terminal


打开vscode，然后打开你创建的Airflow根文件夹（也就是你上面`cd`去的文件夹）
然后运行
```bash
docker compose run airflow-worker airflow info
```
查看各种包的信息


（如果你之前已经在Docker中创建了项目，那你可以跳过前面的步骤）直接在vscode中运行：
```bash
curl -LfO 'https://airflow.apache.org/docs/apache-airflow/2.8.1/airflow.sh'
chmod +x airflow.sh
```
或者运行：
```bash
./airflow.sh info
```


接下来，你可以在浏览器上访问Airflow的UI：
```bash
ENDPOINT_URL="http://localhost:8080/"
curl -X GET  \
    --user "airflow:airflow" \
    "${ENDPOINT_URL}/api/v1/pools"
```


然后你可以打开浏览器，进入`http://localhost:8080`。如果被问到username和password，且你没有预先设置用户名和密码的话，默认上分别是 airflow和airflow（小写）。如此你就可以看到airflow的UI了。


再给一个创建DAG的案例：
在dags文件夹下面创建一个.py文件，然后下面是一个代码案例
![[Screenshot 2024-01-24 at 15.40.17.png]]
```python
from airflow import DAG
from airflow.operators.dummy_operator import DummyOperator
from datetime import datetime, timedelta
import pytz

default_args = {
'owner': 'airflow',
'depends_on_past': False,
'start_date': datetime(2024, 1, 24, 16, 37, 0, tzinfo=pytz.timezone('America/Los_Angeles')),
'email_on_failure': False,
'email_on_retry': False,
'retries': 1,
'retry_delay': timedelta(minutes=5),
}

dag = DAG('my_first_dag',
default_args=default_args,
description='A simple tutorial DAG',
schedule_interval=timedelta(minutes=1),
catchup=False)

task1 = DummyOperator(
task_id='number1',
dag=dag,
)

task2 = DummyOperator(
task_id='number2',
dag=dag,
)

task3 = DummyOperator(
task_id='number3',
dag=dag,
)

task4 = DummyOperator(
task_id='number4',
dag=dag,
)

task5 = DummyOperator(
task_id='number5',
dag=dag,
) 

task1 >> [task2, task3]
task3 >> task4
task4 >> task5
```
点运行后，然后去`http://localhost:8080/`，搜索`my_first_dag`就能看到刚刚创建的DAG了。
上面这段代码是每分钟触发的，你可以根据需求调整最初触发的时间





最后，如果你想要清理你刚刚创建的Airflow和Docker项目，可以在停止Docker后运行：
```bash
docker compose down --volumes --rmi all
```

## 2. 一些基础概念

Operators Categories 运行器分类
* `Sensors` - 是`Operators`的一种，它会在满足某个条件之前一直运行。例如，你可以用它等待上游数据准备就绪后，执行后续的数据处理等。`常见的Sensors`：
	* `HdfsSensor` - 用来等待一个文件或者文件夹被放到HDFS中
	* `NameHivePartitionSensor` - 检查最近一次的Hive table的分区是否可以用作下游数据处理
* `Operators` - 用来触发某种“动作”（例如运行一段bash命令，执行一段python函数，或者执行一个Hive query等）。常见的Operators有：
	* `BashOperator` 
	* `PythonOperator` 
	* `HiveOperator` 
	* `BigQueryOperator` 
	都是用来执行相应的命令或代码。不多赘述。
* `Transfers` - 用来将数据从一个地方转移到另一个地方。常见的有：
	* `MySqlToHiveTransfer` 
	* `S3ToRedshiftTransfer`



### Spark

**Q: What are the key features of Apache Spark?**
A: "Apache Spark is known for its fast in-memory data processing capabilities. It offers high-level APIs in Java, Scala, Python, and R. Key features include Spark SQL for SQL and structured data processing, MLlib for machine learning, GraphX for graph processing, and Spark Streaming for scalable, high-throughput, fault-tolerant stream processing of live data streams."

**Q: Can you explain RDDs in Spark?**
A: "RDDs, or Resilient Distributed Datasets, are the fundamental data structure of Spark. They are immutable, distributed collections of objects. RDDs can be created through deterministic operations on either data on disk or other RDDs. They are fault-tolerant as they track lineage information to rebuild lost data on node failure."

**Spark Core Concepts (RDDs, DataFrames, Transformations, Actions)**
- **RDDs**: Resilient Distributed Datasets (RDDs) are Spark’s fundamental data structure, offering fault-tolerant, parallel data processing. They allow users to perform in-memory computations on large clusters in a fault-tolerant manner.
- **DataFrames**: DataFrames in Spark are similar to those in pandas or R, optimized for big data processing and support various data formats. They are distributed collections of rows under named columns.
- **Transformations and Actions**: Transformations (like `map`, `filter`) create new RDDs/DataFrames and are lazily evaluated. Actions (like `count`, `collect`) trigger computation and return results.
- **Parallel Processing and Fault Tolerance**: Spark achieves parallel processing through distributed computing on clusters, and fault tolerance through lineage information of RDDs.
- **Lazy Evaluation**: Spark optimizes execution through lazy evaluation, meaning computations are only triggered when an action is called.


### Scala

**Q: What is Scala and why is it used with Spark?**
A: "Scala is a high-level, statically-typed programming language that combines functional and object-oriented programming paradigms. It's used with Spark for its concise syntax, functional programming capabilities, and because Spark’s native APIs are written in Scala. This provides more control and optimization opportunities in Spark data processing."

**Q: How does Scala improve Spark applications?**
A: "Scala can improve Spark applications' performance and readability. Its functional nature allows for writing concise and readable code. Scala's case classes and pattern matching facilitate easier manipulation of complex datasets. Moreover, being statically-typed, Scala can help catch errors at compile time, making Spark applications more robust."

**Basic Scala Syntax, Case Classes, Functional Programming**
- **Scala Syntax**: Scala combines object-oriented and functional programming. It supports immutable data structures, type inference, and concise syntax.
- **Case Classes**: Case classes in Scala are regular classes which are immutable by default and decomposable through pattern matching.
- **Functional Programming**: Scala supports higher-order functions, allows functions as values, and provides support for immutability and lazy evaluation.

**Integration of Airflow with Data Systems like Spark**
- **Airflow for Orchestration**: Airflow can orchestrate complex computational workflows, which can include Spark jobs. 
- **Scheduling Spark Jobs**: Airflow can schedule and monitor Spark jobs, manage dependencies, and handle retries and failures.

**Scala's Integration with Spark**
- **Native Support**: Spark is written in Scala, giving it native support for Scala, allowing more functional programming capabilities and concise code.
- **Dataset API**: Scala’s case classes and pattern matching enhance the Dataset API in Spark, improving the clarity and simplicity of complex data processing operations.

### Airflow

**Q: What is Apache Airflow and its use in data engineering?**
A: "Apache Airflow is an open-source platform to programmatically author, schedule, and monitor workflows. In data engineering, it's used for orchestrating complex computational workflows, ETL processes, and data pipelines. Airflow’s scheduler executes tasks on an array of workers while following the specified dependencies."

**Q: How does Airflow manage task dependencies?**
A: "Airflow manages task dependencies using Directed Acyclic Graphs (DAGs). In a DAG, each node is a task, and edges define dependencies among these tasks. Airflow ensures that tasks are executed in the order respecting their dependencies and manages retries and failures."

**Scenarios for Using Airflow to Manage Data Pipelines**
- **ETL Processes**: Automating ETL tasks which involve extracting data from various sources, transforming it, and loading it into a data warehouse - Spark can process data, Airflow can manage the workflow.
- **Machine Learning Pipelines**: Orchestrating steps involved in machine learning pipelines, like data preprocessing, model training, and evaluation.


### System Design
**Q: What are Scalability and Fault Tolerance?** 
A:
* Scalability refers to the ability of a system to handle an increasing amount of work by adding resources. 
* Fault tolerance refers to the ability of a system to continue functioning even in the face of failures or errors.

**Scalability, Efficiency, and Reliability in System Design**
- **Scalability**: Ability to handle growing amounts of work or expanding in capacity.
- **Efficiency**: Optimal use of resources (like CPU, memory) to achieve performance goals.
- **Reliability**: System's ability to function under predefined conditions consistently over time.




### Hadoop

1. **What is Hadoop and its components?**
   Hadoop is an open-source framework for distributed storage and processing of large datasets using simple programming models. It primarily consists of Hadoop Distributed File System (HDFS) for storage, MapReduce for processing, and Yet Another Resource Negotiator (YARN) for cluster management.

2. **How does Hadoop differ from traditional databases?**
   Hadoop is designed for handling large volumes of unstructured or semi-structured data, unlike traditional databases which are optimized for structured data. Hadoop provides scalability and fault tolerance, whereas traditional databases focus more on transactional integrity and consistency.

3. **Explain Hadoop's architecture and its core components.**
   Hadoop's architecture includes HDFS for distributed storage, which splits data into blocks stored across multiple nodes. MapReduce processes data in parallel by dividing tasks into smaller sub-tasks. YARN manages resources in the cluster and schedules jobs.

4. **How does HDFS ensure data reliability and fault tolerance?**
   HDFS stores multiple copies of data blocks at different nodes (replication) to ensure data reliability and fault tolerance. If a node fails, data can be retrieved from another node.

5. **Describe YARN and its advantages.**
   YARN separates the resource management and job scheduling/monitoring functions. It enables better cluster utilization and supports multiple data processing engines, improving scalability and cluster efficiency.

6. **What are some common use cases for Hadoop?**
   Common use cases include big data analytics, data lakes, distributed data processing, and large-scale log or event data analysis.

7. **How do you handle data loss in HDFS?**
   Data loss in HDFS is handled through replication. If a node is lost, HDFS automatically replicates the data from other nodes to maintain the configured replication factor.

### MapReduce

1. **What is MapReduce and how does it work?**
   MapReduce is a programming model for processing large datasets in parallel across a Hadoop cluster. It works in two phases: the Map phase (where it processes and transforms data) and the Reduce phase (where it aggregates or summarizes data).

2. **Explain the difference between a 'map' and a 'reduce' task.**
   'Map' tasks process input data and produce key-value pairs as output. 'Reduce' tasks take these key-value pairs and aggregate them based on keys.

3. **How does MapReduce handle failure?**
   MapReduce handles failures by automatically rerunning failed tasks on different nodes. If a node fails, tasks are rescheduled on other available nodes.

4. **Describe a real-world scenario where you would use MapReduce.**
   A real-world scenario is log analysis, where MapReduce can process large volumes of log data to extract insights, such as user activity or error rates.

5. **What are the limitations of MapReduce?**
   MapReduce can be slow due to its batch processing nature and excessive disk I/O. It's not suitable for interactive data analysis or real-time processing.

6. **How does MapReduce fit into the Hadoop ecosystem?**
   MapReduce is a core component of the Hadoop ecosystem, providing a powerful framework for large-scale data processing and analysis on HDFS.

7. **Differences between Hadoop's MapReduce and Spark's computational model?**
   Spark performs in-memory processing, which is faster than MapReduce's disk-based processing. Spark also offers more flexibility with its DAG execution engine, compared to MapReduce's two-stage paradigm.

### Spark

1. **What is Apache Spark and how is it different from Hadoop?**
   Apache Spark is an open-source, distributed processing system used for big data workloads. It differs from Hadoop in offering in-memory processing, which is faster, and supports more than just MapReduce algorithms.

2. **Explain RDDs in Spark. How do they differ from DataFrames?**
   RDDs (Resilient Distributed Datasets) are a fundamental data structure of Spark that provides an immutable, distributed collection of objects. DataFrames are a higher-level abstraction built on RDDs, optimized for structured and semi-structured data, and provide more straightforward operations.

3. **What is lazy evaluation and how does Spark utilize it?**
   Lazy evaluation in Spark means that the execution will not start until an action is performed. This allows Spark to optimize the execution plan for efficiency.

4. **Describe Spark's fault tolerance mechanisms.**
   Spark achieves fault tolerance through lineage information. If a partition of an RDD is lost, Spark can rebuild it using the lineage graph.

5. **How does Spark achieve high performance and efficiency compared to MapReduce?**
   Spark achieves high performance through in-memory processing, reducing the need for disk I/O. Its DAG execution engine optimizes workflows, making it more efficient than MapReduce's linear approach.

6. **Can you explain Spark's execution model?**
   Spark's execution model involves creating a DAG of stages for each

6. **Can you explain Spark's execution model?**
   Spark's execution model involves creating a DAG (Directed Acyclic Graph) of task stages for each job. It divides the operators into stages of tasks that can be executed in parallel. This model allows for optimization and pipelining, which improves the overall efficiency of data processing.

7. **Discuss a use case where you would prefer Spark over Hadoop.**
   Spark is preferred for iterative algorithms in machine learning, interactive data mining, and real-time data processing due to its in-memory processing capability, which is much faster than Hadoop's disk-based processing.

### Scalability, Efficiency, and Reliability in System Design
- **Scalability**: The ability of a system to handle increased load by adding resources, either by scaling up (more powerful hardware) or scaling out (more machines).
- **Efficiency**: Efficient use of resources (like CPU, memory) to achieve high performance, including optimizing algorithms and reducing unnecessary processing.
- **Reliability**: The system's capability to operate correctly and consistently under defined conditions, including handling failures gracefully and ensuring data integrity.

These answers provide an overview of key concepts and practices in working with Hadoop, MapReduce, and Spark, offering insights into their functionalities, differences, and applications.



















