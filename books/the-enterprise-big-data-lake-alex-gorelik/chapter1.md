## Definition
* A **data puddle** is basically a single-purpose or single-project data mart built using big data technology. It is typically the first step in the adoption of big data technology. The data in a data puddle is loaded for the purpose of a single project or team. It is usually well known and well understood, and the reason that big data technology is used instead of traditional data warehousing is to lower cost and provide better performance.

* A **data pond** is a collection of data puddles. It may be like a poorly designed data warehouse, which is effectively a collection of colocated data marts, or it may be an offload of an existing data warehouse. While lower technology costs and better scalability are clear and attractive benefits, these constructs still require a high level of IT participation. Furthermore, data ponds limit data to only that needed by the project, and use that data only for the project that requires it. Given the high IT costs and limited data availability, data ponds do not really help us with the goals of democratizing data usage or driving self-service and data-driven decision making for business users.

* A **data lake** is different from a data pond in two important ways. First, it supports self-service, where business users are able to find and use data sets that they want to use without having to rely on help from the IT department. Second, it aims to contain data that business users might possibly want even if there is no project requiring it at the time.

* A **data ocean** expands self-service data and data-driven decision making to all enterprise data, wherever it may be, regardless of whether it was loaded into the data lake or not.


The key difference between the *data pond* and the *data lake* is the focus. Data ponds provide a less expensive and more scalable technology alternative to existing relational data warehouses and data marts. Whereas data lake are focused on running routine, production-ready queries, it enable business users to leverage data to make their own decisions by doing ad hoc analysis and experimentation with a variety of new types of data and tools.


## Creating a Successful Data lake
three key prerequisites can be identified:
* The right platform
* The right data
* The right interface

### The right platform
Big data technologies like Hadoop and cloud solutions like Amazon Web Services (AWS), Microsoft Azure, and Google Cloud Platform are the most popular platforms for a data lake. These technologies share several important advantages:
* Volume  
These platforms were designed to scale out—in other words, to scale indefinitely without any significant degradation in performance.

* Cost  
We have always had the capacity to store a lot of data on fairly inexpensive storage, like tapes, WORM disks, and hard drives. But not until big data technologies did we have the ability to both store and process huge volumes of data so inexpensively—usually at one-tenth to one-hundredth the cost of a commercial relational database.

* Variety  
These platforms use filesystems or object stores that allow them to store all sorts of files: Hadoop HDFS, MapR FS, AWS’s Simple Storage Service (S3), and so on. Unlike a relational database that requires the data structure to be predefined (schema on write), a filesystem or an object store does not really care what you write. Of course, to meaningfully process the data you need to know its schema, but that’s only when you use the data. This approach is called schema on read and it’s one of the important advantages  of big data platforms, enabling what’s called “frictionless ingestion.” In other words, data can be loaded with absolutely no processing, unlike in a relational database, where data cannot be loaded until it is converted to the schema and format expected by the database.

* Future-proofing  
Because our requirements and the world we live in are in flux, it is critical to make sure that the data we have can be used to help with our future needs. Today, if data is stored in a relational database, it can be accessed only by that relational database. Hadoop and other big data platforms, on the other hand, are very modular. The same file can be used by various processing engines and programs—from Hive queries (Hive provides a SQL interface to Hadoop files) to Pig scripts to Spark and custom MapReduce jobs, all sorts of different tools and systems can access and use the same files. Because big data technology is evolving rapidly, this gives people confidence that any future projects will still be able to access the data in the data lake.
### The right data
Most data collected by enterprises today is thrown away. Some small percentage is aggregated and kept in a data warehouse for a few years, but most detailed operational data, machine-generated data, and old historical data is either aggregated or thrown away altogether. That makes it difficult to do analytics. For example, if an analyst recognizes the value of some data that was traditionally thrown away, it may take months or even years to accumulate enough history of that data to do meaningful analytics. The promise of the data lake, therefore, is to be able to store as much data as possible for future use.  
  
