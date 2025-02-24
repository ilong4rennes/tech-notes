For every new-emerged technology, we ask two questions:
- Can we solve new problems?
- Can we solve old problems in a better way?
### Big Data Pipeline

1. **Data Ingestion:** Use tools like Apache Kafka or Flume to collect data from various sources (e.g., logs, sensor data, social media).
2. **Storage:** Store raw data in a distributed file system like HDFS or a data lake, ensuring it’s accessible for processing.
3. **Data Processing:** Use frameworks such as MapReduce for batch processing or Spark for faster, in-memory analytics. For real-time processing, consider tools like Apache Storm or Flink.
4. **Query and Analysis:** Run SQL queries with Hive or Impala, or use BI tools for visualization. This step turns processed data into actionable insights.
5. **Machine Learning and Analytics:** Leverage libraries like Spark MLlib, TensorFlow, or PyTorch to analyze patterns, predict trends, or classify data.
6. **Visualization and Reporting:** Create dashboards and reports using tools like Tableau or Power BI to communicate findings.

**1. Data Ingestion and Messaging Systems**
- Apache Kafka
	- **What It Does:** A distributed messaging system that allows for high-throughput, fault-tolerant ingestion of real-time data streams.
- Apache Flume
	- **What It Does:** Specializes in collecting and aggregating large amounts of log data from various sources to HDFS.

**2. Storage and File Systems**
- Hadoop Distributed File System (HDFS)
	- See Below. 
- Data Lakes and Warehouses
	- **Data Lakes:** Centralized repositories that allow you to store all your structured and unstructured data at any scale. They use low-cost storage and are often built on distributed systems.
	- **Data Warehouses:** Optimized for querying and reporting; they integrate and aggregate data from various sources.
	- **Use Cases:** Data lakes are ideal for data exploration and machine learning, whereas data warehouses support business intelligence and analytics.
- **NoSQL Databases:** Such as MongoDB, Cassandra, and HBase, which handle unstructured data more flexibly than traditional SQL databases.
- **Cloud Storage:** Services like Amazon S3, Google Cloud Storage, and Azure Blob Storage provide scalable and cost-effective storage.

**3. Data Processing Frameworks**
- MapReduce
- Apache Spark
	- **What It Does:** An in-memory data processing engine that offers faster processing compared to MapReduce, with built-in modules for SQL, machine learning, stream processing, and graph processing.
- Stream Processing Engines
	- **Apache Storm & Apache Flink:** These tools process data streams in real time, enabling applications such as fraud detection or real-time analytics.
	- **How They Work:** Unlike batch processing, stream processing continuously ingests and processes data, often with low latency.

**4. Query and Analysis Tools**
- **SQL-on-Hadoop Engines**
	- **Examples:** Apache Hive, Impala, Presto.
	- **What They Do:** Enable users to run SQL queries on data stored in HDFS or other big data storage systems.
	- **How to Use:** They provide a familiar SQL interface to non-SQL storage systems, allowing data analysts to leverage their SQL skills for big data processing.

**5. Machine Learning and Advanced Analytics**
- **Apache Spark MLlib:** A scalable machine learning library integrated with Spark.
- **TensorFlow and PyTorch:** Deep learning frameworks that can process large datasets for complex tasks like image recognition or natural language processing.

### Scaling Challenges in Data Management

- **Vertical Scaling**: Upgrading a single machine (limited and expensive).
- **Horizontal Scaling**: Increasing the number of nodes in a distributed system for scalability and reliability.
- **Reliability**: Distributed systems are resilient to node failures through redundancy.

#### Vertical Scaling (Relational Databases - RDBMS)

- **Relational databases** (e.g., MySQL, PostgreSQL, Oracle) are traditionally designed for **vertical scaling**.
- **How it works**: You improve a single machine's resources—adding more CPU, RAM, or storage.
- **Limitations**:
    - Expensive as there's a limit to how much you can upgrade one machine.
    - Can lead to downtime during upgrades.
    - Performance bottlenecks when dealing with huge data volumes.
- **Example**: Increasing the RAM on a MySQL server to handle more queries.

#### Horizontal Scaling (Non-relational Databases - NoSQL)

- **Non-relational databases** (e.g., MongoDB, Cassandra, DynamoDB) are designed for **horizontal scaling**.
- **How it works**: Add more machines (nodes) to the system, distributing the data across multiple servers.
- **Advantages**:
    - Easily handles massive data volumes and high traffic.
    - More cost-effective since it can use commodity hardware.
    - High availability and fault tolerance—if one node fails, others take over (reliability through redundancy).
