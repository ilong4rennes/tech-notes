For every new-emerged technology, we ask two questions:
- Can we solve new problems?
- Can we solve old problems in a better way?
### The Big Data Process Loop

1. **Data Ingestion:** Collect data from multiple sources including sensors, social media, transactional databases, and more.
2. **Data Storage:** Store data in scalable systems, often using distributed file systems (e.g., Hadoop Distributed File System or cloud storage solutions).
3. **Data Processing:** Utilize parallel processing frameworks to clean, organize, and transform data. Batch processing (with tools like Hadoop) and real-time processing (with tools like Apache Spark) are common approaches.
4. **Data Analysis:** Apply statistical models, machine learning algorithms, and data mining techniques to extract insights. Visualization tools then help interpret the data.
5. **Insight Generation:** Convert analyzed data into actionable insights that support decision making. This could involve predictive analytics, trend analysis, or personalized recommendations.
6. **Feedback and Optimization:** Continuously refine data models and processes based on feedback, creating a loop where insights drive further data collection and improved analysis techniques.

- **Distributed Computing Frameworks:**
    - **Hadoop:** An open-source framework that allows for the distributed processing of large data sets across clusters of computers.
    - **Apache Spark:** Offers in-memory data processing capabilities, making it faster for iterative algorithms and real-time data analysis.
- **Data Storage Solutions:**
    - **NoSQL Databases:** Such as MongoDB, Cassandra, and HBase, which handle unstructured data more flexibly than traditional SQL databases.
    - **Cloud Storage:** Services like Amazon S3, Google Cloud Storage, and Azure Blob Storage provide scalable and cost-effective storage.

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

### Horizontal Scaling (Non-relational Databases - NoSQL)

- **Non-relational databases** (e.g., MongoDB, Cassandra, DynamoDB) are designed for **horizontal scaling**.
- **How it works**: Add more machines (nodes) to the system, distributing the data across multiple servers.
- **Advantages**:
    - Easily handles massive data volumes and high traffic.
    - More cost-effective since it can use commodity hardware.
    - High availability and fault tolerance—if one node fails, others take over (reliability through redundancy).
- **Example**: Sharding a MongoDB database across multiple servers to handle larger datasets.

### Hadoop, HDFS, and MapReduce

Hadoop’s **HDFS** (Hadoop Distributed File System) is based on two core components:
- MapReduce
- Google File System (GFS)
![[Screenshot 2025-02-21 at 1.40.27 AM.png]]
#### MapReduce

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

#### Google File System (GFS) / HDFS

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

### Hands-on with MRJob

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

### Paradigm: map – shuffle – reduce
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

> 💡 "As many lines of input, so many mappers" — Every line or chunk of input data can be processed by a separate mapper.

3. Shuffle and Sort Phase (Middle Section)
	- After mapping, the intermediate key-value pairs are **shuffled**:
	    - This step groups all the same keys (e.g., all instances of a word) together, regardless of which mapper produced them.
	    - Example: All pairs like `(word: "data", count: 1)` from different mappers are collected together.
	- The **sorting** process organizes keys to ensure that all data with the same key goes to the same **reducer**.
	- The arrows in the diagram represent how specific colored data blocks are sent across nodes based on their key.

> 💡 All values associated with the same key are directed to the same reducer, even if they came from different nodes.

4. Reduce Phase (Final Section of Each Node)
	- The **reduce function** processes grouped data:
	    - Aggregates values for each key.
	    - Example: In a word count task, the reducer sums up the occurrences of each word.
	- Each reducer outputs the final result for a specific group of keys (e.g., all counts for the word "data").

> 💡 "As many unique tags yielded by the mapper, so may reducers" — The number of reducers depends on the number of unique keys in the data.

5. Output Data (Rightmost Column)
	- All the outputs from reducers are combined into the final result.
	- The final output is typically sorted or organized by key (e.g., alphabetical word count list).

- Understanding the **map-shuffle-reduce** paradigm:
    - Each line of input generates a separate mapper.
    - Each unique key (tag) from the mapper corresponds to a reducer.

