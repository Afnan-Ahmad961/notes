# Database Design

> Auto-generated summaries from articles and videos. Last updated: 2026-08-19.

## Table of Contents

<!-- INDEX START -->
<!-- INDEX END -->

---


## Database Design and Modeling: A Complete Beginner's Guide (OLTP & OLAP)

*Added: 2026-08-19 01:21*

**Source:** [https://www.youtube.com/watch?v=_jf7KZqne-8&t=10507s](https://www.youtube.com/watch?v=_jf7KZqne-8&t=10507s)

## Contents

- [Database Design and Modeling: Overview](#database-design-and-modeling-overview)
- [Database Design and Modeling: Why Database Design and Modeling?](#database-design-and-modeling-why-database-design-and-modeling)
- [Database Design and Modeling: What is Data Modeling?](#database-design-and-modeling-what-is-data-modeling)
- [Database Design and Modeling: Layers of Data Modeling](#database-design-and-modeling-layers-of-data-modeling)
- [Database Design and Modeling: Types of Data Modeling - OLTP vs. OLAP](#database-design-and-modeling-types-of-data-modeling---oltp-vs-olap)
- [Database Design and Modeling: OLTP Database Modeling](#database-design-and-modeling-oltp-database-modeling)
  - [Database Design and Modeling: Requirement Gathering (Conceptual Layer)](#database-design-and-modeling-requirement-gathering-conceptual-layer)
  - [Database Design and Modeling: Logical Layer - Entity-Relationship (ER) Diagram](#database-design-and-modeling-logical-layer---entity-relationship-er-diagram)
  - [Database Design and Modeling: Data Types](#database-design-and-modeling-data-types)
  - [Database Design and Modeling: Keys](#database-design-and-modeling-keys)
  - [Database Design and Modeling: Relationships (Cardinality)](#database-design-and-modeling-relationships-cardinality)
  - [Database Design and Modeling: Normalization](#database-design-and-modeling-normalization)
  - [Database Design and Modeling: Physical Layer (Implementation)](#database-design-and-modeling-physical-layer-implementation)
- [Database Design and Modeling: OLAP Data Modeling (Data Warehousing)](#database-design-and-modeling-olap-data-modeling-data-warehousing)
  - [Database Design and Modeling: Medallion Architecture (Bronze, Silver, Gold Layers)](#database-design-and-modeling-medallion-architecture-bronze-silver-gold-layers)
  - [Database Design and Modeling: Dimensional Data Modeling (Fact and Dimension Tables)](#database-design-and-modeling-dimensional-data-modeling-fact-and-dimension-tables)
  - [Database Design and Modeling: Star Schema vs. Snowflake Schema](#database-design-and-modeling-star-schema-vs-snowflake-schema)
  - [Database Design and Modeling: Slowly Changing Dimensions (SCD)](#database-design-and-modeling-slowly-changing-dimensions-scd)

## Database Design and Modeling: Overview

This course provides a comprehensive guide to database design and modeling, covering both OLTP (Online Transactional Processing) and OLAP (Online Analytical Processing) methodologies. The goal is to equip learners with the skills to design and build a complete OLTP database and then use it to construct an OLAP data warehouse (star schema) from scratch. The learning approach is practical, focusing on building and doing, with step-by-step explanations and real-world examples.

## Database Design and Modeling: Why Database Design and Modeling?

Organizations receive data from various sources like APIs, file servers, and applications, which is typically stored in a database. While it's possible to dump all data directly into a database, this leads to several problems that data modeling aims to solve:

-   **Data Redundancy:** Repeating the same information multiple times, e.g., customer details for every order they place.
    -   **Historical Context:** In the 1980s, storing 1 TB of data could cost \$300-400 million, making data redundancy a massive financial burden. Although storage costs are much lower today, redundancy still increases storage requirements unnecessarily.
-   **Performance Degradation:** Dumping all data into a single, wide table (hundreds of columns) makes querying inefficient, even for small data sets.
-   **Scalability and Maintenance Issues:**
    -   **Inconsistency:** Without a structured model, data can easily become inconsistent.
    -   **Difficulty in Updates:** Frequent changes (e.g., a customer's address) become difficult to manage and update across redundant entries, leading to errors and data integrity problems.

Data modeling addresses these issues by organizing data efficiently, improving performance, and simplifying maintenance.

## Database Design and Modeling: What is Data Modeling?

Data modeling is a technique used to transform messy, unstructured, or poorly organized data into a structured, organized, and efficient format. It involves creating a blueprint for how data will be stored, managed, and accessed within a database. The primary goal is to create a nice, organized structure for structured data (rows and columns) to avoid a single table with thousands of columns and millions of rows.

## Database Design and Modeling: Layers of Data Modeling

Data modeling is typically broken down into several layers:

1.  **Conceptual Layer (Business Layer):**
    -   **Purpose:** Focuses on understanding the business requirements and high-level overview of the data model.
    -   **Activities:** Technical personnel (e.g., team leads) communicate with senior leadership and stakeholders to gather requirements. This involves asking questions about what data is needed, the overall goals, and key performance indicators (KPIs) for reports.
    -   **Output:** A clear understanding of the business problem and the data needed to solve it, expressed in business terms.

2.  **Logical Layer:**
    -   **Purpose:** Translates business requirements into a technical design, defining the structure of the data without specifying the actual database system.
    -   **Activities:** Building an Entity-Relationship (ER) diagram. This involves:
        -   **Identifying Entities:** Major themes or subjects (e.g., Customers, Orders, Products) that will become tables.
        -   **Identifying Attributes:** Characteristics or properties of each entity (e.g., Customer ID, Customer Email, Product Name) that will become columns.
        -   **Defining Relationships:** How entities are connected to each other.
    -   **Output:** A graphical representation of the data model (ER diagram) showing tables, columns, and their relationships. This layer is crucial for understanding how different parts of the data model connect.

3.  **Physical Layer (Implementation Layer):**
    -   **Purpose:** Implements the logical data model in a specific database system.
    -   **Activities:** Writing Data Definition Language (DDL) SQL statements (e.g., `CREATE TABLE`, `ALTER TABLE`) to create the database schema, including tables, columns, data types, primary keys, foreign keys, and other constraints.
    -   **Output:** A functional database with defined tables and relationships, ready for data storage and querying.
    -   **AI Integration:** Modern tools and LLMs (Large Language Models) can automate the generation of SQL scripts from ER diagrams, significantly speeding up this process. The generated scripts still require review by a human expert.

## Database Design and Modeling: Types of Data Modeling - OLTP vs. OLAP

There are two major types of data modeling, each serving different purposes:

1.  **OLTP (Online Transactional Processing):**
    -   **Purpose:** Designed for applications that handle frequent, small, and fast transactions (e.g., placing an order, updating a customer's address). It's focused on operational databases.
    -   **Characteristics:**
        -   **Fast Updates/Writes:** Optimized for quick `INSERT`, `UPDATE`, and `DELETE` operations.
        -   **Frequent Changes:** Data changes constantly, often one record at a time.
        -   **Data Integrity:** High emphasis on data consistency and avoiding redundancy (achieved through normalization).
        -   **Users:** Customers interacting directly with an application (e.g., Amazon users).
        -   **Example:** An e-commerce database where users place orders, update profiles, or cancel items.

2.  **OLAP (Online Analytical Processing):**
    -   **Purpose:** Designed for analytical queries, reporting, and business intelligence. It's focused on data warehousing.
    -   **Characteristics:**
        -   **Fast Reads/Retrievals:** Optimized for querying large volumes of historical data (millions or billions of records) to find insights.
        -   **Infrequent Changes:** Data is typically loaded periodically (e.g., daily, hourly) and is mostly read-only.
        -   **Denormalized Data:** Prioritizes query performance over data redundancy, often using denormalized structures to minimize joins.
        -   **Users:** Internal company users like managers, analysts, and leaders who build reports and analyze trends.
        -   **Example:** A data warehouse built on top of an OLTP database to analyze sales trends over the last 10 years.

**Key Difference:** OLTP focuses on the speed of *changing* data, while OLAP focuses on the speed of *retrieving* and *analyzing* large amounts of data. OLAP models are often built using OLTP databases as their source.

## Database Design and Modeling: OLTP Database Modeling

Building an OLTP data model involves a structured approach, following the conceptual, logical, and physical layers.

### Database Design and Modeling: Requirement Gathering (Conceptual Layer)

This is the initial and most critical phase. It involves interacting with non-technical stakeholders to understand business needs.

-   **The Challenge:** Stakeholders often provide vague requirements (e.g., "build a data model").
-   **The Solution:** The data modeler (e.g., "Ardan") must ask probing questions to extract concrete information.
    -   **Start with KPIs:** Ask about the Key Performance Indicators (KPIs) they want to track or dashboards they need to build (e.g., average sales, 30-day running total).
    -   **Identify Subject, Characteristic, Relation:** From their answers, extract these three elements:
        -   **Subject:** The main topic or entity (e.g., "customers"). Subjects typically become tables.
        -   **Characteristic:** A property or attribute of the subject (e.g., "email" for customers). Characteristics can become columns or, if they can be further broken down, separate tables (objects).
        -   **Relation:** How subjects/objects are connected (e.g., "customers can place orders"). Relations are defined later in the ER diagram.
-   **Example:**
    -   "Customers will have an email." -> Subject: Customers, Characteristic: Email. (Email becomes a column in the Customers table).
    -   "Customers can place orders." -> Subject: Customers, Characteristic: Orders. (Orders can be broken down into products, amounts, etc., so "Orders" becomes a separate table/object).
-   **Requirement Gathering Practices:**
    -   **Waterfall:** A sequential approach where requirements are gathered once, then design, implementation, and testing follow. This is generally not recommended for data modeling as it leads to late feedback and costly rework.
    -   **Agile:** An iterative approach where stakeholders are involved throughout the development cycle. Small improvements are made, and feedback is incorporated continuously, reducing the risk of major reworks. This is the preferred method.

**Real-world Example: Online Superstore Requirements**

After gathering requirements for an online superstore, the following technical requirements are derived:

1.  **Customers:** Store customer information (ID, first name, last name, email, phone, date joined).
2.  **Products:** Store product information (ID, name, category ID, price, stock quantity).
3.  **Order Processing:** Store order details (ID, customer ID, order date, total amount).
4.  **Store Branch Management:** Store branch information (ID, name, location).
5.  **Payment Management:** Store payment details (ID, order ID, method, amount, status, transaction ID).
6.  **Shipment Tracking:** Store shipment details (ID, order ID, carrier, tracking number, status, delivery date).

### Database Design and Modeling: Logical Layer - Entity-Relationship (ER) Diagram

The logical layer translates the gathered requirements into a structured ER diagram.

1.  **Convert Requirements to Entities:** Each major subject or object from the requirements becomes an entity (which will eventually be a table).
    -   `Customers` -> `Customers` entity
    -   `Products` -> `Products` entity
    -   `Order Processing` -> `Orders` entity
    -   `Payment Management` -> `Payments` entity
    -   `Shipment Tracking` -> `Shipments` entity
    -   `Store Branch Management` -> `Store_Branch` entity (initially, `Store_Branch` details might be in `Orders`, but normalization will separate it).
2.  **Identify Attributes for Each Entity:** List all characteristics (columns) for each entity based on the requirements.
    -   **Identifier Column (ID):** For every entity, a unique identifier column (e.g., `customer_id`, `order_id`) is created. This acts like an ID card, uniquely identifying each record.
3.  **ER Diagram Tool:** `draw.io` (or `diagrams.net`) is a popular free online tool for creating ER diagrams.
    -   Use the "Entity Relation" tab to add table objects.
    -   Rename tables and add attributes (columns).
    -   Initially, focus on standalone entities without relationships or keys.

### Database Design and Modeling: Data Types

Data types define the kind of data a column can hold. Choosing the correct data type is crucial for storage efficiency and data integrity.

-   **Numeric Data Types:** For numbers.
    -   `INTEGER`: Whole numbers (e.g., 10, -20). Range: approx. -2 billion to +2 billion.
    -   `BIGINT`: Larger whole numbers, used when `INTEGER` range is insufficient (e.g., for billions of records, common in OLAP).
    -   `FLOAT`: Floating-point numbers with decimals (e.g., 10.5). Single-precision, offering about 7 decimal digits of accuracy. Approximate.
    -   `DOUBLE`: Double-precision floating-point numbers, more accurate and wider range than `FLOAT` (around 15-16 decimal digits). Preferred for scientific, financial, or engineering calculations where high accuracy is vital. Approximate.
    -   `NUMERIC(P, S)`: Exact numeric data type. `P` is the total number of digits, `S` is the number of digits after the decimal point. Essential for currency or precise financial values where approximations are unacceptable.
-   **String Data Types:** For text and characters.
    -   `CHAR(N)`: Fixed-length string. If you define `CHAR(10)` and store "Arden" (5 chars), it will still occupy 10 characters in memory, padding with spaces. Useful when the exact length is known (e.g., country codes like `CA`).
    -   `VARCHAR(N)`: Variable-length string. If you define `VARCHAR(10)` and store "Arden", it will only occupy 5 characters. `N` sets the upper limit. Saves memory for variable-length text.
    -   `TEXT`: Stores very large amounts of textual data (e.g., blog posts, file content).
    -   `NVARCHAR` (or `NCHAR`): Similar to `VARCHAR` but supports Unicode characters (multi-byte character types) for international text (non-English characters).
-   **Date/Time Data Types:**
    -   `DATE`: Stores only the date (YYYY-MM-DD).
    -   `TIME`: Stores only the time (HH:MM:SS).
    -   `TIMESTAMP`: Stores both date and time (YYYY-MM-DD HH:MM:SS).
-   **Other Data Types:**
    -   `BOOLEAN`: For true/false values.
    -   `JSONB`: For storing JSON documents (database-specific, e.g., in PostgreSQL).
    -   `UUID`: Universally Unique Identifier (database-specific).

### Database Design and Modeling: Keys

Keys are fundamental for identifying records and establishing relationships.

-   **Primary Key (PK):**
    -   Uniquely identifies each record (row) in a table.
    -   Must contain unique values (no duplicates).
    -   Cannot contain `NULL` values (must be `NOT NULL`).
    -   A table can have only **one** primary key.
    -   **Example:** `student_id` in a `Students` table.
-   **Candidate Key:**
    -   Any column or set of columns that can uniquely identify a record.
    -   Must be unique and `NOT NULL`.
    -   All primary keys are candidate keys.
    -   **Example:** In a `Students` table, both `student_id` and `email` might be unique and `NOT NULL`, making them both candidate keys.
-   **Alternate Key:**
    -   A candidate key that is *not* chosen as the primary key.
    -   **Example:** If `student_id` is chosen as the primary key, then `email` (if it's a candidate key) becomes an alternate key.
-   **Super Key:**
    -   Any set of columns that can uniquely identify a record.
    -   It's a superset of candidate keys; it can include redundant columns.
    -   **Example:** `student_id`, `(student_id, email)`, `(student_id, email, name)` are all super keys if `student_id` alone is unique.
    -   **Relationship:** Super Key (all combinations) -> Candidate Key (minimal unique combinations) -> Primary Key (the chosen candidate key).
-   **Unique Key:**
    -   Similar to a primary key in that it enforces uniqueness for a column.
    -   **Difference:** A unique key *can* allow one `NULL` value (whereas a primary key cannot).
    -   **Example:** An `email` column might be a unique key, allowing one user to not have an email, but all other emails must be unique.
-   **Foreign Key (FK):**
    -   A column (or set of columns) in one table (the "child" table) that refers to the primary key of another table (the "parent" table).
    -   Establishes a link or relationship between two tables.
    -   Does not need to be unique or `NOT NULL` in the child table.
    -   **Rules:**
        1.  It is the bridge connecting tables.
        2.  It always lives in the child table.
        3.  It always flows from the child table to the parent table.
    -   **Example:** `customer_id` in an `Orders` table, which refers to `customer_id` (PK) in the `Customers` table.
-   **Composite Primary Key:**
    -   A primary key that consists of two or more columns whose values, when combined, uniquely identify each record.
    -   Used when no single column can uniquely identify a record.
    -   **Example:** In an `Order_Items` table, `(order_id, product_id)` might form a composite primary key because an order can have multiple products, and a product can be in multiple orders.

### Database Design and Modeling: Relationships (Cardinality)

Cardinality defines the number of instances of one entity that can be associated with the number of instances of another entity. It's represented using "crow's foot" notation in ER diagrams.

-   **Cardinality Types:**
    -   **Zero (Optional):** Represented by a circle (O). Means zero instances.
    -   **One (Mandatory):** Represented by a single line (|). Means exactly one instance.
    -   **Many:** Represented by a crow's foot symbol. Means one or more instances.
-   **Relationship Types:**
    -   **One-to-One (1:1):** One instance of Entity A relates to exactly one instance of Entity B, and vice-versa.
        -   **Example:** A `Department` has one `Manager`, and a `Manager` manages one `Department`.
        -   Notation: `|--|` (one to one)
    -   **One-to-Many (1:N) or Many-to-One (N:1):** One instance of Entity A relates to many instances of Entity B, but one instance of Entity B relates to only one instance of Entity A.
        -   **Example:** One `Customer` can place many `Orders`, but each `Order` is placed by only one `Customer`.
        -   Notation: `|--<` (one to many) or `<--|` (many to one)
    -   **Many-to-Many (N:M):** Many instances of Entity A relate to many instances of Entity B, and vice-versa.
        -   **Example:** One `Order` can contain many `Products`, and one `Product` can be part of many `Orders`.
        -   **Problem:** Many-to-many relationships cannot be directly implemented in relational databases.
        -   **Solution:** Introduce an **associative entity** (also called a "bridge" or "junction" table) between the two entities. This associative entity will have a composite primary key consisting of the primary keys of the two original entities.
        -   **Example:** For `Orders` and `Products`, create an `Order_Items` table. `Orders` will have a one-to-many relationship with `Order_Items`, and `Products` will have a one-to-many relationship with `Order_Items`. The `Order_Items` table will contain `order_id` and `product_id` (forming a composite primary key).

**Building the ER Diagram with Relationships (Online Superstore Example):**

1.  **Customers and Orders:** One customer can place many orders. Each order belongs to one customer.
    -   Relationship: `Customers` `1--<` `Orders` (One-to-Many)
    -   Foreign Key: `customer_id` in `Orders` table.
2.  **Orders and Payments:** One order can have multiple payments (e.g., partial payments, gift card + credit card). Each payment belongs to one order.
    -   Relationship: `Orders` `1--<` `Payments` (One-to-Many)
    -   Foreign Key: `order_id` in `Payments` table.
3.  **Orders and Shipments:** One order can have multiple shipments (e.g., products shipped separately, or tracking history). Each shipment belongs to one order.
    -   Relationship: `Orders` `1--<` `Shipments` (One-to-Many)
    -   Foreign Key: `order_id` in `Shipments` table.
4.  **Orders and Products (Many-to-Many):**
    -   **Associative Entity:** `Order_Items`
    -   Relationships:
        -   `Orders` `1--<` `Order_Items` (One-to-Many)
        -   `Products` `1--<` `Order_Items` (One-to-Many)
    -   Foreign Keys: `order_id` and `product_id` in `Order_Items` table. `(order_id, product_id)` forms a composite primary key in `Order_Items`.

### Database Design and Modeling: Normalization

Normalization is the process of organizing the columns and tables of a relational database to minimize data redundancy and improve data integrity. It follows a series of "normal forms."

-   **Goal:** Reduce data redundancy (saving storage cost) and improve data integrity.
-   **Iterative Nature:** To achieve a higher normal form, all criteria of the preceding normal forms must be met.
-   **Industry Standard:** Most OLTP databases aim for at least the Third Normal Form (3NF).

1.  **First Normal Form (1NF):**
    -   **Rules:**
        1.  Each table must have a primary key.
        2.  Each column must contain atomic (single) values; no repeating groups or lists within a single column.
    -   **Problem Example:** An `Orders` table with a `phones` column containing multiple phone numbers or a `products` column containing a list of products.
    -   **Solution:** Split multi-valued columns into separate rows or create new tables. This often leads to a composite primary key.
        -   **Example:** For the `Orders` table with multiple products and phones, each product/phone combination gets its own row. The primary key becomes `(order_id, phone, product)`.

2.  **Second Normal Form (2NF):**
    -   **Rules:**
        1.  Must be in 1NF.
        2.  No partial dependency: Every non-key attribute (column not part of the primary key) must be fully dependent on the *entire* primary key. This rule applies only to tables with composite primary keys.
    -   **Problem Example:** In a table with `(order_id, product_id)` as a composite primary key, if `customer_name` is also in the table, `customer_name` is only dependent on `order_id` (a part of the primary key), not `product_id`. This is a partial dependency.
    -   **Solution:** Split the table into two: one for `(order_id, customer_name)` and another for `(order_id, product_id, product_name, quantity, unit_price)`.

3.  **Third Normal Form (3NF):**
    -   **Rules:**
        1.  Must be in 2NF.
        2.  No transitive dependency: Every non-key attribute must be directly dependent on the primary key, not on another non-key attribute.
    -   **Problem Example:** In a table with `(order_id, product_id)` as a composite primary key, if `category_id` and `category_name` are included, `category_name` is dependent on `category_id` (a non-key attribute), not directly on `(order_id, product_id)`. This is a transitive dependency.
    -   **Solution:** Create a separate table for the transitively dependent attributes.
        -   **Example:** For the `Orders` table, `store_branch_name` and `store_branch_location` are dependent on `store_branch_id` (a non-key attribute in `Orders`). To achieve 3NF, create a separate `Store_Branch` table with `store_branch_id` as its primary key, and `store_branch_name`, `store_branch_location` as its attributes. The `Orders` table will then only contain `store_branch_id` as a foreign key.

4.  **Boyce-Codd Normal Form (BCNF):**
    -   **Rules:**
        1.  Must be in 3NF.
        2.  Every determinant must be a super key. A "determinant" is any attribute or set of attributes that determines another attribute.
    -   **Problem Example:** A `Courses` table with `(student, course, instructor)` where `(student, course)` is the primary key. If `instructor` determines `course` (one instructor teaches one course), but `instructor` is not a super key (can have duplicates), then it violates BCNF.
    -   **Solution:** Decompose the table to remove the functional dependency where the determinant is not a super key.
        -   **Example:** Split into `(student, course)` and `(instructor, course)`.
    -   **Practicality:** BCNF is a stricter form of 3NF. While important conceptually, it's not always strictly followed in industry, especially with cheaper storage and higher compute power, as it can sometimes lead to more joins and slightly reduced performance. 3NF is typically the industry standard.

### Database Design and Modeling: Physical Layer (Implementation)

Once the logical ER diagram is complete and normalized, the physical layer involves translating this design into actual SQL code to create the database schema.

-   **Manual SQL Scripting:** Traditionally, data professionals write `CREATE TABLE` statements, define columns with appropriate data types, and add primary key, foreign key, and other constraints. This can be time-consuming for complex schemas.
-   **AI-Assisted Script Generation:** Modern LLMs (like ChatGPT) can generate SQL scripts directly from an ER diagram image.
    -   **Process:** Upload the ER diagram image to the LLM and prompt it to generate a SQL script for a specific database (e.g., MySQL).
    -   **Benefits:** Automates the creation of tables, data types, and especially constraints (primary and foreign keys), significantly reducing development time.
    -   **Review:** Always review the generated script for accuracy, optimal data types, and adherence to specific naming conventions or schema requirements before execution.

## Database Design and Modeling: OLAP Data Modeling (Data Warehousing)

OLAP data modeling focuses on designing databases for analytical purposes, often referred to as data warehousing. It typically involves denormalized structures to optimize for query performance over storage efficiency.

### Database Design and Modeling: Medallion Architecture (Bronze, Silver, Gold Layers)

Modern data warehouses often employ a "Medallion Architecture" to process and store data in layers, moving from raw to refined.

1.  **Bronze Layer (Raw Layer):**
    -   **Purpose:** Ingests raw data directly from OLTP sources without any modifications or transformations. It acts as a replica of the source system.
    -   **Loading:** Data is typically loaded incrementally (only new or changed records) rather than full dumps, to save resources.
    -   **Frequency:** Can be daily, hourly, or real-time, depending on the ingestion architecture.
    -   **Content:** Contains all tables from the OLTP source that are relevant for the data warehouse.

2.  **Silver Layer (Transformed/Enriched Layer):**
    -   **Purpose:** Stores transformed, cleaned, and enriched data.
    -   **Activities:** Data cleaning, standardization, joining data from multiple sources, and sometimes aggregation.
    -   **Modern Practice (OBT - One Big Table):** A common approach is to create one large, denormalized table by joining all necessary tables from the Bronze layer. This OBT is designed for performance, as it reduces the need for complex joins later.
    -   **Content:** A curated, clean, and often wide table ready for analytical modeling.

3.  **Gold Layer (Curated/Dimensional Layer):**
    -   **Purpose:** Stores the final, highly refined data models optimized for specific analytical use cases (e.g., finance, operations, sales reporting).
    -   **Modeling:** This is where dimensional data modeling (star schema, snowflake schema) is applied.
    -   **Characteristics:** Data is denormalized to prioritize query speed for reports and dashboards.
    -   **Content:** Fact tables and Dimension tables.

### Database Design and Modeling: Dimensional Data Modeling (Fact and Dimension Tables)

Dimensional modeling is the core of OLAP data modeling, structuring data into fact and dimension tables.

-   **Dimension Tables (Dim Tables):**
    -   **Purpose:** Provide context or descriptive attributes for the data. They answer "who, what, where, when, how."
    -   **Content:** Primarily textual or descriptive data (e.g., `product_name`, `category_name`, `customer_name`, `delivery_date`).
    -   **Rule:** Do not store aggregatable numbers (measures) in dimension tables. IDs (e.g., `customer_id`, `product_id`) are allowed as they are not aggregated.
    -   **Structure:** Often denormalized; related contextual information can be stored in a single dimension table (e.g., `category_name` and `subcategory_name` in `Dim_Products`).
    -   **Examples:** `Dim_Customers`, `Dim_Products`, `Dim_Date`, `Dim_Payments`, `Dim_Shipments`, `Dim_Branch`.

-   **Fact Tables (Fact Tables):**
    -   **Purpose:** Store quantitative, measurable data (metrics or "facts") that can be aggregated. They answer "how much."
    -   **Content:** Primarily numeric values that can be summed, averaged, counted, etc. (e.g., `quantity_ordered`, `line_amount`, `total_amount`, `payment_amount`).
    -   **Structure:** Contains foreign keys to connect to dimension tables. These foreign keys are typically the IDs from the dimension tables.
    -   **Goal-Specific:** Fact tables are usually designed for a specific business process or goal (e.g., `Fact_Sales`, `Fact_Orders`).
    -   **Surrogate Keys:** Often, surrogate keys (simple, auto-incrementing integer IDs) are used in fact tables instead of the original, potentially complex, business IDs from dimension tables. This improves join performance and readability, though it's not strictly mandatory.
    -   **Example:** `Fact_Sales` or `Fact_Orders` containing `order_id`, `customer_id`, `product_id`, `quantity_ordered`, `line_amount`, `total_amount`.

### Database Design and Modeling: Star Schema vs. Snowflake Schema

These are two common types of dimensional models.

1.  **Star Schema:**
    -   **Structure:** A central fact table directly connected to multiple dimension tables. It resembles a star with the fact table at the center and dimensions radiating outwards.
    -   **Characteristics:**
        -   **Denormalized Dimensions:** Dimension tables are typically denormalized, containing all related descriptive attributes.
        -   **Simplicity:** Easy to understand, design, and query.
        -   **Performance:** Generally offers excellent query performance due to fewer joins (often single-table joins between fact and dimension).
    -   **Preference:** Most common and preferred in modern data warehousing due to its performance benefits and cheaper storage costs.

2.  **Snowflake Schema:**
    -   **Structure:** An extension of the star schema where dimension tables are further normalized into sub-dimensions. This creates a branching structure, resembling a snowflake.
    -   **Characteristics:**
        -   **Normalized Dimensions:** Dimension tables are normalized, leading to more tables.
        -   **Complexity:** More complex to design and maintain due to the increased number of tables and joins.
        -   **Performance:** Can sometimes lead to degraded query performance due to the need for more joins to retrieve contextual information.
    -   **Preference:** Less common in modern solutions compared to star schema, as the benefits of normalization (reduced redundancy) are often outweighed by the performance impact for analytical queries.

### Database Design and Modeling: Slowly Changing Dimensions (SCD)

Slowly Changing Dimensions (SCDs) are a technique for managing and tracking changes in dimension attributes over time. Dimension data is not static; customer addresses, product categories, or employee departments can change.

-   **SCD Type 0: Retain Original:**
    -   **Behavior:** No changes are tracked; the original value is always retained.
    -   **Use Case:** For attributes that are expected to never change (e.g., date of birth, initial credit score). Very rare for most attributes.

-   **SCD Type 1: Overwrite (Upsert):**
    -   **Behavior:** The old attribute value is overwritten with the new value. No history is preserved.
    -   **Implementation:** An `UPDATE` operation on the existing record.
    -   **Use Case:** When historical tracking is not required, and only the most current information is needed. Simplest to implement and maintain.

-   **SCD Type 2: Retain History (Add New Record):**
    -   **Behavior:** A new record is created for each change, preserving the full history of the attribute.
    -   **Implementation:** Add columns like `start_date`, `end_date`, and sometimes a `current_flag` or `version` number.
        -   When a change occurs: The `end_date` of the old record is updated to the date of change, and a new record is inserted with the new attribute values, a new `start_date`, and an `end_date` set to a very distant future date (e.g., 9999-01-01) or `NULL`.
    -   **Use Case:** When historical analysis is crucial (e.g., "What was the customer's state when they placed this order?"). This is a very popular SCD type.

-   **SCD Type 3: Add New Column (Limited History):**
    -   **Behavior:** A new column is added to the dimension table to store only the *previous* value of an attribute. Only a limited history (typically one previous state) is preserved.
    -   **Use Case:** When only the immediate previous state is needed for comparison, and full historical tracking is not required. Less common than Type 2.

-   **Higher SCD Types (4, 5, 6):** More complex types exist (e.g., Type 4 uses a separate history table, Type 5 combines Type 1 and Type 2), but they are rarely implemented in practice due to their complexity and maintenance overhead, especially with large datasets. Type 0, 1, 2, and 3 are the most relevant.

**Implementation of SCDs:**
SCDs can be implemented using various tools and techniques:
-   **SQL:** `MERGE` statements (for Type 1 upserts).
-   **Programming Languages:** PySpark, Spark SQL (for more complex logic, especially Type 2).
-   **Data Orchestration Tools:** DBT (Data Build Tool) often automates SCD implementation.

This concludes the comprehensive overview of database design and modeling, covering both OLTP and OLAP methodologies from conceptual understanding to practical application and modern architectural considerations.

---
