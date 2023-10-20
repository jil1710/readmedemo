
![image](https://github.com/jil1710/readmedemo/assets/125335932/26a866fd-9694-465e-a445-ff87052e235e)


# Elastic Search

- Elasticsearch is a free, open-source search and analytics engine based on the Apache Lucene library. It’s the most popular search engine and has been available since 2010.Elasticsearch is a highly scalable open-source full-text search and analytics engine. It allows you to store, search, and analyze big volumes of data quickly and in near real time. It is generally used as the underlying engine/technology that powers applications that have complex search features and requirements. Elasticsearch provides a distributed system on top of Lucene StandardAnalyzer for indexing and automatic type guessing and utilizes a JSON based REST API to refer to Lucene features.

- It is easy to set up out of the box since it ships with sensible defaults and hides complexity from beginners. It has a short learning curve to grasp the basics so anyone with a bit of efforts can become productive very quickly. It is schema-less, using some defaults to index the data.

- It is built in Java. Easy to use and highly scalable. Often used to provide search functionality to an application with features like auto complete, correcting typos, handling synonyms, logging etc...
 
## Let's Understand Component and Terminology of Elastic search

- **Elasticsearch Components**

  - Cluster

    One or more servers collectively providing indexing and search capabilities form an Elasticsearch cluster. The cluster size can vary from a single node to thousands of nodes, depending on the use cases.

  - Node

    Node is a single physical or virtual machine that holds full or part of your data and provides computing power for indexing and searching your data. Every node is identified with a unique name. If the node identifier is not specified, a random UUID is assigned as a node identifier at the startup. Every node configuration has the property `cluster.name`. The cluster will be formed automatically with all the nodes having the same `cluster.name` at startup.

    - A node has to accomplish several duties like:

        - Storing the data.
        - Performing operations on data (indexing, searching, aggregation, etc.).
        - Maintaining the health of the cluster.
     
- Elasticsearch is standing as a NOSQL DB that store data as document type.

  ![image](https://github.com/jil1710/readmedemo/assets/125335932/482f5abb-1d03-418c-a6f0-468734280553)

- Index

    Index is a container to store data similar to a database in the relational databases. An index contains a collection of documents that have similar characteristics or are logically related. If we take an example of an e-commerce website, there will be one index for products, one for customers, and so on. Indices are identified by the lowercase name. The index name is required to perform the add, update, or delete operations on the document.

- Type

  Type is a logical grouping of the documents within the index. In the previous example of product index, we can further group documents into types, like electronics, fashion, furniture, etc. Types are defined on the basis of documents having similar properties in it. It isn’t easy to decide when to use type over index. Indices have more overheads, so sometimes, it is better to use different types in the same index for better performance. There are a couple of restrictions to use types as well. Two fields having the same name in a different type of document should be of the same data type (string, date, etc).

- Document

    Document is the piece indexed by Elasticsearch. A document is represented in the JSON format. We can add as many documents as we want into an index. The following snippet shows how to create a document of type mobile in the index store.

## Data Type in Elastic Search

- Text

    This data type is used to store full text like product description. These fields participate in full-text search. These type of fields are analyzed while storing which enables to searching these fields by the individual word in it. These type of fields are not used in sorting and aggregation queries.

  ![image](https://github.com/jil1710/readmedemo/assets/125335932/07249449-603f-481d-a521-67b8b2e33daf)


- Keywords

    This type is also used to store text data but unlike Text, it is not analyzed and stored as-is. This is suitable to store information like a user’s mobile number, city, age, etc. These fields are used in filter, aggregation and sorting queries. For e.g. list all users from a particular city, filter by their age.

    ![image](https://github.com/jil1710/readmedemo/assets/125335932/056fa496-1734-45e2-89eb-f9acc9df42c4)


- Numeric

  Elasticsearch supports a wide range of numeric type long, integer, short, byte, double, float.

  ![image](https://github.com/jil1710/readmedemo/assets/125335932/6499525d-39ea-48d6-aa32-c52aa91f3745)


- Boolean & Data

  Elastic Search also support boolean and date type data to store boolean and date object.

  ![image](https://github.com/jil1710/readmedemo/assets/125335932/8ebdd520-719b-40c7-b8cc-44529d67ff92)
  

- Object

  If you know JSON well, this is not a new concept. Elasticsearch also allows storing nested JSON object structure as a document.

  ![image](https://github.com/jil1710/readmedemo/assets/125335932/db792faa-6e87-43e0-8714-fefdfe6b77a2)


- Nested

    The Object data type is not that useful due to its underlying data representation in the Lucene index. Lucene index does not support inner JSON object. ES flattens the original JSON to make it compatible to store in Lucene index. Due to this fields of the multiple inner objects get merged into one leading to wrong search results. Most of the time what you may want to use is Nested Datatype over Object.

  ![image](https://github.com/jil1710/readmedemo/assets/125335932/7b072896-ef01-4d7a-9d4e-2c09cfe6fa6c)


- Geo Point

    This data type is used to store geographical location. It accepts latitude and longitude pair. To give an example, this data type can be used to arrange the user’s photo library by their geographical location or graphically display the locations which are trending on social media news.

- Geo Shape

    It allows storing arbitrary geometric shapes like rectangle, polygon.

- Below is the Scheme or Mapping of ablove data type

- **Example : Look below index :**

  ![image](https://github.com/jil1710/readmedemo/assets/125335932/1df890d9-d4d3-4a36-a166-878c9bc91d90)

  ![image](https://github.com/jil1710/readmedemo/assets/125335932/41ef854a-d683-4b67-a1d4-200300b2ed31)


## Let's implement Elastic Search using ASP.Net Core integration





   