- **Example**: Sharding a MongoDB database across multiple servers to handle larger datasets.

### HDFS

Hadoop’s **HDFS** (Hadoop Distributed File System) is based on two core components:
- MapReduce
- Google File System (GFS)
![[Screenshot 2025-02-21 at 1.40.27 AM.png]]

#### Google File System (GFS)

- GFS is the underlying storage system that allows efficient storage and retrieval of data across distributed machines.
- Hadoop’s HDFS is inspired by GFS and manages how data is stored across the distributed system.

1. **Master Node (GFS Master):**
    - Manages metadata, file structure, and keeps track of which chunk is stored on which node.
    - Directs client requests but doesn't handle the actual data directly.
2. **Chunk Servers (C0, C1, C2, C3, C4):**
    - Store actual data chunks.
    - Each chunk is replicated multiple times across different nodes for **fault tolerance**.
        - For example:
            - **C0** stores chunks replicated on multiple nodes.
            - **C1, C2** also store replicated versions of different chunks.
    - If a node fails, another node with a replica can quickly take over, ensuring **reliability**.
3. **Client Interaction:**
    - Clients communicate with the master to locate data but retrieve it directly from the chunk servers.
    - Minimizes the load on the master and improves scalability.

- **Replication**: Each data block is stored multiple times (replicated) for reliability.
- **Fault Tolerance**: Even if some nodes fail, the data is accessible from other replicas.
- **Scalability**: New nodes can be added to increase storage capacity without affecting performance.

- HDFS (or GFS) handles **storage** by distributing data across machines.
- MapReduce handles **computation** by processing that distributed data in parallel.

#### Hadoop Distributed File System (HDFS)

**Conceptual Architecture of HDFS:** 
- Files are divided into blocks (e.g., a **150GB** file split into three blocks).
- Blocks are distributed across **Data Nodes** for storage.

**Resilience via Replication:**
- Data blocks are replicated across multiple nodes for fault tolerance.
- Commands:
	- Upload file: `% hadoop fs -put foo.csv`
	- List files: `% hadoop fs -ls`
![[Screenshot 2025-02-23 at 3.15.58 PM.png]]

**1. File Storage in HDFS**

- A **150 GB file** is uploaded into the Hadoop ecosystem.
- HDFS automatically divides the file into **blocks** for distributed storage.
    - Each block size is usually 64 GB (default block size in older versions; newer versions often use 128 GB).

**2. NameNode (Metadata Management)**

- The **NameNode** serves as the **master node** responsible for:
    - Storing metadata (e.g., the structure of the file system, location of data blocks, replication information).
    - Tracking which **DataNodes** (worker nodes) hold the replicas of each block.
- Example:
    - Block 1 → Stored in Data Nodes **1, 3, 4**
    - Block 2 → Stored in Data Nodes **2, 3, 4**
    - Block 3 → Stored in Data Nodes **1, 2, 4**

**3. DataNodes (Data Storage & Replication)**

- **DataNodes** are the actual worker nodes where the file blocks are stored.
- HDFS ensures **fault tolerance** by replicating blocks across multiple DataNodes.
    - Default replication factor is **3** (each block exists on three different nodes).

**4. Resilience by Replication**

- If a **DataNode** fails, the system continues to function using the replicated copies on other nodes.
    - Example: If **Data Node 1** fails, the system still has copies of Block 1 on **Data Node 3** and **Data Node 4**.
- Replication ensures **high availability** and **fault tolerance** without data loss.

### MapReduce

- **MapReduce** is a programming model designed for processing massive datasets in parallel by dividing tasks across multiple nodes in a distributed system.

- Analogous to solving a **jigsaw puzzle**:
    - Break the task into manageable pieces.
    - Solve each piece independently (Map).
    - Combine partial solutions (Reduce).

1. **Input Data**:
    - Large datasets are broken into smaller chunks (e.g., lines from a text file or rows from a table).
    - Each chunk is processed by a separate **Map** function running on different nodes.
2. **Map Phase**:
    - Each **Map** function processes its assigned chunk and emits intermediate key-value pairs (`<k, v>`).
    - Example: If counting words in a document, the map function might output pairs like `(word, 1)` for each occurrence.
