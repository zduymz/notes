## What is Elasticsearch?

Elasticsearch is a distributed, open-source search and analytics engine built on Apache Lucene and developed in Java. It started as a scalable version of the Lucene open-source search framework then added the ability to horizontally scale Lucene indices. Elasticsearch allows you to store, search, and analyze huge volumes of data quickly and in near real-time and give back answers in milliseconds. It’s able to achieve fast search responses because instead of searching the text directly, it searches an index. It uses a structure based on documents instead of tables and schemas and comes with extensive REST APIs for storing and searching the data. At its core, you can think of Elasticsearch as a server that can process JSON requests and give you back JSON data.

## Logical Concepts
### Documents
Documents are the basic unit of information that can be indexed in Elasticsearch expressed in JSON.  
You can think of a document like a row in a relational database, representing a given entity — the thing you’re searching for. In Elasticsearch, a document can be more than just text, it can be any structured data encoded in JSON. That data can be things like numbers, strings, and dates. Each document has a unique ID and a given data type, which describes what kind of entity the document is. For example, a document can represent an encyclopedia article or log entries from a web server.  

### Indices
An index is a collection of documents that have similar characteristics. An index is the highest level entity that you can query against in Elasticsearch. You can think of the index as being similar to a database in a relational database schema. Any documents in an index are typically logically related. In the context of an e-commerce website, for example, you can have an index for Customers, one for Products, one for Orders, and so on. An index is identified by a name that is used to refer to the index while performing indexing, search, update, and delete operations against the documents in it.  

### Inverted Index
An index in Elasticsearch is actually what’s called an inverted index, which is the mechanism by which all search engines work. It is a data structure that stores a mapping from content, such as words or numbers, to its locations in a document or a set of documents. Basically, it is a hashmap-like data structure that directs you from a word to a document. An inverted index doesn’t store strings directly and instead splits each document up to individual search terms (i.e. each word) then maps each search term to the documents those search terms occur within. For example, in the image below, the term “best” occurs in document 2, so it is mapped to that document. This serves as a quick look-up of where to find search terms in a given document. By using distributed inverted indices, Elasticsearch quickly finds the best matches for full-text searches from even very large data sets.  

![Visual Representation of an Inverted Index](https://www.knowi.com/wp-content/uploads/2020/03/inverse-index.webp)

## Backend Components
### Cluster
An Elasticsearch cluster is a group of one or more node instances that are connected together. The power of an Elasticsearch cluster lies in the distribution of tasks, searching, and indexing, across all the nodes in the cluster.

### Node
A node is a single server that is a part of a cluster. A node stores data and participates in the cluster’s indexing and search capabilities. An Elasticsearch node can be configured in different ways:

* Master Node — Controls the Elasticsearch cluster and is responsible for all cluster-wide operations like creating/deleting an index and adding/removing nodes.

* Data Node — Stores data and executes data-related operations such as search and aggregation.

* Client Node — Forwards cluster requests to the master node and data-related requests to data nodes.

### Shards
Elasticsearch provides the ability to subdivide the index into multiple pieces called shards. Each shard is in itself a fully-functional and independent “index” that can be hosted on any node within a cluster. By distributing the documents in an index across multiple shards, and distributing those shards across multiple nodes, Elasticsearch can ensure redundancy, which both protects against hardware failures and increases query capacity as nodes are added to a cluster.

### Replicas
Elasticsearch allows you to make one or more copies of your index’s shards which are called “replica shards” or just “replicas”. Basically, a replica shard is a copy of a primary shard. Each document in an index belongs to one primary shard. Replicas provide redundant copies of your data to protect against hardware failure and increase capacity to serve read requests like searching or retrieving a document.

## What is Elasticsearch used for?

### Use Cases
* **Textual Search (searching for pure text)** – Elasticsearch is primarily used where there is lots of text and we want to search any data for the best match with a specific phrase.
* **Product Search** – Elasticsearch is used to facilitate faster product search using properties and name (textual search and structured data).
* **Data Aggregation** – The aggregation’s framework helps provide aggregated data based on a search query. It is based on simple building blocks called aggregations, that can be composed in order to build complex summaries of the data. An aggregation can be seen as a unit-of-work that builds analytic information over a set of documents. The context of the execution defines what this document set is (e.g. a top-level aggregation executes within the context of the executed query/filters of the search request).
* **JSON Document Storage** – A JSON object with some data. It’s the basic information unit in ES. The document is a basic information unit that can be indexed.
* **Geo-Search** – Elasticsearch can be used to geo-localized any product. For example, the search query: ‘all the restaurants that serve pizza within 30 minutes’ can use Elasticsearch to display information of the relevant pizzerias instantly.
* **Auto-Suggest** – It allows the user to start typing a few characters and receive a list of suggested queries as they type.
* **Auto-Complete** – Elasticsearch database helps in autocompleting the search query by completing a search box on partially-typed words, based on the previous searches.
* **Metrics & Analytics** – Elasticsearch analyzes a ton of dashboards consisting of several emails, logs, databases, and syslogs, to help businesses make sense of their data and provide actionable insights.

### Benefit of using Elasticsearch
* **Direct, Easy, and Fast access**: Documents are stored in close proximity to the corresponding metadata in the index. This reduces the number of data reads and as a result increases the search result response.
* **Manages huge amounts of data**: As a comparison to the traditional SQL database management systems that take more than 10 seconds to fetch required search query data, Elasticsearch can do that within a few microseconds (10, to be exact).
* **Scalability of the search engine**: As Elasticsearch has a distributed architecture it enables us to scale up to thousands of servers and accommodate petabytes of data. The customers then need not manage the complexity of distributed design as it has been done automatically.

### DON'T use if
* You are looking for catering to transaction handling.
* You are planning to do a highly intensive computational job in the data store layer.
* You are looking to use this as a primary data store. If this is a requirement, when data is inserted, Elasticsearch has to re-index and this would take some time and wouldn't be available as and when the data was inserted and updated.
* You are looking for an ACID compliant data store.
* You are looking for a durable data store.