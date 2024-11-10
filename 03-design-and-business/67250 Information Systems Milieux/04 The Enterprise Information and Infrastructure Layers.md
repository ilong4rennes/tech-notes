# Business Analytics and Big Data

## Business Analytics

### Types of Analytics
- **Descriptive analytics**: uses data to understand past and present
	- Example in healthcare: Average number of patients readmitted in emergency care in a period -- Link to Key Performance Indicators
- **Predictive analytics**: analyzes past performance to predict future 
	- A four-step process: 
		1. Identify the problem 
		2. Explore historical data (What descriptive analytics has to say) 
		3. Build and validate a model based on the data
		4. Deploy the model on new data to make predictions (with probabilities of accuracy).
	- E.g. Classify patients who are at high risk for a condition such as diabetes or coronary artery disease.
- **Prescriptive analytics**: uses optimization techniques to come up with best recommendations
	- predictive analytics --helps determine what might happen, prescriptive analytics helps determine the best course of **action**
	- E.g. How many beds to allocate for a certain category of patients during a certain period (E.g. Flu season)

### Role Of Data Analytics In Modern Organizations
![[Screen Shot 2024-05-06 at 08.41.12.png]]
#### Sense & Respond: Query and Reporting
- Canned reports: Provide regular summaries of information in a predetermined format. 
- Ad hoc reporting tools: Puts users in control so that they can create custom reports on an as-needed basis by selecting fields, ranges, summary conditions, and other parameters. 
- Dashboards: Heads-up display of critical indicators that allows managers to get a graphical glance at key performance metrics. 
- Online Transaction processing (OLTP): Takes data from standard relational databases and uses the data to facilitate transactions by large numbers of people, typically over the internet. 
- Online analytical processing (OLAP): Takes data from standard relational databases, calculates and summarizes the data
	- dynamic pricing: Changing pricing based on demand conditions
	- Much **more unstructured** data being captured
#### Information Systems for Operations and Analysis
- data warehouse: a set of databases designed to support decision-making in an organization. It is structured for fast online queries and exploration. Data warehouses may aggregate enormous amounts of data from many different operational systems. 
- data mart: a database focused on addressing the concerns of a **specific** problem (e.g., increasing customer retention, improving product quality) or business unit (e.g., marketing, engineering).

- Walmart - A Data-driven Value Chain
	- Source of competitive advantage is scale.
	- Efficiency starts with a proprietary system called Retail Link. 
		- Retail Link—records a sale and automatically triggers inventory reordering, scheduling, and delivery. 
		- inventory turnover ratio: Ratio of a company’s annual sales to its inventory. 
	- Back-office scanners keep track of inventory as supplier shipments come in.
	- Facilitates just-in-time inventory management
	- Data-Driven Prediction
	- Walmart shares sales data only with relevant suppliers
## The Promise of Big Data
- Big data: a collection of data sets so large and complex that it becomes difficult to process using regular database management tools or traditional data processing applications
- 4+1V to characterize big data: Volume, Velocity, Variety, Veracity, Value ![[Screen Shot 2024-05-06 at 10.13.11.png]]
- **Data mining**: The process of using computers to identify hidden patterns in, and to build models from, large datasets
- **Big Data Analytics** goes one step forward by examining the raw data with the purpose of drawing conclusions (making inferences) about that information. 
- The ultimate objective of both is to **help make better decisions (future)**
- E.g. Big Data Helps Fix Boston’s Potholes
	- Crowdsourcing approach

# Database

## What Are The Steps in Building Databases?
1. Conceptual Model (Outcome: ERD)
	- Entity, attribute, relationship
		- Cardinality: the maximum number of relationship instances in which an entity can participate. (1, N)
		- Modality: Optional or Mandatory relationship (minimum number of relationship instances) (0, 1)
		- Resolving Many-to-Many Relationships
2. Logical Model (Outcome: Data dictionary)
	- Conversion
	- Normalization
		- PK: Each row in a table must be uniquely identified by the value of the Primary Key
		- FK: All values of foreign keys must exist as values in the parent table
		- Advantage: No redundancy, integrity, avoids anomalies (update, deletion, insert)
		- 1NF
			- There are no repeating or duplicate fields 
			- Each record is uniquely identified by a primary key (courseid) 
			- Each cell contains only a single value
		- 2NF
			- It is already in 1NF and all nonprimary key fields depend on the key
		- 3NF
			- 2NF is a pre-requisite 
			- No non-key field depends upon another non-key field (all non-key fields depend only on the primary key).
3. Physical Database (Outcome: Database)