3. **Shuffle and Sort**:
    - The framework automatically groups all intermediate data by key.
    - All values associated with the same key are sent to the same **Reducer** (this step happens in the background).
4. **Reduce Phase**:
    - Each **Reduce** function aggregates the values for a specific key.
    - Example: For the word count problem, it sums up all the `1`s associated with each word key.
5. **Results**:
    - The output of each reducer is collected and combined to produce the final result (e.g., total counts of each word across the dataset).

- **Parallelism**: Each node processes data independently.
- **Resilience**: If a node fails, Hadoop automatically reassigns tasks.
- **Scalability**: Easily handles petabytes of data by distributing tasks.
#### MRJob

> `mrjob` lets you write MapReduce jobs in Python.

- **Installation**:
```bash
conda install -c conda-forge mrjob
python -c 'import mrjob; print(mrjob.__version__)'  # Version 0.7.4
```

- **Execution Example**:
```bash
python mrjob-wc.py shakespeare-complete.txt -q
```

- Processes Shakespeare's complete works.
- Counts the number of lines in the dataset (~165,672 lines).

#### Paradigm: map – shuffle – reduce
![[Screenshot 2025-02-21 at 3.36.04 AM.png]]

Example Task: Word Count on Wikipedia Pages
- Count the number of words of different lengths (1-letter, 2-letter, etc.).
- **MapReduce Strategy**:
    - **Map Step**: Break input data into smaller pieces (lines or words).
    - **Shuffle Step**: Group data by common properties (word length).
    - **Reduce Step**: Count the occurrences of words by length.

1. Input Data (Leftmost Column)
	- The input dataset is divided into smaller, manageable **data blocks** (represented by the colored bars: red, green, blue).
	- Each block is processed in parallel by different nodes.
	- The colors signify different categories or keys associated with the data (e.g., words, user IDs, or log types).
2. Map Phase (First Section of Each Node)
	- Each **node** (Node 1, Node 2, Node 3) runs a **mapper function** on its assigned portion of the input data.
	- Mappers process the input data and emit **key-value pairs**:
	    - Example: In a word count problem, if the input is a text line, a mapper might emit pairs like `(word, 1)` for each word.
	- **Parallel Execution**:
	    - Multiple mappers run simultaneously across nodes to speed up processing.

💡 "As many lines of input, so many mappers" — Every line or chunk of input data can be processed by a separate mapper.

3. Shuffle and Sort Phase (Middle Section)
	- After mapping, the intermediate key-value pairs are **shuffled**:
	    - This step groups all the same keys (e.g., all instances of a word) together, regardless of which mapper produced them.
	    - Example: All pairs like `(word: "data", count: 1)` from different mappers are collected together.
	- The **sorting** process organizes keys to ensure that all data with the same key goes to the same **reducer**.
	- The arrows in the diagram represent how specific colored data blocks are sent across nodes based on their key.

💡 All values associated with the same key are directed to the same reducer, even if they came from different nodes.

4. Reduce Phase (Final Section of Each Node)
	- The **reduce function** processes grouped data:
	    - Aggregates values for each key.
	    - Example: In a word count task, the reducer sums up the occurrences of each word.
	- Each reducer outputs the final result for a specific group of keys (e.g., all counts for the word "data").

💡 "As many unique tags yielded by the mapper, so may reducers" — The number of reducers depends on the number of unique keys in the data.

5. Output Data (Rightmost Column)
	- All the outputs from reducers are combined into the final result.
	- The final output is typically sorted or organized by key (e.g., alphabetical word count list).

- Understanding the **map-shuffle-reduce** paradigm:
    - Each line of input generates a separate mapper.
    - Each unique key (tag) from the mapper corresponds to a reducer.

#### Expressing JOIN With MAPREDUCE


#### Conceptual View of MapReduce Process
![[Screenshot 2025-02-23 at 4.05.18 PM.png]]
**Simple Sequential Processing Example:**
```python
def process():     
	ans = []     
	for x in ary:         
		ans.append(f(x))     
	return ans
```
- All input elements (`x1` to `x8`) are processed **one after another** using function `f(x)`: 
	- `f(x1), f(x2), f(x3), ..., f(x8)`
- This method works linearly and is slower for large datasets.

**Map Function Example:**
```python
def mapper(self, key, value):     
	yield key_2, f(value)
```
- Each element is processed **independently and simultaneously**:
	- Separate nodes or processors handle `f(x1)`, `f(x2)`, ..., `f(x8)` at the same time.