So, the data lake is sort of like a *piggy bank*, you often don’t know what you are saving the data for, but you want it in case you need it one day. Moreover, because you don’t know how you will use the data, it doesn’t make sense to convert or treat it prematurely. You can think of it like traveling with your *piggy bank* through different countries, adding money in the currency of the country you happen to be in at the time and keeping the contents in their native currencies until you decide what country you want to spend the money in; you can then convert it all to that currency, instead of needlessly converting your funds (and paying conversion fees) every time you cross a border. To summarize, the goal is to **save as much data as possible in its native format**.  
  
Another challenge with getting the right data is *data silos*. Different departments might hoard their data, both because it is difficult and expensive to provide and because there is often a political and organizational reluctance to share. In a typical enterprise, if one group needs data from another group, it has to explain what data it needs and then the group that owns the data has to implement ETL jobs that extract and package the required data. This is expensive, difficult, and time-consuming, so teams may push back on data requests as much as possible and then take as long as they can get away with to provide the data. This extra work is often used as an excuse to not share data.  
  
With a data lake, because the lake consumes raw data through frictionless ingestion (basically, it’s ingested as is without any processing), that challenge (and excuse) goes away. A well-governed data lake is also centralized and offers a transparent process to people throughout the organization about how to obtain data, so ownership becomes much less of a barrier.
### The Right Interface
Once we have the right platform and we’ve loaded the data, we get to the more difficult aspects of the data lake, where most companies fail—choosing the right interface. To gain wide adoption and reap the benefits of helping business users make data-driven decisions, the solutions companies provide must be self-service, so their users can find, understand, and use the data without needing help from IT. IT will simply not be able to scale to support such a large user community and such a large variety of data.

There are two aspects to enabling self-service: providing data at the right level of expertise for the users, and ensuring the users are able to find the right data.
* PROVIDING DATA AT THE RIGHT LEVEL OF EXPERTISE  

* FIND THE RIGHT DATA
“Most companies that I have spoken with are settling on the “shopping for data” paradigm, where analysts use an Amazon.com-style interface to find, understand, rate, annotate, and consume data. The advantages of this approach are manifold, including:

    * A familiar interface
    * Faceted search  
Search engines are optimized for faceted search. Faceted search is very helpful when the number of possible search results is large and the user is trying to zero in on the right result. For example, if you were to search Amazon for toasters, facets would list manufacturers, whether the toaster should accept bagels, how many slices it needs to toast, and so forth. Similarly, when users are searching for the right data sets, facets can help them specify what attributes they would like in the data set, the type and format of the data set, the system that holds it, the size and freshness of the data set, the department that owns it, what entitlements it has, and any number of other useful characteristics.  
    * Ranking and sorting  
The ability to present and sort data assets, widely supported by search engines, is important for choosing the right asset based on specific criteria.
    * Contextual search  
As catalogs get smarter, the ability to find data assets using a semantic understanding of what analysts are looking for will become more important. For example, a salesperson looking for customers may really be looking for prospects, while a technical support person looking for customers may really be looking for existing customers.
  
## The Data Swamp
While data lakes always start out with good intentions, sometimes they take a wrong turn and end up as data swamps. A data swamp is a data pond that has grown to the size of a data lake but failed to attract a wide analyst community, usually due to a lack of self-service and governance facilities. At best, the data swamp is used like a data pond, and at worst it is not used at all.”

## Roadmap to Data Lake Success
Usually, companies follow this process:

* Stand up the infrastructure (get the Hadoop cluster up and running).
* Organize the data lake (create zones for use by various user communities and ingest the data).
* Set the data lake up for self-service (create a catalog of data assets, set up permissions, and provide tools for the analysts to use).
* Open the data lake up to the users.

### Organizing the Data Lake
Most data lakes that I have encountered are organized roughly the same way, into various zones:
* A raw or landing zone where data is ingested and kept as close as possible to its original state.
* A gold or production zone where clean, processed data is kept.
* A dev or work zone where the more technical users such as data scientists and data engineers do their work. This zone can be organized by user, by project, by subject, or in a variety of other ways. Once the analytics work performed in the work zone gets productized, it is moved into the gold zone.
* A sensitive zone that contains sensitive data.


Take away notes:
* I don't buy those definition much. That could be my language barrier. 