- Results can be combined later using the **reduce phase** (not shown in this diagram).
- Efficient for large datasets, as it reduces overall computation time by utilizing parallelism.

### Spark

Design Goals: How were they achieved?
- Scalable: Horizontal scaling
- Efficient: Parallel operation (map)
- Reliable: HDFS replication, notion of name nodes and data nodes
- Usable: MrJob, Pig, Hive
- Economical: Commercial off the shelf (COTS) components


#### Abstractions in Big Data Processing

**Types of Abstractions:**
1. **Resilient Storage**
    - Example: `hadoop fs -put f1.txt`
    - This command uploads files to **HDFS** (Hadoop Distributed File System) which ensures data is stored reliably across multiple machines with replication to prevent data loss.
2. **Parallel Processing Abstraction**
    - **Map**: Breaks the problem into smaller sub-problems and processes them independently.
    - **Reduce**: Aggregates the outputs from the map phase to produce the final result.
    - These operations run simultaneously on distributed data chunks, improving efficiency.
3. **Parallel Data Structure Abstraction**
    - **Resilient Distributed Dataset (RDD)**: A distributed collection of objects that can be processed in parallel across nodes.
    - **DataFrames**: Like tables in a database but distributed; allows for more optimized queries and operations than RDDs.

> **Quote by Alfred North Whitehead**:
> 
> - "Civilization advances by extending the number of important operations which we can perform without thinking of them."
> - This reflects the power of abstractions, as they allow developers to focus on high-level logic without worrying about low-level data handling.

#### Limitations of Hadoop & Spark’s Advantage

**Limitation of Hadoop**![[Screenshot 2025-02-23 at 6.33.14 PM.png]]
- Hadoop reads and writes data to **disk** after every stage.
    - Example:
        - **Map Phase**: Reads data → Processes → Writes output back to disk.
        - **Reduce Phase**: Reads data again → Processes → Writes output.
- **Problem**:
    - Disk I/O (Input/Output) is **slow** compared to memory.
    - Repeated read-write cycles reduce performance, especially in iterative tasks (e.g., machine learning algorithms).

**Spark’s Advantage**![[Screenshot 2025-02-23 at 6.33.30 PM.png]]
- Spark uses **in-memory computation**:
    - Data is read from disk **once**.
    - All further operations (map, reduce, etc.) are done **in memory** (RAM).
- **Result**:
    - Much faster than Hadoop for multi-step processing (e.g., iterative algorithms).

#### Memory Latencies

**Speed Comparison (From Fastest to Slowest)**
1. **Processor (CPU)**: Executes instructions at nanosecond speeds.
2. **DRAM (RAM)**: Stores temporary data (quick access).
3. **Network**: Transfers data between computers (microsecond delays).
4. **Flash Storage**: Faster than traditional disks but slower than memory.
5. **Hard Disk Drive (HDD)**: Slowest, works in milliseconds.
![[Screenshot 2025-02-23 at 6.37.23 PM.png]]
If Memory = Minute, Network = Weeks, Flash = Months, Disk = Years

> 💡 Processing in **RAM** (as Spark does) is dramatically faster than relying on **disk** (as Hadoop often does).

#### RDD (Resilient Distributed Dataset)
![[Screenshot 2025-02-23 at 6.56.08 PM.png]]
- Core data structure in Spark
- Contains distributed data spread across partitions for parallel processing.
- **Immutable**: Once created, it cannot be changed (new RDDs are created for transformations).
- **Lazily evaluated**: Only executes computations when needed (when an action is called).
- **Resilient**: Automatically recomputes data if a failure occurs.
	- Fault Tolerance: 
		- Hadoop: via HDFS replication
		- Spark: RDD lineage recovery

**How RDD Works:**
- Transformations (like `map`, `filter`) create new RDDs.
- Actions (like `count`, `collect`) trigger computation.
- Tracks **lineage**: A history of transformations to help with fault recovery.

**Three Ways to Create an RDD:**
1. **Loading from an external file:**
```python
lines = sc.textFile("file.txt")
```
- Reads a text file from HDFS or local storage into an RDD.

2. **Parallelizing a collection:**
```python
lines = sc.parallelize([1, 2, 3])
```
- Converts a local Python list into an RDD for distributed processing.

3. **Transforming an existing RDD:**
```python
pyLines = lines.filter(lambda line: "Python" in line)
```
- Filters the RDD to only include lines containing the word `"Python"`.

#### Exercises